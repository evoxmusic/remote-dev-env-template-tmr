# Tomorro Internal Tooling & Integrations

When building apps, tools, and internal projects in this workspace, use the following tools and integrations by default. These are the standards used across Tomorro — follow them unless the user explicitly asks for something different.

---

## Authentication — Google OAuth 2.0

**All apps that require user login must use Google OAuth 2.0**, restricted to the `@tomorro.com` Google Workspace domain.

### Rules

- Always use Google as the OAuth provider — do not implement email/password auth
- Restrict sign-in to `@tomorro.com` accounts only (reject other domains)
- Use the `hd` (hosted domain) parameter to enforce domain restriction
- Store the user's Google profile info (name, email, avatar) in the app's session/database

### Environment Variables

```
GOOGLE_CLIENT_ID=your-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your-client-secret
NEXTAUTH_SECRET=your-random-secret
NEXTAUTH_URL=http://localhost:3100
```

### Recommended Setup — React + Vite

For Vite/React projects, use `@react-oauth/google`:

```bash
npm install @react-oauth/google jwt-decode
```

```jsx
// src/main.jsx
import { GoogleOAuthProvider } from '@react-oauth/google';

<GoogleOAuthProvider clientId={import.meta.env.VITE_GOOGLE_CLIENT_ID}>
  <App />
</GoogleOAuthProvider>
```

```jsx
// src/components/LoginButton.jsx
import { GoogleLogin } from '@react-oauth/google';
import { jwtDecode } from 'jwt-decode';

export function LoginButton({ onLogin }) {
  return (
    <GoogleLogin
      onSuccess={(response) => {
        const user = jwtDecode(response.credential);
        // Verify domain restriction
        if (user.hd !== 'tomorro.com') {
          alert('Please sign in with your @tomorro.com account');
          return;
        }
        onLogin(user);
      }}
      onError={() => console.error('Login failed')}
    />
  );
}
```

### Recommended Setup — Next.js

For Next.js projects, use `next-auth` with the Google provider:

```bash
npm install next-auth
```

```js
// app/api/auth/[...nextauth]/route.js
import NextAuth from 'next-auth';
import GoogleProvider from 'next-auth/providers/google';

const handler = NextAuth({
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
      authorization: {
        params: {
          hd: 'tomorro.com', // Restrict to Tomorro domain
        },
      },
    }),
  ],
  callbacks: {
    async signIn({ account, profile }) {
      // Only allow @tomorro.com accounts
      return profile?.hd === 'tomorro.com';
    },
  },
});

export { handler as GET, handler as POST };
```

### Recommended Setup — Backend Only (Node.js)

For backend API authentication, use `googleapis`:

```bash
npm install googleapis
```

```js
import { OAuth2Client } from 'google-auth-library';

const client = new OAuth2Client(process.env.GOOGLE_CLIENT_ID);

async function verifyToken(idToken) {
  const ticket = await client.verifyIdToken({
    idToken,
    audience: process.env.GOOGLE_CLIENT_ID,
  });
  const payload = ticket.getPayload();

  if (payload.hd !== 'tomorro.com') {
    throw new Error('Unauthorized: only @tomorro.com accounts are allowed');
  }

  return {
    email: payload.email,
    name: payload.name,
    avatar: payload.picture,
  };
}
```

---

## Database — PostgreSQL

**All apps that need persistent data storage must use PostgreSQL.**

### Rules

- Default to PostgreSQL for any project requiring a database
- Use **Prisma** as the ORM for TypeScript/Node.js projects
- For quick prototypes that don't need a real database yet, SQLite via Prisma is acceptable as a temporary stepping stone — but flag to the user that it should be migrated to PostgreSQL before deployment
- Always use environment variables for connection strings — never hardcode credentials
- When deploying via `qovery-deploy`, provision a PostgreSQL database alongside the app

### Environment Variables

```
DATABASE_URL=postgresql://user:password@host:5432/dbname
```

### Recommended Setup — Prisma

```bash
npm install prisma @prisma/client
npx prisma init
```

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

// Example model — adapt to the app's needs
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  avatar    String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

```bash
# Generate the Prisma client after defining models
npx prisma generate

# Create and apply migrations
npx prisma migrate dev --name init
```

```js
// src/lib/db.js
import { PrismaClient } from '@prisma/client';

const globalForPrisma = globalThis;
export const prisma = globalForPrisma.prisma ?? new PrismaClient();

if (process.env.NODE_ENV !== 'production') {
  globalForPrisma.prisma = prisma;
}
```

### SQLite Fallback (Prototyping Only)

For quick local prototyping when no PostgreSQL server is available:

```prisma
// prisma/schema.prisma — temporary SQLite config
datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}
```

Always inform the user: "I'm using SQLite for now so we can prototype quickly. When you're ready to deploy, we'll switch to PostgreSQL."

---

## Messaging & Notifications — Slack + Email

**Use Slack for internal team notifications and email for user-facing communications.**

### Slack — Internal Notifications

Use Slack Incoming Webhooks for simple notifications, or the Bolt SDK for interactive apps.

#### Environment Variables

```
SLACK_WEBHOOK_URL=https://hooks.slack.com/services/T.../B.../xxx
SLACK_BOT_TOKEN=xoxb-...        # Only if using Bolt SDK
SLACK_SIGNING_SECRET=...         # Only if using Bolt SDK
```

#### Simple Webhook Notification

```js
// src/lib/slack.js
export async function notifySlack(message, channel = null) {
  const payload = {
    text: message,
    ...(channel && { channel }),
  };

  const response = await fetch(process.env.SLACK_WEBHOOK_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(payload),
  });

  if (!response.ok) {
    console.error('Slack notification failed:', response.statusText);
  }
}

// Usage
await notifySlack('New deployment completed for *Project X* :rocket:');
```

#### Rich Slack Messages (Block Kit)

```js
await fetch(process.env.SLACK_WEBHOOK_URL, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    blocks: [
      {
        type: 'section',
        text: {
          type: 'mrkdwn',
          text: '*New contract submitted for review*',
        },
      },
      {
        type: 'section',
        fields: [
          { type: 'mrkdwn', text: '*Submitted by:*\nJean Dupont' },
          { type: 'mrkdwn', text: '*Type:*\nNDA' },
        ],
      },
    ],
  }),
});
```

### Email — User-Facing Notifications

Use `nodemailer` for sending emails. Keep the provider configurable via environment variables so it works with any SMTP service.

#### Environment Variables

```
SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USER=notifications@tomorro.com
SMTP_PASS=your-smtp-password
EMAIL_FROM="Tomorro <notifications@tomorro.com>"
```

#### Email Setup

```bash
npm install nodemailer
```

```js
// src/lib/email.js
import nodemailer from 'nodemailer';

const transporter = nodemailer.createTransport({
  host: process.env.SMTP_HOST,
  port: parseInt(process.env.SMTP_PORT || '587'),
  secure: false,
  auth: {
    user: process.env.SMTP_USER,
    pass: process.env.SMTP_PASS,
  },
});

export async function sendEmail({ to, subject, html }) {
  return transporter.sendMail({
    from: process.env.EMAIL_FROM || 'Tomorro <notifications@tomorro.com>',
    to,
    subject,
    html,
  });
}

// Usage
await sendEmail({
  to: 'user@tomorro.com',
  subject: 'Your contract has been approved',
  html: '<h1>Contract Approved</h1><p>Your NDA has been approved and is ready for signature.</p>',
});
```

### When to Use Which

| Scenario | Channel |
|----------|---------|
| Internal team alerts (deployments, errors, new submissions) | Slack |
| User-facing transactional messages (confirmations, approvals) | Email |
| Urgent internal alerts (system down, security) | Slack |
| Periodic reports / digests | Email |
| Interactive workflows (approvals via buttons) | Slack (Bolt SDK) |

---

## Design Assets — Figma

**Tomorro's design source of truth lives in Figma.** When building UI components, reference the Figma team library for accurate specifications.

### Rules

- Before building complex UI components, check if a Figma design exists for it
- The `tomorro-design-system` skill has the implementation-ready tokens (colors, fonts, Tailwind config) — use it as the primary reference for code
- Figma is the reference for layout decisions, spacing details, and component behavior that aren't captured in the design system skill
- If the user shares a Figma link or frame, use it as the spec for pixel-accurate implementation
- When in doubt between Figma and the design system skill, Figma takes precedence (it may have been updated more recently)

### Working with Figma Specs

When the user provides a Figma link or design:

1. Ask the user to describe the key elements or share a screenshot if you can't access the link directly
2. Map Figma layers to Tomorro design system tokens (colors, fonts, spacing)
3. Use the component patterns from the `tomorro-design-system` skill as building blocks
4. Match spacing and layout precisely — Figma measurements are the spec

---

## Environment Variables Summary

Every app built in this workspace should support these environment variables. Create a `.env.example` file in every project:

```bash
# Authentication (Google OAuth)
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
NEXTAUTH_SECRET=
NEXTAUTH_URL=http://localhost:3100

# Database (PostgreSQL)
DATABASE_URL=postgresql://user:password@localhost:5432/myapp

# Slack Notifications
SLACK_WEBHOOK_URL=

# Email (SMTP)
SMTP_HOST=
SMTP_PORT=587
SMTP_USER=
SMTP_PASS=
EMAIL_FROM="Tomorro <notifications@tomorro.com>"
```

Always create this `.env.example` file at project initialization so the user knows which credentials they need to provide. Never commit actual `.env` files — add `.env` to `.gitignore`.

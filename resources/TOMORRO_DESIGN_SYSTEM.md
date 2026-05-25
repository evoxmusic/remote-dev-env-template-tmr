# Tomorro Design System

## Default Behavior

**Apply this design system to EVERY project you build.** All apps, websites, landing pages, dashboards, and tools should use Tomorro's brand identity — colors, typography, component patterns, and logo — unless the user explicitly asks for a different style.

If the user says something like "use different colors" or "I don't want the Tomorro branding," then you may deviate. Otherwise, every pixel should feel like it belongs on [tomorro.com](https://www.tomorro.com).

---

## Brand Identity

Tomorro is an **AI-powered contract management platform** based in Paris. The visual identity is:
- **Clean and professional** — no clutter, generous whitespace
- **Modern and confident** — bold headings, crisp typography
- **Green-dominant** — vibrant green conveys trust, growth, and innovation
- **Dark mode friendly** — the brand works beautifully on both light and dark backgrounds

---

## Color Palette

### Primary Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `green-600` | `#32D200` | Primary actions, links, accents, CTAs |
| `green-900` | `#273F2B` | Dark backgrounds, navigation, footer |
| `green-500` | `#68EF3F` | Hover states, highlights, active elements |

### Extended Green Scale

| Token | Hex | Usage |
|-------|-----|-------|
| `green-100` | `#E7F9DD` | Light green backgrounds, success alerts |
| `green-200` | `#DFFFCB` | Subtle green tints |
| `green-300` | `#CAF1B1` | Light accent backgrounds |
| `green-700` | `#26A200` | Darker green text on light backgrounds |
| `green-800` | `#3E5B43` | Muted dark green |

### Neutral / Grey Scale

| Token | Hex | Usage |
|-------|-----|-------|
| `grey-100` | `#F2F5EB` | Light page backgrounds (light mode) |
| `grey-200` | `#D9DECA` | Borders, dividers |
| `grey-300` | `#B7BDA5` | Muted text, placeholders |
| `grey-500` | `#7E8371` | Secondary text, captions |
| `grey-900` | `#30322A` | Primary text (light mode) |
| `black` | `#000000` | Headings, high-emphasis text |
| `white` | `#FFFFFF` | Text on dark backgrounds, card surfaces |

### Surface Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `beige` | `#FAFBF7` | Page backgrounds (light mode) |
| `dark-900` | `#061302` | Page backgrounds (dark mode) |
| `dark-700` | `#585B4E` | Secondary dark surfaces |
| `dark-400` | `#979C89` | Muted text on dark backgrounds |

### Accent Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `orange` | `#F56C17` | Warnings, attention-drawing highlights |
| `yellow-bg` | `#FEFDE5` | Info/notice backgrounds |

---

## Typography

### Font Families

| Purpose | Font | Fallback | CSS |
|---------|------|----------|-----|
| Display headings (h1, h2) | **Ozik** | Arial, sans-serif | `font-family: 'Ozik', Arial, sans-serif` |
| Body, UI, subheadings (h3–h6, p, buttons, inputs) | **Aeonik** | Arial, sans-serif | `font-family: 'Aeonik', Arial, sans-serif` |
| AI/accent text (quotes, special callouts) | **Instrument Serif** | Georgia, sans-serif | `font-family: 'Instrument Serif', Georgia, sans-serif` |

### Font Loading

If font files are available in the project (check `/public/fonts/`), use `@font-face`:

```css
@font-face {
  font-family: 'Ozik';
  src: url('/fonts/ozik-regular.woff2') format('woff2');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'Ozik';
  src: url('/fonts/ozik-bold.woff2') format('woff2');
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'Aeonik';
  src: url('/fonts/aeonik-regular.woff2') format('woff2');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'Aeonik';
  src: url('/fonts/aeonik-medium.woff2') format('woff2');
  font-weight: 500;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'Aeonik';
  src: url('/fonts/aeonik-bold.woff2') format('woff2');
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}
```

**If font files are NOT available**, use these Google Fonts as fallback:
- **Inter** (replaces Aeonik) — `https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;700&display=swap`
- **Sora** (replaces Ozik) — `https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700&display=swap`
- **Instrument Serif** is available on Google Fonts — `https://fonts.googleapis.com/css2?family=Instrument+Serif&display=swap`

### Type Scale

| Token | Size | Line Height | Usage |
|-------|------|-------------|-------|
| `h1` | `5rem` (80px) | `4.5rem` | Hero headings (Ozik) |
| `h2` | `3.5rem` (56px) | `3rem` | Section headings (Ozik) |
| `h3` | `2.5rem` (40px) | `2.75rem` | Subsection headings (Aeonik) |
| `h4` | `1.5rem` (24px) | `2rem` | Card titles (Aeonik) |
| `h5` | `1.25rem` (20px) | `1.5rem` | Small headings (Aeonik) |
| `h6` | `0.75rem` (12px) | `1rem` | Labels, overlines (Aeonik) |
| `text-xlarge` | `2rem` (32px) | `2.5rem` | Large body text |
| `text-large` | `1.5rem` (24px) | `2rem` | Lead paragraphs |
| `text-medium` | `1.125rem` (18px) | `1.75rem` | Standard body |
| `text-regular` | `1rem` (16px) | `1.5rem` | Default body |
| `text-small` | `0.875rem` (14px) | `1.25rem` | Captions, metadata |
| `text-tiny` | `0.75rem` (12px) | `1.25rem` | Fine print, badges |

### Font Weights

| Token | Value | Usage |
|-------|-------|-------|
| `light` | `300` | Subtle text, large display text |
| `regular` | `400` | Body text, paragraphs |
| `medium` | `500` | UI elements, buttons, labels |
| `bold` | `700` | Headings, emphasis |

---

## Tailwind CSS Configuration

When creating a Tailwind project, use this configuration to map Tomorro's design tokens:

```js
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {
      colors: {
        tomorro: {
          'green-100': '#E7F9DD',
          'green-200': '#DFFFCB',
          'green-300': '#CAF1B1',
          'green-500': '#68EF3F',
          'green-600': '#32D200',
          'green-700': '#26A200',
          'green-800': '#3E5B43',
          'green-900': '#273F2B',
          'grey-100': '#F2F5EB',
          'grey-200': '#D9DECA',
          'grey-300': '#B7BDA5',
          'grey-500': '#7E8371',
          'grey-900': '#30322A',
          'beige': '#FAFBF7',
          'dark-900': '#061302',
          'dark-700': '#585B4E',
          'dark-400': '#979C89',
          'orange': '#F56C17',
          'yellow-bg': '#FEFDE5',
        },
      },
      fontFamily: {
        display: ['Ozik', 'Sora', 'Arial', 'sans-serif'],
        body: ['Aeonik', 'Inter', 'Arial', 'sans-serif'],
        accent: ['"Instrument Serif"', 'Georgia', 'sans-serif'],
      },
      fontSize: {
        'h1': ['5rem', { lineHeight: '4.5rem' }],
        'h2': ['3.5rem', { lineHeight: '3rem' }],
        'h3': ['2.5rem', { lineHeight: '2.75rem' }],
        'h4': ['1.5rem', { lineHeight: '2rem' }],
        'h5': ['1.25rem', { lineHeight: '1.5rem' }],
        'h6': ['0.75rem', { lineHeight: '1rem' }],
      },
    },
  },
  plugins: [],
}
```

Usage examples:
- `bg-tomorro-green-600` — primary green background
- `text-tomorro-grey-900` — dark text
- `font-display` — Ozik headings
- `font-body` — Aeonik body text
- `text-h1` — hero heading size

---

## Component Patterns

### Buttons

**Primary button** (main CTAs):
```html
<button class="bg-tomorro-green-600 hover:bg-tomorro-green-500 text-white font-body font-medium
               px-6 py-3 rounded-lg transition-colors duration-200">
  Get Started
</button>
```

**Secondary button** (dark):
```html
<button class="bg-tomorro-green-900 hover:bg-tomorro-green-800 text-white font-body font-medium
               px-6 py-3 rounded-lg transition-colors duration-200">
  Learn More
</button>
```

**Ghost button** (outline):
```html
<button class="border border-tomorro-grey-200 hover:border-tomorro-green-600 text-tomorro-grey-900
               hover:text-tomorro-green-600 font-body font-medium px-6 py-3 rounded-lg
               transition-colors duration-200">
  Cancel
</button>
```

### Cards

**Light card:**
```html
<div class="bg-white border border-tomorro-grey-200 rounded-xl p-6 shadow-sm">
  <h3 class="font-body font-bold text-tomorro-grey-900 text-lg mb-2">Card Title</h3>
  <p class="font-body text-tomorro-grey-500">Card description text goes here.</p>
</div>
```

**Dark card:**
```html
<div class="bg-tomorro-green-900 rounded-xl p-6">
  <h3 class="font-body font-bold text-white text-lg mb-2">Card Title</h3>
  <p class="font-body text-tomorro-dark-400">Card description text goes here.</p>
</div>
```

### Navigation

```html
<nav class="bg-tomorro-green-900 px-6 py-4">
  <div class="max-w-7xl mx-auto flex items-center justify-between">
    <!-- Tomorro Logo (see Logo section) -->
    <div class="flex items-center gap-8">
      <a href="/" class="text-white font-body font-medium hover:text-tomorro-green-300 transition-colors">Home</a>
      <a href="/about" class="text-white font-body font-medium hover:text-tomorro-green-300 transition-colors">About</a>
    </div>
    <button class="bg-tomorro-green-600 hover:bg-tomorro-green-500 text-white font-body font-medium
                   px-5 py-2.5 rounded-lg transition-colors duration-200">
      Sign In
    </button>
  </div>
</nav>
```

### Form Inputs

```html
<div class="space-y-2">
  <label class="block font-body font-medium text-tomorro-grey-900 text-sm">Email</label>
  <input type="email" placeholder="you@example.com"
         class="w-full border border-tomorro-grey-200 rounded-lg px-4 py-3 font-body text-tomorro-grey-900
                placeholder:text-tomorro-grey-300 focus:outline-none focus:ring-2 focus:ring-tomorro-green-600
                focus:border-transparent transition-all" />
</div>
```

### Status Indicators

| Status | Color | Class |
|--------|-------|-------|
| Success | `#32D200` | `bg-tomorro-green-600 text-white` |
| Warning | `#F56C17` | `bg-tomorro-orange text-white` |
| Info | `#FEFDE5` | `bg-tomorro-yellow-bg text-tomorro-grey-900` |
| Error | `#EF4444` | `bg-red-500 text-white` |

---

## Logo

### SVG — White (for dark backgrounds)

```html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 168 25" fill="white" class="h-6">
  <path fill-rule="evenodd" d="M33.62 15.855a3.52 3.52 0 1 0-.001-7.04 3.52 3.52 0 0 0 0 7.04Zm12.335-3.52c0 6.812-5.524 12.336-12.336 12.336s-12.335-5.524-12.335-12.335C21.284 5.523 26.807 0 33.619 0c6.812 0 12.336 5.524 12.336 12.335Zm56.012 0a3.52 3.52 0 1 1-7.039 0 3.52 3.52 0 0 1 7.039 0Zm-3.519 12.336c6.812 0 12.335-5.524 12.335-12.335C110.783 5.523 105.26 0 98.448 0c-6.812 0-12.335 5.524-12.335 12.335 0 6.812 5.52 12.336 12.335 12.336Zm60.565-12.335a3.52 3.52 0 1 1-7.039 0 3.52 3.52 0 0 1 7.039 0Zm-3.519 12.335c6.812 0 12.335-5.524 12.335-12.335C167.829 5.523 162.306 0 155.491 0c-6.814 0-12.335 5.524-12.335 12.335 0 6.812 5.524 12.336 12.335 12.336h.003ZM80.946 3.293C79.014 1.183 76.354.579 73.909.579h-4.565v4.238a7.644 7.644 0 0 0-1.088-1.524C66.324 1.183 63.664.579 61.219.579h-3.702V.575H48.32V24.03h9.197V9.776h3.74V24.03h9.197V9.776h3.49V24.03h9.197V9.719c0-1.868-.343-4.398-2.2-6.426h.005Zm34.169.136c1.932-2.11 4.592-2.713 7.037-2.713h4.563v9.197h-4.603v14.254h-9.197V9.856c0-1.868.344-4.399 2.198-6.427h.002ZM137.289.716c-2.445 0-5.105.601-7.037 2.713-1.856 2.028-2.2 4.559-2.2 6.427v14.31h9.197V9.917h4.603v-9.2h-4.565.002ZM14.604 9.836v14.093H5.407V9.836H0V.639h5.405V.634h9.197V.64h5.407v9.197h-5.407.002Z" clip-rule="evenodd"/>
</svg>
```

### SVG — Dark (for light backgrounds)

```html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 168 25" class="h-6">
  <path fill="#273F2B" fill-rule="evenodd" d="M33.62 15.855a3.52 3.52 0 1 0-.001-7.04 3.52 3.52 0 0 0 0 7.04Zm12.335-3.52c0 6.812-5.524 12.336-12.336 12.336s-12.335-5.524-12.335-12.335C21.284 5.523 26.807 0 33.619 0c6.812 0 12.336 5.524 12.336 12.335Zm56.012 0a3.52 3.52 0 1 1-7.039 0 3.52 3.52 0 0 1 7.039 0Zm-3.519 12.336c6.812 0 12.335-5.524 12.335-12.335C110.783 5.523 105.26 0 98.448 0c-6.812 0-12.335 5.524-12.335 12.335 0 6.812 5.52 12.336 12.335 12.336Zm60.565-12.335a3.52 3.52 0 1 1-7.039 0 3.52 3.52 0 0 1 7.039 0Zm-3.519 12.335c6.812 0 12.335-5.524 12.335-12.335C167.829 5.523 162.306 0 155.491 0c-6.814 0-12.335 5.524-12.335 12.335 0 6.812 5.524 12.336 12.335 12.336h.003ZM80.946 3.293C79.014 1.183 76.354.579 73.909.579h-4.565v4.238a7.644 7.644 0 0 0-1.088-1.524C66.324 1.183 63.664.579 61.219.579h-3.702V.575H48.32V24.03h9.197V9.776h3.74V24.03h9.197V9.776h3.49V24.03h9.197V9.719c0-1.868-.343-4.398-2.2-6.426h.005Zm34.169.136c1.932-2.11 4.592-2.713 7.037-2.713h4.563v9.197h-4.603v14.254h-9.197V9.856c0-1.868.344-4.399 2.198-6.427h.002ZM137.289.716c-2.445 0-5.105.601-7.037 2.713-1.856 2.028-2.2 4.559-2.2 6.427v14.31h9.197V9.917h4.603v-9.2h-4.565.002ZM14.604 9.836v14.093H5.407V9.836H0V.639h5.405V.634h9.197V.64h5.407v9.197h-5.407.002Z" clip-rule="evenodd"/>
</svg>
```

### Usage Rules

- Use the **white logo** on dark backgrounds (`green-900`, `dark-900`, or any dark surface)
- Use the **dark logo** (`green-900` fill) on light backgrounds (`beige`, `white`, `grey-100`)
- Minimum clear space: the height of the "t" character on all sides
- Do not distort, recolor with non-brand colors, or add effects to the logo

---

## Layout Principles

- **Max content width:** `max-w-7xl` (1280px), centered with `mx-auto`
- **Page padding:** `px-6` minimum on all screen sizes
- **Section spacing:** `py-16` to `py-24` between major sections
- **Card spacing:** `gap-6` to `gap-8` in grid layouts
- **Border radius:** `rounded-lg` (8px) for buttons/inputs, `rounded-xl` (12px) for cards/sections
- **Shadows:** Use sparingly — `shadow-sm` for cards, `shadow-md` for elevated elements (modals, dropdowns)

### Responsive Breakpoints

| Breakpoint | Width | Notes |
|------------|-------|-------|
| Mobile | `< 480px` | Stack everything, `h1` → `3.5rem` |
| Mobile landscape | `480px – 767px` | Two-column grids where possible |
| Tablet | `768px – 1023px` | Side navigation can appear |
| Desktop | `1024px – 1439px` | Full layout |
| Large desktop | `1440px+` | Max-width container, centered |

---

## Dark Mode

Tomorro supports both light and dark themes. When building:

**Light mode (default):**
- Background: `beige` (`#FAFBF7`) or `white`
- Text: `grey-900` (`#30322A`) for body, `black` for headings
- Cards: `white` with `grey-200` borders

**Dark mode:**
- Background: `dark-900` (`#061302`) or `green-900` (`#273F2B`)
- Text: `white` for body, `white` for headings
- Cards: `green-900` or slightly lighter dark surfaces
- Use `dark:` Tailwind prefix for dark mode variants

If the user doesn't specify a preference, default to **light mode** with a Tomorro-green navigation bar (`green-900`).

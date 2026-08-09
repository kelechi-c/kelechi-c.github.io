# Style Template

Design tokens for the personal site. Two palettes, same component CSS — swap the `:root`
block to switch themes. Full component styles live in `styling.css`.

## 1. Black/White (current site)

```css
:root{
  --background:#000000;   /* page */
  --surface:#111111;      /* cards / code blocks */
  --border:#2e2e2e;       /* dividers */
  --text:#ffffff;         /* primary text */
  --muted:#8a8a8a;        /* dates, footer, metadata */
  --accent:#ffffff;       /* links, hover borders */
  --accent-2:#c9c9c9;     /* link hover */
  --blue:#d4d4d4;         /* secondary accent */
  --green:#b0b0b0;        /* tertiary accent */
  --paper-3:#1c1c1c;      /* table headers */
  --ink-2:#b5b5b5;        /* secondary text (card descriptions) */
}
```

**Font:** SF Pro Medium — self-hosted `sfpromedium.otf`, loaded via `@font-face`, used for
body and headings (`font-weight: 500`). Code/labels: JetBrains Mono (system mono fallback).

## 2. Terracotta (decode-lab variant)

The original warm-dark theme from the decode-lab artifacts. Same structure, warmer
surfaces and a terracotta accent.

```css
:root{
  --background:#141413;   /* near-black page */
  --surface:#1f1e1d;      /* cards */
  --border:#3d3d3a;       /* dividers */
  --text:#faf9f5;         /* warm off-white */
  --muted:#73726c;        /* metadata */
  --accent:#d97757;       /* terracotta — links, hovers */
  --accent-2:#e89a7a;     /* lighter terracotta (hover) */
  --blue:#6a9bcc;         /* secondary accent */
  --green:#788c5d;        /* tertiary accent */
  --paper-3:#262624;      /* table headers */
  --ink-2:#b0aea5;        /* secondary text */
}
```

**Font:** Anthropic Sans / Anthropic Serif stacks (fallbacks: Arial / Georgia), mono labels
as above.

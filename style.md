# Style Template — decode-lab artifact theme

The dark design system used by the decode-lab artifact pages (`artifacts/*.html`), extracted
as a reusable template. This is a Claude-brand-inspired warm dark theme: near-black warm
backgrounds, terracotta accent, serif headings, mono labels.

## 1. Palette

Design tokens, defined on `:root`. Everything else in the template references these — never
hardcode a color in component rules.

```css
:root{
  /* surfaces — warm dark ramp */
  --background:#141413;   /* near-black page */
  --surface:#1f1e1d;      /* cards / alternate sections */
  --border:#3d3d3a;       /* subtle dividers */
  --text:#faf9f5;         /* warm off-white */
  --muted:#73726c;        /* metadata / secondary text */
  /* accents */
  --accent:#d97757;       /* terracotta orange — primary */
  --accent-2:#e89a7a;     /* lighter terracotta (hover, on dark) */
  --blue:#6a9bcc;         /* occasional secondary accent */
  --green:#788c5d;        /* occasional tertiary accent */
  /* layout aliases (older names kept so every rule picks up the scheme) */
  --paper:var(--background); --paper-2:var(--surface); --paper-3:#262624; --line:var(--border);
  --ink:var(--text); --ink-2:#b0aea5;
}
```

| token | value | use |
|---|---|---|
| `--background` / `--paper` | `#141413` | page background |
| `--surface` / `--paper-2` | `#1f1e1d` | cards, callouts, code bg |
| `--paper-3` | `#262624` | table headers, stepped panels |
| `--border` / `--line` | `#3d3d3a` | dividers, borders |
| `--text` / `--ink` | `#faf9f5` | primary text |
| `--ink-2` | `#b0aea5` | secondary text, lede, card body |
| `--muted` | `#73726c` | badges, metadata, footer |
| `--accent` | `#d97757` | primary accent (terracotta) |
| `--accent-2` | `#e89a7a` | hover accent, "go" links |

## 2. Typography

- **Body**: `"Anthropic Sans", Arial, sans-serif` — `17px`, `line-height: 1.68`
- **Headings**: `"Anthropic Serif", Georgia, serif` — `font-weight: 300` (light serif look)
- **Code / labels / numbers**: `"JetBrains Mono", monospace`

```css
body{font-family:"Anthropic Sans",Arial,sans-serif;line-height:1.68;font-size:17px}
h1,h2,h3,h4{font-family:"Anthropic Serif",Georgia,serif;font-weight:300;line-height:1.28}
h1{font-size:2.05rem;letter-spacing:-.01em}
code{font-family:"JetBrains Mono",monospace;font-size:.85em}
```

Fonts are not self-hosted — the stacks fall back to Arial/Georgia/system mono. Selection color:

```css
::selection{background:rgba(217,119,87,.32);color:var(--ink)}
```

## 3. Layout

```css
.wrap{max-width:880px;margin:0 auto;padding:56px 28px 80px}   /* single column */
/* lesson pages use a 2-col grid: .wrap{grid-template-columns:250px minmax(0,1fr)} */
```

## 4. Components

### Badge — pill label above the title

```css
.badge{display:inline-block;font-size:.72rem;letter-spacing:.22em;text-transform:uppercase;
  color:var(--muted);border:1px solid var(--line);border-radius:999px;padding:4px 14px;
  background:var(--surface)}
```

```html
<span class="badge">Triton from zero · decode-lab</span>
```

### Page header — h1 with accent dash

The `::after` dash is the signature header mark. The lede sits under the title as secondary text.

```css
h1::after{content:"";display:block;width:2.4rem;height:2px;background:var(--accent);margin-top:.5em;border-radius:2px}
.lede{font-size:1.08rem;color:var(--ink-2);max-width:56em}
```

```html
<header>
  <span class="badge">…</span>
  <h1>Page Title</h1>
  <p class="lede">one-line description in secondary text.</p>
</header>
```

### Cards — link grid with left accent bar

Used for indexes (lesson lists, article lists). Cards are `<a>` elements; the left accent bar
widens on hover, the whole card lifts, and the mono `.go` hint nudges right.

```css
.cards{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:16px;margin:2.2em 0}
.card{position:relative;display:block;overflow:hidden;background:var(--surface);
  border:1px solid var(--border);border-radius:14px;padding:20px 22px 18px 24px;
  text-decoration:none;color:var(--ink);
  transition:transform .15s,border-color .15s,box-shadow .15s}
.card::before{content:"";position:absolute;left:0;top:0;bottom:0;width:3px;
  background:var(--accent);transition:width .16s ease}
.card:hover{transform:translateY(-3px);border-color:var(--accent);box-shadow:0 6px 22px rgba(0,0,0,.34)}
.card:hover::before{width:6px}
.card .n{display:inline-block;font-family:"JetBrains Mono",monospace;font-size:.66rem;
  letter-spacing:.16em;color:var(--accent);text-transform:uppercase;
  background:rgba(217,119,87,.10);border:1px solid rgba(217,119,87,.34);
  border-radius:999px;padding:2px 10px;margin-bottom:11px}
.card h2{font-family:"Anthropic Serif",Georgia,serif;font-weight:300;font-size:1.3rem;
  margin:0 0 .4em;line-height:1.2;padding-right:2.6rem}
.card p{color:var(--ink-2);font-size:.93rem;margin:0}
.card .go{color:var(--accent-2);font-size:.82rem;margin-top:.75em;
  font-family:"JetBrains Mono",monospace;letter-spacing:.04em;transition:transform .15s}
.card:hover .go{transform:translateX(3px)}
```

```html
<div class="cards">
  <a class="card" href="…">
    <span class="n">Lesson 1 of 7</span>
    <h2>Title</h2>
    <p>short description.</p>
    <div class="go">open →</div>
  </a>
</div>
```

Variants used in the artifacts:
- `.card.dim{opacity:.75}` — dimmed/upcoming cards
- last card in a set switches the accent to blue: `.card:last-child::before{background:var(--blue)}`
  (capstone marker), with `.card:last-child .n` recolored to match
- a ghost chapter index can be added via CSS counters: `.card::after{counter-increment:lesson;
  content:counter(lesson,decimal-leading-zero); … opacity:.10}`

### Callout — bordered note box

```css
.callout{border:1px solid var(--line);border-left:4px solid var(--accent);
  background:var(--paper-2);border-radius:8px;padding:12px 18px;margin:1.4em 0}
.callout .ct{font-family:"Anthropic Serif",Georgia,serif;font-weight:600;font-size:.9rem;
  letter-spacing:.06em;text-transform:uppercase;color:var(--accent);display:block;margin-bottom:4px}
```

```html
<div class="callout idea">
  <span class="ct">The core idea</span>
  <p>…</p>
</div>
```

Variants: `.callout.gotcha{border-left-color:var(--ink-2)}` (and its `.ct` recolored),
`.callout.idea{border-left-color:var(--accent-2)}`.

### Footer

```css
footer{margin-top:2.4em;padding-top:1.1em;border-top:1px solid var(--line);
  color:var(--muted);font-size:.9rem}
```

## 5. Other conventions

- `li::marker{color:var(--accent)}` — list bullets pick up the accent
- Links: `a{color:var(--accent);text-underline-offset:3px}`, hover `var(--accent-2)`
- Tables: `background:var(--paper-2)`, `border:1px solid var(--line)`, header row
  `background:var(--paper-3)`, hover row `rgba(250,249,245,.025)`
- `details` disclosure panels: `border:1px solid var(--line);background:var(--paper-2)`,
  `summary` in serif with a `▸` marker that rotates `90deg` when open
- Selection: `::selection{background:rgba(217,119,87,.32)}`
- `html{scroll-padding-top:22px}` keeps anchored sections clear of sticky TOCs

## 6. Responsive

```css
@media(max-width:920px){
  .wrap{grid-template-columns:minmax(0,1fr)}   /* 2-col → single column */
}
@media(max-width:560px){
  body{font-size:16px}
  h1{font-size:1.6rem}
  .wrap{padding:30px 18px 60px}
}
```

## 7. Usage

Copy the tokens + components you need into a `styling.css`, then reference it:

```html
<link rel="stylesheet" href="styling.css">
```

The full template is implemented in `v2/styling.css` (personal site) and in the decode-lab
artifacts (`~/code/ml/decode-lab/artifacts/*.html`, which add lesson-only extras: sticky TOC,
code panels with copy buttons, syntax-token colors, widgets).

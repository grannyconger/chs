# Pit Crew Site Style Guide

House rules for building any new page in the Pit Crew family. Follow this and a new page will look like it shipped with the others.

Abbreviations used below:

- **CSS** = Cascading Style Sheets, the styling language
- **Custom property** = a CSS variable, written `--name`, read back with `var(--name)`
- **rem** = font size relative to the root element, 1rem = 16px by default
- **clamp()** = a CSS function taking `clamp(min, preferred, max)`, used for text that scales with screen width
- **ch** = width of the "0" character in the current font, used for line-length limits

---

## 1. Core principle

One typeface, one accent color at a time, square edges, generous whitespace. Every page is either **navy-dominant** (landing, hero) or **paper-dominant** (content, forms, documents). Never mix the two on one screen.

---

## 2. Tokens

Paste this block into `:root` on every page. Nothing outside this list is an approved color.

css

```css
:root {
  --paper:  #F4F3EF;   /* off-white page background */
  --navy:   #00205b;   /* school navy, headings and dark backgrounds */
  --blue:   #6495ed;   /* cornflower, focus rings and highlights on navy */
  --text:   #2E3949;   /* body copy on paper */
  --dim:    #6B7686;   /* secondary copy, labels, inactive tabs */
  --line:   #D9D7D0;   /* borders and rules on paper */
  --white:  #ffffff;

  /* additional athletics accent colors */
  --athletics-teal:  rgb(0, 113, 144);
  --athletics-aqua:  rgb(188, 228, 229);
  --athletics-tan:   rgb(194, 174, 147);
  --athletics-gold:  rgb(253, 191, 104);

  /* swapped at runtime, see section 3 */
  --accent:      #00205b;
  --accent-soft: rgba(0,32,91,0.08);
}
```

On a navy background, swap the neutrals: `--line` becomes `rgba(255,255,255,0.2)` and `--dim` becomes `rgba(255,255,255,0.65)`.

### Accent palette

Navy is the default accent. Use a distinct accent only when a page has multiple sibling sections that need to be told apart (four crews, four game categories, four event types).

| Use | Hex / RGB | Soft tint |
| --- | --- | --- |
| Default / school | `#00205b` | `rgba(0,32,91,0.08)` |
| Red | `#C0392B` | `rgba(192,57,43,0.08)` |
| Royal blue | `#1B4FA8` | `rgba(27,79,168,0.08)` |
| Teal | `#0E6E66` | `rgba(14,110,102,0.08)` |
| Purple | `#6B3FA0` | `rgba(107,63,160,0.08)` |
| Athletics teal | `rgb(0, 113, 144)` / `#007190` | `rgba(0,113,144,0.08)` |
| Athletics aqua | `rgb(188, 228, 229)` / `#BCE4E5` | `rgba(188,228,229,0.08)` |
| Athletics tan | `rgb(194, 174, 147)` / `#C2AE93` | `rgba(194,174,147,0.08)` |
| Athletics gold | `rgb(253, 191, 104)` / `#FDBF68` | `rgba(253,191,104,0.08)` |

Soft tint is always the accent at 8 percent alpha. Do not eyeball a lighter shade.

---

## 3. The accent swap

Every colored element reads `var(--accent)` rather than a literal hex value. Changing one property repaints the whole page.

js

```js
function setAccent(color, soft) {
  document.documentElement.style.setProperty('--accent', color);
  document.documentElement.style.setProperty('--accent-soft', soft);
}
```

Think of it like the main breaker in a house. Everything downstream is wired to one switch, so you flip it once instead of walking room to room.

Elements that must inherit the accent: divider, top band, section headers, list bullets, tab underline, buttons, spec box tint and left rule, progress fill, page title on a section page.

---

## 4. Typography

`font-family: Arial, Helvetica, sans-serif;` on `body`. No web fonts. No serif. No second family.

| RoleStyle               |                                                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------------- |
| Page title              | `clamp(3rem, 10vw, 5.5rem)`, uppercase, `line-height: 0.9`, `letter-spacing: -0.05em`, navy or accent |
| Section title           | `clamp(1.4rem, 4.5vw, 2rem)`, `line-height: 1.25`, `letter-spacing: -0.02em`, navy                    |
| Lead sentence           | `1.15rem` bold, navy                                                                                  |
| Body                    | `1.05rem`, `line-height: 1.6`                                                                         |
| List item               | `1rem`, `line-height: 1.5`                                                                            |
| Eyebrow / section label | `0.8rem` bold, uppercase, `letter-spacing: 0.15em`, accent                                            |
| Small print             | `0.9rem`, `--dim`                                                                                     |

Big type is tight, small type is loose. Negative letter-spacing on headlines, positive on labels. Cap paragraph width at `500px` centered or `62ch` left-aligned.

---

## 5. Layout

css

```css
body {
  margin: 0; min-height: 100vh;
  background: var(--paper); color: var(--text);
  display: flex; align-items: center; justify-content: center;
  padding: 40px 0;
}
main { width: min(90%, 700px); }
```

`min(90%, 700px)` means 90 percent of the screen on a phone, capped at 700px on a desktop. One container rule, no media query needed for width.

Vertical rhythm: 30px between blocks, 12px between a label and its content, 10px between stacked buttons or list rows.

---

## 6. Components

### Divider

The signature element. Every page has exactly one below the title.

css

```css
.divider { width: 60px; height: 5px; margin: 28px 0; background: var(--accent); }
.center .divider { margin: 28px auto; }
```

### Buttons

Two variants only. Square, uppercase, bold, 2px border, hover inverts fill and text.

css

```css
.button {
  display: inline-block; min-width: 190px; padding: 16px 24px;
  border: 2px solid var(--accent); background: var(--accent); color: var(--white);
  font: bold 1rem/1 Arial, sans-serif;
  text-transform: uppercase; letter-spacing: 0.05em;
  text-decoration: none; cursor: pointer;
  transition: background 0.15s ease, color 0.15s ease;
}
.button:hover { background: transparent; color: var(--accent); }
.button.secondary { background: transparent; color: var(--accent); }
.button.secondary:hover { background: var(--accent); color: var(--white); }

.links { display: flex; justify-content: center; gap: 14px; flex-wrap: wrap; }
```

On a navy background, the primary button fills with `--blue` and takes navy text.

### Top band

An 8px accent stripe at the top of a section page. Cheapest possible way to signal which section you are in.

css

```css
.band { height: 8px; background: var(--accent); margin-bottom: 26px; }
```

### Tabs

css

```css
.tabs { display: flex; border-bottom: 2px solid var(--line); margin-bottom: 34px; }
.tab {
  flex: 1; padding: 12px 4px; margin-bottom: -2px;
  border: none; border-bottom: 4px solid transparent; background: transparent;
  color: var(--dim); font: bold 0.8rem Arial, sans-serif;
  text-transform: uppercase; letter-spacing: 0.04em; cursor: pointer;
}
.tab.on { color: var(--accent); border-bottom-color: var(--accent); }
```

### Lists

Square bullets, never round, never a disc marker.

css

```css
ul { margin: 0; padding: 0; list-style: none; }
ul li {
  padding: 9px 0 9px 18px; position: relative;
  border-bottom: 1px solid var(--line);
}
ul li::before {
  content: ""; position: absolute; left: 0; top: 17px;
  width: 7px; height: 7px; background: var(--accent);
}
```

### Spec strip

Facts at a glance. Tinted background, accent rule on the left.

css

```css
.specs {
  display: flex; gap: 26px; flex-wrap: wrap;
  padding: 14px 16px; margin-bottom: 30px;
  background: var(--accent-soft); border-left: 4px solid var(--accent);
  font-size: 0.9rem; color: var(--dim);
}
.specs b { color: var(--navy); }
```

### Progress bar

css

```css
.progress { height: 4px; background: var(--line); }
.progress span { display: block; height: 100%; width: 0; background: var(--accent); transition: width 0.3s ease; }
```

### Selectable card

For quiz answers, game cards, or any pick-one row. Idle is white with a gray border, hover borders in accent, selected fills navy.

css

```css
.answer {
  display: flex; gap: 14px; width: 100%; text-align: left;
  padding: 16px 18px; border: 2px solid var(--line);
  background: var(--white); color: var(--text);
  font: 1rem/1.45 Arial, sans-serif; cursor: pointer;
}
.answer:hover { border-color: var(--accent); }
.answer.picked { background: var(--navy); border-color: var(--navy); color: var(--white); }
.answer.dim { opacity: 0.35; }
```

---

## 7. Page patterns

**Hero page** (landing, splash). Navy background, centered `main`, title, divider, one paragraph under 25 words, two buttons. Nothing else.

**Content page** (role descriptions, guides, rules). Paper background, left-aligned, in this order: band, tabs if siblings exist, eyebrow label, title, divider, bold lead sentence, one short paragraph, spec strip, then repeating `label + list` blocks, then buttons.

**Flow page** (quiz, form, wizard). Paper background. Counter label, progress bar, question, stacked selectable cards. One decision per screen.

---

## 8. Writing

- Titles are two or three words, uppercase.
- The lead sentence sells. The paragraph explains. Two sentences maximum, then stop.
- Bullets are fragments, not sentences. No trailing periods. Aim for 4 per list, 6 is the ceiling.
- Buttons name the outcome: "Take the Quiz", "Email Mr. Weaver". Never "Submit" or "Click Here".
- Write for a student skimming on a phone between classes. If a section takes more than 20 seconds to read, cut it.

---

## 9. Never

- Rounded corners. `border-radius` is not used anywhere in this system.
- Drop shadows, gradients, or glassmorphism.
- Emoji as the visual system.
- A second typeface or any web font import.
- More than one accent color visible on a single screen, except in a comparison chart.
- A hex value written inline when a token exists for it.
- Animation libraries. CSS transitions at 0.12s to 0.3s cover everything here.

---

## 10. Accessibility and responsive

css

```css
@media (max-width: 500px) {
  .links { flex-direction: column; }
  .links .button { width: 100%; }
}

@media (prefers-reduced-motion: reduce) {
  * { transition: none !important; }
}
```

- Every interactive element gets `:focus-visible { outline: 3px solid var(--blue); outline-offset: 2px; }`.
- Buttons that act on the page are `<button>`. Things that navigate are `<a>`. Do not style a `div` as a button.
- Build content with `textContent` rather than `innerHTML` when inserting text from data, so a stray character cannot break the page.
- Check every page at 375px wide before shipping.

---

## 11. Starter template

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Page Name | Pit Crew</title>
<link rel="icon" type="image/x-icon" href="/favicon.ico">
<style>
  :root {
    --paper:#F4F3EF; --navy:#00205b; --blue:#6495ed;
    --text:#2E3949; --dim:#6B7686; --line:#D9D7D0; --white:#fff;
    --athletics-teal:rgb(0,113,144);
    --athletics-aqua:rgb(188,228,229);
    --athletics-tan:rgb(194,174,147);
    --athletics-gold:rgb(253,191,104);
    --accent:#00205b; --accent-soft:rgba(0,32,91,0.08);
  }
  * { box-sizing: border-box; }
  body {
    margin:0; min-height:100vh; padding:40px 0;
    background:var(--paper); color:var(--text);
    font-family:Arial, Helvetica, sans-serif;
    display:flex; align-items:center; justify-content:center;
  }
  main { width:min(90%, 700px); }
  h1 {
    margin:0; color:var(--navy);
    font-size:clamp(3rem,10vw,5.5rem); line-height:0.9;
    letter-spacing:-0.05em; text-transform:uppercase;
  }
  .divider { width:60px; height:5px; margin:28px 0; background:var(--accent); }
  p { margin:0 0 24px; font-size:1.05rem; line-height:1.6; }
</style>
</head>
<body>
<main>
  <h1>Page Name</h1>
  <div class="divider"></div>
  <p>One sentence that says what this page is for.</p>
</main>
</body>
</html>
```

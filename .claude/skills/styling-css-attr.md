# CSS Properties for Email Templating

> A developer's reference guide for writing CSS that works across email clients.

---

## ✅ Layout & Box Model

- `width`, `max-width`, `min-width`
- `height`, `max-height`, `min-height`
- `margin`, `padding` (and all directional variants)
- `border`, `border-radius` ⚠️ *(limited in Outlook)*
- `box-sizing` ⚠️ *(limited support)*

---

## ✅ Typography

- `font-family`, `font-size`, `font-weight`, `font-style`
- `line-height`, `color`
- `text-align`, `text-decoration`, `text-transform`
- `letter-spacing`, `word-spacing`, `white-space`

---

## ✅ Background & Color

- `background-color`
- `background-image` ⚠️ *(Outlook blocks by default)*
- `background-repeat`, `background-position`, `background-size` ⚠️ *(limited)*

---

## ✅ Display & Visibility

- `display: block | inline | inline-block` *(avoid flex/grid)*
- `visibility`, `overflow`

---

## ✅ Table-Specific

- `border-collapse`, `border-spacing`
- `vertical-align`
- `cellpadding`, `cellspacing` *(HTML attributes)*

---

## ⚠️ Use With Caution

| Property | Issue |
|---|---|
| `border-radius` | Not supported in Outlook |
| `box-shadow` | Not supported in most clients |
| `flexbox / grid` | Not supported in Outlook |
| `position: relative/absolute` | Very limited support |
| `z-index` | Very limited support |
| `transform` | Not supported in Outlook |
| `transition / animation` | Only Gmail & Apple Mail |
| `calc()` | Limited support |
| `var()` | Not supported in most clients |
| `@media` queries | Not supported in Outlook desktop |

---

## ❌ Avoid Entirely

| Property | Reason |
|---|---|
| `float` | Unreliable — use tables instead |
| `display: flex / grid` | Broken in Outlook |
| `clip-path` | Not supported |
| `filter / backdrop-filter` | Not supported |
| CSS custom properties (`--var`) | Stripped by most clients |
| `::before / ::after` | Pseudo-elements not supported |

---

## 🏆 Golden Rules

1. **Inline styles first** — many clients strip `<style>` blocks
2. **Use tables for layout**, not divs
3. **Test in Outlook** — it uses Word's rendering engine
4. **Always have a plain-text fallback**
5. **Test across clients** — use Litmus or Email on Acid
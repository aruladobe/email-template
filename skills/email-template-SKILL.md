---
name: email-template
description: >
  Expert guidance for building cross-client HTML email templates from scratch or
  from reference designs — including full Light Mode and Dark Mode support.
  Use this skill whenever the user wants to create, edit, or audit an HTML email
  template including welcome emails, newsletters, transactional emails, or
  promotional campaigns. Trigger when the user mentions email design, email HTML,
  email clients, Outlook compatibility, responsive email, AJO email, dark mode email,
  multi-client email, email theme, email color scheme, or asks about specific email
  tags, padding, DOCTYPE, typography, or cross-client rendering. Also trigger when
  the user uploads a screenshot of an email and wants it rebuilt as HTML, or when
  they ask "can I use X in email?" or "does X work in email?".
---

# Email Template Skill
> Complete standards and guidelines for building production-grade,
> cross-client HTML email templates with Light Mode and Dark Mode support.

---

## SECTION A — FOUNDATIONS

---

## A1. Before Writing Any Code — Confirm These First

1. Target **max width** — default is `768px` per this skill
2. **Brand colors**, logo, and font preferences
3. **Required sections** — hero, CTA, FAQ, footer, etc.
4. **Email client priority** — Outlook, Gmail, Apple Mail, Yahoo?
5. Is there a **reference design** (screenshot / image) to match?
6. Does the template need **Dark Mode** support?

---

## A2. DOCTYPE Declaration

```html
<!DOCTYPE html>
<html lang="en"
      xmlns="http://www.w3.org/1999/xhtml"
      xmlns:v="urn:schemas-microsoft-com:vml"
      xmlns:o="urn:schemas-microsoft-com:office:office">
```

- Must be the **very first line** — nothing before it, not even a space
- `lang="en"` — required for accessibility
- `xmlns:v` and `xmlns:o` — required for **Outlook VML support**

---

## A3. Required Head Meta Tags

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="format-detection" content="telephone=no, date=no, address=no, email=no">
  <meta name="x-apple-disable-message-reformatting">
  <meta name="color-scheme" content="light dark">
  <meta name="supported-color-schemes" content="light dark">
  <title>Email Title</title>
  <!--[if mso]>
  <noscript><xml><o:OfficeDocumentSettings>
    <o:PixelsPerInch>96</o:PixelsPerInch>
  </o:OfficeDocumentSettings></xml></noscript>
  <![endif]-->
</head>
```

---

## A4. Preheader — Table Pattern (NOT div)

**Always use the table pattern.** The `<div>` preheader can render visibly in some
forward chains and Outlook versions. The `<td>` with inline `display:none` is more
reliably hidden across all clients and survives forward/reply stripping.

```html
<!-- PREHEADER -->
<table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0">
    <tr>
        <td
            style="display:none; max-height:0; overflow:hidden; mso-hide:all;
                   font-size:1px; line-height:1px; color:#f4f4f4;">
            Preheader text here.&#32;&#8199;&#65279;&#847;&zwnj;&nbsp;&#8199;&#65279;&#847;&zwnj;&nbsp;
        </td>
    </tr>
</table>
```

- Keep preheader under **90 characters**
- Pad with zero-width spaces (`&#8199;&#65279;&#847;&zwnj;&nbsp;`) to prevent body text bleeding into inbox preview
- Use `color:#f4f4f4` (matches page background) so it stays invisible if display:none is stripped

---

## A5. HTML Tags — What to Use and Avoid

### ALWAYS SAFE

| Tag | Usage | Key Rule |
|---|---|---|
| `<table>` | All layout | Always add `role="presentation"` |
| `<tr>` | Table row | — |
| `<td>` | Table cell | Primary spacing container |
| `<h1>` `<h2>` `<h3>` | Headings | Always inline `margin:0; padding:0` |
| `<p>` | Body text | Never use `padding` — Outlook ignores it; use `margin` for spacing |
| `<span>` | Inline styling | Safest inline tag — no default margins |
| `<a>` | Links | Always `https://`; add `target="_blank" rel="noopener noreferrer"` |
| `<img>` | Images | Always set `alt`, `width`, `height` attr, `display:block` |
| `<strong>` `<em>` | Emphasis | Or use equivalent inline CSS |

### NEVER USE

| Tag | Reason |
|---|---|
| `<div>` for layout | Outlook ignores — use `<td>` |
| `<section>` `<article>` `<header>` | Not supported in most clients |
| `<video>` `<audio>` | Stripped by most clients |
| `<form>` `<input>` `<button>` | Stripped by Gmail and Outlook |
| `<iframe>` `<script>` `<canvas>` | Blocked by all clients |
| `<link>` | Stylesheets stripped by Gmail |
| `<svg>` | Blocked by Outlook and Gmail |

---

## A6. Padding Rules

```
SAFE:   padding on <td>       ✅ supported by ALL email clients
AVOID:  padding on <p>        ❌ Outlook ignores it — use margin instead
AVOID:  padding on <div>      ❌ Outlook ignores it
AVOID:  margin for layout     ❌ unreliable in Outlook — use <td> padding
```

---

## SECTION B — LAYOUT STRUCTURE

---

## B1. Layout Standards

- **Max width:** `768px` desktop/tablet; `100%` fluid on mobile
- **Wrapper:** outer `<table width="100%">` → inner content table `max-width:768px`
- **Layout engine:** `<table>` `<tr>` `<td>` only — never `<div>` for columns
- `border-collapse:collapse` on ALL tables
- `cellpadding="0" cellspacing="0" border="0"` on ALL tables
- `role="presentation"` on ALL layout tables

---

## B2. Full Boilerplate

```html
<!DOCTYPE html>
<html lang="en"
      xmlns="http://www.w3.org/1999/xhtml"
      xmlns:v="urn:schemas-microsoft-com:vml"
      xmlns:o="urn:schemas-microsoft-com:office:office">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="format-detection" content="telephone=no, date=no, address=no, email=no">
  <meta name="x-apple-disable-message-reformatting">
  <meta name="color-scheme" content="light dark">
  <meta name="supported-color-schemes" content="light dark">
  <title>Email Title</title>
  <!--[if mso]>
  <noscript><xml><o:OfficeDocumentSettings>
    <o:PixelsPerInch>96</o:PixelsPerInch>
  </o:OfficeDocumentSettings></xml></noscript>
  <![endif]-->
  <style type="text/css">
    body,table,td,p,h1,h2,h3,a{-webkit-text-size-adjust:100%;-ms-text-size-adjust:100%;}
    table,td{mso-table-lspace:0pt;mso-table-rspace:0pt;border-collapse:collapse;}
    img{border:0;outline:none;text-decoration:none;-ms-interpolation-mode:bicubic;}
    a[x-apple-data-detectors]{color:inherit!important;text-decoration:none!important;
      font-size:inherit!important;font-family:inherit!important;
      font-weight:inherit!important;line-height:inherit!important;}

    /* IMAGE SWAP — dark mode. Apply class directly on <img>, not wrapper divs */
    .li  {display:block        !important;}  /* light block image  */
    .di  {display:none         !important;}  /* dark  block image  */
    .li-i{display:inline-block !important;}  /* light inline image */
    .di-i{display:none         !important;}  /* dark  inline image */

    @media(prefers-color-scheme:dark){
      /* Background */
      .bg-pg{background-color:#1A1A1A!important;}
      .bg-bd{background-color:#1A1A1A!important;}
      .bg-ft{background-color:#2A2A2A!important;}
      .bg-cp{background-color:#1A1A1A!important;}
      /* Text */
      .tx{color:#E8E8E8!important;}
      .tw{color:#FFFFFF!important;}
      .tm{color:#AAAAAA!important;}
      .tl{color:#6B9FFF!important;}
      .tn{color:#BBBBBB!important;}
      /* Image swap */
      .li  {display:none         !important;}
      .di  {display:block        !important;}
      .li-i{display:none         !important;}
      .di-i{display:inline-block !important;}
    }
    /* Duplicate all @media rules for Outlook App dark mode */
    [data-ogsc] .bg-pg{background-color:#1A1A1A!important;}
    [data-ogsc] .bg-bd{background-color:#1A1A1A!important;}
    [data-ogsc] .bg-ft{background-color:#2A2A2A!important;}
    [data-ogsc] .bg-cp{background-color:#1A1A1A!important;}
    [data-ogsc] .tx{color:#E8E8E8!important;}
    [data-ogsc] .tw{color:#FFFFFF!important;}
    [data-ogsc] .tm{color:#AAAAAA!important;}
    [data-ogsc] .tl{color:#6B9FFF!important;}
    [data-ogsc] .tn{color:#BBBBBB!important;}
    [data-ogsc] .li  {display:none         !important;}
    [data-ogsc] .di  {display:block        !important;}
    [data-ogsc] .li-i{display:none         !important;}
    [data-ogsc] .di-i{display:inline-block !important;}
  </style>
</head>
<body style="margin:0;padding:0;background-color:#F0F0F0;" class="bg-pg">

  <!-- PREHEADER -->
  <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0">
    <tr>
      <td style="display:none; max-height:0; overflow:hidden; mso-hide:all;
                 font-size:1px; line-height:1px; color:#f4f4f4;">
        Preheader text here.&#32;&#8199;&#65279;&#847;&zwnj;&nbsp;
      </td>
    </tr>
  </table>

  <!-- OUTER WRAPPER -->
  <table role="presentation" cellpadding="0" cellspacing="0" border="0"
         width="100%" bgcolor="#F0F0F0" style="background-color:#F0F0F0;" class="bg-pg">
  <tr><td align="center" valign="top">

    <!-- INNER CONTENT TABLE: max 768px -->
    <table role="presentation" cellpadding="0" cellspacing="0" border="0"
           width="768" bgcolor="#FFFFFF"
           style="width:100%;max-width:768px;background-color:#FFFFFF;" class="bg-bd">

      <!-- ROW: CONTENT ROWS GO HERE -->

    </table>

  </td></tr>
  </table>

  <!-- TRACKING PIXEL — always OUTSIDE the last </table> -->
  <img src="https://track.example.com/open" width="1" height="1" alt=""
       style="display:block;border:0;width:1px;height:1px;">
</body>
</html>
```

---

## SECTION C — VERIFIED COMPONENT PATTERNS

---

## C1. Hero Banner with VML Background (Outlook Windows)

The outer `<td>` MUST have `font-size:0;line-height:0;mso-line-height-rule:exactly`
to prevent Outlook inserting whitespace above/below the VML block.

```html
<tr>
  <td bgcolor="#1434CB"
      style="background-color:#1434CB;padding:0;
             font-size:0;line-height:0;mso-line-height-rule:exactly;">
    <!--[if mso]>
    <v:rect xmlns:v="urn:schemas-microsoft-com:vml" fill="true" stroke="false"
            style="width:768px;">
      <v:fill type="solid" color="#1434CB"/>
      <v:textbox style="mso-fit-shape-to-text:true;" inset="32px,24px,32px,24px">
    <![endif]-->
    <table role="presentation" cellpadding="0" cellspacing="0" border="0"
           width="768" bgcolor="#1434CB" style="width:100%;background-color:#1434CB;">
    <tr><td bgcolor="#1434CB"
            style="background-color:#1434CB;padding:24px 32px;mso-padding-alt:24px 32px;">
      <h1 style="margin:0;padding:0;font-family:Tahoma,Arial,sans-serif;
                 font-size:36px;font-weight:400;color:#FFFFFF;line-height:47px;
                 mso-line-height-rule:exactly;">Hero Title</h1>
    </td></tr>
    </table>
    <!--[if mso]></v:textbox></v:rect><![endif]-->
  </td>
</tr>
```

---

## C2. Logo Header Row (Outlook-safe)

The logo `<td>` needs `mso-padding-alt` + `font-size:0;line-height:0` to prevent
Outlook adding extra space around the MSO-conditional `<img>`.

```html
<tr>
  <td bgcolor="#FFFFFF"
      style="background-color:#FFFFFF;padding:24px 32px;
             mso-padding-alt:24px 32px;font-size:0;line-height:0;
             mso-line-height-rule:exactly;" class="bg-hd">
    <!--[if mso]><img src="logo-light.png" width="74" height="24" alt="Brand"
      style="display:block;width:74px;height:24px;border:0;"><![endif]-->
    <!--[if !mso]><!-->
    <img class="li" src="logo-light.png" width="74" height="24" alt="Brand"
         style="display:block;width:74px;height:24px;border:0;">
    <img class="di" src="logo-dark.png"  width="74" height="24" alt="Brand"
         style="display:none;width:74px;height:24px;border:0;">
    <!--<![endif]-->
  </td>
</tr>
```

---

## C3. 1px Divider — Outlook Windows Dark Mode Safe

The `&nbsp;` content approach causes Outlook to render the cell at line-height
(~16–20px) instead of 1px. The correct fix uses explicit `height` attributes +
`max-height` + `overflow:hidden` with an empty cell.

```html
<!-- DIVIDER — 1px rule -->
<table role="presentation" cellpadding="0" cellspacing="0" border="0"
       width="100%" height="1"
       style="margin-bottom:24px;max-height:1px;">
<tr>
  <td height="1"
      style="height:1px;max-height:1px;background-color:#E0E0E0;
             font-size:0;line-height:0;mso-line-height-rule:exactly;
             overflow:hidden;"></td>
</tr>
</table>
```

**Why this works:**
- `height="1"` HTML attr — Outlook respects HTML attrs over CSS for table dimensions
- `max-height:1px` — clips any content overflow
- `overflow:hidden` — forces Outlook to clip to declared height
- Empty cell (no `&nbsp;`) — no character content to force line-height expansion

---

## C4. Image Swap (Dark Mode) — Direct on `<img>` Only

**CRITICAL:** Apply `.li`/`.di` directly on `<img>` tags. Never on wrapper `<div>` or `<td>`.
Wrapper-based swaps fail in Outlook App and Gmail Android.

```html
<!-- Block images (logo, hero, banner) -->
<img class="li" src="image-light.png" width="100" height="40" alt="Alt"
     style="display:block;width:100px;height:40px;border:0;">
<img class="di" src="image-dark.png"  width="100" height="40" alt="Alt"
     style="display:none;width:100px;height:40px;border:0;">

<!-- Inline images (icon in text, CTP symbol) -->
<img class="li-i" src="icon-light.png" width="24" height="24" alt="Icon"
     style="display:inline-block;vertical-align:middle;width:24px;height:24px;border:0;">
<img class="di-i" src="icon-dark.png"  width="24" height="24" alt="Icon"
     style="display:none;vertical-align:middle;width:24px;height:24px;border:0;">
```

**Why inline `display:none` is critical:** When `<style>` is stripped on forward/reply,
`.di { display:none }` is lost. The inline `style="display:none"` persists, keeping the
dark image hidden in the light version.

---

## C5. Bullet List — Pixel-Perfect Alignment

Do NOT use `<ul>/<li>` — Outlook misaligns the bullets. Use a two-cell table per item.
The badge `<div>` must be `6×24px` (matching line-height) with `padding-top:9px` to
vertically centre the 6×6px dot on the first text line.

```html
<table role="presentation" cellpadding="0" cellspacing="0" border="0" width="100%">
  <!-- Bullet item — repeat for each item -->
  <tr>
    <td width="14" valign="top" style="width:14px;padding-right:8px;padding-bottom:12px;">
      <!-- Outer: 6×24px container matching line-height -->
      <!-- Inner: 6×6px dot centred at padding-top:9px = (24-6)/2 -->
      <div style="width:6px;height:24px;padding-top:9px;
                  font-size:0;line-height:0;mso-line-height-rule:exactly;">
        <div class="bd" style="width:6px;height:6px;background-color:#1434CB;
                  border-radius:50%;font-size:0;line-height:0;
                  mso-line-height-rule:exactly;">
          <!--[if mso]>&nbsp;<![endif]-->
        </div>
      </div>
    </td>
    <td valign="top" style="padding-bottom:12px;">
      <p style="margin:0;font-family:Verdana,Arial,sans-serif;font-size:16px;
                font-weight:400;color:#000000;line-height:24px;
                mso-line-height-rule:exactly;" class="tx">Bullet item text here.</p>
    </td>
  </tr>
</table>
```

Add `.bd` to CSS for dark mode bullet dot colour:
```css
@media(prefers-color-scheme:dark){ .bd{background-color:#FFFFFF!important;} }
[data-ogsc] .bd{background-color:#FFFFFF!important;}
```

---

## C6. Chevron Link (Link with Icon)

```html
<table role="presentation" cellpadding="0" cellspacing="0" border="0">
<tr>
  <td valign="middle" style="padding-right:5px;">
    <a href="https://example.com" target="_blank" rel="noopener noreferrer"
       style="color:#1434CB;text-decoration:underline;font-family:Verdana,Arial,sans-serif;
              font-size:16px;font-weight:400;line-height:24px;" class="tl">Link label</a>
  </td>
  <td valign="middle">
    <!--[if mso]><img src="chevron-light.png" width="8" height="16" alt=""
      style="display:block;width:8px;height:16px;border:0;"><![endif]-->
    <!--[if !mso]><!-->
    <img class="li-i" src="chevron-light.png" width="8" height="16" alt=""
         style="display:inline-block;vertical-align:middle;width:8px;height:16px;border:0;">
    <img class="di-i" src="chevron-dark.png"  width="8" height="16" alt=""
         style="display:none;vertical-align:middle;width:8px;height:16px;border:0;">
    <!--<![endif]-->
  </td>
</tr>
</table>
```

---

## SECTION D — DARK MODE

---

## D1. Dark Mode Architecture

Three CSS layers work together — each catches clients the others miss:

| Layer | Catches |
|---|---|
| `@media (prefers-color-scheme:dark)` | Apple Mail, iOS Mail, Outlook.com, Samsung Mail |
| `[data-ogsc]` selector | Outlook App (iOS + Android) |
| Inline `style="display:none"` on `.di` images | All clients on forward/reply |

**Rule:** Every rule in `@media` MUST be duplicated under `[data-ogsc]`. No exceptions.

---

## D2. HTML Comment Safety

**NEVER nest `-->` inside `<!-- -->` comments** — breaks Gmail Android parser, corrupts
entire email in some clients.

```html
<!-- WRONG — nested close breaks Gmail Android -->
<!-- <!--[if mso]>...<![endif]--> -->

<!-- CORRECT — comments and conditionals stay separate -->
<!--[if mso]>...<![endif]-->
```

---

## SECTION E — REPLY & FORWARD SAFETY

---

## E1. What Survives Forward/Reply

When forwarded, the `<style>` block is stripped by Gmail, Outlook, Yahoo, and AOL.

| Element | Survives? | How |
|---|---|---|
| Text colours on `<p>` and `<a>` | ✅ Yes | Must be inline `color:` — NOT class-only |
| Background colours on `<td>` | ✅ Yes | Must have both `bgcolor=""` attr AND inline `background-color:` |
| Font stack | ✅ Yes | Must be inline on every text element |
| `display:none` on dark images | ✅ Yes | Inline style survives; class does not |
| Dark mode (`@media` / `[data-ogsc]`) | ❌ No | Always lost — expected, not fixable |

---

## E2. Required on Every `<a>` Tag

```html
<a href="https://..." target="_blank" rel="noopener noreferrer" style="color:#1434CB;">
```

- `target="_blank"` — opens in browser, not client's embedded frame (which has no session)
- `rel="noopener"` — prevents tab-hijacking via `window.opener`
- `rel="noreferrer"` — suppresses `Referer` header (keeps tracking clean on forward)

---

## E3. Dual Background Pattern (Required on Every `<td>`)

```html
<!-- CORRECT: both attr and inline style — survives all clients including Samsung -->
<td bgcolor="#FFFFFF" style="background-color:#FFFFFF;" class="bg-bd">

<!-- WRONG: inline only — Samsung strips it -->
<td style="background-color:#FFFFFF;">

<!-- WRONG: attr only — modern clients may ignore -->
<td bgcolor="#FFFFFF">
```

---

## SECTION F — CROSS-CLIENT COMPATIBILITY

---

## F1. Client-by-Client Rules

### Gmail
- Inline ALL critical CSS — Gmail strips `<style>` on some versions
- Never use `<link>` stylesheets — stripped
- Use `role="presentation"` on all layout tables
- Avoid `id` selectors — Gmail rewrites them

### Outlook 2007–2021 Windows
- `<table>` for ALL layout — Outlook ignores `<div>`, flex, and grid
- Inline CSS on every element
- Set both HTML `width=""` attribute AND `style="width:Xpx"` on images
- Use `<!--[if mso]>` conditional comments for Outlook-specific overrides
- Background images — VML only, CSS `background-image` is ignored
- Always include web-safe font fallbacks: `Arial, Georgia, Verdana, Tahoma`
- `mso-line-height-rule:exactly` on all text elements
- Hero banner outer `<td>` — add `font-size:0;line-height:0;mso-line-height-rule:exactly`
- Logo header `<td>` — add `mso-padding-alt` + `font-size:0;line-height:0`
- Dividers — use `height="1"` attr + `max-height:1px` + `overflow:hidden` + empty cell

### Yahoo Mail
- Avoid `id` selectors — Yahoo rewrites them
- Inline all critical styles

### Apple Mail / iOS Mail
- Full CSS and media query support
- `format-detection` meta required to suppress auto-linking
- `color-scheme` meta required for Dark Mode `@media` to fire

### Samsung Email
- Requires dual `bgcolor=""` attr + `background-color:` inline on every `<td>`

---

## F2. Feature Support Summary

| Feature | Best Support | No Support |
|---|---|---|
| `@media prefers-color-scheme:dark` | Apple Mail, iOS Mail, Outlook.com | Gmail App, Outlook Win |
| `[data-ogsc]` image swap | Outlook App (iOS + Android) | Gmail App, Apple Mail |
| VML background / button | Outlook 2013–2021 Win | All others |
| Web fonts | Apple Mail, iOS Mail, some webmail | Outlook desktop |
| CSS animation / GIF | Most modern clients | Outlook desktop (first frame only) |
| Background images (CSS) | Apple Mail, iOS, Gmail Web | Outlook desktop → use VML |
| Hover effects | Apple Mail, iOS Mail | Outlook desktop, Gmail App |

---

## SECTION G — ACCESSIBILITY

---

## G1. Accessibility Requirements

- **Color contrast:** Min 4.5:1 body text, 3:1 large text (WCAG AA)
- Verify both light AND dark mode color combinations
- All images must have descriptive `alt` text
- Do not rely on color alone to convey information
- Links must be distinguishable from body text (underline or weight)
- CTA buttons — minimum 44×44px touch target
- Always add `lang` attribute to `<html>` element

---

## SECTION H — CHECKLISTS

---

## H1. Pre-Send Checklist

### Structure
- [ ] DOCTYPE html is line 1
- [ ] All layout uses `<table>` not `<div>`
- [ ] All critical CSS inlined on every element
- [ ] All images have HTML `width` + `height` attrs AND inline `style="width:Xpx"`
- [ ] All images have `alt` text
- [ ] All `<a>` tags have `target="_blank" rel="noopener noreferrer"`
- [ ] All links include `https://` protocol
- [ ] Web-safe font fallbacks defined everywhere
- [ ] `mso-line-height-rule:exactly` on all text elements
- [ ] `border-collapse:collapse` + `role="presentation"` on all tables
- [ ] Every `<td>` with bg colour has both `bgcolor=""` attr AND inline `background-color:`

### Preheader
- [ ] Table pattern used (not `<div>`)
- [ ] Under 90 characters
- [ ] Padded with zero-width spaces
- [ ] `color:#f4f4f4` matches page background

### Outlook Windows Dark Mode
- [ ] Hero banner outer `<td>` has `font-size:0;line-height:0;mso-line-height-rule:exactly`
- [ ] Logo header `<td>` has `mso-padding-alt` + `font-size:0;line-height:0`
- [ ] All dividers use `height="1"` attr + `max-height:1px` + `overflow:hidden` + empty cell
- [ ] VML `<v:rect style="width:Xpx;">` — width matches container exactly

### Dark Mode
- [ ] `color-scheme` and `supported-color-schemes` meta tags present
- [ ] `.li`/`.di` classes on all images — applied directly on `<img>`, not wrappers
- [ ] `.di` images have inline `style="display:none"` (survives `<style>` stripping)
- [ ] Every `@media` rule duplicated under `[data-ogsc]`
- [ ] Dark mode colours pass WCAG contrast requirements

### Reply / Forward Safety
- [ ] Text colours inline on every `<p>` and `<a>` (not class-only)
- [ ] All `<a>` have `target="_blank" rel="noopener noreferrer"`
- [ ] Preheader uses table pattern with inline `display:none`
- [ ] Background colours use dual `bgcolor` attr + inline `background-color`

### Compliance
- [ ] Unsubscribe link present and working
- [ ] Physical mailing address in footer
- [ ] Tracking pixel outside last `</table>` before `</body>`

---

## H2. Dimensions Reference

| Element | Value |
|---|---|
| Email max-width | 768px |
| Mobile breakpoint | 600px |
| Section padding (horizontal) | 32px |
| Section padding (vertical) | 24px |
| Bottom padding before footer | 24px |
| Button padding | 14px vertical × 28px horizontal |
| Button touch target | min 44×44px |
| Font body | 14–16px |
| Font heading | 22–36px |
| Line height body | 1.5 (150%) |
| Preheader text | 90 characters max |
| Subject line | 50 characters max |
| Total email size | 100KB max (excl. images) |

---

## H3. Testing Tools

| Tool | Purpose |
|---|---|
| Litmus | Cross-client rendering and Dark Mode preview |
| Email on Acid | Outlook version rendering, Dark Mode testing |
| Mailtrap | Sandbox send testing |
| PutsMail | Send HTML to any inbox |
| accessible-colors.com | WCAG contrast checker |
| caniemail.com | CSS/HTML support by client |


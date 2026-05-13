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

- Must be the **very first line** — nothing before it, not even a space
- It is **not an HTML tag** — it is a browser/client parser instruction
- Case-insensitive — write lowercase for consistency
- Has **no closing tag**
- Without it, clients fall into **quirks mode** and layout breaks unpredictably

```html
<!DOCTYPE html>
<html lang="en"
      xmlns="http://www.w3.org/1999/xhtml"
      xmlns:v="urn:schemas-microsoft-com:vml"
      xmlns:o="urn:schemas-microsoft-com:office:office">
```

- `lang="en"` — required for accessibility (screen readers)
- `xmlns:v` and `xmlns:o` — required for **Outlook VML support**
  (background images, bulletproof buttons in Outlook 2007–2021)

---

## A3. Required Head Meta Tags

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="format-detection" content="telephone=no, date=no, address=no, email=no">
  <meta name="x-apple-disable-message-reformatting">

  <!-- DARK MODE: Tell clients this template supports both schemes -->
  <!-- Required for Apple Mail @media (prefers-color-scheme: dark) to fire -->
  <meta name="color-scheme" content="light dark">
  <meta name="supported-color-schemes" content="light dark">

  <title>Email Title</title>

  <!--[if mso]>
  <noscript>
    <xml>
      <o:OfficeDocumentSettings>
        <o:PixelsPerInch>96</o:PixelsPerInch>
      </o:OfficeDocumentSettings>
    </xml>
  </noscript>
  <![endif]-->
</head>
```

| Meta Tag | Purpose |
|---|---|
| `charset UTF-8` | Universal character encoding |
| `viewport` | Mobile scaling control |
| `X-UA-Compatible IE=edge` | Forces modern IE/Outlook rendering |
| `format-detection` | Prevents iOS auto-linking phone numbers, dates, addresses |
| `x-apple-disable-message-reformatting` | Prevents Apple Mail from resizing text |
| `color-scheme` | Signals Dark Mode support to Apple Mail and iOS |
| `PixelsPerInch 96` | Ensures consistent 96dpi rendering in Outlook |

---

## A4. HTML Tags — What to Use and Avoid

### ALWAYS SAFE

#### Structure and Layout
| Tag | Usage | Key Rule |
|---|---|---|
| `<table>` | All layout | Always add `role="presentation"` |
| `<tr>` | Table row | — |
| `<td>` | Table cell | Primary spacing container — padding goes here |
| `<tbody>` | Table body | Optional, good practice |

#### Typography
| Tag | Safe | Key Rule |
|---|---|---|
| `<h1>` `<h2>` `<h3>` | Yes | Always inline `margin:0; padding:0` |
| `<p>` | Yes | Never use `padding` — Outlook ignores it |
| `<span>` | Yes | Safest inline tag — no default margins |
| `<strong>` | Yes | Or use `font-weight:700` inline |
| `<em>` | Yes | Or use `font-style:italic` inline |
| `<br>` | Yes | Safe everywhere |
| `<small>` | Yes | Use with inline `font-size` |
| `<sup>` `<sub>` | Yes | Good for trademark and copyright symbols |

#### Links and Images
| Tag | Key Rule |
|---|---|
| `<a>` | Always full `https://` URL; style buttons via `<td>` background |
| `<img>` | Always set `alt`, `width`, `height`, `display:block` |

#### Lists
| Tag | Safe | Key Rule |
|---|---|---|
| `<ul>` `<ol>` | With care | Inline `margin` and `padding` — Outlook may misalign |
| `<li>` | With care | Always inline font styles |

#### Utility
| Tag | Usage |
|---|---|
| `<div>` | Preheader text only — never for layout |
| `<!--[if mso]>` | Outlook-only conditional overrides |
| `<style>` | Media queries and Dark Mode only — never rely on for critical inline styles |

### NEVER USE

| Tag | Reason |
|---|---|
| `<div>` for layout | Outlook ignores — use `<td>` |
| `<section>` `<article>` `<header>` `<footer>` `<nav>` | Not supported in most clients |
| `<video>` `<audio>` | Stripped by most clients |
| `<form>` `<input>` `<button>` | Stripped by Gmail and Outlook |
| `<iframe>` | Blocked by all clients |
| `<script>` | Blocked by all clients |
| `<link>` | Stylesheets stripped by Gmail |
| `<canvas>` | Not supported |
| `<svg>` | Blocked by Outlook and Gmail — use `<img>` instead |

---

## A5. Padding Rules — Cross-Client

### The Golden Rule
```
SAFE:   padding on <td>       — supported by ALL email clients
SAFE:   padding on <th>       — supported
AVOID:  padding on <p>        — Outlook ignores it
AVOID:  padding on <div>      — Outlook ignores it
AVOID:  padding on <table>    — Outlook ignores it
AVOID:  margin on any element — unreliable in Outlook; use padding on <td>
```

### td Padding — Full Client Support Matrix

| Email Client | padding on td | Notes |
|---|---|---|
| Outlook 2007–2019 | Supported | Shorthand `padding: 10px 20px` works |
| Outlook 2021 / 365 | Supported | Full support |
| Outlook.com Web | Supported | Full support |
| Gmail Web | Supported | Full support |
| Gmail Android / iOS | Supported | Full support |
| Apple Mail | Supported | Full support |
| iOS Mail | Supported | Full support |
| Yahoo Mail | Supported | Full support |
| Samsung Email | Supported | Full support |

### Safe Padding Pattern
```html
<!-- Correct — always on td -->
<td style="padding: 24px 32px;">Content</td>

<!-- Explicit longhand for maximum Outlook safety -->
<td style="padding-top:24px; padding-right:32px;
           padding-bottom:24px; padding-left:32px;">Content</td>

<!-- Never — Outlook ignores padding on these elements -->
<p style="padding:20px;">Text</p>
<div style="padding:20px;">Content</div>
<table style="padding:20px;">...</table>
```

### Outlook Padding Edge Cases
- `padding` plus `border` on same `<td>` — add `border-collapse:collapse` on parent `<table>`
- Outlook 2003 — use `cellpadding` attribute as fallback
- Never use `margin` for spacing — always use `padding` on `<td>`

---

---

## SECTION B — LAYOUT, STRUCTURE AND RESPONSIVE

---

## B1. Layout and Structure Standards

- **Max width:** `768px` desktop/tablet; `100%` fluid on mobile
- **Wrapper:** Outer `<table>` at `100%` then inner content table at `max-width:768px`
- **Layout engine:** `<table>` `<tr>` `<td>` only — never `<div>` for columns
- **Multi-column:** Nested tables; collapse to single column on mobile
- **Alignment:** `align="center"` on tables; `text-align` inline on cells
- `border-collapse:collapse` on ALL tables
- `cellpadding="0" cellspacing="0" border="0"` on ALL tables
- `role="presentation"` on ALL layout tables

---

## B2. Full Boilerplate (Desktop + Mobile + Dark Mode Ready)

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

    /* RESET */
    * { box-sizing: border-box; }
    body, table, td, a { -webkit-text-size-adjust:100%; -ms-text-size-adjust:100%; }
    table, td { mso-table-lspace:0pt; mso-table-rspace:0pt; border-collapse:collapse; }
    img { -ms-interpolation-mode:bicubic; border:0; display:block; }
    a[x-apple-data-detectors] { color:inherit !important; text-decoration:none !important; }

    /* IMAGE MODE SWITCHING */
    .light-img { display:block; }
    .dark-img  { display:none; }
    .dark-only { display:none !important; }

    /* DARK MODE via @media prefers-color-scheme */
    /* Targets: Apple Mail, iOS Mail, Outlook.com partial, Office 365 macOS */
    @media (prefers-color-scheme: dark) {
      .email-bg     { background-color:#1a1a1a !important; }
      .email-wrapper{ background-color:#1a1a1a !important; }
      .email-header { background-color:#1e1e1e !important; }
      .email-hero   { background-color:#BDEAFF !important; }
      .email-accent { background-color:#0d1b2a !important; }
      .email-card   { background-color:#1a1a1a !important; }
      .email-footer { background-color:#111111 !important; }
      .text-on-dark { color:#e8e8e8 !important; }
      h1            { color:#2E2E2E !important; }
      h2, h3        { color:#7ab3d4 !important; }
      p, li, td, span { color:#e0e0e0 !important; }
      .accent-text  { color:#bdd5e8 !important; }
      .muted-text   { color:#888888 !important; }
      a.link        { color:#7ab3d4 !important; }
      .footer-link  { color:#7ab3d4 !important; }
      .icon-tint    { color:#7ab3d4 !important; }
      .cta-btn      { background-color:#3a6bbf !important;
                      border-color:#3a6bbf !important; color:#ffffff !important; }
      .light-img    { display:none  !important; }
      .dark-img     { display:block !important; }
      .dark-only    { display:block !important; }
      .faq-question { color: #ffffff !important; }
    }

    /* DARK MODE via [data-ogsc] */
    /* Targets: Outlook App iOS + Android, Outlook.com */
    /* Must duplicate every @media rule above exactly  */
    [data-ogsc] .email-bg     { background-color:#1a1a1a !important; }
    [data-ogsc] .email-wrapper{ background-color:#1a1a1a !important; }
    [data-ogsc] .email-header { background-color:#1e1e1e !important; }
    [data-ogsc] .email-hero   { background-color:#1c2b3a !important; }
    [data-ogsc] .email-accent { background-color:#0d1b2a !important; }
    [data-ogsc] .email-card   { background-color:#1a1a1a !important; }
    [data-ogsc] .email-footer { background-color:#111111 !important; }
    [data-ogsc] .text-on-dark { color:#e8e8e8   !important; }
    [data-ogsc] h1            { color:#2E2E2E   !important; }
    [data-ogsc] h2,
    [data-ogsc] h3            { color:#7ab3d4   !important; }
    [data-ogsc] p,
    [data-ogsc] li,
    [data-ogsc] td,
    [data-ogsc] span          { color:#e0e0e0   !important; }
    [data-ogsc] .accent-text  { color:#bdd5e8   !important; }
    [data-ogsc] .muted-text   { color:#888888   !important; }
    [data-ogsc] a.link        { color:#7ab3d4   !important; }
    [data-ogsc] .footer-link  { color:#7ab3d4   !important; }
    [data-ogsc] .cta-btn      { background-color:#3a6bbf !important;
                                border-color:#3a6bbf !important;
                                color:#ffffff    !important; }
    [data-ogsc] .light-img    { display:none    !important; }
    [data-ogsc] .dark-img     { display:block   !important; }
    [data-ogsc] .dark-only    { display:block   !important; }
    [data-ogsc] .faq-question { color: #ffffff !important; }

    /* RESPONSIVE — MOBILE */
    @media only screen and (max-width:600px) {
      .email-container    { width:100% !important; }
      .email-wrapper      { width:100% !important; }
      .stack-column       { display:block !important; width:100% !important; }
      .mobile-hide        { display:none !important; }
      .hide-mobile        { display:none !important; }
      .mobile-full-width  { width:100% !important; }
      .mobile-text-center { text-align:center !important; }
      .mobile-btn         { width:100% !important; text-align:center !important;
                            display:block !important; }
    }

  </style>
</head>
<body style="margin:0; padding:0; background-color:#ffffff; word-spacing:normal;"
      class="email-bg">

  <!-- PREHEADER — keep under 90 characters -->
  <div style="display:none; max-height:0; overflow:hidden; mso-hide:all;">
    Preview text here &zwnj;&nbsp;&#847;&zwnj;&nbsp;&#847;&zwnj;&nbsp;&#847;&zwnj;
  </div>

  <!-- OUTER WRAPPER -->
  <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0"
         class="email-bg" style="background-color:#ffffff;">
    <tr>
      <td align="center" style="padding:24px 0;">

        <!-- EMAIL CONTAINER — 768px -->
        <table role="presentation" class="email-container email-wrapper" width="768"
               cellpadding="0" cellspacing="0" border="0"
               style="max-width:768px; width:100%; background-color:#ffffff;">

          <!-- CONTENT ROWS GO HERE -->

        </table>
      </td>
    </tr>
  </table>
</body>
</html>
```

---

## B3. Responsive Design Rules — Mobile-First

**Mobile-first means: inline styles are mobile defaults. A `min-width` media query scales up to desktop.**

| Concern | Mobile base (inline) | Desktop (`min-width: 601px`) |
|---|---|---|
| Horizontal padding | `32px` | `.px-desktop → 32px !important` |
| H1 font size | `28px` | `.hero-title → 36px !important` |
| H2 font size | `20px` | `.h2-scale → 24px !important` |
| Hero subtitle | `16px` | `.hero-subtitle → 18px !important` |
| Container width | `100%` fluid | `768px !important` |
| Footer logo cell | stacked block | side-by-side table-cell |

### Mobile-First CSS Pattern

```css
/* DESKTOP ENHANCEMENT (mobile-first) */
@media only screen and (min-width: 601px) {
  .email-container  { width: 768px !important; }
  .px-desktop       { padding-left: 32px !important; padding-right: 32px !important; }
  .hero-title       { font-size: 36px !important; line-height: 44px !important; }
  .hero-subtitle    { font-size: 18px !important; line-height: 26px !important; }
  .h2-scale         { font-size: 24px !important; line-height: 28px !important; }
  .h2-faq           { font-size: 22px !important; line-height: 29px !important; }
}

/* MOBILE — fluid + stack multi-column rows */
@media only screen and (max-width: 600px) {
  .email-container  { width: 100% !important; }
  .email-wrapper    { width: 100% !important; }
  .copyright-logo   { display: block !important; width: 100% !important;
                      text-align: center !important; padding: 0 16px 16px !important; }
}
```

> **Outlook note:** Outlook does not support `@media` queries, so it renders the mobile base
> inline styles. Add `<!--[if mso]>` conditionals on critical padding cells if Outlook
> must receive desktop-size padding.

### Key Rules
- All section `<td>` elements get `padding: Xpx 16px` inline + class `px-desktop`
- Stacking multi-column rows on mobile: add class `copyright-logo` (or `stack-column`) that `display:block` at `max-width:600px`
- Buttons: full width on mobile via `.mobile-btn { width:100% !important; }`
- Images: `max-width:100%; height:auto` on mobile

---

---

## SECTION C — DARK MODE

---

## C1. How Email Clients Handle Dark Mode

| Email Client | Dark Mode Behavior | @media Support | [data-ogsc] Support | Override Possible |
|---|---|---|---|---|
| Apple Mail macOS | No change | Yes | No | Yes |
| iPhone / iPad Mail iOS 16+ | No change | Yes | No | Yes |
| Gmail App iOS | Full invert | No | No | Partial blend-mode |
| Gmail App Android | Partial invert | No | No | Forced BG only |
| Gmail Desktop Web | No change | No | No | N/A |
| Outlook.com | Partial invert | No | Partial | No |
| Outlook App iOS | Partial invert | No | Partial | No |
| Outlook App Android | Partial invert | No | Partial | No |
| Outlook 2021 Windows | Full invert | No | No | VML hack only |
| Office 365 Windows | Full invert | No | No | No |
| Office 365 macOS | Partial invert | Partial | No | Forced BG |
| Yahoo / AOL Mail | No change | No | No | N/A |

> Pure white `#ffffff` backgrounds may be inverted by Apple Mail if color-scheme meta tags are present.

**Key principle:** Design your Light Mode baseline to survive partial and full inversion,
then layer controlled Dark Mode overrides for clients that support them.

---

## C2. Design Tokens — Light and Dark

### Light Mode Tokens
```
Background page:       #ffffff   Outer canvas
Background header:     #1a1a8c   Logo / nav bar
Background hero:       #1b3ebd   Hero band
Background accent:     #d6e8f8   Callout strip
Background card:       #ffffff   Content card
Background footer:     #f5f5f5   Footer band
Background CTA:        #1b3ebd   Primary button

Text on dark:          #ffffff   Text on hero / header
Text primary:          #1a1a1a   Body copy
Text heading:          #1b3ebd   H2/H3 on white
Text accent:           #1b3ebd   Text in accent strip
Text link:             #1b3ebd   Inline hyperlinks
Text muted:            #666666   Footer / legal copy
Icon tint:             #1b3ebd   Icon color
Border:                #dce8f5   Dividers
```

### Dark Mode Token Overrides
```
Background page:       #1a1a1a   Dark charcoal canvas
Background header:     #1e1e1e   Near black
Background hero:       #1c2b3a   Dark desaturated blue
Background accent:     #0d1b2a   Very dark navy
Background card:       #1a1a1a   Dark charcoal
Background footer:     #111111   Deepest charcoal
Background CTA:        #3a6bbf   Lightened for contrast

Text on dark:          #e8e8e8   Off-white
Text primary:          #e0e0e0   Soft white
Text heading:          #7ab3d4   Muted light blue
Text accent:           #bdd5e8   Light blue-white
Text link:             #7ab3d4   Accessible light blue
Text muted:            #888888   Medium gray
Icon tint:             #7ab3d4   Light blue-gray
Border:                #2a3a4a   Dark dividers
```

### Key Dark Mode Design Rules
1. **Headers should be dark in both modes** — deep navy light, near-black dark
2. **Brand blue `#1b3ebd` becomes `#7ab3d4`** in dark — never use pure brand blue on dark backgrounds, it fails WCAG contrast
3. **Hero desaturates, not inverts** — build hero as a dark surface in both modes
4. **Callout strip inverts pale to deep** — ensure text color also flips via CSS class
5. **White cards become dark charcoal `#1a1a1a`** — softer than pure black
6. **Mid-tones survive inversion** — use 40 to 60 percent lightness for decorative elements that cannot be swapped

---

## C3. Image Handling for Dark Mode

```
Does the image have a transparent background?
  YES → Add white glow or stroke so dark elements stay visible on inverted BG.
        Use .light-img / .dark-img swap for full control.

  NO  → Include neutral padding around focal point.
        If BG is white, inversion turns it dark — ensure subject has contrast either way.
        Alternatively flatten to mid-tone or brand color background.
```

### Logo Swap Pattern
```html
<!-- Light Mode logo — shown by default -->
<img class="light-img"
     src="https://cdn.example.com/logo-dark.png"
     width="160" height="48" alt="Brand"
     style="display:block; width:160px; height:48px; border:0;">

<!-- Dark Mode logo — hidden by default, shown via CSS -->
<!--[if !mso]><!-->
<div class="dark-img"
     style="display:none; overflow:hidden; float:left;
            width:0; max-height:0; max-width:0;
            line-height:0; visibility:hidden;" align="center">
  <img src="https://cdn.example.com/logo-white.png"
       width="160" height="48" alt="Brand"
       style="display:block; width:160px; height:48px; border:0;">
</div>
<!--<![endif]-->
```

---

## C4. Gmail iOS Dark Mode Fix

Gmail App on iOS applies full inversion with no `@media` support. Use only where inversion makes text completely unreadable:

```css
u + .email-body .gmail-fix {
  background-color: #ffffff !important;
  mix-blend-mode: screen;
}
```

---

## C5. Outlook Windows Dark Mode Fix

Full inversion in Outlook 2021 / Office 365 Windows cannot be overridden via CSS. Use VML gradient for critical brand sections:

```html
<!--[if mso]>
<v:rect style="width:768px; height:80px;" strokecolor="none">
  <v:fill type="gradient" color="#1a1a2e" color2="#1a1a2e" angle="90"/>
</v:rect>
<![endif]-->
```

---

---

## SECTION D — CONTENT COMPONENTS

---

## D1. Typography Standards

- Font stack: `'Brand Font', Arial, Helvetica, Verdana, sans-serif`
- Web-safe fallbacks always: `Arial, Georgia, Verdana, Tahoma`
- Web fonts work in Apple Mail and iOS — always include system fallback
- Minimum body: `14px` | Minimum heading: `20px`
- Line height: `1.5` or `150%` — always pair with `mso-line-height-rule:exactly`
- Always use `px` — never `em` or `rem`
- Set `color` inline on every `<td>`, `<p>`, `<h1>`–`<h3>` — Outlook strips style block colors

```html
<!-- Heading -->
<h1 style="margin:0 0 12px 0; font-family:Verdana,Arial,Helvetica,sans-serif;
           font-size:32px; font-weight:800; color:#1b3ebd;
           line-height:40px; mso-line-height-rule:exactly;">
  Heading Text
</h1>

<!-- Body paragraph -->
<p style="margin:0 0 16px 0; font-family:Verdana,Arial,Helvetica,sans-serif;
          font-size:15px; color:#1a1a1a;
          line-height:24px; mso-line-height-rule:exactly;">
  Body copy text here.
</p>

<!-- Inline highlight -->
<span style="color:#1b3ebd; font-weight:700;">Highlighted text</span>
```

---

## D2. CTA Button — Outlook VML Safe

```html
<!--[if mso]>
<v:roundrect xmlns:v="urn:schemas-microsoft-com:vml"
             xmlns:w="urn:schemas-microsoft-com:office:word"
             href="https://example.com"
             style="height:48px;v-text-anchor:middle;width:200px;"
             arcsize="8%" strokecolor="#1b3ebd" fillcolor="#1b3ebd">
  <w:anchorlock/>
  <center style="color:#ffffff; font-family:Arial,sans-serif;
                 font-size:15px; font-weight:700;">
    Button Text
  </center>
</v:roundrect>
<![endif]-->
<!--[if !mso]><!-->
<a href="https://example.com" class="mobile-btn cta-btn"
   style="display:inline-block; background-color:#1b3ebd; color:#ffffff;
          font-family:Arial,sans-serif; font-size:15px; font-weight:700;
          text-decoration:none; text-align:center;
          padding:14px 40px; border-radius:4px;
          min-height:44px; min-width:44px;">
  Button Text
</a>
<!--<![endif]-->
```

---

## D3. Images

- Always set `alt` text — many clients block images by default
- Always set both HTML `width`/`height` attributes AND inline CSS
- `display:block` on all images — removes phantom gaps
- Absolute CDN URLs only — no relative paths
- Optimize: less than 200KB per image; JPEG for photos, PNG for logos
- GIF: first frame must convey key message — Outlook desktop shows first frame only
- Never put important text inside images
- Never rely on white image backgrounds — they invert in dark mode clients

### SVG — Not Supported in Email

SVG is **not supported** in email. Never use `<svg>` inline or reference SVG files in email templates.

| Approach | Outcome |
|---|---|
| `<svg>` inline tag | Stripped entirely by Gmail and Outlook — nothing renders |
| `<img src="logo.svg">` | Blocked by Gmail Android; unreliable across clients |
| `background-image: url('data:image/svg+xml,...')` | Stripped by Gmail |
| VML (`<v:*>`) | Outlook Windows only — not a replacement for SVG icons |

**Why Gmail strips SVG:** Gmail's HTML sanitizer removes `<svg>` tags for security reasons. No workaround exists.

**The only reliable approach:**
1. Export SVG as **PNG at 2× resolution** for retina displays (e.g. export a 160px logo at 320px wide, set `width="160"` in HTML)
2. Host the PNG on a CDN with an absolute `https://` URL
3. Use `<img src="https://cdn.example.com/logo.png" width="160" height="48" alt="Brand" style="display:block; width:160px; height:48px; border:0;">`

**WebP note:** WebP is not supported in Outlook desktop. If using WebP, always provide a PNG fallback via a hosting layer (e.g. CDN image transformation).

---

## D4. Icon Row Pattern

```html
<table role="presentation" cellpadding="0" cellspacing="0"
       border="0" width="100%" style="margin-bottom:16px;">
  <tr>
    <td width="44" valign="top" style="padding-right:12px; padding-top:2px;">
      <img src="https://cdn.example.com/icon.png"
           width="28" height="28" alt=""
           style="display:block; width:28px; height:28px; border:0;">
    </td>
    <td valign="top">
      <p style="margin:0; font-family:Verdana,Arial,Helvetica,sans-serif;
                font-size:15px; color:#1a1a1a;
                line-height:22px; mso-line-height-rule:exactly;">
        Icon label text goes here
      </p>
    </td>
  </tr>
</table>
```

---

## D5. Two-Column Layout (Mobile Stacking)

```html
<table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0">
  <tr>
    <td width="48%" valign="top" class="stack-column" style="padding-right:16px;">
      <h2 style="margin:0 0 8px 0; font-family:Arial,sans-serif;
                 font-size:18px; color:#1b3ebd; mso-line-height-rule:exactly;">
        Column One
      </h2>
      <p style="margin:0; font-family:Arial,sans-serif; font-size:15px;
                color:#1a1a1a; line-height:22px; mso-line-height-rule:exactly;">
        Description text.
      </p>
    </td>
    <td width="4%"></td>
    <td width="48%" valign="top" class="stack-column">
      <h2 style="margin:0 0 8px 0; font-family:Arial,sans-serif;
                 font-size:18px; color:#1b3ebd; mso-line-height-rule:exactly;">
        Column Two
      </h2>
      <p style="margin:0; font-family:Arial,sans-serif; font-size:15px;
                color:#1a1a1a; line-height:22px; mso-line-height-rule:exactly;">
        Description text.
      </p>
    </td>
  </tr>
</table>
```

---

## D6. Divider

```html
<tr class="divider">
  <td style="padding:0 32px;">
    <table role="presentation" cellpadding="0" cellspacing="0" border="0" width="100%">
      <tr>
        <td style="border-top:1px solid #e0e0e0; font-size:0; line-height:0;">&nbsp;</td>
      </tr>
    </table>
  </td>
</tr>
```

---

## D7. Footer — CAN-SPAM and GDPR Compliant

```html
<tr>
  <td class="email-footer" style="background-color:#f5f5f5; padding:24px 32px;">

    <p class="muted-text"
       style="margin:0 0 12px 0; font-family:Verdana,Arial,Helvetica,sans-serif;
              font-size:11px; color:#666666; line-height:17px;
              mso-line-height-rule:exactly;">
      This is a service email. You may receive service emails in accordance with our
      <a href="https://example.com/terms" class="footer-link"
         style="color:#1b3ebd; text-decoration:underline;">Terms of Service</a> and
      <a href="https://example.com/privacy" class="footer-link"
         style="color:#1b3ebd; text-decoration:underline;">Privacy Notice</a>.
    </p>

    <!-- Physical address — CAN-SPAM requirement -->
    <p class="muted-text"
       style="margin:0 0 12px 0; font-family:Verdana,Arial,Helvetica,sans-serif;
              font-size:11px; color:#9ca3af; line-height:17px;
              mso-line-height-rule:exactly;">
      Your Company Inc. &bull; 123 Main Street, City, State 00000, Country
    </p>

    <!-- Unsubscribe — CAN-SPAM and GDPR requirement -->
    <p class="muted-text"
       style="margin:0 0 20px 0; font-family:Verdana,Arial,Helvetica,sans-serif;
              font-size:11px; color:#9ca3af; line-height:17px;
              mso-line-height-rule:exactly;">
      <a href="{{unsubscribe_url}}" class="footer-link"
         style="color:#1b3ebd; text-decoration:underline;">Unsubscribe</a>
      &nbsp;&middot;&nbsp;
      <a href="{{preferences_url}}" class="footer-link"
         style="color:#1b3ebd; text-decoration:underline;">Email Preferences</a>
    </p>

    <!-- Copyright row -->
    <table role="presentation" cellpadding="0" cellspacing="0" border="0" width="100%">
      <tr>
        <td valign="middle"
            style="font-family:Arial,sans-serif; font-size:11px; color:#666666;">
          &copy; 2026 Your Brand. All Rights Reserved.
        </td>
        <td align="right" valign="middle">
          <img src="https://cdn.example.com/logo.png"
               width="60" height="20" alt="Brand"
               style="display:block; width:60px; height:20px; border:0;">
        </td>
      </tr>
    </table>

  </td>
</tr>
```

---

---

## SECTION E — CROSS-CLIENT COMPATIBILITY

---

## E1. Client-by-Client Rules

### Gmail
- Inline ALL critical CSS — Gmail strips `<style>` blocks on some versions
- Never use `<link>` stylesheets
- Use `role="presentation"` on all layout tables
- Use class selectors only — avoid `id` selectors

### Outlook 2007–2021 Windows
- `<table>` for ALL layout — Outlook ignores `<div>` flex and grid
- Inline CSS on every element
- Set both HTML `width=""` attribute AND `style="width:Xpx"` on images
- Use `<!--[if mso]>` conditional comments for Outlook-only overrides
- Background images — VML only, CSS background-image is ignored
- Always include web-safe font fallbacks: `Arial, Georgia, Verdana, Tahoma`
- `mso-line-height-rule:exactly` on all text elements
- `border-collapse:collapse` on all tables

### Yahoo Mail
- Avoid `id` selectors — Yahoo rewrites them
- Inline all critical styles

### Apple Mail / iOS Mail
- Full CSS and media query support
- `format-detection` meta required to suppress auto-linking
- `color-scheme` meta required for Dark Mode @media to fire

---

## E2. Feature Support Summary

| Feature | Best Support | No Support |
|---|---|---|
| @media prefers-color-scheme dark | Apple Mail, iOS Mail, Outlook.com partial | Gmail App, Outlook Win |
| [data-ogsc] image swap | Outlook App all, Outlook.com | Gmail App, Apple Mail |
| Forced background color | Gmail App Android, Apple Mail | Outlook Win, Office 365 Win |
| VML background and button | Outlook 2013–2021 Win | All others |
| Gmail iOS blend-mode fix | Gmail App iOS only | All others |
| Web fonts | Apple Mail, iOS Mail, some webmail | Outlook desktop all versions |
| CSS animation and GIF | Most modern clients | Outlook desktop first frame only |
| Background images CSS | Apple Mail, iOS, Gmail Web | Outlook desktop use VML |
| Hover effects | Apple Mail, iOS Mail | Outlook desktop, Gmail App |

---

---

## SECTION F — ACCESSIBILITY

---

## F1. Accessibility Requirements

- **Color contrast:** Minimum 4.5:1 for body text, 3:1 for large text (WCAG AA)
- Check all combinations: light-on-dark AND dark-on-light
- All images must have descriptive `alt` text
- Do not rely on color alone to convey information
- Links must be distinguishable from body text (underline or weight difference)
- CTA buttons must have sufficient padding for touch targets — minimum 44x44px
- Dark Mode color swaps must also meet contrast requirements in their dark state
- Use semantic HTML where possible — headings, paragraphs
- Always add `lang` attribute to `<html>` element

---

---

## SECTION G — CHECKLISTS AND REFERENCE

---

## G1. Required Email Elements

Every template must include:
- [ ] DOCTYPE html as line 1
- [ ] lang="en" on html element
- [ ] All required meta tags including color-scheme
- [ ] Preheader text hidden, under 90 characters, with zero-width spaces
- [ ] Brand logo in header
- [ ] Main content area
- [ ] CTA button VML Outlook-safe, minimum touch target 44x44px
- [ ] Footer with:
  - [ ] Physical mailing address (CAN-SPAM / GDPR)
  - [ ] Unsubscribe link one-click, no friction
  - [ ] Email preferences link
  - [ ] Privacy policy link
  - [ ] Terms of service link
- [ ] role="presentation" on all layout tables
- [ ] border-collapse:collapse on all tables
- [ ] All critical CSS inlined on every element

---

## G2. Dimensions and Spacing Reference

| Element | Size |
|---|---|
| Email max-width | 768px |
| Mobile breakpoint | 600px |
| Mobile render test | 375px |
| Header image | 768px wide, 200–300px tall |
| Hero image | 768px wide, 300–500px tall |
| Body padding | 20–40px horizontal |
| Button padding | 14px vertical, 28px horizontal |
| Button touch target | min 44x44px |
| Font body | 14–16px |
| Font heading | 22–34px |
| Line height body | 1.5 (150%) |
| Preheader text | 90 characters max |
| Subject line | 50 characters max |
| Total email size | 100KB max excluding images |
| Image size | 200KB max per image |

---

## G3. Pre-Send Checklist

### Code and Structure
- [ ] DOCTYPE html is line 1
- [ ] All layout uses table not div
- [ ] All critical CSS inlined on every element
- [ ] All images have width HTML attribute AND style="width:Xpx" inline
- [ ] All images have descriptive alt text
- [ ] All links include https:// protocol
- [ ] Web-safe font fallbacks defined everywhere
- [ ] line-height paired with mso-line-height-rule:exactly
- [ ] border-collapse:collapse on all tables
- [ ] HTML validated — no broken tags
- [ ] Total email size 100KB max excluding images

### Dark Mode
- [ ] color-scheme and supported-color-schemes meta tags present
- [ ] .light-img / .dark-img classes on all logo and hero images
- [ ] @media prefers-color-scheme dark block covers all background and text classes
- [ ] [data-ogsc] block duplicates all the same overrides
- [ ] Dark Mode colors pass WCAG contrast requirements
- [ ] CTA button color updated for dark context — lightened version

### Content and Compliance
- [ ] Preheader text set — under 90 characters, not a duplicate of subject line
- [ ] Subject line under 50 characters
- [ ] Unsubscribe link present and working
- [ ] Physical mailing address in footer
- [ ] Privacy policy and Terms of Service links present
- [ ] Plain-text version included
- [ ] Spam score checked

### Testing
- [ ] Gmail web, Gmail iOS, Gmail Android
- [ ] Outlook 2016, 2019, Outlook Web App
- [ ] Apple Mail macOS and iOS
- [ ] Yahoo Mail
- [ ] Renders correctly at 768px desktop and 375px mobile
- [ ] Dark Mode tested in Apple Mail, iOS Mail, Outlook App
- [ ] Outlook desktop Windows — check full inversion behavior
- [ ] Gmail App iOS — check full inversion behavior
- [ ] All dynamic tokens and personalization fields tested with data

---

## G4. Testing Tools

| Tool | Purpose |
|---|---|
| Litmus | Cross-client rendering and Dark Mode preview |
| Email on Acid | Outlook version rendering, Dark Mode testing |
| Mailtrap | Sandbox send testing |
| PutsMail | Send HTML to any inbox for real client testing |
| Accessible Colors — accessible-colors.com | WCAG contrast checker |
| Can I Email — caniemail.com | CSS and HTML support by email client |
| HowToTarget.email | Email client targeting cheat sheet |

---

---

## SECTION H — NEW TEMPLATE VALIDATION PROTOCOL

> **REQUIRED:** Run this full validation on every new template before declaring it production-ready.
> Fail on any item marked **[BLOCKER]**. Flag items marked **[WARN]** for review.

---

## H1. How to Use This Validation

When a new template is created or edited, apply each check below in order.
For each item:
- **PASS** — requirement met
- **FAIL [BLOCKER]** — template cannot ship; fix before proceeding
- **WARN** — deviation from standard; requires justification or fix

Report format is defined in H6.

---

## H2. Structure and DOCTYPE Checks

| # | Check | Severity | How to Verify |
|---|---|---|---|
| S1 | `<!DOCTYPE html>` is the absolute first line — no whitespace before it | BLOCKER | Read line 1 of the HTML file |
| S2 | `<html>` has `lang="en"`, `xmlns:v`, and `xmlns:o` attributes | BLOCKER | Inspect `<html>` opening tag |
| S3 | All layout uses `<table>` / `<tr>` / `<td>` — no `<div>` for columns or rows | BLOCKER | Search for `<div` used as layout wrapper |
| S4 | `role="presentation"` present on every layout `<table>` | BLOCKER | Grep for `<table` — all must have `role="presentation"` |
| S5 | `cellpadding="0" cellspacing="0" border="0"` on every `<table>` | BLOCKER | Grep for `<table` — verify all three attributes |
| S6 | `border-collapse:collapse` on every `<table>` and `<td>` in CSS reset | BLOCKER | Check `<style>` reset block |
| S7 | Max-width is `768px` (or explicitly justified deviation) | WARN | Check `max-width` value on email container |
| S8 | Outer wrapper `<table>` is `width="100%"`; inner container is `max-width:768px` | BLOCKER | Verify two-level table structure |
| S9 | No `<script>` tags in template body | BLOCKER | Grep for `<script` |
| S10 | No `<svg>` tags used for icons or logos (use `<img>` instead) | BLOCKER | Grep for `<svg` — SVGs are blocked by Outlook and Gmail |
| S11 | No `<div>` used for layout — `<div>` allowed only for preheader and preview toggles removed in production | BLOCKER | Grep for `<div` — verify purpose of each |
| S12 | No `<form>`, `<input>`, `<button>`, `<iframe>`, `<canvas>` tags | BLOCKER | Grep for each forbidden tag |
| S13 | No `<link>` stylesheet imports | BLOCKER | Grep for `<link rel` |
| S14 | No `@import` inside `<style>` — includes Google Fonts import | BLOCKER | Check `<style>` block for `@import` |

---

## H3. Meta Tags and Head Checks

| # | Check | Severity | How to Verify |
|---|---|---|---|
| M1 | `<meta charset="UTF-8">` present | BLOCKER | Check `<head>` |
| M2 | `<meta name="viewport" content="width=device-width, initial-scale=1.0">` present | BLOCKER | Check `<head>` |
| M3 | `<meta http-equiv="X-UA-Compatible" content="IE=edge">` present | BLOCKER | Check `<head>` |
| M4 | `<meta name="format-detection" content="telephone=no, date=no, address=no, email=no">` present | BLOCKER | Check `<head>` — prevents iOS auto-linking |
| M5 | `<meta name="x-apple-disable-message-reformatting">` present | BLOCKER | Check `<head>` |
| M6 | `<meta name="color-scheme" content="light dark">` present | BLOCKER | Check `<head>` — required for Apple Mail dark mode |
| M7 | `<meta name="supported-color-schemes" content="light dark">` present | BLOCKER | Check `<head>` |
| M8 | `<title>` tag present and not empty | WARN | Check `<head>` |
| M9 | Outlook `<!--[if mso]>` PixelsPerInch block present | BLOCKER | Check `<head>` for `o:PixelsPerInch` |

---

## H4. Preheader Check

| # | Check | Severity | How to Verify |
|---|---|---|---|
| P1 | Preheader `<div>` present immediately after `<body>` opening | BLOCKER | Read first `<div>` in `<body>` |
| P2 | Preheader uses `display:none; max-height:0; overflow:hidden; mso-hide:all;` | BLOCKER | Verify inline style on preheader `<div>` |
| P3 | Preheader text is under 90 characters | BLOCKER | Count characters (excluding zero-width spacers) |
| P4 | Preheader contains zero-width spacers (`&zwnj;&nbsp;&#847;`) to prevent fallback | WARN | Check after preheader text content |
| P5 | Preheader text is NOT a duplicate of the subject line | WARN | Compare against provided subject line |

---

## H5. Inline CSS and Typography Checks

| # | Check | Severity | How to Verify |
|---|---|---|---|
| T1 | All critical CSS is inlined on every `<td>`, `<p>`, `<h1>`–`<h3>` | BLOCKER | Spot-check 5+ elements — no element relies solely on `<style>` block |
| T2 | All text elements have `font-family` inline with web-safe fallback | BLOCKER | Grep for `<p ` and `<h` — all must have `font-family:` |
| T3 | All `<p>` and `<h1>`–`<h3>` have `color` inline | BLOCKER | Grep for `<p ` and `<h` — all must have `color:` |
| T4 | `mso-line-height-rule:exactly` paired with every `line-height` | BLOCKER | Grep for `line-height` — verify pairing |
| T5 | `margin:0` or explicit margin on every `<h1>`–`<h3>` — no default UA margins | BLOCKER | Inspect all heading elements |
| T6 | No `padding` on `<p>` or `<div>` elements — padding only on `<td>` | BLOCKER | Grep for `<p style` containing `padding` |
| T7 | Font sizes use `px` units — no `em` or `rem` | BLOCKER | Grep for `em` or `rem` in inline styles |
| T8 | Body text minimum `14px`; heading text minimum `20px` | WARN | Spot-check font-size values |

---

## H6. Image Checks

| # | Check | Severity | How to Verify |
|---|---|---|---|
| I1 | All `<img>` have `alt` attribute (can be empty string for decorative images) | BLOCKER | Grep for `<img` — all must have `alt=` |
| I2 | All `<img>` have both HTML `width=""` attribute AND `style="width:Xpx"` | BLOCKER | Inspect all `<img>` tags |
| I3 | All `<img>` have both HTML `height=""` attribute AND `style="height:Xpx"` | BLOCKER | Inspect all `<img>` tags |
| I4 | `display:block` on all `<img>` | BLOCKER | Inspect all `<img>` inline styles |
| I5 | All image `src` URLs are absolute (start with `https://`) — no relative paths | BLOCKER | Grep for `src="` — must start with `https://` or be a placeholder |
| I6 | Light/dark logo swap uses `.light-img` / `.dark-img` class pattern | WARN | Check header logo section |
| I7 | Logo has `aria-label` or meaningful `alt` | BLOCKER | Inspect logo `<img>` or SVG |

---

## H7. Dark Mode Checks

| # | Check | Severity | How to Verify |
|---|---|---|---|
| D1 | `@media (prefers-color-scheme: dark)` block present in `<style>` | BLOCKER | Check `<style>` block |
| D2 | `[data-ogsc]` block duplicates all `@media` dark mode overrides | BLOCKER | Check `<style>` block — must mirror every dark rule |
| D3 | `.email-bg`, `.email-header`, `.email-hero`, `.email-footer` classes covered in dark block | BLOCKER | Check dark mode class coverage |
| D4 | `h1`, `h2`, `h3`, `p`, `td`, `span` color overrides in dark block | BLOCKER | Inspect dark mode text overrides |
| D5 | CTA button `.cta-btn` has a lightened dark-mode color override | BLOCKER | Check `.cta-btn` in dark block — `#3a6bbf` or equivalent |
| D6 | Dark mode text colors pass WCAG 4.5:1 contrast on dark backgrounds | BLOCKER | Manually verify using Accessible Colors tool |
| D7 | `.light-img` / `.dark-img` image swaps defined in `<style>` reset and dark block | WARN | Check class definitions in both locations |

---

## H8. CTA Button Check

| # | Check | Severity | How to Verify |
|---|---|---|---|
| B1 | CTA button uses Outlook VML `<v:roundrect>` wrapped in `<!--[if mso]>` | BLOCKER | Check button HTML structure |
| B2 | Non-Outlook fallback button uses `<a>` styled as button — not `<button>` element | BLOCKER | Check `<!--[if !mso]><!-->` button markup |
| B3 | Button minimum touch target `44x44px` — check `min-height` and `padding` | BLOCKER | Verify button `padding` values |
| B4 | Button `href` uses `https://` URL or valid template token | BLOCKER | Inspect `href` value |
| B5 | `.mobile-btn` class on non-Outlook button for full-width mobile stacking | WARN | Inspect button `class` attribute |

---

## H9. Footer and Compliance Checks

| # | Check | Severity | How to Verify |
|---|---|---|---|
| C1 | Footer contains physical mailing address (CAN-SPAM requirement) | BLOCKER | Read footer section for street address |
| C2 | Unsubscribe link present — one-click, no friction | BLOCKER | Locate `{{unsubscribe_url}}` or equivalent token |
| C3 | Email preferences link present | WARN | Locate preferences link in footer |
| C4 | Privacy policy link present | BLOCKER | Locate privacy link in footer |
| C5 | Terms of service link present | BLOCKER | Locate terms link in footer |
| C6 | Copyright line present with correct year | WARN | Read footer copyright text |
| C7 | All footer links use `https://` or valid template tokens — no `href="#"` placeholders | BLOCKER | Inspect all `<a href=` in footer |

---

## H10. Visa-Specific Brand Checks

| # | Check | Severity | How to Verify |
|---|---|---|---|
| V1 | Visa brand blue `#1434CB` used for headers, hero, and CTAs | WARN | Inspect background-color on header and hero `<td>` |
| V2 | Visa logo present in header — SVG must have Outlook `<!--[if mso]>` image fallback | BLOCKER | Check header section for MSO conditional |
| V3 | Visa logo present in footer | WARN | Check footer for logo image or SVG |
| V4 | All `href="#"` placeholder links replaced with real URLs or valid AJO / template tokens before production send | BLOCKER | Grep for `href="#"` — must be zero in production template |
| V5 | "Click to Pay" trademark reference in footer if template features Click to Pay | WARN | Check footer for EMVCo trademark line |
| V6 | Contact email `checkoutwithvisa@visa.com` or equivalent Visa contact present in footer for error-report path | WARN | Check footer for contact email |

---

## H11. Mobile Responsiveness Checks

| # | Check | Severity | How to Verify |
|---|---|---|---|
| R1 | `@media only screen and (max-width:600px)` block present in `<style>` | BLOCKER | Check `<style>` block |
| R2 | `.email-container` or `.email-wrapper` set to `width:100% !important` in mobile block | BLOCKER | Check mobile media query |
| R3 | Multi-column rows use `.stack-column` class that collapses to `display:block` on mobile | WARN | Check any side-by-side column layouts |
| R4 | CTA button uses `.mobile-btn` for full-width on mobile | WARN | Check mobile block for `.mobile-btn` rule |
| R5 | `h1` font size reduced to `24px` minimum on mobile | WARN | Check mobile `h1` override |
| R6 | Horizontal padding reduced to `16–20px` on mobile via `.px-mobile` or `.mobile-padding` | WARN | Check mobile padding overrides |

---

## H12. Common Mistakes — Auto-Flag List

When reviewing a new template, explicitly check for these patterns that signal common errors:

```
GREP TARGETS — any hit is a likely BLOCKER:

grep -n "<div"              → Check each — layout divs are forbidden
grep -n "<svg"              → All SVGs must be removed; replace with <img>
grep -n "<script"           → Scripts are stripped by all email clients
grep -n "<form\|<input\|<button\|<iframe\|<canvas"  → All forbidden tags
grep -n "@import"           → Google Fonts @import stripped by Gmail
grep -n 'href="#"'          → Placeholder links must be replaced
grep -n "padding:" | grep "<p\|<div\|<table"        → Padding on forbidden elements
grep -n "margin:" | grep "left\|right"              → Horizontal margins unreliable in Outlook
grep -n "font-size.*em\|font-size.*rem"             → em/rem forbidden
grep -n "display:flex\|display:grid"               → Flex/Grid ignored by Outlook
grep -n "border-radius"     → Works in most clients except Outlook — acceptable if VML fallback present
```

---

## H13. Validation Report Template

After running all checks, produce a validation report in this format:

```
TEMPLATE VALIDATION REPORT
===========================
Template:     [filename]
Date:         [YYYY-MM-DD]
Reviewer:     Claude / [human]

BLOCKERS (must fix before shipping):
  [S10] SVG icons found — replace with <img> tags (lines 121, 138, 154, 172)
  [M6]  color-scheme meta tag missing
  [C1]  No physical mailing address in footer

WARNINGS (fix or justify):
  [S7]  Max-width is 600px, not 768px — confirm intentional
  [D7]  No .light-img / .dark-img swap on logo

PASSED:
  [S1]  DOCTYPE correct
  [S4]  role="presentation" on all tables
  [M1–M5] All required meta tags present (except M6)
  [T4]  mso-line-height-rule:exactly correctly paired
  [B1–B3] CTA button Outlook-safe and correct touch target
  ...

VERDICT: FAIL — 3 blockers must be resolved
```

---

---

---

## SECTION I — VISA / CTP FIGMA DESIGN TOKENS

> **Source:** Figma file `CTP to SRC Comms Updates` — key `XiPE3HFLRSUAlWj75K7AMn`, node `44-3330`
> **Last synced:** 2026-05-07
> **CSS variables file:** `template/figma-variables.css`
> Note: CSS custom properties are not supported in email clients — always resolve to the hex/px literals below when writing inline styles.

---

## I1. Brand Color Tokens

| Token | Hex | Usage |
|---|---|---|
| `--visa-blue` | `#1434cb` | CardHeader background, hero band, CTA button, H1 title |
| `--visa-blue-active` | `#0d68f2` | Links, "Manage my cards", active state text |
| `--visa-blue-nav` | `#0042b3` | Navigation arrows, secondary blue |
| `--visa-blue-light` | `#e8edfc` | Identifiers container / light panel background |
| `--visa-blue-pale` | `#f2f4f8` | Linked details section background |

---

## I2. Text Color Tokens

| Token | Hex | Usage |
|---|---|---|
| `--text-heading` | `#1434cb` | Hero title on white |
| `--text-primary` | `#141413` | Body copy (warm near-black) |
| `--text-section` | `#292829` | Section headings (How it works, FAQ) |
| `--text-card` | `#1f242e` | Card details, bullet text, SF Pro labels |
| `--text-meta` | `#22242a` | Email client metadata (subject, sender) |
| `--text-secondary` | `#374053` | Status bar, secondary muted elements |
| `--text-footer` | `#4a4a4a` | Footer copy, legal text |
| `--text-muted` | `#686b70` | Timestamps, "to me" metadata |
| `--text-placeholder` | `#848fa4` | Tertiary / placeholder / additional info line |
| `--text-on-brand` | `#ffffff` | White text on Visa blue backgrounds |

---

## I3. Background Color Tokens

| Token | Hex | Usage |
|---|---|---|
| `--bg-page` | `#f0f0f0` | EmailLayout outer page background |
| `--bg-card` | `#ffffff` | Email body / main card background |
| `--bg-near-white` | `#fcfcfe` | Nav bar, EU 2024 Draft frame |
| `--bg-section-light` | `#f5f5f5` | Identifiers section |
| `--bg-panel-blue` | `#f2f4f8` | Linked details panel |
| `--bg-container-blue` | `#e8edfc` | Identifier container (light blue) |
| `--bg-tag` | `#e7e9ef` | Chip / tag background |
| `--bg-divider` | `#e6e6e6` | EmailLayout border, subtle dividers |
| `--bg-header` | `#1434cb` | CardHeader / email hero band |
| `--bg-footer` | `#fafafa` | Footer band background |
| `--bg-button` | `#1434cb` | Primary CTA button fill |

---

## I4. Dark Mode Token Overrides

| Token | Hex | Overrides |
|---|---|---|
| `--dark-bg-page` | `#1a1a1a` | `--bg-page` |
| `--dark-bg-card` | `#1a1a1a` | `--bg-card` |
| `--dark-bg-header` | `#1e1e1e` | `--bg-header` |
| `--dark-bg-hero` | `#0d1b3e` | Hero section band |
| `--dark-bg-panel` | `#0d1b2a` | `--bg-panel-blue` |
| `--dark-bg-footer` | `#111111` | `--bg-footer` |
| `--dark-bg-button` | `#3a6bbf` | `--bg-button` — lightened for contrast |
| `--dark-text-heading` | `#bdd5e8` | `--text-heading` |
| `--dark-text-primary` | `#e0e0e0` | `--text-primary` |
| `--dark-text-section` | `#7ab3d4` | `--text-section` |
| `--dark-text-footer` | `#888888` | `--text-footer` |
| `--dark-text-link` | `#7ab3d4` | `--visa-blue-active` links |
| `--dark-border` | `#2a3a4a` | `--bg-divider` |

---

## I5. Typography Scale (Figma Specs)

| Role | Family | Size | Weight | Line Height | Color token |
|---|---|---|---|---|---|
| Hero / Display | Helvetica | 36px | 700 | 47px | `--text-heading` `#1434cb` |
| CTA / Page title | Inter | 32px | 600 | 48px | `--text-heading` `#1434cb` |
| Card title | Inter | 20px | 600 | 30px | `#1a1a1a` |
| Section heading H2 | Figtree / Montserrat | 21px | 700 | 25px | `--text-section` `#292829` |
| FAQ question H3 | Figtree / Montserrat | 18px | 700 | 22px | `--text-primary` `#141413` |
| Body copy | Figtree / Montserrat | 14–18px | 400 | 21–22px | `--text-primary` `#141413` |
| Body (Figma Make) | Inter | 14px | 400 | 21px | `--text-footer` `#4a4a4a` |
| Link / Manage text | Figtree | 18px | 500 | — | `--visa-blue-active` `#0d68f2` |
| Identifier / Account | Helvetica | 16px | 700 | 24px | `#000000` |
| Footer / legal | Visa Dialect UI / Helvetica | 11px | 400 | 16px | `--text-footer` `#4a4a4a` |
| Email subject | Open Sans | 19px | 400 | 26px | `--text-meta` `#22242a` |

**Email-safe font stack:** `Arial, Helvetica, Verdana, sans-serif`

---

## I6. Layout and Spacing Tokens (from Figma Frame Measurements)

| Token | Value | Usage |
|---|---|---|
| `--email-desktop-width` | `768px` | Desktop email container max-width |
| `--email-mobile-width` | `393px` | Mobile frame reference width |
| `--email-mobile-break` | `600px` | `@media` breakpoint |
| `--pad-header-v` | `24px` | CardHeader top/bottom padding |
| `--pad-header-h` | `32px` | CardHeader left/right padding |
| `--pad-content-v` | `32px` | CardContent top/bottom padding |
| `--pad-content-h` | `32px` | CardContent left/right padding |
| `--pad-footer-v` | `24px` | Footer top/bottom padding |
| `--pad-footer-h` | `32px` | Footer left/right padding |
| `--pad-section-v` | `24px` | Identifiers / panel sections |
| `--pad-mobile-h` | `16px` | Mobile horizontal padding |
| `--gap-layout` | `32px` | EmailLayout top-level item gap |
| `--gap-section` | `24px` | How it works, FAQ section gap |
| `--gap-item` | `16px` | Items within sections |
| `--gap-icon-text` | `12px` | Icon-to-text spacing |
| `--gap-paragraph` | `8px` | Heading to paragraph gap |
| `--icon-size` | `28px` | How it works step icons |
| `--icon-col-width` | `44px` | Icon column width in icon rows |
| `--logo-width` | `72px` | Visa logo width |
| `--logo-height` | `24px` | Visa logo height |

---

## I7. Quick-Reference Inline Values for Email HTML

Use these resolved values directly in template inline styles (no CSS variable needed):

```html
<!-- Hero / Header band -->
<td style="background-color:#1434cb; padding:24px 32px;">

<!-- Hero H1 title -->
<h1 style="font-family:Tahoma,Helvetica,Arial,sans-serif;
           font-size:36px; font-weight:400; color:#ffffff;
           line-height:47px; mso-line-height-rule:exactly; margin:0;">

<!-- Section heading H2 -->
<h2 style="font-family:Verdana,Arial,Helvetica,sans-serif;
           font-size:36px; font-weight:400; color:#292829;
           line-height:47px; mso-line-height-rule:exactly; margin:0 0 16px 0;">

<!-- FAQ question H3 -->
<h3 style="font-family:Verdana,Arial,Helvetica,sans-serif;
           font-size:18px; font-weight:400; color:#141413;
           line-height:22px; mso-line-height-rule:exactly; margin:0 0 8px 0;">

<!-- Body copy -->
<p style="font-family:Verdana,Arial,Helvetica,sans-serif;
          font-size:14px; font-weight:400; color:#141413;
          line-height:21px; mso-line-height-rule:exactly; margin:0 0 18x 0;">

<!-- Link -->
<a style="color:#0d68f2; text-decoration:underline; font-weight:500;">

<!-- Footer / legal -->
<p style="font-family:Verdana,Arial,Helvetica,sans-serif;
          font-size:11px; color:#4a4a4a;
          line-height:16px; mso-line-height-rule:exactly; margin:0 0 12px 0;">

<!-- Identifiers panel background -->
<td style="background-color:#e8edfc; padding:24px;">

<!-- Linked details panel background -->
<td style="background-color:#f2f4f8; padding:24px;">

<!-- Footer band -->
<td style="background-color:#fafafa; padding:24px 32px;">

<!-- Divider border -->
<td style="border-top:1px solid #E5E5E5; font-size:0; line-height:0;">&nbsp;</td>
```

---

*Last updated: May 2026 | Standards: CAN-SPAM, GDPR, WCAG AA, Email on Acid, Litmus verified patterns*
*Sources: SKILL.md v1 (base standards), SKILL.md v2 (email-template skill), best-theme-newsletter-skills.md (dark mode and theming)*

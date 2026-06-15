---
name: visa-ctp-email-styling
description: >
  Complete design system and styling guide for building Visa Click to Pay (CTP)
  branded HTML email templates. Use this skill whenever the user wants to create,
  edit, or style any email related to Visa Click to Pay, CTP onboarding, CTP
  transactional messages, or any Visa-branded email that should match the CTP
  design system. Trigger when the user mentions "CTP email", "Click to Pay email",
  "Visa email template", "CTP welcome email", "CTP styling", "Visa email design
  tokens", or asks to build a new email that matches the existing Visa CTP look
  and feel. Always use this skill before writing any HTML or CSS for a Visa CTP
  email — it defines the exact colors, fonts, spacing, layout structure, and
  component patterns extracted directly from the approved Figma design.
---

# Visa Click to Pay — Email Styling Skill

> Design tokens, verified component patterns, and construction rules for producing
> on-brand Visa CTP HTML emails that match the approved Figma reference design.
> All patterns in this skill are production-verified across Gmail, Outlook Windows
> (light + dark mode), Apple Mail, iOS Mail, Samsung Email, and Yahoo Mail.

---

## STEP 0 — Read This First

Before writing any HTML or CSS for a Visa CTP email:

1. Read this file for **Visa CTP visual identity** (colors, fonts, spacing, components)
2. Read the `email-template` skill for **cross-client HTML structure** (DOCTYPE, tables, inline CSS, Outlook VML)
3. Read `references/components.md` for **full copy-paste HTML snippets**

---

## SECTION 1 — DESIGN TOKENS

### 1.1 Color Palette

#### Light Mode

| Token | Hex | Usage |
|---|---|---|
| Primary Visa Blue | `#1434CB` | Hero bg, headings, links, CTA, bullet dots |
| Info Surface | `#C7EDFF` | Subheader band background |
| Surface 1 | `#FFFFFF` | Email body background |
| Surface 2 | `#F5F5F5` | Legal footer background |
| Page Background | `#F0F0F0` | Outer wrapper background |
| Text Default | `#000000` | Primary body copy |
| Text Subtle | `#4A4A4A` | Footer text, secondary copy |
| Text Link | `#1434CB` | Inline hyperlinks |
| Divider | `#E0E0E0` | 1px horizontal rules |
| Hero H1 | `#FFFFFF` | Hero heading text |
| Hero Subline | `#C7EDFF` | Hero subline ("Your card on-demand") |

#### Dark Mode Overrides

| Element | Light | Dark |
|---|---|---|
| Page bg (`.bg-pg`) | `#F0F0F0` | `#1A1A1A` |
| Header bg (`.bg-hd`) | `#FFFFFF` | `#1A1A1A` |
| Body bg (`.bg-bd`) | `#FFFFFF` | `#1A1A1A` |
| Footer bg (`.bg-ft`) | `#F5F5F5` | `#2A2A2A` |
| Copyright bg (`.bg-cp`) | `#FFFFFF` | `#1A1A1A` |
| Body text (`.tx`) | `#000000` | `#E8E8E8` |
| White heading (`.tw`) | `#1434CB` | `#FFFFFF` |
| Accent heading (`.th`) | `#1434CB` | `#6B9FFF` |
| OTP subtitle (`.ts`) | `#4A4A4A` | `#AAAAAA` |
| Footer text (`.tm`) | `#4A4A4A` | `#AAAAAA` |
| Links (`.tl`) | `#1434CB` | `#6B9FFF` |
| Notice text (`.tn`) | `#000000` | `#BBBBBB` |
| Bullet dot (`.bd`) | `#1434CB` | `#FFFFFF` |

#### CSS Classes — Full Block

```css
/* LIGHT IMAGE (default visible) */
.li  {display:block        !important;}
.di  {display:none         !important;}
.li-i{display:inline-block !important;}
.di-i{display:none         !important;}

@media(prefers-color-scheme:dark){
  .bg-pg{background-color:#1A1A1A!important;}
  .bg-hd{background-color:#1A1A1A!important;}
  .bg-bd{background-color:#1A1A1A!important;}
  .bg-ft{background-color:#2A2A2A!important;}
  .bg-cp{background-color:#1A1A1A!important;}
  .tx {color:#E8E8E8!important;}
  .tw {color:#FFFFFF!important;}
  .th {color:#6B9FFF!important;}
  .ts {color:#AAAAAA!important;}
  .tm {color:#AAAAAA!important;}
  .tl {color:#6B9FFF!important;}
  .tn {color:#BBBBBB!important;}
  .bd {background-color:#FFFFFF!important;}
  .li  {display:none         !important;}
  .di  {display:block        !important;}
  .li-i{display:none         !important;}
  .di-i{display:inline-block !important;}
}
/* Duplicate EVERY rule for Outlook App dark mode */
[data-ogsc] .bg-pg{background-color:#1A1A1A!important;}
[data-ogsc] .bg-hd{background-color:#1A1A1A!important;}
[data-ogsc] .bg-bd{background-color:#1A1A1A!important;}
[data-ogsc] .bg-ft{background-color:#2A2A2A!important;}
[data-ogsc] .bg-cp{background-color:#1A1A1A!important;}
[data-ogsc] .tx {color:#E8E8E8!important;}
[data-ogsc] .tw {color:#FFFFFF!important;}
[data-ogsc] .th {color:#6B9FFF!important;}
[data-ogsc] .ts {color:#AAAAAA!important;}
[data-ogsc] .tm {color:#AAAAAA!important;}
[data-ogsc] .tl {color:#6B9FFF!important;}
[data-ogsc] .tn {color:#BBBBBB!important;}
[data-ogsc] .bd {background-color:#FFFFFF!important;}
[data-ogsc] .li  {display:none         !important;}
[data-ogsc] .di  {display:block        !important;}
[data-ogsc] .li-i{display:none         !important;}
[data-ogsc] .di-i{display:inline-block !important;}
```

### 1.2 Typography

| Role | Size | Line-height | Weight | Font |
|---|---|---|---|---|
| Hero H1 | 36px | 47px | 400 | Tahoma, Arial |
| Body Heading | 36px | 47px | 400 | Verdana |
| Sub-heading (`h2`) | 22px | 29px | 700 | Verdana Bold |
| OTP Code | 36px | 47px | 400 | Verdana |
| OTP Subtitle | 18px | 23px | 400 | Verdana |
| Body Copy | 16px | 24px | 400 | Verdana |
| Footer / Legal | 11px | 16px | 400 | Verdana |
| Unable-to-view | 10px | 14px | 400 | Verdana |

**Font stacks:**
- Hero: `Tahoma, Arial, Helvetica, sans-serif`
- All else: `Verdana, Arial, Helvetica, sans-serif`
- Always include full web-safe fallback stack

**Letter spacing:** Hero H1 uses `letter-spacing:0.5px`. All other text `0px`.

### 1.3 Spacing

| Element | Value |
|---|---|
| Container max-width | `768px` |
| Section horizontal padding | `32px` |
| Section vertical padding | `24px` |
| Gap after heading (to body text) | `40px` (heading `margin:0 0 40px 0`) |
| Gap between body paragraphs | `20px` |
| Gap between content blocks | `24px` or `40px` (per Figma `gap-[24px]` / `gap-[40px]`) |
| Bottom padding before footer | `24px` |
| Bullet item gap | `12px` |
| Footer paragraph gap | `12px` |

---

## SECTION 2 — LAYOUT STRUCTURE

### 2.1 Row Order (every template)

```
┌──────────────────────────────────────────────────────┐
│  PREHEADER table (hidden)                            │
├──────────────────────────────────────────────────────┤
│  Unable-to-view notice    bg:#FFFFFF  padding:12 16  │
├──────────────────────────────────────────────────────┤
│  Visa Logo header         bg:#FFFFFF  padding:24 32  │
│    ↳ Visa_blue.png (light) / Visa_white.png (dark)   │
│    ↳ 74×24px                                         │
├──────────────────────────────────────────────────────┤
│  CTP Hero Banner          bg:#1434CB                 │
│    ↳ "Click to Pay" Tahoma 36px white                │
│    ↳ "Your card on-demand" Tahoma 22px #C7EDFF       │
│    ↳ VML v:rect for Outlook Windows                  │
├──────────────────────────────────────────────────────┤
│  Body content row(s)      bg:#FFFFFF  padding:32 32  │
│    ↳ Heading (36px .tw)                              │
│    ↳ Body paragraphs (16px .tx)                      │
│    ↳ Action blocks (divider + link+chevron)          │
├──────────────────────────────────────────────────────┤
│  Legal footer             bg:#F5F5F5  padding:24 32  │
│    ↳ 3 paragraphs: service notice / no-reply / CTP   │
├──────────────────────────────────────────────────────┤
│  Copyright row            bg:#FFFFFF  padding:24 32  │
│    ↳ "© [year] Visa. All Rights Reserved." + logo    │
├──────────────────────────────────────────────────────┤
│  Tracking pixel (outside last </table>)              │
└──────────────────────────────────────────────────────┘
```

### 2.2 Container Setup

```html
<!-- OUTER: full-width page bg -->
<table role="presentation" cellpadding="0" cellspacing="0" border="0"
       width="100%" bgcolor="#F0F0F0" style="background-color:#F0F0F0;" class="bg-pg">
<tr><td align="center" valign="top">

  <!-- INNER: 768px content container -->
  <table role="presentation" cellpadding="0" cellspacing="0" border="0"
         width="768" bgcolor="#FFFFFF"
         style="width:100%;max-width:768px;background-color:#FFFFFF;" class="bg-bd">

    <!-- All template rows go here -->

  </table>
</td></tr>
</table>
```

---

## SECTION 3 — VERIFIED COMPONENT PATTERNS

All patterns are production-verified. Use exactly as shown.

### 3.1 Preheader (Table Pattern — Required)

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

### 3.2 Unable-to-View Row

```html
<tr>
  <td bgcolor="#FFFFFF"
      style="background-color:#FFFFFF;padding:12px 16px;" class="bg-hd">
    <p style="margin:0;font-family:Verdana,Arial,Helvetica,sans-serif;
              font-size:10px;color:#000000;line-height:14px;
              mso-line-height-rule:exactly;" class="tn">
      If you are unable to view this message correctly,&#32;<a
        href="{{view_online_url}}" target="_blank" rel="noopener noreferrer"
        style="color:#1434CB;text-decoration:underline;" class="tl">click here</a>.
    </p>
  </td>
</tr>
```

### 3.3 Visa Logo Header Row

```html
<tr>
  <td bgcolor="#FFFFFF"
      style="background-color:#FFFFFF;padding:24px 32px;
             mso-padding-alt:24px 32px;font-size:0;line-height:0;
             mso-line-height-rule:exactly;" class="bg-hd">
    <!--[if mso]><img src="https://[CDN]/Visa_blue.png" width="74" height="24"
      alt="Visa" style="display:block;width:74px;height:24px;border:0;"><![endif]-->
    <!--[if !mso]><!-->
    <img class="li" src="https://[CDN]/Visa_blue.png"
         width="74" height="24" alt="Visa"
         style="display:block;width:74px;height:24px;border:0;">
    <img class="di" src="https://[CDN]/Visa_white.png"
         width="74" height="24" alt="Visa"
         style="display:none;width:74px;height:24px;border:0;">
    <!--<![endif]-->
  </td>
</tr>
```

**Why `mso-padding-alt` + `font-size:0;line-height:0`:** Outlook Windows adds implicit
whitespace around MSO-conditional images unless the cell collapses its line-height.

### 3.4 CTP Hero Banner

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
      <h1 style="margin:0 0 12px 0;padding:0;
                 font-family:Tahoma,Arial,Helvetica,sans-serif;
                 font-size:36px;font-weight:400;color:#FFFFFF;line-height:47px;
                 letter-spacing:0.5px;mso-line-height-rule:exactly;">Click to Pay</h1>
      <p  style="margin:0;padding:0;
                 font-family:Tahoma,Arial,Helvetica,sans-serif;
                 font-size:22px;font-weight:400;color:#C7EDFF;line-height:29px;
                 mso-line-height-rule:exactly;">Your card on-demand</p>
    </td></tr>
    </table>
    <!--[if mso]></v:textbox></v:rect><![endif]-->
  </td>
</tr>
```

**Critical:** outer `<td>` must have `font-size:0;line-height:0;mso-line-height-rule:exactly`
to prevent Outlook Windows inserting whitespace above/below the hero banner in dark mode.

### 3.5 Body Heading

```html
<p style="margin:0 0 40px 0;padding:0;
          font-family:Verdana,Arial,Helvetica,sans-serif;
          font-size:36px;font-weight:400;color:#1434CB;line-height:47px;
          letter-spacing:0.5px;mso-line-height-rule:exactly;" class="tw">
  Heading text here
</p>
```

- `.tw` → `#1434CB` light / `#FFFFFF` dark — all main section headings
- `.th` → `#1434CB` light / `#6B9FFF` dark — used on `<h2>` sub-headings only
- `margin:0 0 40px 0` — 40px gap between heading and first body paragraph

### 3.6 Body Paragraph

```html
<p style="margin:0 0 20px 0;font-family:Verdana,Arial,Helvetica,sans-serif;
          font-size:16px;font-weight:400;color:#000000;line-height:24px;
          mso-line-height-rule:exactly;" class="tx">
  Paragraph text. Last paragraph in block has <code>margin:0</code>.
</p>
```

### 3.7 Action Block (Divider + Link + Chevron)

```html
<!-- ACTION BLOCK ROW -->
<tr>
  <td bgcolor="#FFFFFF"
      style="background-color:#FFFFFF;padding:24px 32px 24px 32px;" class="bg-bd">

    <!-- 1px Divider — Outlook Windows safe -->
    <table role="presentation" cellpadding="0" cellspacing="0" border="0"
           width="100%" height="1" style="margin-bottom:24px;max-height:1px;">
    <tr>
      <td height="1"
          style="height:1px;max-height:1px;background-color:#E0E0E0;
                 font-size:0;line-height:0;mso-line-height-rule:exactly;
                 overflow:hidden;"></td>
    </tr>
    </table>

    <!-- Description text -->
    <p style="margin:0 0 20px 0;font-family:Verdana,Arial,Helvetica,sans-serif;
              font-size:16px;font-weight:400;color:#000000;line-height:24px;
              mso-line-height-rule:exactly;" class="tx">
      Description text here.
    </p>

    <!-- Link + Chevron -->
    <table role="presentation" cellpadding="0" cellspacing="0" border="0">
    <tr>
      <td valign="middle" style="padding-right:5px;">
        <a href="https://example.com" target="_blank" rel="noopener noreferrer"
           style="color:#1434CB;text-decoration:underline;
                  font-family:Verdana,Arial,Helvetica,sans-serif;
                  font-size:16px;font-weight:400;line-height:24px;
                  mso-line-height-rule:exactly;" class="tl">Link Label</a>
      </td>
      <td valign="middle" style="padding:0;">
        <!--[if mso]><img src="https://[CDN]/chevron_blue.png" width="8" height="16"
          alt="" style="display:block;width:8px;height:16px;border:0;"><![endif]-->
        <!--[if !mso]><!-->
        <img class="li-i" src="https://[CDN]/chevron_blue.png"
             width="8" height="16" alt=""
             style="display:inline-block;vertical-align:middle;
                    width:8px;height:16px;border:0;">
        <img class="di-i" src="https://[CDN]/arrow-right-white.png"
             width="8" height="16" alt=""
             style="display:none;vertical-align:middle;
                    width:8px;height:16px;border:0;">
        <!--<![endif]-->
      </td>
    </tr>
    </table>

  </td>
</tr>
```

**Divider rules:**
- `height="1"` HTML attribute — Outlook respects this over CSS `height:1px`
- `max-height:1px` on both `<table>` and `<td>` — prevents expansion
- `overflow:hidden` — clips Outlook rendering to declared height
- **Empty cell — no `&nbsp;`** — any text character forces line-height expansion

### 3.8 Bullet List

Figma badge pattern: outer `<div>` is `6px wide × 24px tall` (matches `line-height:24px`).
Inner dot `6×6px` centred at `padding-top:9px` = `(24 − 6) ÷ 2`.

```html
<table role="presentation" cellpadding="0" cellspacing="0" border="0" width="100%">

  <!-- Bullet item (repeat per item; last item has no padding-bottom) -->
  <tr>
    <td width="14" valign="top"
        style="width:14px;padding-right:8px;padding-bottom:12px;">
      <div style="width:6px;height:24px;padding-top:9px;
                  font-size:0;line-height:0;mso-line-height-rule:exactly;">
        <div class="bd"
             style="width:6px;height:6px;background-color:#1434CB;
                    border-radius:50%;font-size:0;line-height:0;
                    mso-line-height-rule:exactly;">
          <!--[if mso]>&nbsp;<![endif]-->
        </div>
      </div>
    </td>
    <td valign="top" style="padding-bottom:12px;">
      <p style="margin:0;font-family:Verdana,Arial,Helvetica,sans-serif;
                font-size:16px;font-weight:400;color:#000000;line-height:24px;
                mso-line-height-rule:exactly;" class="tx">
        Bullet item text here.
      </p>
    </td>
  </tr>

</table>
```

- `.bd` class → `#1434CB` light / `#FFFFFF` dark (must be in CSS block)
- `<!--[if mso]>&nbsp;<![endif]-->` — Outlook needs content to paint the bg colour
- `border-radius:50%` is ignored by Outlook (renders as square) — acceptable
- Last bullet item: remove `padding-bottom:12px` from both cells

### 3.9 Legal Footer

```html
<tr>
  <td bgcolor="#F5F5F5"
      style="background-color:#F5F5F5;padding:24px 32px;" class="bg-ft">
    <!-- Para 1: service email notice with ToS + Privacy links -->
    <p style="margin:0 0 12px 0;font-family:Verdana,Arial,Helvetica,sans-serif;
              font-size:11px;font-weight:400;color:#4A4A4A;line-height:16px;
              mso-line-height-rule:exactly;" class="tm">
      This is a service email from Visa. Please note that you may receive service
      emails in accordance with Visa&#8217;s
      <a href="{{tos_url}}" target="_blank" rel="noopener noreferrer"
         style="color:#1434CB;text-decoration:underline;" class="tl">Terms of Service</a>
      and
      <a href="{{privacy_url}}" target="_blank" rel="noopener noreferrer"
         style="color:#1434CB;text-decoration:underline;" class="tl">Privacy Notice</a>,
      whether or not you elect to receive promotional email.
    </p>
    <!-- Para 2: no-reply notice with support email -->
    <p style="margin:0 0 12px 0;font-family:Verdana,Arial,Helvetica,sans-serif;
              font-size:11px;font-weight:400;color:#4A4A4A;line-height:16px;
              mso-line-height-rule:exactly;" class="tm">
      This email was sent from an address that cannot accept incoming messages.
      If you think you received this email in error, please disregard it or let us
      know at
      <a href="mailto:checkoutwithvisa@visa.com" target="_blank"
         rel="noopener noreferrer"
         style="color:#1434CB;text-decoration:underline;"
         class="tl">checkoutwithvisa@visa.com</a>.
    </p>
    <!-- Para 3: CTP icon trademark notice -->
    <p style="margin:0;font-family:Verdana,Arial,Helvetica,sans-serif;
              font-size:11px;font-weight:400;color:#4A4A4A;line-height:16px;
              mso-line-height-rule:exactly;" class="tm">
      Click to Pay icon&#32;<!--[if !mso]><!-->
      <img class="li-i" src="https://[CDN]/icon_CTP_gray_light.png"
           width="13" height="8" alt="Click to Pay"
           style="display:inline-block;vertical-align:middle;
                  width:13px;height:8px;border:0;">
      <img class="di-i" src="https://[CDN]/icon_CTP_gray_dark.png"
           width="13" height="8" alt="Click to Pay"
           style="display:none;vertical-align:middle;
                  width:13px;height:8px;border:0;">
      <!--<![endif]--> is a trademark owned and used with permission of EMVCo, LCC.
    </p>
  </td>
</tr>
```

### 3.10 Copyright Row

```html
<tr>
  <td bgcolor="#FFFFFF"
      style="background-color:#FFFFFF;padding:24px 32px;" class="bg-cp">
    <table role="presentation" cellpadding="0" cellspacing="0"
           border="0" width="100%">
    <tr>
      <td valign="middle"
          style="font-family:Verdana,Arial,Helvetica,sans-serif;font-size:11px;
                 font-weight:400;color:#4A4A4A;line-height:16px;
                 mso-line-height-rule:exactly;" class="tm">
        &copy; 2026 Visa. All Rights Reserved.
      </td>
      <td align="right" valign="middle" width="74"
          style="padding-left:16px;width:74px;">
        <!--[if mso]><img src="https://[CDN]/Visa_blue.png" width="74" height="24"
          alt="Visa" style="display:block;width:74px;height:24px;border:0;"><![endif]-->
        <!--[if !mso]><!-->
        <img class="li" src="https://[CDN]/Visa_blue.png"
             width="74" height="24" alt="Visa"
             style="display:block;width:74px;height:24px;border:0;">
        <img class="di" src="https://[CDN]/Visa_white.png"
             width="74" height="24" alt="Visa"
             style="display:none;width:74px;height:24px;border:0;">
        <!--<![endif]-->
      </td>
    </tr>
    </table>
  </td>
</tr>
```

---

## SECTION 4 — CDN ASSET REFERENCE

Base URL: `https://visaclicktopay-mid-prod1-res.adobe-campaign.com/res/visaclicktopay_mid_prod1/`

| Asset | File | Dimensions | Usage |
|---|---|---|---|
| Visa logo light | `Visa_blue.png` | 74×24px | Header + copyright (light) |
| Visa logo dark | `Visa_white.png` | 74×24px | Header + copyright (dark) |
| CTP icon large light | `icon_CTP_black_light.png` | 26×16px | Inline in body text (light) |
| CTP icon large dark | `icon_CTP_white_dark.png` | 26×16px | Inline in body text (dark) |
| CTP icon footer light | `icon_CTP_gray_light.png` | 13×8px | Footer trademark line (light) |
| CTP icon footer dark | `icon_CTP_gray_dark.png` | 13×8px | Footer trademark line (dark) |
| Chevron light | `chevron_blue.png` | 8×16px | Link+chevron component (light) |
| Chevron dark | `arrow-right-white.png` | 8×16px | Link+chevron component (dark) |

---

## SECTION 5 — TEMPLATE CATALOGUE

### 5.1 Template Types vs Structure

| Template | Gap style | Body paragraphs | Action blocks | Footer |
|---|---|---|---|---|
| Welcome | `gap-[40px]` | How-it-works + FAQ | None | Full 3-para |
| OTP | `gap-[40px]` | 1 (expiry notice) | Support only | 2-para (no service email) |
| OTP with Terms | `gap-[40px]` | 1 + Terms para | Support only | 2-para |
| Info Updated | `gap-[40px]` | 1 short | FAQ + Support | Full 3-para |
| Added Card | `gap-[40px]` | 2 | FAQ + Support | Full 3-para |
| Couldn't Add Card | `gap-[40px]` | 3 | FAQ + Support | Full 3-para |
| Account Closed v1 | `gap-[40px]` | 3 | Support only | Full 3-para |
| Account Closed v2 | `gap-[40px]` | 4 (+ bank-managed note) | Support only | Full 3-para |
| Shell Account Deletion | `gap-[40px]` | 3 | None | Full 3-para |
| IOC Migrated | `gap-[40px]` | 2 + bullet list + closing | None | Full 3-para |
| First-Time Use | `gap-[40px]` | 3 (inline links) | FAQ only | Full 3-para |
| Terms Update | `gap-[40px]` | intro + h2 + 5-bullet + 3 closing | None | Full 3-para |
| Remember Me Enabled | `gap-[40px]` | 2 | Divider + plain inline link (no chevron) | Full 3-para |
| Remember Me Disabled | `gap-[24px]` | 1 | Inline link block + Support+chevron | Full 3-para |
| Enrollment Attempt | `gap-[24px]` | 4 blocks (24px spacing) | FAQ only | Full 3-para |
| Removed Card | `gap-[40px]` | 2 | FAQ + Support | Full 3-para |

### 5.2 Action Block Variants

**FAQ block:**
```
"Need help or have a question? Visit our FAQs for more information."
View FAQ ›
```

**Support block:**
```
"If you didn't make this request or don't recognize this activity,
please contact customer support."
Contact Visa Customer Support ›
```

**Plain inline-link block (no chevron):**
```
"You can update this feature anytime by visiting the Manage Account section
in the [Visa Click to Pay consumer portal]."
```
(Used in Remember Me Enabled and as first block in Remember Me Disabled)

---

## SECTION 6 — SPECIAL TYPOGRAPHY

### 6.1 Non-Breaking Hyphen

Use `&#8209;` for compound words that must not break across lines:
- `one&#8209;time` → "one‑time"
- `self&#8209;managed` → "self‑managed"
- `Bank&#8209;managed` → "Bank‑managed"

### 6.2 Typographic Apostrophes and Quotes

| Character | Entity | Use |
|---|---|---|
| Right single quote / apostrophe | `&#8217;` | We'll, You're, didn't |
| Left double quote | `&#8220;` | Opening "Terms" |
| Right double quote | `&#8221;` | Closing "Terms" |
| Em dash | `&#8212;` | — in body copy |
| Non-breaking space | `&nbsp;` | Force space before chevron |
| Non-breaking hyphen | `&#8209;` | one‑time, self‑managed |

### 6.3 OTP Code

OTP number uses `.tw` class — renders `#1434CB` in light, `#FFFFFF` in dark:

```html
<p style="margin:0 0 10px 0;padding:0;
          font-family:Verdana,Arial,Helvetica,sans-serif;
          font-size:36px;font-weight:400;color:#1434CB;line-height:47px;
          letter-spacing:0.5px;mso-line-height-rule:exactly;" class="tw">
  {{otp_code}}
</p>
<p style="margin:0 0 32px 0;
          font-family:Verdana,Arial,Helvetica,sans-serif;
          font-size:18px;font-weight:400;color:#4A4A4A;line-height:23px;
          mso-line-height-rule:exactly;" class="ts">
  is your one-time code
</p>
```

---

## SECTION 7 — PRODUCTION CHECKLIST

Before delivering any Visa CTP email:

### Structure
- [ ] Preheader uses `<table>/<td>` pattern (not `<div>`)
- [ ] Container `width="768"` + `max-width:768px`
- [ ] All `<td>` have both `bgcolor=""` attr AND inline `background-color:`
- [ ] All `<a>` have `target="_blank" rel="noopener noreferrer"`
- [ ] All text colours inline (not class-only)
- [ ] Tracking pixel outside last `</table>`

### Outlook Windows Dark Mode
- [ ] Hero outer `<td>`: `font-size:0;line-height:0;mso-line-height-rule:exactly`
- [ ] Logo header `<td>`: `mso-padding-alt` + `font-size:0;line-height:0`
- [ ] All dividers: `height="1"` attr + `max-height:1px` + `overflow:hidden` + empty cell
- [ ] VML: `style="width:768px;"` matches container width exactly
- [ ] No `<!--[if mso]>&nbsp;<![endif]-->` inside divider cells

### Dark Mode
- [ ] All 16 CSS classes present: `.bg-pg` `.bg-hd` `.bg-bd` `.bg-ft` `.bg-cp` `.tx` `.tw` `.th` `.ts` `.tm` `.tl` `.tn` `.bd` `.li` `.di` `.li-i` `.di-i`
- [ ] Every `@media` rule duplicated under `[data-ogsc]`
- [ ] `.bd` class present if template has bullet lists
- [ ] Dark image variants exist and `.di` / `.di-i` images have inline `style="display:none"`

### Typography
- [ ] Hero H1: Tahoma 36px white, `letter-spacing:0.5px`
- [ ] Body heading: Verdana 36px `.tw`, `margin:0 0 40px 0`
- [ ] Body copy: Verdana 16px `.tx`, `line-height:24px`
- [ ] `mso-line-height-rule:exactly` on ALL text elements
- [ ] Typographic apostrophes (`&#8217;`) not straight quotes

### Content
- [ ] `<<placeholders>>` encoded as `&lt;&lt;Name&gt;&gt;`
- [ ] Footer has all 3 paragraphs (service / no-reply / CTP icon trademark)
- [ ] Copyright year current


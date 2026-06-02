# Email Dark Mode & Forwarding — Style Fix

## The Problem

When an email is **forwarded**, email clients (Gmail, Outlook, etc.) strip the `<style>` block from `<head>`. This removes all class-based dark mode overrides — `@media (prefers-color-scheme: dark)` and `[data-ogsc]` selectors — causing styles to break inconsistently.

| Scenario | `<style>` block | Result |
|---|---|---|
| Email received directly | ✅ Present | Intentional dark mode works |
| Email forwarded | ❌ Stripped | Auto-dark applies randomly |

---

## How It Broke (Before Fix)

### Directly Received Email
The `<style>` block is intact, so `@media (prefers-color-scheme: dark)` fires and applies intentional overrides:

```
User OS = Dark Mode
  → @media (prefers-color-scheme: dark) fires
  → Class overrides apply:
      .email-hero  → background: #BDEAFF  (lavender)
      .email-body  → background: #2E2E2E  (dark gray)
      h1, p        → color: #e8e8e8       (light text)
  → ✅ Intentional dark mode design renders correctly
```

### Forwarded Email (Before Fix)
The `<style>` block is stripped. The email client's auto-dark engine kicks in and randomly inverts the inline light-mode colors:

```
User OS = Dark Mode
  → <style> is gone — no @media query fires
  → Only inline styles remain:
      hero td  → background: #1434CB  (Visa blue)
      body td  → background: #ffffff  (white)
      p        → color: #333333       (dark text)
  → Gmail auto-dark transforms:
      #1434CB  → desaturated dark slate
      #ffffff  → dark gray
      #333333  → near-white (inverted)
  → ❌ Inconsistent, broken color scheme
```

---

## The Fix

Added `color-scheme: light` as an **inline style** to three key elements:

```html
<!-- body -->
<body style="... color-scheme: light;">

<!-- Outer wrapper table -->
<table style="... color-scheme: light;">
  <tr>
    <td style="... color-scheme: light;">

      <!-- Email container -->
      <table style="... color-scheme: light;">
```

### Forwarded Email (After Fix)

```
User OS = Dark Mode
  → <style> is gone — no @media query fires
  → Only inline styles remain (same as before)
  → Gmail auto-dark sees color-scheme: light
  → Auto-dark skips transformation:
      "this element is declared as light-mode"
  → Inline styles render as-is:
      hero td  → background: #1434CB  (Visa blue)
      body td  → background: #ffffff  (white)
  → ✅ Consistent light mode — no random inversion
```

---

## Why This Doesn't Break Intentional Dark Mode

`color-scheme: light` and `@media (prefers-color-scheme: dark)` are two **independent** systems:

| | `@media` query | `color-scheme` |
|---|---|---|
| What it is | CSS styling rule | A hint to the rendering engine |
| Strips on forward | Yes — removed with `<style>` | No — survives as inline style |
| Controls | Which CSS rules apply | Whether auto-dark inverts colors |
| Priority | Overrides inline styles when present | Only acts when no CSS context exists |

When the `<style>` block is present (received email), `@media (prefers-color-scheme: dark)` **always wins** — the `color-scheme: light` hint is effectively ignored. When the `<style>` block is stripped (forwarded email), `color-scheme: light` acts as the fallback guard preventing auto-dark inversion.

---

## Files Changed

| File | Change |
|---|---|
| `template/welcome-mail.html` | Added `color-scheme: light` to `<body>`, outer wrapper `<table>`, `<td>`, and email container `<table>` |

---

## Known Limitation

This fix significantly improves forwarded email rendering but is not a 100% guarantee across all email clients. Auto-dark mode implementation varies by client and version. The forwarded email will consistently show the **light mode design** using the inline styles already present in the template.

---
name: ajo-email
description: >
  Expert guidance for Adobe Journey Optimizer (AJO) email campaigns — covering newsletters,
  promotional emails, transactional messages, and lifecycle journeys. Use this skill whenever
  the user mentions AJO, Adobe Journey Optimizer, email campaigns, email templates, promotional
  emails, newsletter setup, email deliverability, Outlook rendering, Litmus testing, campaign
  publishing, journey email actions, or any Adobe Experience Cloud email workflow. Also trigger
  for questions about AJO campaign types (Action, API-triggered, Orchestrated), email channel
  configuration, audience segmentation, frequency capping, personalization, approval workflows,
  or campaign reporting in AJO. If the user asks about building, testing, validating, publishing,
  or troubleshooting emails in any Adobe marketing platform, use this skill.
---

# AJO Email Best Practices Skill

Comprehensive guidance for designing, building, testing, and publishing email campaigns in
Adobe Journey Optimizer. Follow the sections below based on the user's task.

---

## 1. Campaign Type Selection

| Goal | Campaign Type |
|---|---|
| Newsletter / promotional batch send | **Action campaign (Scheduled — Marketing)** |
| Real-time event-driven message | **API-triggered campaign** |
| Multi-step complex workflow | **Orchestrated campaign** |
| Abandoned cart / welcome journey | **Journey with email action** |

> Action campaigns require recipients to be **opted in**. Transactional campaigns do not.

---

## 2. Prerequisites Checklist

Before creating any email campaign, confirm:
- [ ] Audience segment exists in **Adobe Experience Platform**
- [ ] **Email channel configuration** (preset) is set up under Administration → Channels
- [ ] User has **Campaign Manager** or **Campaign Admin** role
- [ ] **IP warming** completed if sending from a new IP or domain
- [ ] **DMARC record** configured for all delegated subdomains
- [ ] **Frequency / rule sets** defined for the campaign category (e.g. "Promotional — max 3/week")

---

## 3. Publishing a Promotional Email Campaign (Step-by-Step)

### Step 1 — Create Campaign
- Journey Management → Campaigns → **Create campaign**
- Type: **Action campaign** → Category: **Marketing**

### Step 2 — Configure Properties
- Set campaign **name**, **description**, **tags**, **access labels**
- Assign **priority score** if conflict management is enabled

### Step 3 — Add Email Action
- Actions tab → **Add action** → Select **Email**
- Choose **email channel configuration** (preset)
- Optionally: assign **frequency cap rule set**, enable **Send Time Optimization**

### Step 4 — Design Email Content
- Click **Edit content** → enter **Subject line** (mandatory) + From name/address
- Click **Edit email body** → launch **Email Designer**
- Build with drag-and-drop or import HTML / select template
- Add **personalization** (profile attributes, dynamic content, decisioning offers)
- Insert mandatory **unsubscribe / opt-out link**
- **Lock** compliance content (unsubscribe links, legal disclaimers) in templates
- Click **Save**

### Step 5 — Select Audience
- Audience tab → **Select audience** (AEP segment)
- Set **Identity type** (Email, CRM ID, etc.)
- Check estimated **profile count** for pre-send visibility
- Add **exclusion rules** if needed

### Step 6 — Schedule
- Schedule tab → **Send once** or **Recurring**
- Set start date/time or choose **As soon as activated**
- Optionally enable **Wave sending** for batched delivery
- Note: max execution window is **12 hours**

### Step 7 — Preview & Test
- Click **Simulate content** → preview with test profiles
- Send **proof emails** to seed list / internal Outlook inbox
- Use **Litmus integration** (Render email button) to check cross-client rendering
- Run **spam analysis check**
- Verify personalization tokens and unsubscribe flow

### Step 8 — Approval (if required)
- Click **Request approval** → select approver(s)
- Approver approves/rejects from their interface
- Status moves to **Ready to activate** after approval

### Step 9 — Review & Activate
- Click **Review to activate** — AJO runs pre-flight validation
- Resolve all **red errors** (block activation)
- Address **yellow warnings** (recommended)
- Verify: Properties, Audience, Actions, Schedule summary
- Click **Activate** → status becomes **Scheduled** or **Live**

> ⚠️ Active campaigns cannot be edited. Duplicate to make changes.

### Step 10 — Monitor
- Open **Campaign report** for real-time metrics
- Track: delivered, opened, clicked, bounced, unsubscribed
- Monitor **excluded profiles** (delivery rate / suppression limits)

---

## 4. Newsletter Best Practices

- Use **Action campaigns** (not Journeys) for scheduled newsletter sends
- Apply **channel rule sets** to separate newsletter vs. promotional frequency caps
- Set clear **entry/exit criteria**: enter on subscribe, exit after welcome series or inactivity
- Keep messages **relevant, timely, and contextual** — irrelevant messages drive unsubscribes
- Use **subscription lists** for granular newsletter opt-in consent
- Honor opt-out requests **immediately** — rapid unsubscribe processing protects sender reputation
- Maintain **suppression lists** — excluded addresses protect sending reputation

---

## 5. Deliverability Best Practices

- Complete **IP warming** before large-volume sends from a new IP/domain
- Configure **SPF, DKIM, and DMARC** for all sending subdomains
- Use **one-click unsubscribe** with a single validation button only — no friction
- Keep addresses on **suppression list** excluded from all future sends
- Monitor **complaint rates** — high complaints signal relevance or consent issues
- Use **wave sending** for large batches to avoid overwhelming landing pages / call centers
- Check **spam score** before activating using AJO's built-in spam analysis tool

---

## 6. Outlook Email Validation

### Method 1 — Litmus Integration (Native in AJO)
1. Email Designer → **Simulate content** → **Render email**
2. **Connect your Litmus account** (top right)
3. Click **Run test** → review rendering across all Outlook versions

### Method 2 — Proof to Outlook Inbox
1. Simulate content → **Send proof**
2. Add internal Outlook email address or seed list
3. Open received email directly in Outlook desktop / Outlook Web App

### Method 3 — External Tools
- **Email on Acid** — cross-version Outlook rendering
- **Mailtrap** — sandbox testing
- **PutsMail** — HTML email to any inbox

### Common Outlook Rendering Issues & Fixes

| Issue | Root Cause | Fix |
|---|---|---|
| Layout breaks / columns collapse | Outlook ignores `<div>` styles | Use `<table>` for all layout |
| Images wrong size | CSS resize not respected | Set `width` attribute in HTML (not just CSS) |
| Background images missing | CSS background not supported | Use **VML** with conditional comments |
| Custom fonts not rendering | Web fonts unsupported in desktop Outlook | Use web-safe fallback fonts (Arial, Georgia, Verdana) |
| Dark mode color inversion | Outlook auto-inverts some colors | Use transparent images + explicit color properties |
| Text line-height inconsistent | Outlook ignores `line-height` | Add `mso-line-height-rule:exactly` |
| Hero banner extra padding | VML `<v:textbox>` bleeds into surrounding rows | Add `font-size:0;line-height:0;mso-line-height-rule:exactly` on outer hero `<td>` |
| Divider renders tall | `&nbsp;` in 1px divider cell forces line-height | Use `height="1"` attr + `max-height:1px` + `overflow:hidden` + empty cell |
| Header logo row extra space | `padding` on `<td>` with MSO-only `<img>` adds implicit spacing | Add `mso-padding-alt` + `font-size:0;line-height:0` on logo header `<td>` |

### Outlook Coding Checklist
- [ ] All layout uses `<table>` not `<div>`
- [ ] CSS is **inline** on every element (not in `<style>` block only)
- [ ] Images have both HTML `width` attribute AND inline `style="width:Xpx"`
- [ ] All images have `alt` text
- [ ] All links include `https://` protocol and `target="_blank" rel="noopener noreferrer"`
- [ ] Web-safe fallback fonts defined (Arial, Georgia, Verdana)
- [ ] Background images use VML conditional comments
- [ ] Conditional comments used for Outlook-specific overrides (`<!--[if mso]>`)
- [ ] Hero banner outer `<td>` has `font-size:0;line-height:0;mso-line-height-rule:exactly`
- [ ] Divider `<td>` has `height="1"` attr + `max-height:1px` + `overflow:hidden` (no `&nbsp;`)
- [ ] Logo header `<td>` has `mso-padding-alt` + `font-size:0;line-height:0`
- [ ] Tested in Outlook desktop, Outlook Web App, and Outlook mobile
- [ ] Tested in Outlook Windows **dark mode** specifically

---

## 7. Reply & Forward Safety

When a recipient forwards or replies to an email, the `<style>` block is stripped by
Gmail, Outlook, Yahoo, and most clients. To survive forward/reply:

**Already safe (inline — survives stripping):**
- Text colours on `<p>` and `<a>` — must be inline, not class-only
- Background colours — must have both `bgcolor=""` attr AND `background-color:` inline on `<td>`
- Font stack — must be inline on every text element
- `display:none` on dark-mode images — inline style survives; class does not

**What is always lost on forward (expected, not fixable):**
- `@media (prefers-color-scheme:dark)` — dark mode is lost on forward in all clients
- `[data-ogsc]` Outlook App dark mode — also lost

**Required on all `<a>` tags:**
```html
<a href="https://..." target="_blank" rel="noopener noreferrer" style="color:#1434CB;">Link</a>
```
- `target="_blank"` — opens link in browser, not client's embedded frame
- `rel="noopener noreferrer"` — prevents tab-hijacking + suppresses referrer on forward

**Preheader — use table pattern, not div:**
```html
<!-- PREHEADER -->
<table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0">
    <tr>
        <td style="display:none; max-height:0; overflow:hidden; mso-hide:all;
                   font-size:1px; line-height:1px; color:#f4f4f4;">
            Preheader text here &#32;&#8199;&#65279;&#847;&zwnj;&nbsp;
        </td>
    </tr>
</table>
```
The `<div>` preheader pattern can render visibly in some forward chains.
The table `<td>` pattern with `display:none` inline is more reliably hidden.

---

## 8. Personalization

- Use **profile attributes** for first-name, loyalty tier, language
- Use **decisioning / offers** for dynamic content blocks
- Use **multilingual support** — AJO auto-updates copy to match subscriber's preferred language
- **Lock template elements** so governance content cannot be removed
- Test personalization with **multiple test profiles** covering different attribute values
- Use **CSV/JSON sample data** to test up to 30 scenarios without creating test profiles

---

## 9. Reporting & Monitoring Key Metrics

| Metric | What to watch |
|---|---|
| Delivered rate | Should be >95%; low rate signals deliverability issue |
| Open rate | Benchmark varies by industry; track trends over time |
| Click-through rate | Indicates content relevance |
| Bounce rate | Hard bounces → suppress immediately |
| Unsubscribe rate | >0.5% signals relevance or frequency issue |
| Spam complaints | Should be <0.1% |
| Excluded profiles | Profiles skipped due to caps or suppression |

---

## Reference Files

- `references/deliverability.md` — Full IP warming, DMARC, authentication setup guide
- `references/outlook-coding.md` — Outlook-specific HTML/CSS code patterns and VML examples

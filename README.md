# Treasure Island Dubai — Website

Static website for Treasure Island, the kids' indoor playground at Le Gourmet,
2nd Level, Galeries Lafayette, The Dubai Mall.

No build step, no framework, no external JavaScript libraries. Upload the folder
and it runs.

---

## Run it locally

From inside this `website/` folder:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000

(Opening `index.html` directly with `file://` mostly works, but a local server is
closer to how it will actually behave.)

---

## Files

```
website/
├── index.html        Home
├── play.html         The nine play zones
├── birthdays.html    Party packages + enquiry form
├── programs.html     Playgroup, workshops, holiday camps
├── events.html       Events + private hire + enquiry form
├── gallery.html      Filterable photo gallery with lightbox
├── about.html        Story, values, safety
├── contact.html      Contact, map, FAQ
├── robots.txt
├── sitemap.xml
└── assets/
    ├── css/style.css   All styling (design tokens at the top)
    ├── js/app.js       All interaction
    ├── img/            32 photos × 3 sizes, WebP
    └── logo*.webp/png  Logo + favicons
```

---

## Before it goes live — things to change

These are the only places with placeholder or assumed information.

| What | Where | Current value |
|---|---|---|
| **Domain** | `<link rel="canonical">` in every page, plus `robots.txt` and `sitemap.xml` | `https://www.treasureislanddubai.ae/` — a placeholder. Find and replace it with the real domain. |
| **Party prices** | `birthdays.html`, the three `.pack-price` blocks | Currently "On request". Replace with real prices when confirmed, or leave as-is to drive WhatsApp enquiries. |
| **Entry price** | `contact.html`, the "How much does entry cost?" FAQ | Says to message for current rates. |
| **Phone numbers** | Every page | `+971 50 473 8452` (booking/WhatsApp) and `+971 4 382 7333` (landline). Both sourced from public listings — confirm they are the right ones to publish. |
| **Email** | Footer, "Email" social icon | `hello@treasureislanddubai.ae` — does not exist yet. |
| **Programme days** | `events.html`, the "What's on" cards | Shows a standing weekly pattern (Mon / Weekends / Seasonal / School breaks). Swap in real dates as they are set. |
| **Testimonials** | `index.html`, the `.quote` blocks | Written as realistic examples. Replace with real reviews before launch. |
| **Social links** | Footer + contact page | Only Instagram is linked. Add Facebook / TikTok if they exist. |

### Also worth doing after launch
- Set `og:image` to a **full absolute URL** (`https://yourdomain.com/assets/img/entrance-hero.webp`) — some social platforms will not resolve the relative path.
- Add Google Analytics / Tag Manager before `</head>`.
- Claim and complete the Google Business Profile — it matters more than anything on this site for "playground in Dubai" style searches.

---

## How the forms work

There is no backend. Each form (`birthdays.html`, `events.html`, `contact.html`)
collects the answers, formats them into a readable WhatsApp message and opens
`wa.me` with it pre-filled. Nothing is stored or emailed by the website itself.

To switch to a real form endpoint later (Formspree, a CRM, a mail script),
replace the `data-wa="..."` attribute handling in `assets/js/app.js` →
`forms()`.

The WhatsApp number used by the forms is set per-form:

```html
<form class="form" data-wa="971504738452" data-wa-title="*Birthday party enquiry*">
```

---

## Editing content

**Colours, fonts, spacing, shadows** — all defined as CSS custom properties at
the very top of `assets/css/style.css`, under `:root`. Change them there and the
whole site follows.

```css
--navy:#004368;   /* brand blue, sampled from the logo */
--gold:#FFBA1A;   /* brand gold, sampled from the logo */
```

**Adding a gallery photo** — drop three sizes into `assets/img/`
(`name.webp` ~1500px, `name@900.webp`, `name@500.webp`) and copy an existing
`<figure>` block in `gallery.html`. The `data-cat` attribute controls which
filter button shows it.

**Animations** — every effect is driven by a `data-` attribute:

| Attribute | Effect |
|---|---|
| `data-reveal` | Fades/slides in on scroll. Values: `left`, `right`, `scale`, `mask`, `blur` |
| `data-stagger="90"` | On a parent — delays each child reveal by 90ms |
| `data-px="0.12"` | Parallax; negative values move the other way |
| `data-tilt="6"` | 3D tilt toward the cursor |
| `data-magnet="0.3"` | Button pulls toward the cursor |
| `data-count="9"` | Counts up when scrolled into view |
| `data-zoom="0.1"` | Slow scroll-linked zoom on an image |
| `class="split"` | Heading animates in word by word |

Everything is disabled automatically for visitors who have
"Reduce Motion" turned on in their OS settings.

---

## Browser support

Chrome, Safari, Firefox and Edge — current versions, desktop and mobile.
Images are WebP only (supported everywhere since 2020).

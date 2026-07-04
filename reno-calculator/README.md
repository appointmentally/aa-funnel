# Reno Calculator Funnel — Monarch clone, improved

> **2026-07-04 Suby/Hormozi overhaul (10-agent audit → 28 changes, all applied + deployed).**
>
> **What each brand is now:** a named HVCO funnel — Trio **"Reno Blueprint"**, Modern Living
> **"Real-Cost Report"** (villain H1: *"Been quoted S$50K+…"*), June **"Honest Reno Report"**.
> Post-submit = **calendar booking page first** (GHL widget embed, prefilled), report below,
> sticky book-bar follows the scroll, Hormozi offer card ("estimate is a range → quote is exact",
> "yours to keep even if we never speak"). No-URL fallback: tracked BookIntent button →
> truthful "we'll WhatsApp you at <phone>" primer.
>
> **Ad↔LP congruence engine (Suby hack #6):** append `?h=cost|see|quote|keys|resale|budget`
> to any ad URL → the LP H1/subhead/gate/close mirror that angle; `?type=hdb|condo|landed`
> + `?u=bto|resale` skips the landing and pre-selects identity (`&start=land` = escape hatch
> to A/B the landing). `headline_variant` + `ad_identity` + `flat_type` ride the webhook
> payload → rank angles by **cost-per-BOOKED** in GHL. Full 27-headline test bank (7 angles,
> exact LP mirrors): see `HEADLINE-BANK.md`.
>
> **Also shipped:** HDB flat-type chips (2-room Flexi→Executive; sets beds/baths/size),
> realistic resale quick-fill (4-room full scope ≈ S$34k–63k), zero-scope auto-rescue +
> "adjust my scope" re-entry, gate rebuilt (locked-goods bullets, timeline-first, key-collection
> chips, 3-up blurred-render teaser, "20 seconds" line), scroll-reset + iOS-zoom mobile fixes,
> sessionStorage resume (per-brand key — same GH Pages origin!), PDPA consent naming brand +
> channels with `consent_text` evidence in payload, "about 2 minutes" everywhere, image-first
> style cards, async fonts + LCP preload, ui-ux-pro-max compliance (SVG icons, 44px targets,
> focus rings, reduced motion).
>
> **OWNER CHECKLIST (launch blockers first):**
> 1. ⛔ `CONFIG.calendarUrl` per brand — GHL **widget embed** URL
>    (`https://api.leadconnectorhq.com/widget/booking/<calendarId>` — NOT the normal calendar
>    page link, which refuses iframes). Until set: fallback BookIntent flow runs.
> 2. ⛔ `CONFIG.webhookUrl` per brand — GHL inbound-webhook; otherwise leads never reach GHL.
> 3. GHL iframe snippet must forward query params: `iframe.src = BASE + location.search`
>    — or the ?h/?type congruence params never reach the page.
> 4. Real testimonials (verbatim + permission) → `CONFIG.testimonial`; HDB licence no.;
>    Google rating (ship only if ≥4.5); Trio CaseTrust/factory-specifics confirmation.
> 5. Ad ops: every ad URL carries `?h=<angle>&type=<identity>[&u=bto|resale]`; name ads
>    `<angle>-<identity>-<n>`; weekly, rank angles by cost-per-booked via `headline_variant`;
>    paste winning ad headlines verbatim into the `HEADLINES` map.

> **Now a 3-brand family.** Each brand has its own folder + aesthetic so the funnels don't
> look like copies of Monarch or of each other. All share the same engine, pricing table
> and render gallery (`reno-calculator/renders/`, referenced via `../reno-calculator/renders/`
> from the variants — don't delete this folder).
>
> | Brand | Live URL | Aesthetic | WhatsApp prefilled |
> |---|---|---|---|
> | The Trio Design Studio | /aa-funnel/reno-calculator/ | Warm editorial: paper/terracotta/gold, Poppins, sidebar layout | — (fill in) |
> | Modern Living Interior Design | /aa-funnel/reno-modern-living/ | Architectural dark: graphite/teal, Space Grotesk, centered column | 6594888181 |
> | June Interior Design | /aa-funnel/reno-june/ | Boutique minimalist: cream/sage, Fraunces serif, arches + pills | 6591556958 |
> | Haus Werkz Interior | /aa-funnel/reno-haus-werkz/ | Werkstatt/Bauhaus: concrete white, blueprint cobalt + mustard, Archivo, 2px technical borders — HVCO "Build Plan"; ships REAL proof (since 2016, HDB Licence HB-07-5638B, CaseTrust, 2-yr warranty risk-reversal). Their validated ad angle = $28,888 package anchor (Framework D) — confirm price is current before anchoring ads on it | 6598303830 |
>
> **CRO pack (all three):** tap-to-start landing (no separate Start click), live estimate
> ticker, one-tap typical-scope quick-fill, gated render reveal, email optional
> (`CONFIG.emailRequired`), consent-by-submit (no checkbox), SG phone validation,
> autofocus + Enter-to-submit, WhatsApp-first CTAs on the report, testimonial slot
> (`CONFIG.testimonial` — fill with a REAL client quote only), lazy-loaded images,
> Meta pixel events. Still to wire per brand: `webhookUrl`, `pixelId`, `bookingUrl`
> (+ `whatsappNumber` for Trio).

Funnel-hacked from `monarch-reno-calculator.com` (Base44 React app). Rebuilt as a single
self-contained `index.html` + pre-rendered A.I style gallery. Trio-branded by default,
rebrandable per client via the `CONFIG` block at the top of the `<script>`.

**Live URL:** https://appointmentally.github.io/aa-funnel/reno-calculator/
(deployed as a folder in the `appointmentally/aa-funnel` GitHub Pages repo — same pipeline
as the yourapptally funnel bundles)

## What was cloned (Monarch teardown)

- **Flow:** landing (property type) → home details (new/resale, beds/baths, size, areas) →
  one step per selected room (Living, Kitchen, Bedrooms, Bathrooms) → Other Works →
  A.I Design Studio (style pick + renders) → lead gate → budget report.
- **Pricing model (copied exactly):** each work category has Light/Moderate/Extensive tiers
  with a lo–hi S$ range (`TIERS` in index.html = Monarch's exact table). Displayed high =
  `costHigh × 0.8`. Bedrooms/Bathrooms line items multiply by the bed/bath count.
  Property type, size and new/resale do NOT affect the number (same as Monarch — size is
  captured for the lead record only).
- **Styles:** Monarch's style writeups, trimmed to the 6 most in-demand SG styles
  (Modern Minimalist, Scandinavian, Japandi, Muji/Wabi-sabi, Industrial, Smart Luxury).
- **Render prompts:** Monarch's exact SG-specific prompts (HDB: split aircon, 2.6 m ceiling,
  cove lighting, vinyl floors, "as seen on Qanvast") used to pre-generate the gallery.

## What was improved

1. **Instant renders, zero marginal cost.** Monarch generates renders live per user
   (~30–60 s wait, can fail, costs API credits per lead). We pre-generated the full
   HDB gallery (6 styles × living/bedroom/kitchen, Higgsfield nano-banana-pro, 2 credits/img,
   36 credits total) and serve static JPGs — renders appear the instant a style is tapped.
2. **Tighter lead gate.** Monarch shows all 3 renders BEFORE asking for details (only the
   PDF is gated). Here the living-room render shows sharp as proof, bedroom + kitchen are
   blurred with a 🔒 "Unlocked in your free report" overlay → the form actually gates value.
3. **Live estimate ticker.** Running S$ lo–hi total animates in the sidebar (desktop) and
   sticky bottom bar (mobile) as works are selected. Monarch shows no total until the end.
4. **Mobile-first.** Sticky bottom nav (Back / est. chip / Next), thumb-sized tier buttons,
   safe-area padding. Ad traffic is ~95% mobile.
5. **One-tap "typical scope" quick-fill** per room (Moderate on common works, + Light hacking
   auto-added for resale units) — cuts completion time and anchors a realistic estimate.
6. **SG lead qualification.** Timeline chips ("ASAP — keys collected", "1–3 months",
   "3–6 months", "6+ months / exploring"), SG mobile validation (8/9/3/6 + 8 digits),
   PDPA consent checkbox.
7. **CRM + setter wiring.** On submit, POSTs the full JSON payload (contact, property,
   per-room works + tiers, estimate range, style, timeline, floor-plan base64 if <1.5 MB,
   URL query incl. fbclid/UTM, referrer) to `CONFIG.webhookUrl`. Report page CTAs:
   book-consult button (`CONFIG.bookingUrl`) + WhatsApp deep link with a pre-filled message
   containing their estimate + style (drops straight into the WhatsApp setter).
8. **Meta pixel events** when `CONFIG.pixelId` is set: PageView, StartCalculator,
   StepCompleted, StyleSelected, QuickFill, FloorPlanUploaded, Lead (with value).
   Also postMessages every event to the parent page for GHL-side pixels.
9. **On-page report + print-to-PDF** (no jsPDF dependency): itemised per-room breakdown
   with subtotals, renders, style writeup, disclaimer; "Save / print my report" uses print CSS.
10. **Form resilience:** typed name/phone/email survive re-renders (Monarch-style step
    re-render wipes were fixed by persisting to state on input).

## Wiring checklist (per client)

Edit `CONFIG` at the top of the `<script>` in `index.html`:

| Key | What to put |
|---|---|
| `brandName/brandShort/brandSub` | Client brand (default: The Trio Design Studio) |
| `webhookUrl` | GHL workflow **Inbound Webhook** trigger URL (or n8n webhook). Payload keys: `name, phone, email, timeline, property_type, unit_status, bedrooms, bathrooms, size, size_unit, areas[], design_style, estimate_low, estimate_high, works[], floor_plan_provided, floor_plan_base64, query, referrer` |
| `whatsappNumber` | e.g. `6591234567` — enables the WhatsApp CTA on the report |
| `bookingUrl` | GHL calendar link — enables the book-consult CTA |
| `pixelId` | Meta pixel ID for in-iframe event firing |

GHL embed (Custom JS/HTML element, full-width section, padding 0):

```html
<iframe id="renoFrame" src="https://appointmentally.github.io/aa-funnel/reno-calculator/"
        style="width:100%;border:0;display:block;min-height:100vh" scrolling="no"
        title="Reno Cost Calculator"></iframe>
<script>
  window.addEventListener("message", function (e) {
    if (e.data && e.data.renoCalcHeight)
      document.getElementById("renoFrame").style.height = e.data.renoCalcHeight + "px";
  });
</script>
```

Or point a subdomain / GHL custom domain at it, or run it as the ad's landing page directly.

## Files

- `index.html` — the whole app (vanilla JS, no build step)
- `renders/{style}-{living|bedroom|kitchen}.jpg` — pre-generated HDB gallery (18 imgs, ~3.9 MB)
- `render-jobs.json` — Higgsfield job IDs used to generate the gallery
- `recon/` — Monarch teardown screenshots + verification screenshots

## Extending

- **Condo/landed galleries:** re-run the prompts in `render-jobs.json`'s pattern with the
  condo/landed variants (in Monarch's bundle; saved in the teardown) and key the manifest by
  property type.
- **More styles:** add to `STYLES` + drop 3 images in `renders/`.
- **Adjust pricing:** edit `TIERS` (numbers are Monarch's market table — Trio may want its
  factory-direct rates here).

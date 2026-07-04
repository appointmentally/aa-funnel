# Headline Test Bank — ad ↔ LP congruence (Suby hack #6)

Format: `ANGLE | ad headline -> LP H1 mirror`. The LP mirror fires automatically via `?h=<angle>`.
`{home}` is replaced by the `?type`/`?u` identity (HDB / condo / landed home / BTO / resale flat).
`[hl]…[/hl]` renders as the brand accent (gold on Trio, teal on ML, italic serif on June).

Run 20–40 ad headlines per Suby; when one wins inside its angle, paste its exact wording into
that angle's `HEADLINES.h1` in CONFIG; after 2+ winning batches promote it to the default H1.

## COST
- COST | What will your {home} reno actually cost? Find out in 2 minutes — free. -> See what your {home} reno will [hl]actually cost[/hl] — line by line, in 2 minutes.
- COST | Stop guessing your reno budget. Get it itemised, line by line, in 2 minutes. -> See what your {home} reno will [hl]actually cost[/hl] — line by line, in 2 minutes.
- COST | Renovating your {home}? Know your real number before you talk to any ID. -> See what your {home} reno will [hl]actually cost[/hl] — line by line, in 2 minutes.
- COST | The reno cost nobody will tell you upfront — itemised, free, in 2 minutes. -> See what your {home} reno will [hl]actually cost[/hl] — line by line, in 2 minutes.

## SEE
- SEE | See your {home} renovated in 6 designer styles — before you spend a cent. -> See your new {home} [hl]before[/hl] you renovate — in the style you'd actually pick.
- SEE | Don't imagine your new home. See it — free A.I concept renders of a home like yours. -> See your new {home} [hl]before[/hl] you renovate — in the style you'd actually pick.
- SEE | Your new living room, rendered in seconds. Pick a style and see a {home} like yours. -> See your new {home} [hl]before[/hl] you renovate — in the style you'd actually pick.
- SEE | Would you renovate without seeing the end result first? Now you don't have to. -> See your new {home} [hl]before[/hl] you renovate — in the style you'd actually pick.

## QUOTE
- QUOTE | Collecting ID quotes? Check every line against market rates first — free. -> Check any ID quote against [hl]typical market rates[/hl] — line by line, free.
- QUOTE | Same flat, very different quotes. Know the market rate before you sign anything. -> Check any ID quote against [hl]typical market rates[/hl] — line by line, free.
- QUOTE | Bring a market-rate breakdown to your next ID meeting. Takes 2 minutes to build. -> Check any ID quote against [hl]typical market rates[/hl] — line by line, free.
- QUOTE | Before you sign that reno quote — run it against this free line-by-line estimate. -> Check any ID quote against [hl]typical market rates[/hl] — line by line, free.

## KEYS
- KEYS | Collecting your BTO keys soon? See it designed & priced before day one. -> Collecting keys soon? See your {home} [hl]designed & priced[/hl] before day one.
- KEYS | Key collection coming up? 2 minutes now saves weeks of showroom rounds. -> Collecting keys soon? See your {home} [hl]designed & priced[/hl] before day one.
- KEYS | From bare BTO to done-up home — see the design and the number first. -> Collecting keys soon? See your {home} [hl]designed & priced[/hl] before day one.
- KEYS | Your BTO date is fixed. Your reno budget shouldn't be a mystery. -> Collecting keys soon? See your {home} [hl]designed & priced[/hl] before day one.

## RESALE
- RESALE | Renovating a resale flat? The budget lives in what's behind the walls. -> Renovating a [hl]resale[/hl] {home}? Price the works most people forget.
- RESALE | Resale renos have line items BTO owners never see. Price yours properly — free. -> Renovating a [hl]resale[/hl] {home}? Price the works most people forget.
- RESALE | Hacking, rewiring, waterproofing — the resale works everyone forgets to budget. -> Renovating a [hl]resale[/hl] {home}? Price the works most people forget.
- RESALE | Buying resale? Estimate the reno before you fall in love with the flat. -> Renovating a [hl]resale[/hl] {home}? Price the works most people forget.

## BUDGET
- BUDGET | Set your reno budget before a showroom sets it for you. -> Set your reno budget [hl]before the showroom[/hl] sets it for you.
- BUDGET | Walk into every ID meeting already knowing your numbers. -> Set your reno budget [hl]before the showroom[/hl] sets it for you.
- BUDGET | The reno budget spreadsheet, done for you in 2 minutes. -> Set your reno budget [hl]before the showroom[/hl] sets it for you.
- BUDGET | Your ID shouldn't be the first person to tell you what your reno costs. -> Set your reno budget [hl]before the showroom[/hl] sets it for you.

## VILLAIN (ML default)
- VILLAIN (ML default) | Why are resale renos quoted S$50-90K? Here's the uncomfortable truth. -> Been quoted S$50K+ for your reno? See what it should [hl]actually cost[/hl] — line by line.
- VILLAIN (ML default) | Been quoted S$50K+ for your reno? Check it line by line — free. -> Been quoted S$50K+ for your reno? See what it should [hl]actually cost[/hl] — line by line.
- VILLAIN (ML default) | Your reno quote has padding in it. See where — line by line, in 2 minutes. -> Been quoted S$50K+ for your reno? See what it should [hl]actually cost[/hl] — line by line.

## Owner inputs (from the 10-agent audit)
1. LAUNCH BLOCKER — GHL calendar permalink per brand (Trio, Modern Living, June) pasted into CONFIG.calendarUrl (plus optional external bookingUrl fallback). Until set, the funnel cannot produce a BOOKED.
2. LAUNCH BLOCKER — GHL inbound-webhook URL per brand into CONFIG.webhookUrl; leads currently never reach GHL, so the AI setter can never chase.
3. GHL Tracking Code: update each brand's full-screen-iframe snippet to forward query params (iframe.src = BASE + location.search) — otherwise ?h/?type/?u never reach the page and the whole congruence engine is dead.
4. 1-2 verbatim client testimonials per brand, ≤160 chars, with first name + flat type + estate (e.g. '— Mdm Tan, 4-room resale, Tampines') and written permission → CONFIG.testimonial. June: source from june.sg / Google reviews with permission. Ship nothing invented.
5. Trio claim verification: CaseTrust accreditation current? '500+ resale homes' count current? Factory-ownership wording (carpentry, sintered-stone, vinyl flooring) accurate? These run in Trio's live ads — confirm before the LP repeats them; unconfirmed claims stay off.
6. HDB Registered Renovation Contractor licence number per brand (publicly checkable in the HDB directory) → appended to #l-trust. Highest-leverage proof asset for a 25-45 HDB/BTO audience.
7. Google Business Profile rating + review count per brand — ship in #l-trust only if ≥4.5 (format: '★ 4.8 on Google (63 reviews)').
8. Confirm the free consult actually includes floor-plan pricing and a layout-tailored concept (it's the setter's standard offer but verify), plus consult duration if we want to state it — the report CTA copy and floor-plan-upload bridge depend on this.
9. 2-3 real completed-project photos per brand to replace the generic AI hero background (optional upgrade after C27).
10. 30-min task per brand: sense-check the 15 tier price ranges against the last 10-20 signed quotations — unlocks upgrading the transparency bullet to 'Ranges sense-checked against {Brand}'s recent signed quotations', a claim no competitor can dispute.
11. Ad ops discipline: every ad destination URL carries ?h=<angle>&type=<identity>[&u=bto|resale] (no naked links), ads named <angle>-<identity>-<n>, weekly angle ranking by cost-per-BOOKED via the headline_variant field; winning ad headlines pasted verbatim into HEADLINES.
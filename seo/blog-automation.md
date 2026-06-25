# Appointment Ally — Auto-Blog Engine (spec + assets)

**Decision:** full auto-publish, **2 posts/week** (e.g. Mon + Thu 9am SGT). Safeguards: varied topic bank (no repeats), post-publish Telegram with live link (yank-a-dud visibility), quality-bar prompt.

**Blocker:** GHL **Private Integration token** (Settings → Private Integrations → View Blogs + Edit Blogs scopes). Needed for the publish step.

---

## n8n workflow design
1. **Schedule Trigger** — Mon & Thu, 09:00 SGT.
2. **Get next topic** — pull the next unused row from the topic bank (n8n Data Table `aa_blog_topics`: `topic`, `used`, `publishedUrl`). Mark used after publish so it never repeats. When the bank runs low, Telegram-ping "topics running low".
3. **AI writer** (Anthropic/Claude node) — uses the prompt below; returns JSON `{title, urlSlug, metaDescription, keywords, bodyHtml, imageAltText}`.
4. **GHL Create Blog Post** — HTTP POST `https://services.leadconnectorhq.com/blogs/posts` with the private-integration token (`Authorization: Bearer <token>`, `Version: 2021-07-28`), body: `locationId=GDcgigHpdSmn1aVMTmsm`, `blogId`, `authorId`, `categories:[categoryId]`, `title`, `content` (bodyHtml), `description` (metaDescription), `urlSlug`, `imageUrl` (default logo), `imageAltText`, `status:"PUBLISHED"`, `publishedAt` (now).
5. **Update topic row** — set `used=true`, store the live URL.
6. **Telegram** — "📝 Published: {title} — {url}".

(IDs for blogId / authorId / categoryId get fetched once via `GET /blogs/` + `/blogs/authors` + `/blogs/categories` after the token exists.)

---

## Topic bank (buyer-query SEO topics — rotate, no repeats)
All naturally position pay-per-show / showroom appointments / Singapore ID firms. No "AI" mentions.

1. Pay-per-show vs buying renovation portal leads: what actually fills a showroom
2. Why your interior design leads don't show up — and how to fix the no-show rate
3. How much should a Singapore interior design firm spend on marketing?
4. Why renovation-portal leads stop working as your firm grows
5. How to get exclusive renovation leads (not shared with five other firms)
6. The 5-minute rule: the fastest way to follow up with renovation enquiries
7. How to keep your interior design showroom busy in a slow renovation season
8. Facebook vs Instagram ads for interior design firms in Singapore
9. How to qualify renovation leads so your designers stop wasting time
10. What a healthy cost-per-appointment looks like for a Singapore ID firm
11. How to scale an interior design firm beyond word-of-mouth referrals
12. Booked vs showed-up: the appointment metric that actually grows your firm
13. How established ID firms keep their showroom busy without discounting
14. Should interior design firms run their own ads or outsource them?
15. The hidden cost of cheap renovation leads (and what to buy instead)

---

## AI writer prompt (used by the n8n AI node each run)
SYSTEM:
> You write SEO articles for Appointment Ally, a Singapore agency that books guaranteed **showed-up showroom appointments** for **interior design firms** on a **pay-per-show** basis (clients only pay when a homeowner shows up). Audience: owners/marketers of established Singapore interior design firms. Rules: NEVER mention AI, chatbots, or "setters". Use only these public stats when relevant: 134 firms, 500,000+ homeowner conversations, 500,000+ leads, 20,000+ showed-up appointments. Be specific and genuinely useful — no generic filler, no invented client names, no fake numbers. Singapore English, confident, plain.

USER:
> Write a 900–1200 word SEO article on: "{{topic}}".
> Structure: a 2–3 sentence intro that names the reader's problem; 4–6 `<h2>` sections; a short "Frequently asked questions" `<h2>` with 4 `<strong>`question + answer pairs; end with a one-line CTA linking to https://yourapptally.com/optin.
> Return ONLY valid JSON:
> `{"title": "...", "urlSlug": "kebab-case-with-primary-keyword", "metaDescription": "<=155 chars", "keywords": "comma,separated,6-8 buyer-intent terms", "imageAltText": "...", "bodyHtml": "<p>…</p><h2>…</h2>… (clean HTML, no <h1>, no markdown)"}`
> The title must contain the primary keyword and read like a benefit. The bodyHtml must NOT repeat the title as an h1 (GHL renders the title separately).

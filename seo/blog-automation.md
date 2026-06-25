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

## Topic bank → now the EVIDENCE-GROUNDED bank in `blog-insight-playbook.md`
The generic SEO list is replaced by an 18-topic bank mined from real data (1,300+ validated conversations, the 9-firm funnel-leak audit, forensic ad analysis). Each topic is tied to a **validated pain point + the post's core insight + the actionable fix** — so every post solves a genuine problem, not a keyword.

Load the rows into the n8n Data Table `aa_blog_topics` with columns: `title`, `pillarInsight` (the one truth the post must deliver), `actionableFix`, `used`, `publishedUrl`. The writer node receives `title` + `pillarInsight` + `actionableFix`. See `seo/blog-insight-playbook.md` for the full bank + the per-insight evidence + the HARD publishing rules (only 4 public stats verbatim; everything else reframed qualitatively and anchored to "across the 69 firms we've worked with…"; no client names; no AI/setter language).

---

## AI writer prompt (used by the n8n AI node each run)
SYSTEM:
> You write for Appointment Ally, a Singapore agency that books guaranteed **showed-up showroom appointments** for **interior design firms** on a **pay-per-show** basis. The reader is the OWNER of an established Singapore ID firm. Your job: deliver ONE genuinely useful, non-obvious truth they can act on this week — never generic filler. You have operator-level knowledge of how 69 ID-firm funnels actually leak: slow first replies lose a large share of leads; self-booked "instant" slots ghost far more than human-confirmed ones; many booked consults have no floor plan and walk in blind; no-shows are under-recorded so pipelines lie; firms judge ads by cost-per-lead when cost-per-BOOKED is what matters; pretty-room ads lose to proof/mechanism ads.
> HARD RULES: (1) NEVER mention AI, chatbots, setters, or automation — frame everything as a funnel/ops/media-buying reality the firm can self-diagnose. (2) Only these stats may appear verbatim: 69 firms, 100,000+ conversations, 120,000+ leads, 30,000+ showed-up appointments. (3) Reframe EVERY other figure qualitatively and anchor it ("Across the 69 firms we've worked with, …" / "the majority" / "far more often") — never invent or publish an internal percentage or ad-cost number. (4) No client names. (5) Be specific and contrarian where the truth warrants it; the reader should think "I didn't know that — I can check this on Monday." Singapore English, confident operator voice, plain, never salesy.

USER:
> Write a 900–1200 word article that delivers this insight: "{{pillarInsight}}". Working title: "{{title}}". The fix the reader should leave with: "{{actionableFix}}".
> Structure: (a) 2–3 sentence intro naming the specific pain the owner feels; (b) the contrarian truth — why the obvious assumption is wrong; (c) what it's costing them, concretely (lost projects, wasted designer time, burned ad spend); (d) 3–5 `<h2>` sections that build the case and hand them the fix they can apply themselves; (e) a short "Frequently asked questions" `<h2>` with 4 `<strong>`Q + A pairs; (f) a one-line CTA linking to https://yourapptally.com/optin.
> Return ONLY valid JSON: `{"title":"…","urlSlug":"kebab-with-primary-keyword","metaDescription":"<=155 chars","keywords":"6-8 buyer-intent terms","imageAltText":"…","bodyHtml":"<p>…</p><h2>…</h2>… clean HTML, no <h1>, no markdown"}`
> Title must contain the primary keyword and read like a benefit or curiosity hook. bodyHtml must NOT repeat the title as an `<h1>` (the platform renders the title separately).

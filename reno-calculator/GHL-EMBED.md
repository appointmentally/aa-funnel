# Hosting the funnels on your own domains via GHL

The github.io URL is only the asset host. Each brand's funnel should be SERVED from the
client's own domain through a GHL page that wraps it in a full-screen iframe — the address
bar shows the client's domain, exactly like the yourapptally.com setup.

## Per-brand snippet (paste into a GHL "Custom JS/HTML" element on a blank funnel page,
## section set to Full Width with 0 padding — one page per brand)

Replace BASE with the brand's URL:
- Trio:          https://appointmentally.github.io/aa-funnel/reno-calculator/
- Modern Living: https://appointmentally.github.io/aa-funnel/reno-modern-living/
- June:          https://appointmentally.github.io/aa-funnel/reno-june/
- Haus Werkz:    https://appointmentally.github.io/aa-funnel/reno-haus-werkz/

```html
<style>html,body{margin:0;padding:0;height:100%;overflow:hidden}</style>
<iframe id="renoFrame" title="Reno Calculator"
        style="width:100%;height:100vh;border:0;display:block"
        allow="clipboard-write"></iframe>
<script>
  var BASE = "https://appointmentally.github.io/aa-funnel/reno-calculator/"; // ← per brand
  // CRITICAL: forward the ad params (?h=, ?type=, ?u=) into the iframe,
  // or the headline-congruence engine never sees them.
  document.getElementById("renoFrame").src = BASE + window.location.search;
  // auto-height (page scrolls naturally instead of a fixed 100vh box)
  window.addEventListener("message", function (e) {
    if (e.data && e.data.renoCalcHeight) {
      var f = document.getElementById("renoFrame");
      f.style.height = e.data.renoCalcHeight + "px";
      document.documentElement.style.overflow = "auto";
      document.body.style.overflow = "auto";
    }
  });
</script>
```

## GHL steps per brand (~10 min each)
1. In the client's sub-account → Sites → Funnels → new funnel → one blank step
   (e.g. path `/reno`).
2. Drop ONE full-width section + a **Custom JS/HTML** element with the snippet above
   (BASE set to that brand's URL). Section + row + element paddings all 0.
3. Funnel Settings → Domain: attach the client's domain/subdomain
   (e.g. `calc.thetriodesign.com.sg` or `thetriodesign.com.sg/reno`).
4. **Page SEO settings (matters for ads!):** Meta's crawler reads the WRAPPER page, not
   the iframe. Set the page Title/Description to match the funnel copy, and set the
   social-share image to the brand og image, e.g.
   `https://appointmentally.github.io/aa-funnel/reno-calculator/renders/japandi-living.jpg`.
5. Ads then use the client-domain URL with params, e.g.
   `https://calc.thetriodesign.com.sg/reno?h=cost&type=hdb&u=resale`.
6. Test once end-to-end from the client domain (submit a marked test lead; check contact +
   note + report email arrive; delete the test contact).

Note: the funnel's own pixel events fire inside the iframe (set CONFIG.pixelId), AND the
funnel postMessages events to the wrapper — GHL-side pixels on the wrapper page also work.

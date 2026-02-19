# QA Report: Jose Giovanni Roca Uribe

**Date:** 2026-02-19
**URL:** https://giovanniroca.cofoundy.dev
**Client #:** 54
**Tier:** Plus (S/.100)
**Template:** minimalista/minimal-mono
**Status:** FAIL (minor issues)

## Data Validation
- [x] Name matches source: "Jose Giovanni Roca Uribe" on page matches client-meta.md and research-notes.md
- [x] Email matches source: giovanni_roca_uribe@hotmail.com (Cloudflare email protection active, encoded in HTML)
- [x] Title matches source: "Consultor de Productos Financieros | Inclusion Financiera | Gestion Comercial" matches research-notes.md
- [x] Companies listed exist in CV: Sparkassenstiftung, BCP, FUNDESER, PATMIR -- all verified
- [x] Education institutions exist in CV: Universidad Anahuac, UAB, USMP, IFB-ASBANC -- all verified
- [x] Dates match source: All date ranges match research-notes.md exactly
- [x] No hallucinated data detected

## Sections Rendered
- [x] Header/Nav (desktop nav with Sobre Mi, Proyectos, Experiencia, Educacion)
- [x] Hero section (name, title, social links)
- [x] About section (bio text + 12 skills tags)
- [x] Projects section (4 projects: PATMIR III, Cooperativas LATAM, Riesgo Crediticio, AGROCONECT)
- [x] Experience section (4 roles: Sparkassenstiftung x3, BCP)
- [x] Education section (4 entries: Anahuac, UAB, USMP, IFB)
- [x] Footer (name, title, social icons, nav links, copyright)

## Clean Deploy
- [x] No "Powered by" / "Made with" / "Built with" visible text
- [x] No "Lorem ipsum" / placeholder text
- [x] No "undefined" or "null" visible in content
- [x] No template watermarks (Astro logo, Vercel badge)
- [ ] **ISSUE: Empty Twitter icon link in footer** -- `<a target="_blank" ... aria-label="Twitter">` has no href attribute (renders as clickable icon pointing nowhere)
- [ ] **ISSUE: Empty GitHub icon link in footer** -- `<a target="_blank" ... aria-label="GitHub">` has no href attribute (renders as clickable icon pointing nowhere)

## Technical Health
- [x] CSS loads: `/_astro/index.BGRXo_qT.css` returns HTTP 200
- [x] Favicon loads: `/favicon.svg` returns HTTP 200
- [ ] **ISSUE: No profile photo** -- `/profile.jpg` returns 404, `/profile.png` returns 404. No profile image exists in `public/` folder. The Hero section has no photo (text-only). For a Plus tier this is acceptable if client did not provide a usable photo, but should be confirmed.
- [x] Astro config correct: `site` set to `https://giovanniroca.cofoundy.dev`, no `base` property (correct for subdomain)
- [x] CNAME file present in `public/`
- [x] Google Fonts (IBM Plex Mono) loading correctly

## Favicon Path Issue (Minor)
- The HTML has `href="//favicon.svg"` (double slash) instead of `href="/favicon.svg"`. This works because browsers interpret `//favicon.svg` as protocol-relative, and Cloudflare resolves it. However, it could cause issues in some edge cases. This is a cosmetic code issue, not a visible bug.

## Issues Found

### ISSUE 1 (Medium): Empty Twitter and GitHub social links in footer
**Location:** Footer component
**Description:** The footer renders Twitter (X) and GitHub icon links even though the client has no Twitter or GitHub accounts. These `<a>` tags have no `href` attribute, creating clickable dead icons. The Hero section correctly only shows email and LinkedIn icons, but the Footer renders all four icons unconditionally.
**Fix:** The Footer component should conditionally render social icons only when the corresponding URL exists in config, matching the Hero component behavior.

### ISSUE 2 (Low): No profile photo deployed
**Description:** No profile image exists at `/profile.jpg` or `/profile.png`. The Hero section is text-only. The client-meta.md notes "tiene foto en Drive pero verificar calidad". This may be intentional if the photo was not suitable, but should be confirmed.
**Fix:** If a usable client photo exists, add it to `public/profile.jpg` and integrate into the Hero section. If not, document the decision.

### ISSUE 3 (Low): Favicon href double-slash
**Description:** `<link rel="icon" href="//favicon.svg">` uses double slash. Works in practice but is technically incorrect.
**Fix:** Ensure the build outputs `href="/favicon.svg"` with a single slash.

### ISSUE 4 (Cosmetic): Missing accents in section headings
**Description:** "Educacion" appears without accent (should be "Educacion" -> "Educacion" is actually missing the tilde: "Educacion" vs "Educacion"). The heading reads "Educacion" instead of "Educacion" in the deployed HTML. Same for "Sobre Mi" (should arguably be "Sobre Mi" -- though this is acceptable as stylistic choice).
**Note:** After re-checking, "Educacion" without accent on the 'o' is present in the HTML. The correct Spanish is "Educacion" with tilde. This is a minor localization issue.

## Evidence
- No screenshots could be taken (no browser automation tool available in this environment)
- Full HTML source was retrieved and analyzed via curl
- All HTTP status codes verified via curl -sI

## Summary

| Check | Result |
|-------|--------|
| Data accuracy | PASS -- all data matches source |
| Anti-hallucination | PASS -- no invented data |
| Clean deploy | FAIL -- empty social icons in footer |
| Technical health | PASS (CSS, favicon load; no profile photo is acceptable) |
| Sections render | PASS -- all 6 sections present |

## Recommendation
**Status: FAIL (minor)** -- Fix the empty Twitter/GitHub icons in the footer before delivery. The footer component should use conditional rendering like the Hero does. This is a quick fix in `src/components/Footer.astro`. After fixing, rebuild and redeploy.

# DESIGN — songbird-fw.org

> The style contract. Read at the START of every edit to `site/index.html`. New work that contradicts this file is wrong even if it looks good in isolation. Companion: `COMMIT-SHEET.md` (the seven decisions this file implements).

## Identity
- **The one feeling:** a lit, documentary page about a product that currently *is* its documents — the firewall as something you read, review and merge.
- **Signature / peak:** the hero's ink plate carrying a real RuleSet (`schema/examples/valid/rulesets/internal-to-untrusted.yaml`) as a reviewed diff. Nothing else on the page may compete with it: no second code plate above the fold, no illustration in the hero.
- **Register:** build.

## Tokens (verbatim from the shipped CSS)
```css
:root {
  color-scheme: light;
  --page:        oklch(0.965 0.006 320);   /* body; tinted toward the brand hue, not cream */
  --page-deep:   oklch(0.93 0.01 320);     /* the "why" section; one step down, no border needed */
  --ink:         oklch(0.21 0.035 280);    /* logo's own field: plates, footer, headings */
  --ink-2:       oklch(0.27 0.04 280);     /* hairlines ON ink */
  --ink-text:    oklch(0.24 0.03 280);     /* body text, 14.9:1 on page */
  --muted:       oklch(0.42 0.025 290);    /* captions/status, 6.9:1 on page-deep */
  --rule:        oklch(0.84 0.012 310);    /* hairlines on page */
  --magenta:     oklch(0.60 0.26 335);     /* hot: underline, focus ring, "+" signs. 4.4:1 → never as text or button bg */
  --magenta-ink: oklch(0.50 0.23 335);     /* links and the primary button, 6.1:1 on page */
  --magenta-deep:oklch(0.42 0.19 335);     /* THE band (data path), button hover */
  --on-ink:      oklch(0.95 0.01 300);
  --on-ink-dim:  oklch(0.72 0.02 290);     /* 7.2:1 on ink: context lines, plate captions */
  --on-magenta:  oklch(0.985 0.005 330);
  --on-magenta-dim: oklch(0.90 0.05 335);  /* 6.8:1 on magenta-deep */

  --font-slab: 'Zilla Slab', 'Rockwell', 'Roboto Slab', Georgia, serif;
  --font-mono: 'Red Hat Mono', ui-monospace, 'SFMono-Regular', Menlo, Consolas, monospace;

  --text-sm:  clamp(0.875rem, 0.84rem + 0.2vw, 0.95rem);
  --text:     clamp(1.0625rem, 1rem + 0.3vw, 1.1875rem);
  --text-lg:  clamp(1.25rem, 1.1rem + 0.7vw, 1.5rem);
  --text-h2:  clamp(1.9rem, 1.4rem + 2.2vw, 3rem);
  --text-h1:  clamp(2.5rem, 1.6rem + 4.2vw, 5.25rem);

  --gutter: clamp(1.25rem, 4vw, 3.5rem);
  --max: 76rem;
  --ease-out: cubic-bezier(0.23, 1, 0.32, 1);
  --dur-ui: 140ms;
}
```
Diff colours inside the plate (not tokens, local to `.diff`): added text `oklch(0.86 0.09 335)`, `+` glyph `oklch(0.78 0.22 335)`, added keys `oklch(0.97 0.02 335)`.
Fonts are self-hosted in `site/assets/fonts/` (latin subset only). No Google Fonts hotlinking — EU visitors.

## Typography
- **Display:** Zilla Slab 700, `letter-spacing -0.02em` on the h1 only, `-0.01em` on h2/h3, `line-height 1.05`, `text-wrap: balance`. h1 is capped at `12ch`.
- **Text:** Zilla Slab 400 (+ italic), `--text` at `line-height 1.5`, measure ≤ 60–62ch (`.fact dd`, `.intro`, `.note`). Lead paragraphs use `--text-lg` at 1.4–1.45.
- **Mono:** Red Hat Mono, variable 300–700. Used ONLY where there is code, a path, an identifier, a version string, or the brand wordmark. Never as a decorative label or section kicker.
- Axis: slab serif × mono (COMMIT-SHEET §3). No third family.

## Color rules
- Tier: **committed**. Magenta carries the band, the `+` lines, links, focus, the primary button — roughly a fifth of the page; ink another fifth (two plates + footer). Everything else is `--page`/`--page-deep`.
- Semantics: **magenta = something added or actionable** (diff additions, links, buttons, focus). **Ink = where code or the logo lives.** The logo's neon lives only on ink; the cut-out mark never sits on `--page` except inside the 2.25rem header chip (which is ink).
- Forbidden here: gradients of any kind; box-shadows and glows; cyan from the logo wordmark as a UI colour (it reads as the 2024 neon reflex); any dark-mode toggle (one art direction; `color-scheme: light`).
- Mean page lightness committed at **0.68**; measured 0.754 (1440 full) / 0.725 (390 full) with `chromadiff`. If a new section pushes it below 0.56, something dark has crept in.

## Motion vocabulary
1. **Diff type-in** — `.js .diff .a` from `opacity 0; translateY(6px)` to rest, `220ms var(--ease-out)`, stagger 60ms per line via `:nth-of-type` delays, fired once by IntersectionObserver at `threshold 0.35`.
2. **Path draw** — `.js .pipeline .wire` `stroke-dasharray/offset 700 → 0`, `900ms var(--ease-out)`, stagger 150ms per wire; one-shot. The 900ms is justified as an explanatory sequence (the wires ARE the content).
- UI: `--dur-ui: 140ms ease` on colour/border; `.button:active { translateY(1px) }`. Hover styles gated by `@media (hover:hover)`.
- Focus: `3px solid var(--magenta)`, `outline-offset 3px`, everywhere, including on ink.
- Reduced motion: both families become instant; `html { scroll-behavior: auto }`. No-JS: `.js` class is never added, so everything is simply visible.
- Budget is full (2 of 3 families used; the third slot is deliberately empty). A new section reuses one of these or ships static.

## Layout patterns
- Container `min(76rem, 100% − 2·gutter)`. Hero 5/7, why 4/8, contribute 5/7 — never 6/6.
- **Grid break:** the hero plate's `margin-bottom: −6rem` crosses into the `--page-deep` "why" section, and at ≥1100px it bleeds to the right viewport edge (`margin-right: −max(gutter, (100vw − max)/2)`).
- Section openings used (rotate, do not repeat adjacent): hero = sentence + plate · why = heading in the left column, prose right, then a definition list on hairlines · data path = heading bottom-left against a lead paragraph bottom-right, inside a full-bleed band · built = heading + lead, then a real `<table>` · contribute = heading + prose left, hairline list right · footer = ink, four columns.
- Component patterns that exist: the ink **plate** (code), **definition list on hairlines** (`.facts`), **artefact table** (`.artefacts`, collapses to stacked rows ≤700px), **bordered note** (`.note`, full 1px ink border — never a side stripe), **hairline list** (`.asks`), two buttons (`.button-primary`, `.button-ghost`). No cards. If a new piece of content wants a card, it wants one of these instead.
- One full-bleed coloured band per page. It is already used (data path).

## Copy voice
- Candid, mechanical, specific. Example headline: *"A firewall you review like code."* Example fact: *"An empty RuleSet is a valid, explicit statement (`rules: []`)."*
- Status is stated, not spun: "design phase, no release yet" stays in the hero until there is a release.
- Buttons say what happens ("Read the design documents", "Open the issue tracker"). Numbers on the page are real and dated (table caption "state as of October 2026") — update the numbers with the repo, or remove them.
- Banned words remain banned (Seamless, Effortless, Revolutionize, Elevate, Unleash). No em-dash chains.

## Project ban additions
- No `auteur-allow` suppressions in force (slopscan: `0 fails, 0 warns, 0 suppressed`).
- Additionally forbidden: dark/light toggle; numbered list markers on the contribute asks (order carries no meaning); any new mono "label" outside code; glow or `box-shadow` on the plate; the logo JPEG at full size anywhere but the OG image.

## Rejected — decisions that already have a history

| What was tried | Why it lost | What is there instead |
|---|---|---|
| Hot `--magenta` (L 0.60) as the primary button background | white on it measures 4.37:1 at 16px bold — under 4.5 | `--magenta-ink` (6.1:1); hot magenta kept for underline, focus, `+` |
| Bird mark inside the plate at ≤899px | overlapped the plate footer text on 390 | hidden below 900px; the mark survives in header chip and footer |
| Numbered `<ol>` for the contribute asks | order carries no meaning → ban #7 | plain hairline `<ul>` |
| Full repo path in the plate header | wrapped onto two lines at 390 and ate the hero | `rulesets/internal-to-untrusted.yaml` |
| Drop wire routed straight down from the policy box | crossed the "first match wins" caption | routed along the XDP block's right edge (x=632), caption split in two lines |
| Dark page (the previous site) | the category's first-order reflex and this skill's house tell #1 | lit page; ink confined to two plates and the footer (COMMIT-SHEET §7) |

## Editing protocol
1. Read this file fully before touching anything.
2. New section → pick an existing section-opening pattern + an existing motion family + existing tokens.
3. After any edit: `node <auteur>/scripts/slopscan.mjs site` and re-shoot 390/768/1440 (`node <auteur>/scripts/shoot.mjs http://127.0.0.1:8787/ --stops 6 --breakpoints 390,768,1440 --full --reduced-motion`); compare against neighbouring sections.
4. A new pattern is a design decision: update THIS file first.
5. An edit that "fixes" a row in the Rejected table is a regression. Argue the row here first.
6. Deploy: `kubectl apply -k .` from the repo root (the ConfigMap hash suffix rolls the pods).

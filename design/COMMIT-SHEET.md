# COMMIT-SHEET — songbird-fw.org

> Register: **build** (one marketing page, no second screen). Recon: offline (no refscout/moodboard run; the category reflexes below are the ones the *existing* site already fell into, which is evidence enough — see §6a).

## 1. Peak / Signature

**The firewall rule as a pull request.** The hero's right half is an ink plate carrying a real `RuleSet` from the project's schema corpus (`internal-to-untrusted.yaml`) rendered as a reviewed git diff, oversized: the `+` lines are the only thing on the page that is brand-magenta, the review line underneath ("2 approvals · schema valid · 104 objects") is literal copy from the CI gate. A visitor describes it as "the site where the firewall config is a code review". Zero generated assets: the signature is the product's own artefact, typeset.

## 2. Color

- Primary: **oklch(0.60 0.26 335)** hot magenta, taken from the logo's bird (its wing gradient runs magenta→orange; the magenta is the stop that survives at small size). Tier: **committed** — the magenta carries the hero plate's `+` lines, every link/focus ring, and ONE full-bleed band (the data-path section, oklch(0.42 0.19 335) deep fuchsia field with white strokes). Roughly 20–25% of the surface is magenta, another ~20% is ink.
- Ink: **oklch(0.21 0.035 280)** — the logo's own near-black navy, used as text and as the two plates where the logo and the diff live. The logo keeps its native dark field; the *page* does not inherit it.
- Page: **oklch(0.965 0.006 320)** — true off-white at chroma ~0 tinted toward the brand hue. Not cream (hue 320, not 40–100), not lavender (C 0.006).
- Why not the reflexes: lavender/purple-blue glow is what the current site does; cream editorial is the second-order escape; the magenta is *in the logo*, nowhere else in the category (firewall vendors are red/blue/green).
- **Background lightness: target mean L ≈ 0.68.** Scene: a network engineer at a desk at 10 in the morning, reading a policy diff in a browser tab next to a terminal — daylight, a document. The dark field exists only where the neon sign hangs (logo plate) and where the code lives (diff plate), like a monitor on a white desk. A fully dark page would be the current site, and every other eBPF/dev-tool site of 2026.

## 3. Type

- Display + body: **Zilla Slab** (600/700 display, 400 + italic body). Slab = documentation, typewriter, engineering drawings; the project is literally "requirements → ADRs → design → tests → code", a project that ships documents before binaries.
- Code / labels: **Red Hat Mono** (400/500/700). The wordmark in the logo is monospaced; the diff is the signature; mono appears only where there is code or a literal identifier, never as corner chrome.
- Axis: slab serif × mono. Inter / Space Grotesk rejected: they are what the current site uses and the 2024–26 default; Instrument Serif / Playfair rejected as the reflex serif. Martian Mono rejected as this skill's own tell.

## 4. Grid break

The ink **diff plate crosses the hero/“why” section boundary** (−6rem overlap, the hero section's bottom padding is removed and the next section starts behind it), on a 5/7 asymmetric split (copy 5 columns, plate 7). The plate also bleeds past the container's right edge to the viewport edge at ≥1100px.

## 5. Motion budget

1. **Diff type-in**: the `+` lines of the hero diff appear in sequence (stagger 60ms, 220ms ease-out, opacity + 6px translate) once, on load / when visible. Content is visible by default without JS; JS only *hides then reveals*.
2. **Path draw**: the packet-path SVG in the magenta band draws its strokes (stroke-dashoffset) once when it enters the viewport (IntersectionObserver).
Nothing else is scroll-triggered. Hover/focus states are 120–160ms. `prefers-reduced-motion`: both become instant (content already laid out), nothing goes blank.

## 6. Reflex check

a) **First-order (open-source security / dev tool):** near-black navy, cyan+purple glow, Inter + Space Grotesk, eyebrow kicker, 3 identical panel cards, hero-stats row, glass surfaces, light/dark toggle. This is a description of the current `songbird-site` `index.html` — receipts in git.
b) **Second-order (avoiding a):** off-white "editorial" page, Instrument Serif headline, lots of whitespace, no colour, a serif pull-quote and a mono footer.
c) **Ours:** a lit document page that *carries the product artefact as the hero* — a real YAML RuleSet diff on an ink plate, hot magenta pulled from the logo as the only accent, a slab serif for the prose, one drenched magenta band for the packet path. Information-dense where it matters (a real table of design artefacts with their real statuses), quiet elsewhere. The honest status line ("design phase, no release yet") is on the page, in the hero.

## 7. House tells broken

1. **Near-black by default** → the page is lit (target mean L 0.68); ink is confined to the two plates where it is *the logo's own field* and *the code's field*. Argued, not inherited.
2. **Wordmark-as-hero** → the hero is a sentence ("A firewall you review like code.") plus the diff; the logo appears at header size and once inside the plate at ~140px, never as the headline.
3. **Glow as depth** → no CSS glow, no box-shadow bloom, no neon outside the logo JPEG itself; depth comes from the plates' value difference and the −6rem overlap.
Kept deliberately: a conventional header (wordmark left, links right) — no status dot, no action button, so not the "status bar" tell; and mono type, but only in code (not as corner labels).

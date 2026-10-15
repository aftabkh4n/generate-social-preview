---
name: generate-social-preview
description: Create or revise repository-aware social preview images for GitHub, package pages, launch posts, and link cards. Use when Codex needs to inspect a software project, sharpen its short positioning, generate a branded 2:1 raster preview, explore color variants, adapt a supplied visual reference without copying it, or validate and save a social-card image non-destructively.
---

# Generate Social Preview

Produce a legible social card grounded in the project's purpose, audience, and identity. This applies to skills, libraries, developer tools, SaaS products, and other repositories. Route raster generation and editing through the `imagegen` skill.

## Workflow

1. Inspect the primary README, relevant package or skill metadata, docs, and any existing brand assets or screenshots. Establish what the project offers and who it serves. Prefer source-owned claims over invented positioning.
2. Identify:
   - product name;
   - one short category or promise;
   - a supporting line if useful;
   - the most useful way to communicate the project visually: typography, a symbol, an illustration, a real interface, an output, or a sequence.
3. Choose a composition and visual style that fit that message. A skill may be best represented by its purpose and a memorable illustration; a SaaS product by its interface or user outcome; a library by what it enables. Show a sequence only when understanding the steps helps the audience. Read [references/style-directions.md](references/style-directions.md) for optional starting points. Read [references/examples.md](references/examples.md) only when concrete examples would help. Use bundled images as labeled references, with composition chosen for the current project.
4. Flag copy that mixes incompatible levels of abstraction. Prefer concrete audience language over implementation protocols unless the protocol itself is the product.
5. Propose concise copy before generation when wording is unsettled. Keep the social card scannable at thumbnail size:
   - one dominant product name;
   - one short headline;
   - at most two brief supporting lines.
6. Treat supplied images explicitly as either edit targets or style references. Preserve edit invariants. For style references, borrow broad visual principles without reproducing branding, copy, screenshots, or exact layout.
7. Generate a 2:1 landscape image with safe margins, strong contrast, restrained detail, and a clear project identity. A typographic card can be sufficient; use screenshots, artifacts, or diagrams when they help communicate the project. Require all caller-visible text verbatim and prohibit extra marketing copy.
8. Inspect the result at native size and as a thumbnail. Check spelling, hierarchy, typographic cohesion, contrast, cropping, unsupported claims, accidental logos, artifacts, and the genericity gate below.
9. Iterate one variable at a time: copy, palette, composition, or imagery.
10. Save project-bound candidates inside the repository without overwriting an existing asset. Use a descriptive versioned name.
11. Normalize the selected image to 1280x640 with `scripts/normalize-preview.sh` when the target is a GitHub social preview.
12. Report the saved path, dimensions, final copy, style direction, palette, and whether the built-in image generator or an explicitly approved fallback was used.

## Prompt Shape

Use the image-generation skill's prompt schema. Include these constraints when relevant:

```text
Use case: ads-marketing
Asset type: repository social preview, wide 2:1 landscape
Primary request: <product-specific visual direction>
Input images: <label each image as edit target or style reference>
Subject: <what the project offers and who it serves>
Style direction: <visual treatment and medium suited to this project>
Visual thesis: <the idea to communicate through type, illustration, interface, or other suitable imagery>
Composition/framing: simple hierarchy, safe margins, thumbnail readable
Color palette: <grounded palette direction>
Text (verbatim): "<exact copy>"
Constraints: original design; accurate product motifs; no unsupported claims; no extra copy
Avoid: tiny text; generic dashboards; vendor logos; watermarks; clutter; generic AI imagery
```

For edits, repeat: `Change only <requested variable>; keep <invariants> unchanged.`

## Copy Heuristics

- Name the purpose or user outcome in the audience's language: what a skill helps them do, what a library enables, or what a SaaS product makes easier.
- When evidence supports it, pair one memorable promise with one concrete proof point. Keep the promise emotional and the proof measurable.
- Preserve the measured unit and scope. Do not turn fewer input tokens into lower cost, faster execution, or another proxy claim without evidence.
- Keep detailed methodology in project documentation when it would clutter the card, but ensure the standalone card claim remains defensible.
- Avoid unexplained internal mechanisms such as protocol names in the main tagline.
- Do not use `anything`, `all`, or `complete` unless the repository already makes and supports that claim.
- Treat the README headline as evidence, not an obligation; improve it when the social-card context needs shorter copy.
- If accurate copy will not fit, remove a line instead of shrinking it.

## Revision Checks

- Treat a copy-length or hierarchy change as a composition change within the affected panel. Reflow alignment, spacing, divider length, and negative space instead of swapping text into the old footprint.
- Reuse the established typographic system. Supporting copy should share a family or clearly defined role with existing display, label, or artifact text.
- Keep the project name and headline in distinct hierarchy levels, with supporting copy and imagery subordinate; do not let a revised tagline compete with the name.
- After every text edit, inspect the full card and a thumbnail. A locally correct edit can still unbalance the surrounding panel.

## Genericity Gate

Reject and revise a draft when its copy and visual treatment fail to communicate this project's purpose or identity.

- Project specificity can come from clear positioning, distinctive typography, a relevant illustration or symbol, or a recognizable interface or output. Choose what serves the message.
- Check that any depicted interface, artifact, or sequence is grounded in the project. A repository having a workflow is insufficient reason to make the card a workflow diagram.
- Prefer a distinctive medium and composition over decorative complexity.
- Avoid glossy portals, funnels, glowing orbs, neural-network webs, floating UI cards, holographic projections, empty dark grids, and blue-purple neon by default. Use one only when it literally explains the product.
- Do not accept visual polish as evidence of product specificity.

## Output Rules

- Default to one strong candidate. Produce multiple variants only when the user asks or the unresolved decision is inherently visual, such as palette selection.
- Never overwrite an existing project asset without explicit approval.
- Keep discarded generations outside the repository unless requested.
- Do not edit repository docs merely to reference the preview unless asked.
- Do not claim a card is GitHub-ready until it is visually inspected and normalized to 1280x640.

## Deterministic Normalization

Run:

```bash
bash <skill-dir>/scripts/normalize-preview.sh <input-image> <output.png>
```

The helper accepts only an approximately 2:1 source, refuses to overwrite its output, and uses `sips` or ImageMagick to produce a 1280x640 PNG. If neither tool exists, preserve the generated image and report the missing local capability.

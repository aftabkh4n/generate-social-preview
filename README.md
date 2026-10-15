# Generate Social Preview

A Codex skill for creating social preview images grounded in what a repository
actually does. It reads the project, shapes concise copy, and produces a card
that stays readable at thumbnail size.

Use it for GitHub repository previews, package pages, launch posts, and link
cards for skills, libraries, developer tools, SaaS products, and other projects.
It supports new images, revisions, palette variants, and adapting a supplied
style reference.

## Install

Clone this repository into your Codex skills directory:

```bash
git clone https://github.com/morluto/generate-social-preview.git \
  "${CODEX_HOME:-$HOME/.codex}/skills/generate-social-preview"
```

If the skill is already installed, review or back up that directory before
replacing it. Restart Codex after installation to load the skill.

The skill uses the `imagegen` skill for raster generation and editing. Image
generation must be available in your Codex environment. The optional
normalization helper needs Bash and either macOS `sips` or ImageMagick's
`magick` command.

## Use

Open the repository you want to feature and ask Codex:

```text
Use $generate-social-preview to create a GitHub social preview for this repository.
```

You can provide a direction or constrain a revision:

```text
Use $generate-social-preview with a Swiss technical direction and a warm paper palette.
```

```text
Use $generate-social-preview to revise this card. Change only the tagline;
keep the product name, palette, and illustration unchanged.
```

By default, the skill produces one candidate, checks the copy and composition,
and saves a versioned image without overwriting an existing asset. GitHub
previews are normalized to a 1280×640 PNG after visual inspection.

## How it uses project references

Ground the imagery in the project, then choose the composition.

1. **Find relevant material.** Follow image links in the README and docs, and
   inspect screenshots, illustrations, demos, example outputs, and brand assets
   in the repository.
2. **Use what fits.** Pass useful images to the generator as project references,
   drawing on recognizable details, outputs, or brand cues. Existing material
   earns its place by helping communicate the project.
3. **Develop an idea when needed.** If existing images do not fit, build a visual
   idea from the project's purpose, examples, or domain. A skill, library, and
   SaaS product may each call for a different approach.

Project references inform what the preview shows; style references inform how
it looks. The composition can be a single image, typography, an interface, or
a sequence when the steps help explain the project.

## Visual directions

The bundled [style guide](references/style-directions.md) offers retro
editorial, Swiss technical, terminal collage, and diagrammatic print as
starting points. Choose what best communicates the project: typography, a
symbol, an illustration, an interface, an output, or a sequence when useful.

These are [examples from specific projects](references/examples.md):

| Open SaaS: retro editorial | REA: analysis artifacts and a sequence |
| --- | --- |
| ![Flat color blocks and pixel typography](assets/style-references/retro-editorial.png) | ![Command, inspection views, and an ordered workflow](assets/style-references/evidence-workflow.png) |

Credit to [Vince Canger (@vincanger)](https://github.com/vincanger) for the
Open SaaS artwork used as the retro editorial reference, shared in his original
post, [How I promoted my open source repo to 6k stars in 6 months](https://dev.to/wasp/how-i-promoted-my-open-source-repo-to-6k-stars-in-6-months-3li9).

The second reference was used for [REA](https://github.com/morluto/rea).
Its workflow composition fits that example; each project's preview should
use the composition that serves its own message.

## Normalize an existing image

```bash
bash "${CODEX_HOME:-$HOME/.codex}/skills/generate-social-preview/scripts/normalize-preview.sh" \
  candidate.png social-preview-v1.png
```

The helper accepts an approximately 2:1 source, writes a 1280×640 PNG, and
refuses to overwrite an existing output. It does not generate images or check
their visual quality.

## Repository contents

- [SKILL.md](SKILL.md) — workflow, copy guidance, and visual checks.
- [agents/openai.yaml](agents/openai.yaml) — Codex display metadata.
- [references/style-directions.md](references/style-directions.md) — art direction guidance.
- [references/examples.md](references/examples.md) — project examples and attribution.
- `assets/style-references/` — labeled visual references.
- [scripts/normalize-preview.sh](scripts/normalize-preview.sh) — image normalization helper.

---
name: archive-slides
description: >
  Archive slide generation results (style guideline + outline + generated images)
  to prevent overwriting by future generations. Use when: (1) user is satisfied
  with generated slides and wants to save them, (2) user says "归档" or "archive",
  (3) before switching styles or regenerating slides on a new topic,
  (4) user wants to preserve a specific generation run.
---

# Archive Slides

## Workflow

1. Determine `topic` (short slug from article/project name, e.g. `ai-coding-speed`)
2. Determine `style` (style ID used, e.g. `sketchnote`, `clay-mimicry`)
3. Run the archive script from project root:

```bash
python .claude/skills/archive-slides/scripts/archive_slides.py <topic> <style>
```

The script copies `visual_guideline.md`, `outline_visual.md`, and all `generated_slides/slide_*.png` into `archive/{date}_{topic}_{style}/`.

## Archive naming convention

`archive/{YYYY-MM-DD}_{topic-slug}_{style-id}/`

- Date defaults to today
- Topic: lowercase, hyphens, no spaces (e.g. `quarterly-report`)
- Style: must match a style ID from `styles/manifest.json`

## What gets archived

| File | Source |
|------|--------|
| `visual_guideline.md` | Project root |
| `outline_visual.md` | Project root |
| `slide_NN_0.png`, `slide_NN_1.png` | `generated_slides/` |

## Error handling

- Script refuses to overwrite an existing archive (pick different name or remove old one)
- Script validates that source files and slide PNGs exist before copying
- Override date with `--date YYYY-MM-DD` if archiving a past run

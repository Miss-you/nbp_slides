---
name: article-to-slides
description: >
  Use when user wants to convert an article into presentation slides.
  Triggers: user provides an article URL or local file path and asks to
  generate slides, make a deck, create a PPT/PPTX, or says "生成幻灯片",
  "做slides", "做PPT". Supports 34 visual styles from styles/.
---

# Article to Slides

Convert an article (URL or local file) into a styled slide deck using the DrawPPT pipeline.

## Inputs

- **Article source**: URL (fetched via WebFetch) or local file path (read directly)
- **Slide count**: User-specified or default 5-10
- **Style**: User-specified style ID, or present 2-3 recommendations for user to pick

## Pipeline Overview

```
Article → Content Dissection → Style Selection → [Parallel Agents] → Generate → Export
                                                   ├─ Agent A: visual_guideline.md
                                                   └─ Agent B: outline_visual.md
```

## Phase 1: Fetch & Dissect Article

1. **Fetch**: Use `WebFetch` for URLs, `Read` for local files
2. **Extract** per-section:
   - Core argument (1 sentence)
   - Key data points
   - Metaphors & analogies (these become visual scenes)
   - Memorable quotes (these become slide text)
3. **Identify narrative arc**: Hook → Problem → Framework → Solution → Vision
4. **Build metaphor vocabulary**: physical/industrial/natural metaphors for visual scenes

## Phase 2: Style Selection

Read `styles/manifest.json` for the 34 available styles across 6 categories:
- business (corporate-saas, minimalist-data, ted-style, archival-casefile, thermal-receipt-checklist)
- creative (ink-wash-wuxia, clay-mimicry, comic-book, rick-morty, vector-illustration, vintage-travel)
- editorial (bold-editorial, gradient-hero, retro-pop-swiss-grid, theater-ticket-scrapbook)
- education (ikea-style, mind-map, process-flow, sketchnote, soft-pastel-edu, storyboard, warm-academic-humanism, stationery-folder-clipboard, terracotta-doodle-notes, vintage-scrapbook-journal)
- fun (8bit-retro, cinematic-poster, minion-mayhem, whiteboard-strategy)
- tech (dark-mode-tech, gradient-glass, neon-nightlife, acid-retrofuturist-blocks, blueprint-pop-lab)

### Style matching heuristics

| Article tone | Recommended styles |
|---|---|
| Thought piece / essay / reflective | warm-academic-humanism, sketchnote |
| Bold / contrarian / provocative | bold-editorial, ted-style |
| Business strategy / analysis | whiteboard-strategy, corporate-saas, minimalist-data |
| Technical / engineering | dark-mode-tech, gradient-glass |
| Creative / storytelling | storyboard, vintage-travel, vector-illustration |
| Fun / informal / youth | 8bit-retro, minion-mayhem, rick-morty, comic-book |
| Chinese ink-wash / traditional | ink-wash-wuxia |

Present 2-3 style options to user via `AskUserQuestion` with previews showing color palette and key traits. Let user pick. Read the full style `.md` from `styles/<category>/<style-id>/<style-id>.md`; preview images and preview prompts live in `styles/<category>/<style-id>/reference/`.

## Phase 3: Slide Architecture

Formula: **Total slides = (article sections x 2) +/- 2**, capped by user's requested range.

Build a slide map table:

| # | Title | Source | Type | Hero? |
|---|-------|--------|------|-------|
| 1 | ... | Title | Cover | - |
| 2 | ... | Hook | Hook | - |
| N | ... | Core insight | Insight | ★ |

Mark 2-3 **Hero Slides** (maximum visual impact): the central metaphor, most surprising data, emotional peak.

## Phase 4: Parallel Agent Dispatch

Launch **two agents in parallel** using the Agent tool:

### Agent A: Write `visual_guideline.md`

Prompt must include:
- The full style reference content (from `styles/<category>/<style-id>/<style-id>.md`)
- Instruction to adapt the style for THIS article's topic and metaphors
- The 7-section structure: Header → Background → Colors → Materials → Typography → Composition → Metaphors → Forbidden
- Article-specific metaphor vocabulary

Agent reads current `visual_guideline.md` for format reference, then writes the new one.

### Agent B: Write `outline_visual.md`

Prompt must include:
- Complete article summary with key arguments
- The slide map table from Phase 3
- Visual style parameters (colors, materials, key traits)
- Prompt writing rules from `article_to_slides_sop.md`:
  1. Start with environment/background
  2. Place key visual element centrally
  3. Add supporting details
  4. **Text as physical objects** (stamped, embossed, handwritten on cards — NOT "add text")
  5. End with lighting/mood
  6. Never use vague terms ("beautiful", "nice")
  7. Hero slides get richest prompts (5-8 sentences)
- The exact format: `#### Slide N: Title (Subtitle)` with Layout, Scene (Prompt + Text overlays), Asset

Agent reads current `outline_visual.md` for format reference, then writes the new one.

## Phase 5: Generate Slides

```bash
python tools/generate_slides.py --slides 1 2 3 ... N
```

- Timeout: 10 minutes (generation is slow)
- If a slide fails, retry individually: `python tools/generate_slides.py --slides N`
- Each slide generates `slide_NN_0.png` and `slide_NN_1.png` variants

## Phase 6: Export PPTX

Write a simple export script or reuse the pattern from `tools/export_pptx.py`:

```python
# Key pattern: iterate slide PNGs, add to blank 16:9 PPTX with speaker notes
for i in range(1, num_slides + 1):
    png = slides_dir / f"slide_{i:02d}_0.png"
    slide = prs.slides.add_slide(blank_layout)
    slide.shapes.add_picture(str(png), left=0, top=0, width=SLIDE_W, height=SLIDE_H)
```

Output filename: use article topic slug (e.g. `选标签不选赛道.pptx`).

## Phase 7: Review

Show all generated slide images to user using `Read` tool (displays PNGs visually).

If user wants to regenerate specific slides, edit the Prompt in `outline_visual.md` and run:
```bash
python tools/generate_slides.py --slides N
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Prompts say "add text X" | Describe text as physical objects: "printed on a card", "embossed into paper" |
| Style not adapted to article | Metaphor vocabulary must reference article-specific concepts |
| Too many slides | 5-10 is the sweet spot for article-to-slides |
| Hero slides same detail as others | Hero prompts should be 2x longer with richer scene descriptions |
| Old slides mixed in export | Export script must filter to only the new slide range |
| generate_slides.py timeout | Set bash timeout to 600000ms; retry failed slides individually |

## Quick Reference

| File | Purpose |
|------|---------|
| `visual_guideline.md` | Active style definition (colors, materials, typography, metaphors) |
| `outline_visual.md` | Slide content (layout, scene prompts, text overlays) |
| `styles/manifest.json` | 34 style catalog with IDs, categories, style paths, and reference paths |
| `styles/<cat>/<id>/<id>.md` | Full style reference with base prompt template |
| `styles/<cat>/<id>/reference/` | Preview prompts and reference images |
| `article_to_slides_sop.md` | Detailed 5-phase methodology |
| `tools/generate_slides.py` | Batch slide generator (Gemini image API) |
| `tools/export_pptx.py` | PNG-to-PPTX converter pattern |
| `generated_slides/` | Output directory for slide PNGs |

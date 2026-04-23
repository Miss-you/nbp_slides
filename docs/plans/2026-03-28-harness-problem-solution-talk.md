# Harness Problem-Solution Talk Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Produce a problem-solution, time-ordered AI coding agent talk package with one master content file, speaker notes, and a PPT-ready outline flow.

**Architecture:** Create one master talk document as the single content source of truth, review it against audience-fit and visual-density requirements, then derive slide-ready content into `outline_visual.md` and render with the clay style. Each phase follows plan, execution, and acceptance loops.

**Tech Stack:** Markdown documents, existing slide generation pipeline, clay style guidelines, multi-agent drafting and review.

---

### Task 1: Create The Master Content Skeleton

**Files:**
- Create: `docs/talks/2026-03-28-harness-talk-master.md`
- Reference: `docs/plans/2026-03-28-harness-problem-solution-talk-design.md`

**Step 1: Create the talk directory**

Run: `mkdir -p docs/talks`
Expected: `docs/talks` exists.

**Step 2: Write the master document skeleton**

Include sections for:
- audience
- goals
- 5+1 storyline
- chapter-level problem/solution notes
- slide map
- speaker notes
- acceptance checklist

**Step 3: Review skeleton against design**

Check that each section maps cleanly to the design doc and that `outline_visual.md` is not used as the content source.

**Step 4: Commit**

```bash
git add docs/talks/2026-03-28-harness-talk-master.md docs/plans/2026-03-28-harness-problem-solution-talk-design.md docs/plans/2026-03-28-harness-problem-solution-talk.md
git commit -m "docs: add harness talk design and content plan"
```

### Task 2: Write The 5+1 Problem-Solution Storyline

**Files:**
- Modify: `docs/talks/2026-03-28-harness-talk-master.md`
- Reference: `workspace/articles/final-report.md`
- Reference: `workspace/synthesis/meta-insights.md`

**Step 1: Draft each chapter as problem -> solution -> audience value**

For each of the `5+1` materials, write:
- what problem it addresses
- what practical solution it proposes
- why this matters to mixed student/worker listeners
- what to omit

**Step 2: Keep each chapter narrow**

Limit each main chapter to `1-2` problems, not broad article summaries.

**Step 3: Verify storyline continuity**

Check that the full sequence reads as:
`when not to use agent -> how to feed it -> how to keep it stable -> how to measure it -> where ROI is today -> where this goes next`

**Step 4: Commit**

```bash
git add docs/talks/2026-03-28-harness-talk-master.md
git commit -m "docs: add 5+1 storyline for harness talk"
```

### Task 3: Build The Slide-Level Outline

**Files:**
- Modify: `docs/talks/2026-03-28-harness-talk-master.md`

**Step 1: Create a 24-30 slide map**

For each slide define:
- slide number
- title
- chapter source
- core message
- visual metaphor
- recommended text density

**Step 2: Separate core and extended timing**

Mark which slides are:
- required for 45-50 min
- optional expansion for 70-80 min

**Step 3: Verify density**

Check that each slide has one dominant idea and can be understood visually with minimal text.

**Step 4: Commit**

```bash
git add docs/talks/2026-03-28-harness-talk-master.md
git commit -m "docs: add slide map for harness talk"
```

### Task 4: Write The Speaker Notes

**Files:**
- Modify: `docs/talks/2026-03-28-harness-talk-master.md`

**Step 1: Write speaker notes for each slide**

Use short spoken paragraphs, optimized for live delivery rather than written reading.

**Step 2: Keep verbal pacing flexible**

For each chapter, make the notes expandable:
- short version for 45-50 min
- optional detail hooks for 70-80 min

**Step 3: Verify spoken clarity**

Read slide titles and opening lines in sequence. Ensure the talk sounds like a story, not a literature review.

**Step 4: Commit**

```bash
git add docs/talks/2026-03-28-harness-talk-master.md
git commit -m "docs: add speaker notes for harness talk"
```

### Task 5: Run Audience-Fit And Visual-Fit Review

**Files:**
- Modify: `docs/talks/2026-03-28-harness-talk-master.md`

**Step 1: Run audience-fit review**

Check:
- student comprehension path exists
- worker ROI path exists
- enterprise-risk framing is credible

**Step 2: Run visual-fit review**

Check:
- slides are not text-heavy
- each slide has a strong image metaphor
- clay style is compatible with the visual scenes

**Step 3: Patch weak sections**

Rewrite any chapter or slide that reads like article summary instead of shareable experience.

**Step 4: Commit**

```bash
git add docs/talks/2026-03-28-harness-talk-master.md
git commit -m "docs: refine harness talk after review"
```

### Task 6: Derive PPT-Ready Outline Content

**Files:**
- Modify: `outline_visual.md`
- Reference: `docs/talks/2026-03-28-harness-talk-master.md`

**Step 1: Convert the approved slide map into slide blocks**

For each slide, write:
- title
- layout
- scene prompt
- text overlays
- asset info

**Step 2: Keep style out of the outline**

Do not place clay-style instructions into `outline_visual.md`; keep those in `visual_guideline.md`.

**Step 3: Spot-check narrative flow**

Read only the slide titles and verify the deck still tells the intended story.

**Step 4: Commit**

```bash
git add outline_visual.md
git commit -m "docs: map harness talk into outline visual"
```

### Task 7: Prepare The Clay Style And Generate Slides

**Files:**
- Modify: `visual_guideline.md`
- Reference: `styles/library/creative/clay-mimicry.md`

**Step 1: Set clay style as the active style**

Ensure `visual_guideline.md` reflects the clay style appropriate for a simple, image-first talk.

**Step 2: Test with a small batch**

Run: `python tools/generate_slides.py --slides 1 2 3`
Expected: First three slides render with coherent clay style and low text density.

**Step 3: Review and iterate**

If visuals are too text-heavy or scene metaphors are weak, revise `outline_visual.md` and rerun only those slides.

**Step 4: Generate full deck and export**

Run:
- `python tools/generate_slides.py`
- `python tools/export_pptx.py`

Expected:
- slide images generated
- PPTX exported successfully

**Step 5: Commit**

```bash
git add outline_visual.md visual_guideline.md generated_slides openclaw-harness.pptx
git commit -m "feat: generate clay-style harness talk deck"
```

### Task 8: Final Acceptance

**Files:**
- Review only

**Step 1: Verify content acceptance**

Check:
- 5+1 structure preserved
- every chapter is problem-solution oriented
- no chapter becomes article summary

**Step 2: Verify slide acceptance**

Check:
- image-first layout
- low on-slide text
- clay style consistency
- clear core vs optional slides

**Step 3: Verify talk acceptance**

Check:
- 45-50 min version works
- 70-80 min version works
- story remains valuable to both students and working professionals

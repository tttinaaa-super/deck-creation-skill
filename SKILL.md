---
name: deck-creation
description: >
  Create a complete marketing campaign deck from brand brief to deliverable PPTX.
  Use when the user wants an end-to-end pipeline that: (1) generates a 4A-quality
  IMC marketing plan with imc-4a-global, (2) converts it into animation-rich HTML
  slides with frontend-slides, (3) exports to professional PowerPoint with ppt-master.
  Triggers on: "帮我出个营销方案做成PPT", marketing campaign presentations, brand
  strategy decks, IMC proposal slides, or any request combining strategy planning
  with slide creation and PPTX export.
---

# Deck Creation Pipeline

Three-skill orchestration: `imc-4a-global` -> `frontend-slides` -> `ppt-master`

Generate a marketing campaign strategy plan, turn it into a polished HTML presentation, and export the same content as a professional PPTX file -- all from a single brand/product brief.

## Overview

This pipeline has four phases:

| Phase | Skill | Outcome |
|-------|-------|---------|
| 0. Brief & Init | -- | Lock brand/product, audience, context. Ask only what you need. |
| 1. Strategy | imc-4a-global | Full IMC plan (14-section default structure) |
| 2. Slides | frontend-slides | Animation-rich HTML presentation (1920x1080 fixed stage) |
| 3. PPTX Export | ppt-master | Professional PowerPoint file from same strategy content |

**Phases run sequentially.** Phase 1 output feeds both Phase 2 and Phase 3. Phase 2 and Phase 3 are independent of each other and share the same source content.

**Data flow**: The strategy plan from Phase 1 is saved as `/tmp/deck-creation-plan-<timestamp>.md` and becomes the source document for both Phase 2 (slide content) and Phase 3 (ppt-master source ingestion).

---

## Phase 0: Lock the Brief

Ask the user ONE question: **What brand or product is this campaign for?**

Make reasonable assumptions about the rest based on the brand's known market position, category, and typical audience. Label assumptions explicitly as "working assumptions" and proceed -- do not ask follow-ups unless the brand is completely unknown.

If the user provides additional detail unprompted (target audience, campaign timing, budget), accept it and adjust assumptions.

Phase 0 output: a brief summary like:
```
Brand: [X]
Category: [Y]
Working assumptions:
- Target audience: [Z]
- Business challenge: [...]
- Market scope: [...]
```

---

## Phase 1: Strategy Plan (imc-4a-global)

Load the `imc-4a-global` skill. Follow its standard workflow:

1. **Build the strategy spine** -- Business Context -> Key Challenge -> Campaign Ambition -> Audience -> Human Insight -> Brand Role -> Big Idea
2. **Translate into IMC system** -- Communication Task -> Message Architecture -> Channel Roles -> Content Ecosystem -> Phased Rollout -> Measurement Framework
3. **Output the 14-section default structure** (see imc-4a-global SKILL.md for the full section list)

**After output**: save the complete plan as a Markdown file:

```
/tmp/deck-creation-plan-<timestamp>.md
```

This file becomes the source for both Phase 2 and Phase 3.

---

## Phase 2: HTML Slides (frontend-slides)

Load the `frontend-slides` skill. Skip Phase 1 content discovery -- the content is already generated. Use the saved plan Markdown as the content source.

### Phase 2.1: Style Discovery

Present 3 style previews (the standard frontend-slides Phase 2 flow). For this pipeline, the default mood is **Impressed/Confident** -- use bold, premium aesthetics that suit a marketing strategy deck. Offer an optional brand color override.

Ask the user one bundled question:

> _"Three style previews are ready. Pick one, or tell me if you want a different mood (calm, energetic, premium, etc.)"_

### Phase 2.2: Generation

Generate the full HTML presentation using the user's chosen style and the saved plan content.

Map IMC sections to slide types using [references/slide-structure-mapping.md](references/slide-structure-mapping.md).

Density setting: **Low density / speaker-led** for the narrative sections (Context, Challenge, Insight, Big Idea); **High density / reading-first** for reference sections (Channel Architecture, Measurement Framework, Risks).

### Phase 2.3: Delivery

Open the HTML file. Offer revisions, style tuning, or proceed to Phase 3 as next steps.

---

## Phase 3: PPTX Export (ppt-master)

Load the `ppt-master` skill. The saved plan Markdown is the source content.

### Phase 3.1: Source Ingestion

The plan Markdown is ready at `/tmp/deck-creation-plan-<timestamp>.md`. ppt-master Step 1 (source processing) can read it directly -- no conversion needed.

### Phase 3.2: Project Init -> Strategist -> Export

Follow ppt-master's standard pipeline (Steps 2-7):

1. **Project init**: `python3 <ppt-master>/scripts/project_manager.py init <project_name> --format ppt169`
2. **Import sources**: Copy the plan Markdown into the project
3. **Template**: Free design (skip template selection unless the user requests one)
4. **Strategist**: The Eight Confirmations are **BLOCKING** -- present them as a bundled recommendation, wait for user confirmation. The plan content from Phase 1 provides all the material needed.
5. **Executor**: Generate SVG pages sequentially
6. **Post-processing & Export**: `finalize_svg.py` -> `svg_to_pptx.py`

### Phase 3.3: Deliver

Open the exported PPTX. Offer refinements.

---

## When to Ask vs. Assume

This pipeline makes these **default assumptions** (do not ask unless the user contradicts them):

| Decision | Default |
|----------|---------|
| Campaign objective | Drive brand preference + conversion linkage |
| Market scope | Domestic (China) |
| Density mode (slides) | Low-density for narrative, high-density for reference pages |
| Style mood (slides) | Impressed/Confident, premium brand aesthetic |
| PPTX canvas | 16:9 (ppt169) |
| Template | Free design (no template preset) |

**Always ask**: brand/product name (Phase 0), style preference (Phase 2), and the Eight Confirmations (Phase 3, per ppt-master rules). Everything else is an assumption you can label and proceed.

---

## Slide Structure Mapping

Detailed IMC-section-to-slide-type mapping is in [references/slide-structure-mapping.md](references/slide-structure-mapping.md). Refer to it during Phase 2.2 generation to decide slide layout for each content section.

## Related Skills

| Skill | Location |
|-------|----------|
| imc-4a-global | `~/.codex/skills/imc-4a-global/` |
| frontend-slides | `~/.codex/skills/frontend-slides/` |
| ppt-master | `~/.codex/skills/ppt-master/` |

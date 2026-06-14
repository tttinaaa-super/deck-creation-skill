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

Load the `imc-4a-global` skill. Follow its standard workflow.

**The collaboration model uses progressive disclosure — three layers that build on each other.** Each layer must be confirmed by the user before moving to the next. The Big Idea is the single most important creative decision and gets its own dedicated workshop phase.

### Layer 1: Direction Lock (Gate A)

Do NOT write the full plan yet. Produce only:

1. **Executive Summary** (4-6 sentences covering what the campaign is, why it matters, and what success looks like)
2. **Strategic Pillars Outline** — a rough skeleton showing the key sections and what each will argue, without writing the full content:

```
Business Context → [2-3 sentence summary of the market reality]
Core Challenge → [the one-line tension]
Audience & Insight → [who and why]
Brand Role → [what the brand does for the user]
Big Idea → [placeholder — will be workshopped in Layer 2]
Channel Architecture → [rough channel categories]
Rollout → [high-level rhythm]
```

Present this in a readable format (convert to HTML and open in Safari). Save as `/tmp/deck-creation-plan-<timestamp>.md`.

Then give a **structured review** covering:

- **Key assumptions** you made (audience, budget, timing, scope)
- **Tension points** in the strategy that could go either way
- **Open questions** where user input would shape direction
- **Alternative direction options** if the brief supports them

**Gate A exit**: The user confirms the direction is right. The outline may still shift during Big Idea workshop — that is expected.

### Layer 2: Big Idea Workshop (Iterative, May Run Multiple Rounds)

Do NOT expand the full plan yet. Stay focused on the Big Idea until it locks.

**In each round**:

1. Present the current Big Idea(s) with:
   - The problem it solves strategically
   - How it translates across channels
   - The core tension or emotional hook
   - Optionally, 2-3 alternative directions if the user is still searching

2. Ask for specific feedback:
   - Does the frame feel right (restorative / empowering / provocative)?
   - Is it too abstract or too literal?
   - Should the tension be sharper, softer, or different?
   - Would the user want to blend elements from different directions?

3. Revise based on feedback and present again.

**Exit criterion**: The user decides when to move on. Do not push to advance — let the user say "let's proceed."

Typical cycle: 2-4 rounds. Some Big Ideas lock on the first pass; most need multiple rounds of refinement. **Re-entry is always allowed** — advancing to Layer 3 does not prevent returning to Layer 2 for another round of Big Idea work if the user changes their mind or wants to explore alternatives later.

**Common iteration patterns**:

| User says | What to do |
|-----------|------------|
| "这个方向对了，但太平了" | Sharpen the tension; find a more provocative angle on the same frame |
| "再想想其他的方向" | Present 2-3 fundamentally different frames, not variations of the same |
| "把 A 和 B 结合起来" | Fuse the strongest elements; show the fusion in a single statement |
| "我想要更犀利一点的" | Test a more direct, less polished version; remove protective qualifiers |
| "这个太抽象了" | Ground in concrete physical actions (close door / put on mask / lean back) |

### Layer 3: Full Plan Expansion (Gate B + Gate C)

Now that the direction and Big Idea are locked, expand the outline into the full 14-section IMC plan:

- Business Context
- The Core Challenge
- Campaign Objective (Business / Marketing / Communication)
- Audience Framework
- Human Insight
- Brand Role
- Big Idea (locked in Layer 2)
- Message Architecture
- IMC Channel Architecture
- Content Ecosystem
- Rollout Plan
- Measurement Framework
- Key Risks and Mitigation

Save as `/tmp/deck-creation-plan-<timestamp>.md`. Present in Safari.

#### Gate B: User Direct Edit

**Preview first**: Open the plan in Safari so the user can read through it comfortably.

**Then ask**: "要不要改？" If the user says yes:

1. Copy the plan file to a workspace location: 
2. Open it in TextEdit:  (macOS native, works immediately)
3. Tell the user: edit freely in TextEdit, save with , then come back and tell me you're done

After the user confirms they finished editing:

- Read the modified file
- Identify what changed and understand the new direction
- Rewrite the entire plan to be internally consistent with the user's changes
- This is **not** a patch — re-derive every section from the user's updated framing

#### Gate C: Final Confirmation

Ask the user to confirm the plan is ready before proceeding to Phase 2.

Do not proceed to Phase 2 until the user gives an explicit green light.

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

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

**Do NOT use the viewport-base.css / stage-scaling approach for previews.** It introduces CSS cascade issues (position overrides, visibility/opacity toggling) that can render slides blank. Save viewport-base.css for the final full presentation only.

Instead, generate 3 style previews using a **simple centered stage**:

- Fixed-size stage div (960x540 pixels) centered in the viewport body
- Slides use `display:none` / `display:flex` for switching (not visibility/opacity)
- No fixed 16:9 scaling JavaScript
- Use the `#dts` dot-navigation pattern (proven to work reliably in Safari)

**Show at least 5 slides per style**, not just the title slide. The user needs to see how different content types render:

1. Title slide (Big Idea + brand)
2. Section divider or business context slide
3. Human Insight / quote slide
4. Channel architecture or grid/comparison slide
5. Big Idea closing slide

The default mood for previews is **Impressed/Confident** -- use bold, premium aesthetics. Choose 3 distinct styles:

- 1 safe preset from STYLE_PRESETS.md matching the mood
- 1 bold template from the template pack
- 1 wildcard custom design

Open all 3 in Safari and ask the user one bundled question:

> _"Three style previews (5 slides each) are open in Safari. Use arrow keys to navigate. Pick one, or tell me if you want a different mood (calm, energetic, premium, etc.)"_

After the user picks a style, that choice informs the full presentation in Phase 2.2.

### Phase 2.2: Generation

Generate the full HTML presentation using the user's chosen style and the saved plan content.

**CRITICAL — Do NOT use viewport-base.css for the final presentation.** The visibility/opacity cascade + position:absolute approach in viewport-base.css causes blank slides and broken navigation in Safari. Use the proven approach instead:

- **Stage**: centered fixed-size div (960x540px), no JavaScript scaling, no deck-stage transform
- **Slide switching**: `display:none` / `display:flex` — NOT `visibility/opacity/pointer-events`
- **Slide content**: each slide wraps content in a `.page` div; background/glow/vignette are child elements
- **Navigation**: JavaScript `addEventListener('keydown')` for arrow keys — NOT inline `onclick`

**Required features in the final presentation**:

1. **Thumbnail sidebar (left)**: 200-240px wide, each item shows a cloned preview of the slide (170x96px) generated via JavaScript on load. Clone strips background elements (`.bg`, `.vg`, `.gl`), sets the clone to `position:absolute;width:960px;height:540px;transform:scale(0.177)`, and appends to the preview container. Click listeners use `addEventListener`, not inline onclick. Current slide auto-scrolls into view.

2. **Edit mode**: Press E to toggle. When active, all `.page` divs get `contenteditable=true` and body gets `.edit-mode` class for cursor styling. Press E again to remove contenteditable. Ctrl+S saves the stage HTML to localStorage with a **versioned key** (`deck_slides_v{N}`). Do NOT auto-restore on page load — that causes stale data from old versions to corrupt new slides.

3. **Full 13-17 slide content** covering all IMC sections: Title, Executive Summary, Business Context, Core Challenge, Campaign Objective, Human Insight, Brand Role, Big Idea, Message Architecture, Channel Architecture, Rollout Plan, Key Risks, Closing.

4. **Text contrast**: Card body text (`cd p`, `.mst`, `.pt p`, `.bd`) must use `#d0c8be` or brighter. Do NOT use `#a09888` or darker — it becomes unreadable at small sizes on dark backgrounds.

5. **localStorage isolation**: Use a **version-specific key** (e.g., `dd_slides_v5`) that changes with each major output to prevent cross-version data corruption. Never auto-load saved data on initialization.

Map IMC sections to slide types using [references/slide-structure-mapping.md](references/slide-structure-mapping.md).

Density setting: **Low density / speaker-led** for the narrative sections (Context, Challenge, Insight, Big Idea); **High density / reading-first** for reference sections (Channel Architecture, Measurement Framework, Risks).

### Phase 2.3: Delivery

Open the HTML file. Before declaring delivery complete, run this **verification checklist**:

- [ ] All slides render full content (not just titles). Check especially grid/card pages and long text slides.
- [ ] Arrow key navigation works: → ↓ advance, ← ↑ go back.
- [ ] Sidebar thumbnail navigation works: clicking a thumbnail jumps to that slide.
- [ ] Edit mode: press E, text becomes editable, press E again exits.
- [ ] Ctrl+S saves edits (check the confirmation message appears).
- [ ] Text contrast is adequate on ALL slides. Cards, body text, and metadata must be `#c0` or brighter on dark backgrounds.
- [ ] On mobile/tablet viewports (if checked), the 960x540 stage may overflow. This is acceptable for a review deck.

If any items fail, fix and re-verify. Then offer revisions or proceed to Phase 3.

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

**Notes system (add to each presentation)**:

The slide deck must include a per-slide notes system accessible via **N key**. Implementation:

1. **Global slide index**: Track the current slide with `window._curSlide = idx;` added inside the `goTo` function (the REAL `goTo`, not `window.goTo` — the navigation code uses the local `goTo` inside an IIFE, so `window.goTo` wrapping does NOT work). Read the current slide in `nOpen()` via `window._curSlide`.

2. **Notes panel**: Fixed-position panel (bottom-right, 280px wide) with textarea. On N keypress, read `_nD[window._curSlide]` to load notes for that slide. Save on input to `_nD[window._curSlide]` and persist to localStorage via key `ddn4`.

3. **Auto-close on navigation**: In the `goTo` function, add `document.getElementById('np').classList.remove('show')` before the page transition to close the notes panel when navigating away.

4. **Auto-show on return**: After the page transition in `goTo`, check if `window._nD[idx]` has content. If so, auto-open the notes panel and show the saved notes. This prevents the user from missing their notes when flipping back.

5. **Versioned localStorage clearing**: On page load, check `localStorage.getItem('dd_ver')`. If it doesn't match the current version string, clear `ddn4` and update `dd_ver`. This must run BEFORE the notes data is loaded.

6. **Sidebar note indicators**: Add a small dot indicator (`.nd`) to each sidebar item. On note save/load, call `updateDots()` to toggle `.show` class on dots that have non-empty notes data, giving the user a visual overview of which slides have notes.

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

---

## Practical Notes (from testing)

These are hard-won lessons from running the pipeline end-to-end. Update them as new issues surface.

### Preview HTML must be simple

viewport-base.css uses `position: absolute; inset: 0; visibility: hidden;` for slide stacking. Combined with extra CSS overrides (like `position: relative; display: flex;`), slides can render completely blank. For previews in Phase 2.1, use a minimal HTML structure instead:

- Centered stage, fixed size, no scaling JS
- `display:none` / `display:flex` for slide switching
- All styling inline in one CSS block

### Big Idea iterations need session tracking

During Layer 2 (Big Idea Workshop), save each round's version so the user can compare. A simple approach: save each iteration to `/tmp/deck-creation-bigidea-round-{n}.md` and mention the file when presenting.

### The user confirms, not you

Never ask "is this good enough?" — the user decides when to advance. For Big Idea, explicitly ask: "Do you want to lock this, try another round, or explore alternatives?"

### Phase 1 presentations

When presenting the plan in Layer 1 or Layer 3, always:
1. Generate an HTML version and open in Safari for comfortable reading
2. Save the Markdown to `/tmp/` and also copy to workspace for easy editing
3. Give a structured review BEFORE asking for confirmation

### goTo scope issue (critical for notes tracking)

The v5 slide navigation code uses an IIFE (immediately-invoked function expression). The `goTo` function is local inside the IIFE, and only exported as `window.goTo = goTo` for debugging. This means wrapping `window.goTo` to track the current slide has NO effect — the actual navigation always uses the internal `goTo`.

**Fix**: Insert `window._curSlide = idx;` directly into the real `goTo` function via text replacement before the function body. Use `window._curSlide` (not a plain `_curSlide` variable, which would be scoped to the IIFE and inaccessible from the notes code).

### Notes system: auto-close and auto-show

The notes panel should:
- **Close automatically** when navigating to a new slide (prevent misleading persistence)
- **Re-open automatically** when returning to a slide that has saved notes (prevent missed notes)
- Each slide has **independent notes** keyed by `_nD[slideIndex]`
- The notes panel is toggled with **N key** (not spacebar/enter conflicts)

Implementation: In the `goTo` function, add close logic before the page transition (`document.getElementById('np').classList.remove('show')`) and auto-show logic after (`if (window._nD && window._nD[idx]) { /* open panel, set textarea value */ }`).

### Versioned localStorage and note clearing

Each new version of the deck must clear old notes to avoid cross-version contamination:
- Use a version key: `localStorage.setItem('dd_ver', 'v20')`
- On page load, check `dd_ver`. If it doesn't match, clear `ddn4` and update `dd_ver`
- The version check must run **before** the notes data is loaded
- Use a separate `<script>` block before `var _nD = {};` to ensure ordering

### Image sourcing and selection

**Source**: Use Unsplash direct HTTPS URLs (`images.unsplash.com`) — free for commercial use, reliable, no licensing concerns for pitch decks.

**Selection rules**:
- **Car interior slides** (Title, Closing): Must show NO people. No jeans, no hands, no silhouettes. A luxury car dashboard or rear seat with warm ambient lighting is ideal.
- **Human Insight slides**: Avoid full-body shots or visible clothing. Use close-up objects related to relaxation (candle, aromatherapy, texture, steam) instead of people.
- **Business Context slides**: Use premium cityscapes. Avoid images that look like low-income areas. Shanghai/Hong Kong skyline at night works well.
- **Big Idea slides**: Abstract light/door imagery. Avoid literal doors.
- All images must have a 55% black overlay (`.img-overlay`) to ensure text readability on dark backgrounds.

### Text contrast on dark slides

Body text and card content on dark backgrounds must use `#c0`-`#d0` brightness range (`#c0b8a8` / `#d0c8be`). The default `#a09888` is too dim at small (12-14px) sizes and causes readability complaints.
---
### Phase 3 ppt-master: Image Handling

ppt-master's native DrawingML converter cannot embed external URLs. Before finalize_svg.py, all image hrefs in SVGs must point to local files in images/.

Workflow:
1. Download images to images/ during Setup phase.
2. Use relative paths: href=../images/01_title.jpg in SVG image tags.
3. After finalize_svg.py, verify it reports N image(s) aligned + embedded.

### Phase 3 ppt-master: Speaker Notes

total_md_split.py requires notes/total.md to exist. Write speaker notes as level-1 headings matching SVG filenames.

### Phase 3 ppt-master: Python Runtime

Use the bundled Python (not system python3). Install python-pptx via pip with escalated permissions.

```bash
BUNDLED_PYTHON="/Users/tina/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3"
```

---

## Slide Structure Mapping

Detailed IMC-section-to-slide-type mapping is in [references/slide-structure-mapping.md](references/slide-structure-mapping.md). Refer to it during Phase 2.2 generation to decide slide layout for each content section.

## Related Skills

| Skill | Location |
|-------|----------|
| imc-4a-global | `~/.codex/skills/imc-4a-global/` |
| frontend-slides | `~/.codex/skills/frontend-slides/` |
| ppt-master | `~/.codex/skills/ppt-master/` |

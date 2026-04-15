---
name: graphics-compiler
description: >
  Generate PPT/Word-ready graphics using a deterministic, compiler-grade pipeline.
  Use this skill whenever the user asks to create, draw, design, or visualize
  any graphic — including process flows, timelines, dashboards, matrices, KPI tiles,
  architecture diagrams, hypothesis trees, value driver trees, maturity models,
  stakeholder maps, journey maps, gap analyses, or any chart or diagram intended
  for use in PowerPoint, Word, Google Slides, Canva, or any other tool.
  Also trigger for casual requests like "make a quick flow for this", "sketch out
  a 2x2", "I need something I can paste into my deck", or "visualize this data".
  Output is SVG (final/Office-ready), Mermaid (draft/iteration), Python+PNG
  (data charts), or PPT Shape Spec (no-tools fallback). Always trigger this skill
  — never attempt graphics ad-hoc without it.
---

# Graphics Compiler

Generates copy-paste-ready graphics via a strict 3-layer pipeline:

    User input → [Phase 1] GraphicSpec → [Phase 2] LayoutPlan → [Phase 3] Artifact

**Non-negotiable rules:**
- Single-pass execution with internal phases
- No pixel values in Phase 1 or 2
- Renderer (Phase 3) makes zero design or layout decisions
- Never expose GraphicSpec or LayoutPlan unless user explicitly asks to see them
- If `variants = three_pack`, output all three consistently

For brand profiles, graphic type assemblies, and renderer templates, read the
reference files in `references/` as needed (see index below).

---

## Phase 1 — Intent Parser

Convert user input → `GraphicSpec v1.1`. No geometry, colors, fonts, or renderer syntax.

**Detect:**
- Verbs: flow, compare, track, show, map, break down, visualize
- Nouns: timeline, dashboard, matrix, chart, diagram, process, architecture

**Set execution flags:**
- `mode`: "draft" (Mermaid, iteration-first) | "final" (SVG/PNG, artifact-first). Default: final
- `client_safe`: true removes internal names via `redaction_map`. Default: true
- `confidence`: if < 0.7, populate `ranked_alternatives`

**GraphicSpec v1.1 schema:**
```
meta:     { mode, client_safe, confidence }
intent:   { primary_type, ranked_alternatives?, verbs_detected?, nouns_detected? }
content:  { title, subtitle?, entities: { items[], groups?, relationships? },
            metrics?, notes?, redaction_map? }
style:    { tone, brand: { profile, override_tokens? }, emphasis? }
constraints: { max_items, max_items_per_group, max_words_per_label,
               accessibility: { high_contrast, screen_reader_safe } }
output:   { target, preferred_format, canvas_hint, variants }
```

**Primary types (13):** process | timeline | matrix_2x2 | kpi_tiles | architecture |
maturity_model | stakeholder_map | hypothesis_tree | value_driver_tree |
dashboard | journey_map | gap_analysis | unknown

**unknown fallback:** render as process + ask:
> "Rendered as a process flow — did you mean [top alternative] or [second alternative]?"

---

## Phase 2 — Layout Engine

Convert `GraphicSpec` → `LayoutPlan`. Grid-based logic only. No rendering syntax.

### Token cascade (resolve in this order)
```
Global defaults → Named brand profile → Per-request override_tokens
```
Token vocabulary (closed set — 12 tokens):
`--accent` `--neutral` `--success` `--warning` `--danger`
`--bg` `--text` `--muted` `--font` `--radius` `--stroke` `--mode`

Status mapping: `good → --success` | `warning → --warning` | `risk → --danger`

Logical scales (Layer 2 only):
- `--radius`: small=4px, medium=8px, large=16px
- `--stroke`: thin=0.5px, medium=1px, thick=2px

→ For full brand profile token values, read `references/brand-profiles.md`

### Layout mode by graphic type
| Type | Mode | Flow |
|---|---|---|
| process, timeline, journey_map | linear | L→R |
| kpi_tiles, matrix_2x2, maturity_model, stakeholder_map | grid | varies |
| hypothesis_tree, value_driver_tree | tree | T→D |
| architecture, gap_analysis, dashboard | layered | T→B |
| unknown | linear (fallback) | L→R |

**grid mode:** remainder items on last row are centered, not left-aligned.
**dashboard:** uses `layered` at top level; sections internally use `grid` for KPI tiles.

### Grid system
- 12-column logical grid
- Placement: `{ row, col_start, col_span, row_span }` — no absolute x/y
- `items_per_row = min(max_items_per_group, items.length)`
- `col_span = floor(12 / items_per_row)`

### Z-order (strict)
`Container` → `Divider` → `Node` → `Metric` → `Badge` → `Label` → `Arrow`

### Emphasis rules
- Root grid: `emphasis=true` → `col_span+1` if space allows
- Inside Container: token highlight only, no size change

### Collision handling (in order)
1. Increase row_span
2. Push subsequent rows down
3. Reduce items_per_row
4. Visual overflow hint (never overlap)

### Native primitives (7 — renderer knows only these)
`Node` `Arrow` `Label` `Badge` `Divider` `Container` `Metric`

Composed primitives (expand before rendering):
- `KpiTile` → Node + Metric + Label + Badge?
- `Swimlane` → Container + Label + Node[] + Divider
- `CalloutCard` → Node + Label + Arrow
- `StatusStrip` → Badge[] + Divider
- `SectionHeader` → Label + Divider

→ For graphic type assemblies (which primitives each type uses), read `references/graphic-types.md`

---

## Phase 3 — Renderer

Convert `LayoutPlan` → artifact. **No layout or style decisions.**

### Dispatch
| Condition | Format |
|---|---|
| Box/arrow layouts, `mode=final` | SVG |
| Dense graphs, `mode=draft` | Mermaid |
| Numeric/data charts | Python + PNG |
| User says "no tools" / "recreate in PPT" | Shape Spec |

### SVG (primary)
- Elements: rect, line, text only (Convert-to-Shape friendly)
- Canvas: white (`--mode=light`) or near-black (`--mode=dark`)
- Stroke: use `--stroke` resolved value
- Radius: use `--radius` resolved value
- Apply `redaction_map` text substitution before output
- Insert: PPT → Insert → Pictures | Word → Insert → Pictures

### Mermaid (draft)
- Nodes + edges only; include 3-line render instructions
- Render via: Mermaid Live Editor / Microsoft Loop / VS Code extension

### Python + PNG
- matplotlib, 300 DPI, white background, `bbox_inches='tight'`
- Install: `pip install matplotlib pandas --break-system-packages`
- Save to `/mnt/user-data/outputs/`

### Shape Spec (fallback)
- Per-shape: `{ type, x, y, w, h, fill_token, text }`
- Follow with 6-step "build in PPT" instructions
- Coordinates derived from grid → canvas pixel conversion

→ For renderer code templates, read `references/renderer-templates.md`

---

## Example prompts (trigger patterns)

```
"Create a 5-step process: Discover → Profile → Validate → Improve → Report"
→ type=process, mode=final, format=svg

"Draft a layered architecture: Data → Ingestion → Processing → Insights → Consumption"
→ type=architecture, mode=draft, format=mermaid

"Build KPI tiles: Coverage 87%, Adoption 62%, Pending 14, SLA breaches 3"
→ type=kpi_tiles, mode=final, format=svg, profile=stoplight

"Make a 2×2 impact vs effort with these 6 initiatives: [list]"
→ type=matrix_2x2, mode=final, format=svg

"Stakeholder map for this project — client_safe please"
→ type=stakeholder_map, client_safe=true, redaction_map populated

"Quick sketch of our hypothesis tree — draft is fine"
→ type=hypothesis_tree, mode=draft, format=mermaid
```

---

## Reference files index

| File | When to read |
|---|---|
| `references/brand-profiles.md` | Resolving token cascade for any named profile |
| `references/graphic-types.md` | Primitive assembly for each of the 13 types |
| `references/renderer-templates.md` | SVG, Mermaid, Python, Shape Spec code templates |

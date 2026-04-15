# Graphic Types — Primitive Assembly

Read this file when Phase 2 needs to expand a graphic type into native primitives.

Rule: Every type assembles from the 7 native primitives only.
Composed primitives (KpiTile, Swimlane, etc.) must be expanded before LayoutPlan is finalized.

Native primitives: Node · Arrow · Label · Badge · Divider · Container · Metric

---

## 1. Process
*Workflows, step-by-step flows*
- Node × N (steps)
- Arrow (sequence, L→R)
- Container? (optional phase grouping)
- Profile fit: Corporate, Stoplight

## 2. Timeline
*Milestones, roadmaps, delivery phases*
- Container (horizontal band / baseline)
- Node (milestones, equally spaced)
- Divider (center baseline)
- Badge? (dates, phase labels)
- Profile fit: Corporate, Earthy

## 3. Matrix 2×2
*Impact vs effort, risk vs reward, any 2-axis quadrant*
- Container × 4 (quadrant cells)
- Divider (x-axis + y-axis)
- Label (axis titles, quadrant names)
- Node (items placed in cells by score)
- Profile fit: Corporate, Client-safe

## 4. KPI Tiles
*Dashboards, scorecards, metric overviews*
- KpiTile (composed) → expands to: Node + Metric + Label + Badge?
- Grid layout, max 4 per row
- Profile fit: Stoplight, Corporate

## 5. Architecture
*System layers, platform zones, component maps*
- Container (layers / zones, stacked)
- Node (components within layers)
- Arrow (data flow / control flow between nodes)
- Label (layer titles, top-left of each Container)
- Profile fit: Tech, Corporate

## 6. Maturity Model
*Capability assessment, phased growth (Level 1–5)*
- Container (grid cells, 5 columns × N rows)
- Label (level headers: L1–L5, capability names)
- Node (capability items within cells)
- Badge? (current status per cell)
- Flow: Bottom→Top (L1 at bottom)
- Profile fit: Stoplight, Corporate

## 7. Stakeholder Map
*Influence vs interest, power vs engagement*
- Matrix_2x2 as base (composes from its primitives)
- Node (stakeholders, positioned by power/interest score)
- Badge? (role label, engagement level)
- Profile fit: Corporate, Client-safe

## 8. Hypothesis Tree
*Consulting MECE problem decomposition*
- Node (hypotheses, root → branches → leaves)
- Arrow (parent → child, top-down)
- Container? (branch grouping)
- Profile fit: Corporate, Client-safe

## 9. Value Driver Tree
*Financial/operational drivers, metric decomposition*
- Node (drivers at each level)
- Arrow (influence relationships, top-down)
- Metric (value contribution at each node)
- Badge? (impact rating)
- Profile fit: Corporate, Tech

## 10. Dashboard
*Mixed executive view — orchestrates multiple types*
- SectionHeader (expands to: Label + Divider) per section
- KpiTile (expands to: Node + Metric + Label + Badge?) for metric sections
- StatusStrip (expands to: Badge[] + Divider) for governance sections
- Node (insight / narrative callouts)
- Divider (section separators)
- Layout: layered at top level; grid within KPI sections
- Profile fit: Stoplight, Corporate

## 11. Journey Map
*Customer/user journey across stages and touchpoints*
- Swimlane (expands to: Container + Label + Node[] + Divider) per persona/row
- Node (touchpoints)
- Arrow (progression, L→R)
- Metric? (NPS, pain score per stage)
- Profile fit: Corporate, Earthy

## 12. Gap Analysis
*Current state → gap → future state (3-zone)*
- Container × 3 (current | gap | future)
- Node[] (items per zone)
- Arrow (direction of change, L→R)
- Label (zone titles: "Current State", "Gap", "Future State")
- Badge? (gap severity)
- Profile fit: Corporate, Stoplight

## 13. unknown (fallback)
- Render as Process assembly
- Surface ranked_alternatives as one-line clarifying prompt

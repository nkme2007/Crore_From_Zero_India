# graphics-compiler

A Claude skill that generates copy-paste-ready graphics via a deterministic, compiler-grade pipeline.

## What it does

Converts plain-language requests into presentation-ready graphics — SVG, Mermaid diagrams, Python charts, or PowerPoint shape specs — without any ad-hoc design decisions.

**Trigger it whenever a user asks to:**
- Create a process flow, timeline, or architecture diagram
- Build KPI tiles, dashboards, or maturity models
- Draw a 2×2 matrix, stakeholder map, or hypothesis tree
- Visualize data for use in PowerPoint, Word, Google Slides, or Canva

## How it works

Every graphic goes through a strict 3-phase pipeline:

```
User input → [Phase 1] GraphicSpec → [Phase 2] LayoutPlan → [Phase 3] Artifact
```

| Phase | Role |
|---|---|
| **Phase 1 — Intent Parser** | Converts user input into a structured `GraphicSpec`. No geometry or colors. |
| **Phase 2 — Layout Engine** | Converts `GraphicSpec` into a `LayoutPlan` using a 12-column logical grid. No rendering syntax. |
| **Phase 3 — Renderer** | Converts `LayoutPlan` into the final artifact. Zero layout or style decisions. |

## Output formats

| Format | When used |
|---|---|
| **SVG** | Box/arrow layouts, `mode=final` (default) |
| **Mermaid** | Dense graphs, `mode=draft` |
| **Python + PNG** | Numeric/data charts |
| **Shape Spec** | No-tools fallback for manual PPT recreation |

## Supported graphic types (13)

`process` · `timeline` · `matrix_2x2` · `kpi_tiles` · `architecture` · `maturity_model` · `stakeholder_map` · `hypothesis_tree` · `value_driver_tree` · `dashboard` · `journey_map` · `gap_analysis` · `unknown`

## Example prompts

```
"Create a 5-step process: Discover → Profile → Validate → Improve → Report"
"Build KPI tiles: Coverage 87%, Adoption 62%, Pending 14, SLA breaches 3"
"Make a 2×2 impact vs effort with these 6 initiatives: [list]"
"Draft a layered architecture: Data → Ingestion → Processing → Insights → Consumption"
"Quick sketch of our hypothesis tree — draft is fine"
```

## Repo structure

```
graphics-compiler/
├── SKILL.md                          # Main skill definition
└── references/
    ├── brand-profiles.md             # Token values for named brand profiles
    ├── graphic-types.md              # Primitive assembly for each graphic type
    └── renderer-templates.md         # SVG, Mermaid, Python & Shape Spec templates
```

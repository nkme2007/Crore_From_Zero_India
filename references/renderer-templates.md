# Renderer Templates

Read this file during Phase 3 for format-specific output patterns.
Renderer receives LayoutPlan (resolved tokens + grid positions + native primitives).
Renderer must not alter layout or tokens.

---

## SVG Template (primary, Office-ready)

```svg
<svg xmlns="http://www.w3.org/2000/svg"
     width="1600" height="900"
     viewBox="0 0 1600 900">

  <!-- Background -->
  <rect width="1600" height="900" fill="{{--bg}}"/>

  <!-- Title -->
  <text x="48" y="52"
        font-family="{{--font}}" font-size="28" font-weight="700"
        fill="{{--text}}">{{title}}</text>

  <!-- Node example -->
  <rect x="{{x}}" y="{{y}}" width="{{w}}" height="{{h}}"
        rx="{{--radius}}"
        fill="{{--bg}}" stroke="{{--neutral}}" stroke-width="{{--stroke}}"/>
  <text x="{{cx}}" y="{{cy}}"
        font-family="{{--font}}" font-size="18"
        text-anchor="middle" dominant-baseline="central"
        fill="{{--text}}">{{label}}</text>

  <!-- Arrow example -->
  <defs>
    <marker id="arrowhead" markerWidth="8" markerHeight="6"
            refX="8" refY="3" orient="auto">
      <polygon points="0 0, 8 3, 0 6" fill="{{--neutral}}"/>
    </marker>
  </defs>
  <line x1="{{x1}}" y1="{{y1}}" x2="{{x2}}" y2="{{y2}}"
        stroke="{{--neutral}}" stroke-width="{{--stroke}}"
        marker-end="url(#arrowhead)"/>

</svg>
```

**SVG rules:**
- Elements: rect, line, text, polygon only (Convert-to-Shape friendly)
- No gradients, filters, or blur
- White background for light mode, #0F172A for dark mode
- PPT insert: Insert → Pictures → This Device → select .svg
- PPT edit: right-click → Convert to Shape (Office 365)

**Canvas sizes:**
- 16:9 slide: 1600 × 900
- A4 doc: 1240 × 1754
- 1:1 icon: 800 × 800
- 3:1 timeline: 2400 × 800

---

## Mermaid Template (draft)

```
graph LR
    A[Step 1] --> B[Step 2] --> C[Step 3]
```

For architecture/layered:
```
graph TB
    subgraph Layer1["Data Sources"]
        A[System A] 
        B[System B]
    end
    subgraph Layer2["Processing"]
        C[Engine]
    end
    Layer1 --> Layer2
```

**Render steps (always include these 3 lines):**
1. Paste code at mermaid.live or in Microsoft Loop
2. Export as SVG or PNG
3. Insert into PPT/Word via Insert → Pictures

---

## Python + PNG Template (data charts)

```python
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt
import numpy as np

# Token values (resolved from brand profile)
ACCENT  = "{{--accent-hex}}"
NEUTRAL = "{{--neutral-hex}}"
BG      = "white"
TEXT    = "{{--text-hex}}"
MUTED   = "{{--muted-hex}}"

plt.rcParams.update({
    'font.family': 'DejaVu Sans',
    'font.size': 14,
    'axes.spines.top': False,
    'axes.spines.right': False,
    'axes.grid': True,
    'grid.alpha': 0.25,
    'figure.facecolor': BG,
    'axes.facecolor': BG,
})

fig, ax = plt.subplots(figsize=(14, 8))

# --- chart code here ---

ax.set_title("{{title}}", fontsize=20, fontweight='bold',
             color=TEXT, pad=16)

plt.tight_layout()
plt.savefig('/mnt/user-data/outputs/chart.png',
            dpi=300, bbox_inches='tight',
            facecolor=BG, transparent=False)
plt.close()
print("Saved.")
```

**Install:** `pip install matplotlib pandas numpy --break-system-packages`

---

## Shape Spec Template (no-tools fallback)

Output this format when user says "no tools", "recreate manually", or "build in PPT":

```
SHAPE SPEC — {{title}}
Slide size: 16:9 (33.87cm × 19.05cm)
Font: Calibri
Colors: Primary={{--accent}} Neutral={{--neutral}} Text={{--text}}

SHAPES:
1. Rectangle | x:1cm  y:1cm  w:6cm  h:2cm | fill:{{--accent}}  text:"{{label}}" | font:18pt white bold
2. Arrow     | from:(7cm,2cm) to:(9cm,2cm) | color:{{--neutral}} width:2pt
3. Rectangle | x:9cm  y:1cm  w:6cm  h:2cm | fill:{{--bg}}  stroke:{{--neutral}} text:"{{label}}" | font:18pt

BUILD STEPS:
1. Open PPT, set slide to 16:9
2. Insert → Shapes → Rectangle for each box above
3. Right-click each shape → Format Shape → apply fill/stroke colors
4. Insert → Shapes → Lines → Arrow for each connector
5. Add text boxes with labels per spec
6. Group all shapes: Ctrl+A → Group
```

# Brand Profiles — Token Values

Read this file when resolving `GraphicSpec.style.brand.profile` in Phase 2.

Resolution order: Global Defaults → Named Profile → Per-request override_tokens

---

## Global Defaults

```
--accent:  blue
--neutral: gray
--success: green
--warning: amber
--danger:  red
--bg:      white
--text:    near-black
--muted:   cool-gray
--font:    office-sans      (Calibri / Arial)
--radius:  medium           (8px)
--stroke:  thin             (0.5px)
--mode:    light
```

---

## Corporate
*Use for: Cognizant work, executive presentations, client deliverables*

```
--accent:  brand-blue       (#2563EB)
--neutral: midnight-blue    (#0F172A)
--text:    midnight-blue    (#0F172A)
--muted:   cool-gray        (#64748B)
--mode:    light
```
Inherits all other global defaults.

---

## Earthy
*Use for: Vaaraahi Realty, real estate, sustainability, warm brands*

```
--accent:  ochre            (#B45309)
--neutral: slate            (#475569)
--success: olive            (#65A30D)
--warning: sand             (#D97706)
--danger:  brick            (#B91C1C)
--bg:      off-white        (#FEFCE8)
--text:    charcoal         (#1C1917)
--muted:   warm-gray        (#78716C)
--radius:  large            (16px)
--mode:    light
```

---

## Tech
*Use for: architecture diagrams, engineering, product/platform work*

```
--accent:  electric-blue    (#3B82F6)
--neutral: graphite         (#374151)
--bg:      near-black       (#0F172A)
--text:    white            (#F8FAFC)
--muted:   steel            (#94A3B8)
--radius:  small            (4px)
--mode:    dark
```
Note: --mode=dark means renderer uses dark canvas logic for contrast.

---

## Stoplight
*Use for: ops dashboards, governance tracking, dental claims, RAG status*

```
--accent:  blue             (#2563EB)
--success: green            (#16A34A)
--warning: amber            (#D97706)
--danger:  red              (#DC2626)
--neutral: gray             (#6B7280)
--text:    black            (#111827)
--muted:   light-gray       (#D1D5DB)
--mode:    light
```
Inherits --bg=white, --font=office-sans, --radius=medium, --stroke=thin.

---

## Client-safe
*Auto-applied when meta.client_safe=true AND no explicit profile requested*
*Use for: external client output, redacted deliverables*

```
--accent:  neutral-blue     (#3B82F6)
--success: muted-green      (#4ADE80 at 70% saturation)
--warning: muted-amber      (#FCD34D at 70% saturation)
--danger:  muted-red        (#FCA5A5 at 70% saturation)
--text:    dark-gray        (#374151)
--muted:   light-gray       (#D1D5DB)
--mode:    light
```
Intentionally avoids distinctive brand signaling.
Always pair with redaction_map population in Phase 1.

---

## Override rules
- `override_tokens` may only reference tokens in the closed vocabulary
- No new token names permitted
- Only one profile active at a time
- `client_safe` profile takes precedence over others when auto-applied

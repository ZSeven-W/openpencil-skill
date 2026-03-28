---
name: openpencil-design
description: Use when designing UI with OpenPencil — creating layouts via op CLI, batch design DSL, or MCP tools. Covers PenNode schema, semantic roles, typography, color, spacing, and common component patterns.
---

# OpenPencil Design

Generate production-quality vector designs by writing PenNode JSON trees. Use the `op` CLI or MCP tools to create, read, update, and delete nodes on the OpenPencil canvas.

## When to Use

- Creating or modifying UI designs in `.op` files
- Using the `op` CLI to script design operations
- Designing via MCP tools (`batch_design`, `insert_node`, `design_skeleton`)
- Need reference for PenNode schema, roles, or layout rules

## Quick Reference — `op` CLI

```bash
op start [--desktop|--web]        # Launch app
op design '<dsl>'                 # Batch design (inline, @file, or stdin)
op insert '<json>'                # Insert node
op update <id> '<json>'           # Update node
op delete <id>                    # Delete node
op move <id> <parent> [index]     # Move node
op copy <id> <parent>             # Deep-copy node
op replace <id> '<json>'          # Replace node
op get [--depth N]                # Get document tree
op export <react|html|vue|...>    # Export code
op page list|add|remove|rename    # Page operations
op vars / op vars:set '<json>'    # Variables
op themes / op themes:set '<json>'# Themes
```

Global flags: `--file <path>`, `--page <id>`, `--pretty`. Inputs: inline string, `@filepath`, or `-` (stdin).

## Batch Design DSL

One operation per line. Bind results with `name=` for later reference.

```
root=I(null, { "type": "frame", "width": 1200, "layout": "vertical" })
nav=I(root, { "type": "frame", "role": "navbar", "height": 72 })
U(nav, { "fill": [{"type": "solid", "color": "#FFFFFF"}] })
card2=C(card1, grid, { "name": "Card 2" })
M(sidebar, main, 0)
D(old_section)
R(old_btn, { "type": "rectangle", "role": "button" })
```

| Op | Syntax | Action |
|----|--------|--------|
| `I` | `name=I(parent, { node })` | Insert |
| `U` | `U(ref, { updates })` | Update |
| `C` | `name=C(source, parent, { overrides })` | Copy |
| `R` | `name=R(ref, { node })` | Replace |
| `M` | `M(ref, parent, index?)` | Move |
| `D` | `D(ref)` | Delete |

## PenNode Schema

### Common Properties

```json
{
  "type": "frame|rectangle|text|ellipse|line|polygon|path|image|group",
  "name": "Display Name",
  "role": "semantic-role",
  "x": 0, "y": 0,
  "rotation": 0, "opacity": 1, "visible": true
}
```

### Container Properties (frame, rectangle, group, ellipse)

```json
{
  "width": 400,              // number | "fill_container" | "fit_content"
  "height": 300,
  "layout": "vertical",      // "none" | "vertical" | "horizontal"
  "gap": 16,
  "padding": [16, 24],       // number | [v, h] | [top, right, bottom, left]
  "justifyContent": "center", // "start" | "center" | "end" | "space_between" | "space_around"
  "alignItems": "center",    // "start" | "center" | "end"
  "clipContent": true,
  "cornerRadius": 12,        // number | [tl, tr, br, bl]
  "fill": [{ "type": "solid", "color": "#FFFFFF" }],
  "stroke": { "thickness": 1, "fill": [{ "type": "solid", "color": "#E5E7EB" }] },
  "effects": [{ "type": "shadow", "offsetX": 0, "offsetY": 4, "blur": 12, "spread": 0, "color": "rgba(0,0,0,0.08)" }],
  "children": []
}
```

### Text

```json
{
  "type": "text",
  "content": "Hello",          // string or StyledTextSegment[]
  "fontSize": 16, "fontFamily": "Inter", "fontWeight": 600,
  "textAlign": "center",       // "left" | "center" | "right"
  "textGrowth": "fixed-width", // "auto" | "fixed-width" | "fixed-width-height"
  "lineHeight": 1.5, "letterSpacing": 0,
  "fill": [{ "type": "solid", "color": "#111111" }]
}
```

Rich text: `"content": [{ "text": "Bold ", "fontWeight": "bold" }, { "text": "normal" }]`

### Path (Icons)

```json
{ "type": "path", "name": "HeartIcon", "width": 24, "height": 24,
  "fill": [{ "type": "solid", "color": "#111111" }] }
```

PascalCase + "Icon" suffix. Auto-resolved from Lucide set. Common: `SearchIcon`, `MenuIcon`, `HomeIcon`, `UserIcon`, `SettingsIcon`, `MailIcon`, `HeartIcon`, `StarIcon`, `CheckIcon`, `XIcon`, `ChevronRightIcon`, `ArrowRightIcon`, `ZapIcon`, `ShieldIcon`, `CodeIcon`, `LockIcon`, `SparklesIcon`, `PlayIcon`, `BellIcon`, `EyeIcon`, `DownloadIcon`, `PlusIcon`, `GlobeIcon`, `LayersIcon`.

### Image

```json
{ "type": "image", "src": "https://example.com/photo.jpg", "width": 400, "height": 300 }
```

### Line

```json
{ "type": "line", "x2": 200, "y2": 0,
  "stroke": { "thickness": 1, "fill": [{ "type": "solid", "color": "#E5E7EB" }] } }
```

### Fill Types

```json
{ "type": "solid", "color": "#3B82F6" }
{ "type": "linear_gradient", "angle": 135,
  "stops": [{ "offset": 0, "color": "#6366F1" }, { "offset": 1, "color": "#8B5CF6" }] }
{ "type": "radial_gradient", "cx": 0.5, "cy": 0.5, "radius": 0.5,
  "stops": [{ "offset": 0, "color": "#FFF" }, { "offset": 1, "color": "#000" }] }
```

### Design Variables

Reference with `$` prefix: `"color": "$primaryColor"`, `"gap": "$spacing"`.

## Semantic Roles

Roles declare intent — the engine applies smart defaults. Always prefer roles over manual styling.

| Category | Roles |
|----------|-------|
| **Layout** | `section`, `row`, `column`, `centered-content`, `divider`, `spacer` |
| **Navigation** | `navbar`, `nav-links`, `nav-link` |
| **Interactive** | `button`, `icon-button`, `badge`, `tag`, `pill`, `input`, `form-input`, `search-bar` |
| **Cards** | `card`, `feature-card`, `stat-card`, `pricing-card`, `image-card` |
| **Content** | `hero`, `feature-grid`, `cta-section`, `footer`, `testimonial`, `stats-section` |
| **Typography** | `heading`, `subheading`, `body-text`, `caption`, `label` |
| **Media** | `avatar`, `icon`, `phone-mockup`, `screenshot-frame` |
| **Table** | `table`, `table-row`, `table-header`, `table-cell` |
| **Form** | `form-group` |

Key defaults:
- `navbar` → height: 56-72, horizontal, space_between, center-aligned
- `button` → padding: [12, 24], cornerRadius: 8, centered
- `card` → vertical, gap: 12, cornerRadius: 12, padding: 24
- `heading` → lineHeight: 1.2, letterSpacing: -0.5
- `body-text` → fill_container, textGrowth: fixed-width, lineHeight: 1.5

## Layout Rules

1. **NEVER set x/y on children inside layout containers** — engine positions them
2. **Siblings must use same width strategy** — all `fill_container` or all fixed
3. **NEVER `fill_container` inside `fit_content` parent** — circular dependency
4. Cards in horizontal row: ALL `width: "fill_container"`, `height: "fill_container"`

### Sizing Decision

| Question | Answer |
|----------|--------|
| Stretch to fill? | `"fill_container"` |
| Shrink to content? | `"fit_content"` |
| Exact size? | number (px) |

### Design Type Sizing

| Type | Width | Height |
|------|-------|--------|
| Landing page | 1200 | 0 (auto) |
| Mobile screen | 375 | 812 |
| Dashboard | 1200 | 0 (auto) |

## Design Principles

### Typography

```
Display:    40-56px  700  letterSpacing: -1.5  lineHeight: 1.1   "Space Grotesk"
Heading:    28-36px  700  letterSpacing: -0.5  lineHeight: 1.2   "Space Grotesk"
Subheading: 20-24px  600  letterSpacing: -0.25 lineHeight: 1.3   "Space Grotesk"
Body:       15-18px  400  letterSpacing: 0     lineHeight: 1.5   "Inter"
Caption:    13-14px  400  letterSpacing: 0     lineHeight: 1.4   "Inter"
```

CJK: use `"Noto Sans SC/JP/KR"`, lineHeight >= 1.3, letterSpacing: 0 always.

### Color

```
Primary text:   #111111       Secondary: #6B7280     Subtle: #9CA3AF
Background:     #FFFFFF       Surface:   #F9FAFB     Border: #E5E7EB
```

Max 2 saturated colors. WCAG AA: 4.5:1 body, 3:1 large. Dark bg: `#0F172A`, not `#000000`.

### Spacing (8px grid)

```
Related:    8-16px     Components: 16-24px
Groups:     24-32px    Sections:   48-80px    Page padding: 80px
```

### Shadows

```json
// Subtle (cards)
{ "type": "shadow", "offsetY": 1, "blur": 3, "color": "rgba(0,0,0,0.05)" }
// Medium (dropdowns)
{ "type": "shadow", "offsetY": 4, "blur": 12, "color": "rgba(0,0,0,0.08)" }
// Elevated (modals)
{ "type": "shadow", "offsetY": 8, "blur": 24, "spread": -4, "color": "rgba(0,0,0,0.12)" }
```

### Copy Rules

Headlines: 2-6 words. Subtitles: max 15 words. Buttons: 1-3 words. No lorem ipsum. No emoji as icons.

## Layered Workflow (MCP)

For complex multi-section pages:

1. **`design_skeleton`** — create section structure, get section IDs + content guidelines
2. **`design_content`** (per section) — populate with children, `postProcess: true`
3. **`design_refine`** — full-tree validation, auto-fixes

## Common Patterns

### Navbar

```json
{ "type": "frame", "role": "navbar", "width": "fill_container", "height": 72,
  "layout": "horizontal", "padding": [0, 80], "justifyContent": "space_between", "alignItems": "center",
  "fill": [{"type": "solid", "color": "#FFFFFF"}],
  "stroke": {"thickness": 1, "fill": [{"type": "solid", "color": "#F3F4F6"}]},
  "children": [
    {"type": "text", "content": "Brand", "fontSize": 20, "fontWeight": 700, "fontFamily": "Space Grotesk"},
    {"type": "frame", "role": "nav-links", "layout": "horizontal", "gap": 32, "width": "fit_content", "height": "fit_content", "children": [
      {"type": "text", "role": "nav-link", "content": "Features", "fontSize": 15},
      {"type": "text", "role": "nav-link", "content": "Pricing", "fontSize": 15}
    ]},
    {"type": "rectangle", "role": "button", "padding": [10, 24], "cornerRadius": 8,
      "fill": [{"type": "solid", "color": "#111111"}],
      "children": [{"type": "text", "content": "Get Started", "fontSize": 14, "fontWeight": 600, "fill": [{"type": "solid", "color": "#FFFFFF"}]}]}
  ]}
```

### Hero

```json
{ "type": "frame", "role": "hero", "width": "fill_container", "height": "fit_content",
  "layout": "vertical", "padding": [100, 80], "gap": 24, "alignItems": "center",
  "children": [
    {"type": "text", "role": "heading", "content": "Build something great",
      "fontSize": 56, "fontWeight": 700, "fontFamily": "Space Grotesk",
      "textAlign": "center", "letterSpacing": -1.5, "lineHeight": 1.1,
      "textGrowth": "fixed-width", "width": 800},
    {"type": "text", "role": "subheading", "content": "The modern platform for teams who ship fast.",
      "fontSize": 18, "textAlign": "center", "lineHeight": 1.6,
      "textGrowth": "fixed-width", "width": 560,
      "fill": [{"type": "solid", "color": "#6B7280"}]},
    {"type": "frame", "layout": "horizontal", "gap": 12, "width": "fit_content", "height": "fit_content", "children": [
      {"type": "rectangle", "role": "button", "padding": [14, 32], "cornerRadius": 10,
        "fill": [{"type": "solid", "color": "#111111"}],
        "children": [{"type": "text", "content": "Start Free", "fontSize": 16, "fontWeight": 600, "fill": [{"type": "solid", "color": "#FFFFFF"}]}]},
      {"type": "rectangle", "role": "button", "padding": [14, 32], "cornerRadius": 10,
        "fill": [{"type": "solid", "color": "#F3F4F6"}],
        "children": [{"type": "text", "content": "View Demo", "fontSize": 16, "fontWeight": 600}]}
    ]}
  ]}
```

### Feature Card (in horizontal grid, ALL cards must use fill_container)

```json
{ "type": "rectangle", "role": "feature-card",
  "width": "fill_container", "height": "fill_container",
  "layout": "vertical", "padding": 28, "gap": 16, "cornerRadius": 16,
  "fill": [{"type": "solid", "color": "#F9FAFB"}],
  "children": [
    {"type": "path", "name": "ZapIcon", "width": 24, "height": 24, "fill": [{"type": "solid", "color": "#111111"}]},
    {"type": "text", "content": "Lightning Fast", "fontSize": 20, "fontWeight": 600},
    {"type": "text", "role": "body-text", "content": "Sub-second builds with smart caching.",
      "fontSize": 15, "lineHeight": 1.6, "fill": [{"type": "solid", "color": "#6B7280"}]}
  ]}
```

### Form Input

```json
{ "type": "frame", "role": "form-group", "layout": "vertical", "gap": 8, "width": "fill_container",
  "children": [
    {"type": "text", "role": "label", "content": "Email", "fontSize": 14, "fontWeight": 500},
    {"type": "rectangle", "role": "form-input", "width": "fill_container", "height": 48,
      "cornerRadius": 8, "layout": "horizontal", "padding": [0, 16], "alignItems": "center",
      "fill": [{"type": "solid", "color": "#FFFFFF"}],
      "stroke": {"thickness": 1, "fill": [{"type": "solid", "color": "#D1D5DB"}]},
      "children": [
        {"type": "path", "name": "MailIcon", "width": 18, "height": 18, "fill": [{"type": "solid", "color": "#9CA3AF"}]},
        {"type": "text", "content": "you@example.com", "fontSize": 15, "fill": [{"type": "solid", "color": "#9CA3AF"}]}
      ]}
  ]}
```

### Footer

```json
{ "type": "frame", "role": "footer", "width": "fill_container", "height": "fit_content",
  "layout": "horizontal", "padding": [48, 80], "gap": 80,
  "fill": [{"type": "solid", "color": "#F9FAFB"}],
  "children": [
    {"type": "frame", "layout": "vertical", "gap": 16, "width": 240, "children": [
      {"type": "text", "content": "Brand", "fontSize": 20, "fontWeight": 700, "fontFamily": "Space Grotesk"},
      {"type": "text", "content": "Building the future of design.", "fontSize": 14, "lineHeight": 1.6, "fill": [{"type": "solid", "color": "#6B7280"}]}
    ]},
    {"type": "frame", "layout": "vertical", "gap": 12, "width": "fit_content", "children": [
      {"type": "text", "content": "Product", "fontSize": 14, "fontWeight": 600},
      {"type": "text", "content": "Features", "fontSize": 14, "fill": [{"type": "solid", "color": "#6B7280"}]},
      {"type": "text", "content": "Pricing", "fontSize": 14, "fill": [{"type": "solid", "color": "#6B7280"}]}
    ]}
  ]}
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Setting x/y inside layout container | Remove x/y — engine auto-positions |
| Cards with different width strategies | All siblings: same sizing (`fill_container`) |
| `fill_container` child in `fit_content` parent | Use fixed width or switch parent to `fill_container` |
| Pure black text `#000000` | Use `#111111` or `#0F172A` |
| Heavy drop shadows | Use subtle `rgba(0,0,0,0.05-0.12)` |
| Emoji as icons | Use path nodes with icon names |
| Lorem ipsum placeholder | Write realistic, concise copy |
| Fixed height on text | Use `textGrowth: "fixed-width"` instead |
| Space Grotesk for CJK | Use `"Noto Sans SC/JP/KR"` |
| Negative letterSpacing on CJK | Always 0 for CJK text |
| Missing `postProcess: true` | Always set for MCP tool calls |

## DSL Full Example — Landing Page

```
root=I(null, {"type":"frame","name":"Landing","width":1200,"height":0,"layout":"vertical","fill":[{"type":"solid","color":"#FFFFFF"}]})

nav=I(root, {"type":"frame","role":"navbar","width":"fill_container","height":72,"layout":"horizontal","padding":[0,80],"justifyContent":"space_between","alignItems":"center"})
I(nav, {"type":"text","content":"Acme","fontSize":20,"fontWeight":700,"fontFamily":"Space Grotesk"})
links=I(nav, {"type":"frame","role":"nav-links","layout":"horizontal","gap":32,"width":"fit_content","height":"fit_content"})
I(links, {"type":"text","role":"nav-link","content":"Features","fontSize":15})
I(links, {"type":"text","role":"nav-link","content":"Pricing","fontSize":15})
I(nav, {"type":"rectangle","role":"button","padding":[10,24],"cornerRadius":8,"fill":[{"type":"solid","color":"#111111"}],"children":[{"type":"text","content":"Get Started","fontSize":14,"fontWeight":600,"fill":[{"type":"solid","color":"#FFFFFF"}]}]})

hero=I(root, {"type":"frame","role":"hero","width":"fill_container","height":"fit_content","layout":"vertical","padding":[100,80],"gap":24,"alignItems":"center"})
I(hero, {"type":"text","role":"heading","content":"Ship faster with Acme","fontSize":56,"fontWeight":700,"fontFamily":"Space Grotesk","textAlign":"center","letterSpacing":-1.5,"lineHeight":1.1,"textGrowth":"fixed-width","width":800})
I(hero, {"type":"text","role":"subheading","content":"The platform that turns ideas into production apps in minutes.","fontSize":18,"textAlign":"center","lineHeight":1.6,"textGrowth":"fixed-width","width":560,"fill":[{"type":"solid","color":"#6B7280"}]})
btns=I(hero, {"type":"frame","layout":"horizontal","gap":12,"width":"fit_content","height":"fit_content"})
I(btns, {"type":"rectangle","role":"button","padding":[14,32],"cornerRadius":10,"fill":[{"type":"solid","color":"#111111"}],"children":[{"type":"text","content":"Start Free","fontSize":16,"fontWeight":600,"fill":[{"type":"solid","color":"#FFFFFF"}]}]})
I(btns, {"type":"rectangle","role":"button","padding":[14,32],"cornerRadius":10,"fill":[{"type":"solid","color":"#F3F4F6"}],"children":[{"type":"text","content":"View Demo","fontSize":16,"fontWeight":600}]})

feat=I(root, {"type":"frame","role":"section","width":"fill_container","height":"fit_content","layout":"vertical","padding":[80,80],"gap":48,"alignItems":"center"})
I(feat, {"type":"text","role":"heading","content":"Everything you need","fontSize":36,"fontWeight":700,"fontFamily":"Space Grotesk","textAlign":"center","letterSpacing":-0.5})
grid=I(feat, {"type":"frame","role":"feature-grid","width":"fill_container","layout":"horizontal","gap":24})
c1=I(grid, {"type":"rectangle","role":"feature-card","width":"fill_container","height":"fill_container","layout":"vertical","padding":28,"gap":16,"cornerRadius":16,"fill":[{"type":"solid","color":"#F9FAFB"}],"children":[{"type":"path","name":"ZapIcon","width":24,"height":24,"fill":[{"type":"solid","color":"#111111"}]},{"type":"text","content":"Lightning Fast","fontSize":20,"fontWeight":600},{"type":"text","role":"body-text","content":"Sub-second builds with smart caching.","fontSize":15,"lineHeight":1.6,"fill":[{"type":"solid","color":"#6B7280"}]}]})
c2=C(c1, grid, {})
U(c2+"/0", {"name":"ShieldIcon"})
U(c2+"/1", {"content":"Enterprise Security"})
U(c2+"/2", {"content":"SOC 2 certified with end-to-end encryption."})
c3=C(c1, grid, {})
U(c3+"/0", {"name":"GitBranchIcon"})
U(c3+"/1", {"content":"Git-Native Workflow"})
U(c3+"/2", {"content":"Preview deploys on every push with instant rollback."})
```

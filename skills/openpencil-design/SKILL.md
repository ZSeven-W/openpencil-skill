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
# App control
op start [--desktop|--web]           # Launch app
op stop                              # Stop running instance
op status                            # Check if running

# Document
op open [file.op]                    # Open file or connect to live canvas
op save <file.op>                    # Save current document
op get [--depth N] [--pretty]        # Get document tree
op selection [--depth N]             # Get current canvas selection
op read-nodes [id...] [--depth N] [--vars]  # Read node subtree(s) with optional variable resolution
op layout [--parent P] [--depth N]   # Snapshot layout tree with computed positions
op find-space [--direction D] [--width N] [--height N]  # Find empty space on canvas

# Node operations
op insert '<json>' [--parent P]     # Insert node (--index N, --post-process)
op update <id> '<json>'              # Update node
op delete <id>                       # Delete node
op move <id> <parent> [index]        # Move node
op copy <id> <parent>                # Deep-copy node
op replace <id> '<json>'             # Replace node

# Batch design
op design '<dsl>'                    # Batch design DSL (inline, @file, or stdin) [--canvas-width N]

# Layered workflow
op design:skeleton '<json>'          # Create section structure
op design:content <id> '<json>'      # Populate section content
op design:refine --root-id <id>      # Validate + auto-fix (resolves icons) [--canvas-width N]

# Import
op import:svg <file.svg> [--parent P]       # Import SVG as editable nodes
op import:figma <file.fig> [--out out.op]   # Convert Figma .fig to .op document

# Pages
op page list                         # List all pages
op page add [--name N]               # Add a new page
op page remove <id>                  # Remove a page
op page rename <id> '<name>'         # Rename a page
op page reorder <id> <index>         # Move page to position
op page duplicate <id>               # Clone page with new IDs

# Variables & Themes
op vars / op vars:set '<json>'       # Variables (--replace to replace all)
op themes / op themes:set '<json>'   # Themes (--replace to replace all)
op theme:save <file.optheme>         # Save current theme as preset file
op theme:load <file.optheme>         # Load a theme preset file
op theme:list <directory>            # List .optheme presets in directory

# Codegen pipeline
op codegen:plan '<json>'             # Submit codegen plan (framework, rootIds, options)
op codegen:submit '<json>'           # Submit a code chunk for a node
op codegen:assemble [--framework F]  # Assemble all submitted chunks into final output
op codegen:clean                     # Clear codegen state
```

Global flags: `--file <path>`, `--page <id>`, `--pretty`. Inputs: inline string, `@filepath`, or `-` (stdin).

## Building Designs — Two Approaches

### Approach 1: `op insert` (Recommended)

The most reliable way to build designs. Use `--parent` to specify the parent node. Capture the returned `nodeId` to reference later. **Always finish with `design:refine`** to resolve icons and validate layout.

```bash
# Create root frame, capture its ID
ROOT=$(op insert '{"type":"frame","name":"Page","width":375,"height":812,"layout":"vertical"}' \
  | python3 -c "import sys,json; print(json.load(sys.stdin)['nodeId'])")

# Insert children using --parent
op insert --parent "$ROOT" '{"type":"text","content":"Hello","fontSize":28,"fontWeight":700}'

# Post-process: resolve icons, validate layout
op design:refine --root-id "$ROOT"
```

### Approach 2: Batch Design DSL

One operation per line. Bind results with `name=` for later reference. Best for simple, flat structures.

> **Limitation:** The DSL parser cannot handle deeply nested JSON (e.g., `children` arrays with nested objects, or multiple levels of array nesting). Keep each `I()` call to a **single level of nesting**. For complex nodes with children, use separate `I()` calls for parent and children, or use `op insert --parent`.

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
| `G` | `name=G(parent, "search", "query")` | Generate image via search |

**DSL safe pattern** — always insert parent and children separately:

```
btn=I(form, {"type":"rectangle","role":"button","width":"fill_container","height":50,"cornerRadius":12,"fill":[{"type":"solid","color":"#111111"}],"layout":"horizontal","justifyContent":"center","alignItems":"center"})
I(btn, {"type":"text","content":"Submit","fontSize":16,"fontWeight":600,"fill":[{"type":"solid","color":"#FFFFFF"}]})
```

## STRICT JSON Rules

When emitting PenNode JSON (via `op insert`, `op design`, `batch_design`, `insert_node`), you MUST produce strictly valid JSON. Common mistakes that break parsing:

- **Every property MUST have both a key and a value**. NEVER emit `": 50` or `: 50` without a key name. This often happens when you truncate/reformat — double-check.
- **Every key MUST be a double-quoted non-empty string.**
- **`fill` is ALWAYS an array**: `"fill": [{"type": "solid", "color": "#hex"}]`. Shorthand like `"fill": "#hex"` works but the array form is the canonical shape.
- **`stroke` is an object with a `fill` array**: `"stroke": {"thickness": 1, "fill": [{"type": "solid", "color": "#hex"}]}`. NEVER `"stroke": {"thickness": 1, "color": "#hex"}` or `"stroke": "#hex"` (parser auto-converts these but the correct shape is preferred).
- **NO trailing commas** before `}` or `]`.
- **NO comments** inside JSON (`//` or `/* */`).
- Use **straight double quotes** `"`, not smart/curly quotes.
- **`content` for text, NOT `text`**: `{"type": "text", "content": "Hello"}`.
- **`iconFontName` for icons, NOT `iconName` or `icon`**: `{"type": "icon_font", "iconFontName": "lock"}`.
- Before finalizing the JSON, mentally verify: every key has a value, every value has a key, all brackets balance.

## PenNode Schema

### Common Properties

```json
{
  "type": "frame|rectangle|text|ellipse|line|polygon|path|image|icon_font|group|ref",
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
  "stroke": { "thickness": 1, "fill": [{ "type": "solid", "color": "#E5E7EB" }], "align": "inside", "dashPattern": [5, 3] },
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

### Icons — Two Options

#### Option A: `icon_font` (RECOMMENDED — renders directly, no post-processing needed)

```json
{ "type": "icon_font", "name": "Lock Icon", "iconFontName": "lock",
  "width": 20, "height": 20,
  "fill": [{ "type": "solid", "color": "#6B7280" }] }
```

**Field is `iconFontName` (NOT `iconName`, NOT `icon`).** Values are lowercase kebab-case Lucide names: `mail`, `lock`, `eye`, `eye-off`, `chrome`, `apple`, `message-circle`, `x`, `arrow-right`, `search`, `heart`, `star`, `check`, `plus`, `bell`, `home`, `user`, `settings`, `chevron-right`, `download`, `globe`, `layers`, `zap`, `shield`, `play`.

Works in ALL contexts: CLI, MCP tools, or direct `.op` files — no `design:refine` required.

#### Option B: `path` (requires post-processing)

```json
{ "type": "path", "name": "HeartIcon", "width": 24, "height": 24,
  "fill": [{ "type": "solid", "color": "#111111" }] }
```

PascalCase + "Icon" suffix. Auto-resolved from Lucide set during post-processing.

> **Path icons need post-processing.** After inserting path nodes, run `op design:refine --root-id <id>` or use `op insert --post-process`. Without this, path icons won't render visually. The standalone MCP server (used by ACP agents) does NOT have hook implementations registered, so path icons will NOT resolve there — **prefer `icon_font` in MCP contexts.**

### Image

```json
{ "type": "image", "src": "https://example.com/photo.jpg", "width": 400, "height": 300,
  "objectFit": "crop", "cornerRadius": 12 }
```

AI image placeholders (resolved by `design:refine`):

```json
{ "type": "image", "width": 400, "height": 300,
  "imagePrompt": "A modern office workspace with natural light",
  "imageSearchQuery": "modern office workspace" }
```

Image adjustments (all -100 to 100): `exposure`, `contrast`, `saturation`, `temperature`, `tint`, `highlights`, `shadows`.

### Polygon

```json
{ "type": "polygon", "polygonCount": 6, "width": 80, "height": 80, "cornerRadius": 4,
  "fill": [{ "type": "solid", "color": "#6366F1" }] }
```

### Icon Font

```json
{ "type": "icon_font", "iconFontName": "lucide:home", "width": 24, "height": 24,
  "fill": [{ "type": "solid", "color": "#111111" }] }
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
{ "type": "image", "url": "https://example.com/texture.jpg", "mode": "fill" }
```

Image fill modes: `fill`, `fit`, `crop`, `tile`, `stretch`. Image fill also supports adjustment filters (`exposure`, `contrast`, `saturation`, etc.).

### Ref Node (Component Instance)

```json
{ "type": "ref", "ref": "reusable-frame-id",
  "descendants": { "child-id": { "content": "Override text" } } }
```

References a `frame` with `reusable: true`. Override specific descendant properties via `descendants`.

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

## Layered Workflow

For complex multi-section pages, use the three-step skeleton → content → refine flow:

| Step | MCP Tool | CLI Equivalent |
|------|----------|----------------|
| 1. Create section structure | `design_skeleton` | `op design:skeleton '<json>'` |
| 2. Populate each section | `design_content` (with `postProcess: true`) | `op design:content <section-id> '<json>'` |
| 3. Validate + auto-fix | `design_refine` | `op design:refine --root-id <id>` |

`design:refine` resolves icon names → SVG paths, fixes layout issues, and validates the tree. **Always run as the final step.**

## Codegen Pipeline

For incremental, framework-aware code generation from the design tree:

| Step | CLI Command | MCP Tool | Description |
|------|------------|----------|-------------|
| 1. Plan | `op codegen:plan '<json>'` | `codegen_plan` | Declare framework, root node IDs, and options |
| 2. Submit | `op codegen:submit '<json>'` | `codegen_submit_chunk` | Submit generated code for individual nodes |
| 3. Assemble | `op codegen:assemble --framework react` | `codegen_assemble` | Combine all chunks into the final output |
| 4. Clean | `op codegen:clean` | `codegen_clean` | Clear server-side codegen state |

The plan JSON shape:
```json
{ "framework": "react", "rootIds": ["frame-1"], "options": { "tailwind": true } }
```

The submit JSON shape:
```json
{ "nodeId": "card-1", "code": "<Card className=\"...\">...</Card>", "imports": ["Card"] }
```

Supported frameworks: `react`, `html`, `vue`, `svelte`, `flutter`, `swiftui`, `compose`, `rn` (React Native), `css`.

## Multi-Page Documents

```bash
op page list                          # List all pages with IDs
op page add --name "Settings"         # Add a new page
op page remove <page-id>              # Remove a page
op page rename <page-id> 'New Name'   # Rename a page
op page reorder <page-id> 2           # Move page to index 2
op page duplicate <page-id>           # Clone page with new IDs
```

Use `--page <id>` on any command to target a specific page. Without it, commands operate on the first page.

## Common Patterns

Patterns below show `op insert --parent` commands. Each pattern is copy-paste ready.

### Navbar

```bash
NAV=$(op insert --parent "$ROOT" '{"type":"frame","role":"navbar","width":"fill_container","height":72,"layout":"horizontal","padding":[0,80],"justifyContent":"space_between","alignItems":"center","fill":[{"type":"solid","color":"#FFFFFF"}],"stroke":{"thickness":1,"fill":[{"type":"solid","color":"#F3F4F6"}]}}' | ID)
op insert --parent "$NAV" '{"type":"text","content":"Brand","fontSize":20,"fontWeight":700,"fontFamily":"Space Grotesk"}'
LINKS=$(op insert --parent "$NAV" '{"type":"frame","role":"nav-links","layout":"horizontal","gap":32,"width":"fit_content","height":"fit_content"}' | ID)
op insert --parent "$LINKS" '{"type":"text","role":"nav-link","content":"Features","fontSize":15}'
op insert --parent "$LINKS" '{"type":"text","role":"nav-link","content":"Pricing","fontSize":15}'
CTA=$(op insert --parent "$NAV" '{"type":"rectangle","role":"button","padding":[10,24],"cornerRadius":8,"fill":[{"type":"solid","color":"#111111"}],"layout":"horizontal","justifyContent":"center","alignItems":"center"}' | ID)
op insert --parent "$CTA" '{"type":"text","content":"Get Started","fontSize":14,"fontWeight":600,"fill":[{"type":"solid","color":"#FFFFFF"}]}'
```

### Hero

```bash
HERO=$(op insert --parent "$ROOT" '{"type":"frame","role":"hero","width":"fill_container","height":"fit_content","layout":"vertical","padding":[100,80],"gap":24,"alignItems":"center"}' | ID)
op insert --parent "$HERO" '{"type":"text","role":"heading","content":"Build something great","fontSize":56,"fontWeight":700,"fontFamily":"Space Grotesk","textAlign":"center","letterSpacing":-1.5,"lineHeight":1.1,"textGrowth":"fixed-width","width":800}'
op insert --parent "$HERO" '{"type":"text","role":"subheading","content":"The modern platform for teams who ship fast.","fontSize":18,"textAlign":"center","lineHeight":1.6,"textGrowth":"fixed-width","width":560,"fill":[{"type":"solid","color":"#6B7280"}]}'
BTNS=$(op insert --parent "$HERO" '{"type":"frame","layout":"horizontal","gap":12,"width":"fit_content","height":"fit_content"}' | ID)
B1=$(op insert --parent "$BTNS" '{"type":"rectangle","role":"button","padding":[14,32],"cornerRadius":10,"fill":[{"type":"solid","color":"#111111"}],"layout":"horizontal","justifyContent":"center","alignItems":"center"}' | ID)
op insert --parent "$B1" '{"type":"text","content":"Start Free","fontSize":16,"fontWeight":600,"fill":[{"type":"solid","color":"#FFFFFF"}]}'
B2=$(op insert --parent "$BTNS" '{"type":"rectangle","role":"button","padding":[14,32],"cornerRadius":10,"fill":[{"type":"solid","color":"#F3F4F6"}],"layout":"horizontal","justifyContent":"center","alignItems":"center"}' | ID)
op insert --parent "$B2" '{"type":"text","content":"View Demo","fontSize":16,"fontWeight":600}'
```

### Feature Card (in horizontal grid, ALL cards must use fill_container)

```bash
CARD=$(op insert --parent "$GRID" '{"type":"rectangle","role":"feature-card","width":"fill_container","height":"fill_container","layout":"vertical","padding":28,"gap":16,"cornerRadius":16,"fill":[{"type":"solid","color":"#F9FAFB"}]}' | ID)
op insert --parent "$CARD" '{"type":"path","name":"ZapIcon","width":24,"height":24,"fill":[{"type":"solid","color":"#111111"}]}'
op insert --parent "$CARD" '{"type":"text","content":"Lightning Fast","fontSize":20,"fontWeight":600}'
op insert --parent "$CARD" '{"type":"text","role":"body-text","content":"Sub-second builds with smart caching.","fontSize":15,"lineHeight":1.6,"fill":[{"type":"solid","color":"#6B7280"}]}'
```

### Form Input

```bash
GRP=$(op insert --parent "$FORM" '{"type":"frame","role":"form-group","layout":"vertical","gap":8,"width":"fill_container"}' | ID)
op insert --parent "$GRP" '{"type":"text","role":"label","content":"Email","fontSize":14,"fontWeight":500}'
INP=$(op insert --parent "$GRP" '{"type":"rectangle","role":"form-input","width":"fill_container","height":48,"cornerRadius":10,"layout":"horizontal","padding":[0,16],"gap":10,"alignItems":"center","fill":[{"type":"solid","color":"#F9FAFB"}],"stroke":{"thickness":1,"fill":[{"type":"solid","color":"#E5E7EB"}]}}' | ID)
op insert --parent "$INP" '{"type":"path","name":"MailIcon","width":18,"height":18,"fill":[{"type":"solid","color":"#9CA3AF"}]}'
op insert --parent "$INP" '{"type":"text","content":"you@example.com","fontSize":15,"fill":[{"type":"solid","color":"#9CA3AF"}]}'
```

### Footer

```bash
FOOTER=$(op insert --parent "$ROOT" '{"type":"frame","role":"footer","width":"fill_container","height":"fit_content","layout":"horizontal","padding":[48,80],"gap":80,"fill":[{"type":"solid","color":"#F9FAFB"}]}' | ID)
COL1=$(op insert --parent "$FOOTER" '{"type":"frame","layout":"vertical","gap":16,"width":240}' | ID)
op insert --parent "$COL1" '{"type":"text","content":"Brand","fontSize":20,"fontWeight":700,"fontFamily":"Space Grotesk"}'
op insert --parent "$COL1" '{"type":"text","content":"Building the future of design.","fontSize":14,"lineHeight":1.6,"fill":[{"type":"solid","color":"#6B7280"}]}'
COL2=$(op insert --parent "$FOOTER" '{"type":"frame","layout":"vertical","gap":12,"width":"fit_content"}' | ID)
op insert --parent "$COL2" '{"type":"text","content":"Product","fontSize":14,"fontWeight":600}'
op insert --parent "$COL2" '{"type":"text","content":"Features","fontSize":14,"fill":[{"type":"solid","color":"#6B7280"}]}'
op insert --parent "$COL2" '{"type":"text","content":"Pricing","fontSize":14,"fill":[{"type":"solid","color":"#6B7280"}]}'
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
| Missing post-process after insert | Run `op design:refine --root-id <id>` after building the tree |
| Icons inserted but not visible | Path nodes need `design:refine` or `--post-process` to resolve SVG |
| Using DSL `I()` with inline `children` | DSL parser fails on nested JSON — insert parent and children separately |
| Missing `postProcess: true` in MCP | Always set for MCP tool calls |

## Full Example — `op insert` Workflow (Recommended)

Build a complete mobile login page using `op insert --parent`. This is the most reliable approach.

```bash
#!/bin/bash
set -e
ID() { python3 -c "import sys,json; print(json.load(sys.stdin)['nodeId'])"; }

# Root frame (mobile)
ROOT=$(op insert '{"type":"frame","name":"Login","width":375,"height":812,"layout":"vertical","fill":[{"type":"solid","color":"#FFFFFF"}]}' | ID)

# Header
TOP=$(op insert --parent "$ROOT" '{"type":"frame","width":"fill_container","height":"fit_content","layout":"vertical","padding":[80,32,40,32],"gap":14,"alignItems":"center"}' | ID)
op insert --parent "$TOP" '{"type":"path","name":"ShieldIcon","width":48,"height":48,"fill":[{"type":"solid","color":"#6366F1"}]}'
op insert --parent "$TOP" '{"type":"text","content":"Welcome Back","fontSize":28,"fontWeight":700,"fontFamily":"Space Grotesk","letterSpacing":-0.5,"textAlign":"center"}'

# Form
FORM=$(op insert --parent "$ROOT" '{"type":"frame","width":"fill_container","height":"fit_content","layout":"vertical","padding":[0,32],"gap":20}' | ID)

# Email input
GRP=$(op insert --parent "$FORM" '{"type":"frame","role":"form-group","layout":"vertical","gap":8,"width":"fill_container"}' | ID)
op insert --parent "$GRP" '{"type":"text","role":"label","content":"Email","fontSize":14,"fontWeight":500}'
INP=$(op insert --parent "$GRP" '{"type":"rectangle","role":"form-input","width":"fill_container","height":48,"cornerRadius":10,"layout":"horizontal","padding":[0,16],"gap":10,"alignItems":"center","fill":[{"type":"solid","color":"#F9FAFB"}],"stroke":{"thickness":1,"fill":[{"type":"solid","color":"#E5E7EB"}]}}' | ID)
op insert --parent "$INP" '{"type":"path","name":"MailIcon","width":18,"height":18,"fill":[{"type":"solid","color":"#9CA3AF"}]}'
op insert --parent "$INP" '{"type":"text","content":"you@example.com","fontSize":15,"fill":[{"type":"solid","color":"#9CA3AF"}]}'

# Login button
BTN=$(op insert --parent "$FORM" '{"type":"rectangle","role":"button","width":"fill_container","height":50,"cornerRadius":12,"fill":[{"type":"solid","color":"#111111"}],"layout":"horizontal","justifyContent":"center","alignItems":"center"}' | ID)
op insert --parent "$BTN" '{"type":"text","content":"Sign In","fontSize":16,"fontWeight":600,"fill":[{"type":"solid","color":"#FFFFFF"}]}'

# IMPORTANT: resolve icons + validate layout
op design:refine --root-id "$ROOT"
```

## DSL Example — Landing Page

DSL is suitable for simpler structures. **Avoid inline `children`** — insert parent and children as separate operations.

```
root=I(null, {"type":"frame","name":"Landing","width":1200,"height":0,"layout":"vertical","fill":[{"type":"solid","color":"#FFFFFF"}]})

nav=I(root, {"type":"frame","role":"navbar","width":"fill_container","height":72,"layout":"horizontal","padding":[0,80],"justifyContent":"space_between","alignItems":"center"})
I(nav, {"type":"text","content":"Acme","fontSize":20,"fontWeight":700,"fontFamily":"Space Grotesk"})
links=I(nav, {"type":"frame","role":"nav-links","layout":"horizontal","gap":32,"width":"fit_content","height":"fit_content"})
I(links, {"type":"text","role":"nav-link","content":"Features","fontSize":15})
I(links, {"type":"text","role":"nav-link","content":"Pricing","fontSize":15})
cta=I(nav, {"type":"rectangle","role":"button","padding":[10,24],"cornerRadius":8,"fill":[{"type":"solid","color":"#111111"}],"layout":"horizontal","justifyContent":"center","alignItems":"center"})
I(cta, {"type":"text","content":"Get Started","fontSize":14,"fontWeight":600,"fill":[{"type":"solid","color":"#FFFFFF"}]})

hero=I(root, {"type":"frame","role":"hero","width":"fill_container","height":"fit_content","layout":"vertical","padding":[100,80],"gap":24,"alignItems":"center"})
I(hero, {"type":"text","role":"heading","content":"Ship faster with Acme","fontSize":56,"fontWeight":700,"fontFamily":"Space Grotesk","textAlign":"center","letterSpacing":-1.5,"lineHeight":1.1,"textGrowth":"fixed-width","width":800})
I(hero, {"type":"text","role":"subheading","content":"Turn ideas into production apps in minutes.","fontSize":18,"textAlign":"center","lineHeight":1.6,"textGrowth":"fixed-width","width":560,"fill":[{"type":"solid","color":"#6B7280"}]})
btns=I(hero, {"type":"frame","layout":"horizontal","gap":12,"width":"fit_content","height":"fit_content"})
b1=I(btns, {"type":"rectangle","role":"button","padding":[14,32],"cornerRadius":10,"fill":[{"type":"solid","color":"#111111"}],"layout":"horizontal","justifyContent":"center","alignItems":"center"})
I(b1, {"type":"text","content":"Start Free","fontSize":16,"fontWeight":600,"fill":[{"type":"solid","color":"#FFFFFF"}]})
b2=I(btns, {"type":"rectangle","role":"button","padding":[14,32],"cornerRadius":10,"fill":[{"type":"solid","color":"#F3F4F6"}],"layout":"horizontal","justifyContent":"center","alignItems":"center"})
I(b2, {"type":"text","content":"View Demo","fontSize":16,"fontWeight":600})

feat=I(root, {"type":"frame","role":"section","width":"fill_container","height":"fit_content","layout":"vertical","padding":[80,80],"gap":48,"alignItems":"center"})
I(feat, {"type":"text","role":"heading","content":"Everything you need","fontSize":36,"fontWeight":700,"fontFamily":"Space Grotesk","textAlign":"center","letterSpacing":-0.5})
grid=I(feat, {"type":"frame","role":"feature-grid","width":"fill_container","layout":"horizontal","gap":24})
c1=I(grid, {"type":"rectangle","role":"feature-card","width":"fill_container","height":"fill_container","layout":"vertical","padding":28,"gap":16,"cornerRadius":16,"fill":[{"type":"solid","color":"#F9FAFB"}]})
I(c1, {"type":"path","name":"ZapIcon","width":24,"height":24,"fill":[{"type":"solid","color":"#111111"}]})
I(c1, {"type":"text","content":"Lightning Fast","fontSize":20,"fontWeight":600})
I(c1, {"type":"text","role":"body-text","content":"Sub-second builds with smart caching.","fontSize":15,"lineHeight":1.6,"fill":[{"type":"solid","color":"#6B7280"}]})
c2=C(c1, grid, {})
U(c2+"/0", {"name":"ShieldIcon"})
U(c2+"/1", {"content":"Enterprise Security"})
U(c2+"/2", {"content":"SOC 2 certified with end-to-end encryption."})
c3=C(c1, grid, {})
U(c3+"/0", {"name":"GitBranchIcon"})
U(c3+"/1", {"content":"Git-Native Workflow"})
U(c3+"/2", {"content":"Preview deploys on every push with instant rollback."})
```

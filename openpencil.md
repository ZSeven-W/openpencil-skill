# OpenPencil Design Skill

You are an expert UI/UX designer working with **OpenPencil**, an AI-native vector design tool. You create designs by generating PenNode JSON trees or using the `op` CLI / MCP tools.

---

## 1. Quick Reference — `op` CLI

The `op` command controls a running OpenPencil instance (desktop app or web server).

```bash
# App lifecycle
op start [--desktop|--web]     # Launch app
op stop                        # Stop app
op status                      # Check connection

# Design (batch DSL)
op design '<dsl>'              # Inline DSL
op design @landing.txt         # From file
cat layout.dsl | op design -   # From stdin

# Document
op open [path.op]              # Open document
op save [path.op]              # Save document
op get [--depth N]             # Get document tree
op selection                   # Get selected nodes

# Nodes
op insert '<json>'             # Insert node
op update <id> '<json>'        # Update node properties
op delete <id>                 # Delete node
op move <id> <parent> [index]  # Move node
op copy <id> <parent>          # Deep-copy node
op replace <id> '<json>'       # Replace node entirely

# Pages
op page list                   # List pages
op page add <name>             # Add page
op page remove <id>            # Remove page
op page rename <id> <name>     # Rename page

# Variables & Themes
op vars                        # List variables
op vars:set '<json>'           # Set variables
op themes                      # List themes
op themes:set '<json>'         # Set themes

# Import & Export
op import:figma <file.fig>     # Import Figma file
op import:svg <file.svg>       # Import SVG
op export <format> [--out .]   # Export code
# Formats: react, html, vue, svelte, flutter, swiftui, compose, rn, css

# Layout
op layout [id]                 # Get layout snapshot
op find-space <w> <h>          # Find empty canvas area
```

All commands accept `--file <path>` (target .op file), `--page <id>` (target page), `--pretty` (formatted output). JSON arguments support inline string, `@filepath`, or `-` (stdin).

---

## 2. Batch Design DSL

The most powerful way to create designs. Each line is one operation. Use variable bindings (`name=`) to reference created nodes in later lines.

### Operations

```
# Insert — I(parent, { nodeData })
root=I(null, { "type": "frame", "name": "Page", "width": 1200, "layout": "vertical" })
nav=I(root, { "type": "frame", "role": "navbar", "height": 72 })
hero=I(root, { "type": "frame", "role": "hero", "height": 600 })

# Update — U(nodeRef, { updates })
U(nav, { "fill": [{"type": "solid", "color": "#FFFFFF"}] })

# Copy — C(sourceId, parent, { overrides })
card2=C(card1, grid, { "name": "Card 2" })

# Replace — R(nodeRef, { newNodeData })
R(old_btn, { "type": "rectangle", "role": "button", "width": 200 })

# Move — M(nodeRef, newParent, index?)
M(sidebar, main, 0)

# Delete — D(nodeRef)
D(old_section)
```

### Full Example — Landing Page

```
root=I(null, { "type": "frame", "name": "Landing Page", "width": 1200, "height": 0, "layout": "vertical", "fill": [{"type": "solid", "color": "#FFFFFF"}] })

nav=I(root, { "type": "frame", "name": "Nav", "role": "navbar", "width": "fill_container", "height": 72, "layout": "horizontal", "padding": [0, 80], "justifyContent": "space_between", "alignItems": "center" })
logo=I(nav, { "type": "text", "content": "Acme", "fontSize": 20, "fontWeight": 700, "fontFamily": "Space Grotesk" })
links=I(nav, { "type": "frame", "role": "nav-links", "layout": "horizontal", "gap": 32, "width": "fit_content", "height": "fit_content" })
I(links, { "type": "text", "role": "nav-link", "content": "Features", "fontSize": 15 })
I(links, { "type": "text", "role": "nav-link", "content": "Pricing", "fontSize": 15 })
I(links, { "type": "text", "role": "nav-link", "content": "Docs", "fontSize": 15 })
cta=I(nav, { "type": "rectangle", "role": "button", "width": "fit_content", "height": "fit_content", "padding": [10, 24], "cornerRadius": 8, "fill": [{"type": "solid", "color": "#111111"}], "children": [{ "type": "text", "content": "Get Started", "fontSize": 14, "fontWeight": 600, "fill": [{"type": "solid", "color": "#FFFFFF"}] }] })

hero=I(root, { "type": "frame", "name": "Hero", "role": "hero", "width": "fill_container", "height": "fit_content", "layout": "vertical", "padding": [100, 80], "gap": 24, "alignItems": "center" })
I(hero, { "type": "text", "role": "heading", "content": "Ship faster with Acme", "fontSize": 56, "fontWeight": 700, "fontFamily": "Space Grotesk", "textAlign": "center", "letterSpacing": -1.5, "lineHeight": 1.1, "textGrowth": "fixed-width", "width": 800 })
I(hero, { "type": "text", "role": "subheading", "content": "The developer platform that turns ideas into production apps in minutes, not months.", "fontSize": 18, "textAlign": "center", "lineHeight": 1.6, "textGrowth": "fixed-width", "width": 560, "fill": [{"type": "solid", "color": "#6B7280"}] })
hero_btns=I(hero, { "type": "frame", "layout": "horizontal", "gap": 12, "width": "fit_content", "height": "fit_content" })
I(hero_btns, { "type": "rectangle", "role": "button", "padding": [14, 32], "cornerRadius": 10, "fill": [{"type": "solid", "color": "#111111"}], "children": [{ "type": "text", "content": "Start Free", "fontSize": 16, "fontWeight": 600, "fill": [{"type": "solid", "color": "#FFFFFF"}] }] })
I(hero_btns, { "type": "rectangle", "role": "button", "padding": [14, 32], "cornerRadius": 10, "fill": [{"type": "solid", "color": "#F3F4F6"}], "stroke": { "thickness": 1, "fill": [{"type": "solid", "color": "#E5E7EB"}] }, "children": [{ "type": "text", "content": "View Demo", "fontSize": 16, "fontWeight": 600, "fill": [{"type": "solid", "color": "#111111"}] }] })

features=I(root, { "type": "frame", "name": "Features", "role": "section", "width": "fill_container", "height": "fit_content", "layout": "vertical", "padding": [80, 80], "gap": 48, "alignItems": "center" })
I(features, { "type": "text", "role": "heading", "content": "Everything you need", "fontSize": 36, "fontWeight": 700, "fontFamily": "Space Grotesk", "textAlign": "center", "letterSpacing": -0.5 })
grid=I(features, { "type": "frame", "role": "feature-grid", "width": "fill_container", "layout": "horizontal", "gap": 24 })
card1=I(grid, { "type": "rectangle", "role": "feature-card", "width": "fill_container", "height": "fill_container", "layout": "vertical", "padding": 28, "gap": 16, "cornerRadius": 16, "fill": [{"type": "solid", "color": "#F9FAFB"}], "children": [{ "type": "path", "name": "ZapIcon", "width": 24, "height": 24, "fill": [{"type": "solid", "color": "#111111"}] }, { "type": "text", "content": "Lightning Fast", "fontSize": 20, "fontWeight": 600 }, { "type": "text", "role": "body-text", "content": "Sub-second builds with incremental compilation and smart caching.", "fontSize": 15, "lineHeight": 1.6, "fill": [{"type": "solid", "color": "#6B7280"}] }] })
card2=C(card1, grid, {})
U(card2+"/0", { "name": "ShieldIcon" })
U(card2+"/1", { "content": "Enterprise Security" })
U(card2+"/2", { "content": "SOC 2 certified with end-to-end encryption and role-based access." })
card3=C(card1, grid, {})
U(card3+"/0", { "name": "GitBranchIcon" })
U(card3+"/1", { "content": "Git-Native Workflow" })
U(card3+"/2", { "content": "Preview deployments on every push with instant rollbacks." })
```

---

## 3. PenNode Schema

Every element is a `PenNode`. Core properties shared by all types:

```json
{
  "id": "auto-generated",
  "type": "frame|rectangle|text|ellipse|line|polygon|path|image|group",
  "name": "Display Name",
  "role": "semantic-role",
  "x": 0, "y": 0,
  "rotation": 0,
  "opacity": 1,
  "visible": true,
  "locked": false
}
```

### Container Properties (frame, rectangle, group, ellipse)

```json
{
  "width": 400,
  "height": 300,
  "layout": "none|vertical|horizontal",
  "gap": 16,
  "padding": 16,
  "justifyContent": "start|center|end|space_between|space_around",
  "alignItems": "start|center|end",
  "clipContent": true,
  "cornerRadius": 8,
  "fill": [{ "type": "solid", "color": "#FFFFFF" }],
  "stroke": { "thickness": 1, "fill": [{ "type": "solid", "color": "#E5E7EB" }] },
  "effects": [{ "type": "shadow", "offsetX": 0, "offsetY": 4, "blur": 12, "spread": 0, "color": "rgba(0,0,0,0.08)" }],
  "children": []
}
```

**Sizing values:** fixed number (px), `"fill_container"` (stretch), `"fit_content"` (shrink-wrap)

**Padding formats:** `16` (uniform), `[16, 24]` (vertical, horizontal), `[16, 24, 16, 24]` (top, right, bottom, left)

**cornerRadius formats:** `8` (uniform), `[8, 8, 0, 0]` (topLeft, topRight, bottomRight, bottomLeft)

### Text Node

```json
{
  "type": "text",
  "content": "Hello World",
  "fontSize": 16,
  "fontFamily": "Inter",
  "fontWeight": "normal|bold|500|600|700",
  "fontStyle": "normal|italic",
  "textAlign": "left|center|right|justify",
  "textAlignVertical": "top|middle|bottom",
  "textGrowth": "auto|fixed-width|fixed-width-height",
  "lineHeight": 1.5,
  "letterSpacing": 0,
  "fill": [{ "type": "solid", "color": "#111111" }]
}
```

**Rich text** — use `content` as array:

```json
{
  "content": [
    { "text": "Bold ", "fontWeight": "bold" },
    { "text": "and ", "fontWeight": "normal" },
    { "text": "colored", "fill": [{ "type": "solid", "color": "#3B82F6" }] }
  ]
}
```

### Path Node (Icons)

```json
{
  "type": "path",
  "name": "HeartIcon",
  "width": 24,
  "height": 24,
  "fill": [{ "type": "solid", "color": "#111111" }]
}
```

Icon names use PascalCase + "Icon" suffix. The system auto-resolves to SVG paths from the Lucide icon set. Common icons:

| Icon | Name | | Icon | Name |
|------|------|-|------|------|
| Search | SearchIcon | | Menu | MenuIcon |
| Home | HomeIcon | | User | UserIcon |
| Settings | SettingsIcon | | Mail | MailIcon |
| Heart | HeartIcon | | Star | StarIcon |
| Check | CheckIcon | | X/Close | XIcon |
| ChevronRight | ChevronRightIcon | | ArrowRight | ArrowRightIcon |
| Phone | PhoneIcon | | Globe | GlobeIcon |
| Zap | ZapIcon | | Shield | ShieldIcon |
| GitBranch | GitBranchIcon | | Code | CodeIcon |
| Clock | ClockIcon | | Eye | EyeIcon |
| Download | DownloadIcon | | Upload | UploadIcon |
| Play | PlayIcon | | Plus | PlusIcon |
| Bell | BellIcon | | Lock | LockIcon |
| Sparkles | SparklesIcon | | Layers | LayersIcon |

### Image Node

```json
{
  "type": "image",
  "src": "https://example.com/photo.jpg",
  "width": 400,
  "height": 300
}
```

### Line Node

```json
{
  "type": "line",
  "x2": 200,
  "y2": 0,
  "stroke": { "thickness": 1, "fill": [{ "type": "solid", "color": "#E5E7EB" }] }
}
```

### Ellipse Node

```json
{
  "type": "ellipse",
  "width": 48,
  "height": 48,
  "fill": [{ "type": "solid", "color": "#3B82F6" }]
}
```

### Fill Types

```json
// Solid
{ "type": "solid", "color": "#3B82F6", "opacity": 1.0 }

// Linear gradient
{ "type": "linear_gradient", "angle": 135, "stops": [
  { "offset": 0, "color": "#6366F1" },
  { "offset": 1, "color": "#8B5CF6" }
]}

// Radial gradient
{ "type": "radial_gradient", "cx": 0.5, "cy": 0.5, "radius": 0.5, "stops": [
  { "offset": 0, "color": "#FFFFFF" },
  { "offset": 1, "color": "#000000" }
]}
```

### Design Variables

Reference variables with `$` prefix. The engine resolves them at render time.

```json
{
  "fill": [{ "type": "solid", "color": "$primaryColor" }],
  "gap": "$spacing",
  "cornerRadius": "$radius"
}
```

---

## 4. Semantic Roles

Roles declare intent — the engine applies smart defaults. Always prefer roles over manual properties.

### Layout Roles
| Role | Defaults |
|------|----------|
| `section` | width: fill_container, gap: 24, padding: [60, 80] |
| `row` | layout: horizontal, gap: 16, alignItems: center |
| `column` | layout: vertical, gap: 16 |
| `centered-content` | alignItems: center |
| `divider` | height: 1, fill: border color |
| `spacer` | height: 32 |

### Navigation Roles
| Role | Defaults |
|------|----------|
| `navbar` | height: 56-72, layout: horizontal, justifyContent: space_between, alignItems: center |
| `nav-links` | layout: horizontal, gap: 32 |
| `nav-link` | fontSize: 15, cursor pointer |

### Interactive Roles
| Role | Defaults |
|------|----------|
| `button` | padding: [12, 24], height: 44, cornerRadius: 8, centered |
| `icon-button` | width: 40, height: 40, centered |
| `badge` | padding: [4, 10], cornerRadius: 99, small text |
| `tag` | padding: [6, 12], cornerRadius: 6 |
| `pill` | padding: [8, 16], cornerRadius: 99 |
| `input` / `form-input` | width: fill_container, height: 48, cornerRadius: 8 |
| `search-bar` | width: fill_container, height: 44, cornerRadius: 22 |

### Content Roles
| Role | Defaults |
|------|----------|
| `card` | layout: vertical, gap: 12, cornerRadius: 12, padding: 24 |
| `feature-card` | Same as card, for feature grids |
| `stat-card` | Centered stats display |
| `pricing-card` | Pricing plan layout |
| `hero` | Large prominent section |
| `feature-grid` | Horizontal card layout |
| `cta-section` | Call-to-action section |
| `footer` | Page footer |
| `testimonial` | Quote section |

### Typography Roles
| Role | Defaults |
|------|----------|
| `heading` | lineHeight: 1.2, letterSpacing: -0.5 |
| `subheading` | lineHeight: 1.3 |
| `body-text` | width: fill_container, textGrowth: fixed-width, lineHeight: 1.5 |
| `caption` | lineHeight: 1.3, small text |
| `label` | fontSize: 14, fontWeight: 500 |

### Media Roles
| Role | Defaults |
|------|----------|
| `avatar` | Circular (cornerRadius: 50%), clipContent |
| `icon` | width: 24, height: 24 |
| `phone-mockup` | Phone frame shape |
| `screenshot-frame` | Screenshot holder |

---

## 5. Design Principles — Making It Beautiful

### Typography Scale

```
Display:    40-56px, fontWeight: 700, letterSpacing: -1 to -1.5, lineHeight: 1.1
Heading:    28-36px, fontWeight: 700, letterSpacing: -0.5, lineHeight: 1.2
Subheading: 20-24px, fontWeight: 600, letterSpacing: -0.25, lineHeight: 1.3
Body:       15-18px, fontWeight: 400, lineHeight: 1.5-1.6
Caption:    13-14px, fontWeight: 400, lineHeight: 1.4
```

**Font choices:**
- Headlines: `"Space Grotesk"` or `"Manrope"` (modern geometric sans)
- Body: `"Inter"` (highly legible)
- CJK text: `"Noto Sans SC"` / `"Noto Sans JP"` / `"Noto Sans KR"` — NEVER use Space Grotesk for CJK

**CJK-specific rules:**
- lineHeight: headings 1.3-1.4, body 1.6-1.8 (wider than Latin)
- letterSpacing: ALWAYS 0 (negative spacing causes CJK overlap)
- Button width: each CJK char is approximately fontSize wide

### Color Palette

Use restrained, purposeful color:

```
Primary text:     #111111 or #0F172A (near-black, not pure black)
Secondary text:   #6B7280 or #64748B (muted gray)
Subtle text:      #9CA3AF or #94A3B8
Background:       #FFFFFF
Surface:          #F9FAFB or #F8FAFC (barely gray)
Border:           #E5E7EB or #E2E8F0
Primary action:   One strong color (#111111 for minimal, or a brand color)
Accent:           One highlight color, used sparingly
```

**Rules:**
- Maximum 2 saturated colors in a design
- WCAG AA contrast: 4.5:1 for body text, 3:1 for large text
- Default to light theme unless user specifies dark/cyber/neon
- Dark backgrounds: use `#0F172A` or `#111827`, not `#000000`

### Spacing Grid (8px base)

```
Related elements:    8-16px  (label to input, icon to text)
Component internal:  16-24px (card padding, button group gap)
Between components:  24-32px (card to card, form fields)
Between sections:    48-80px (hero to features, features to CTA)
Page padding:        80px horizontal (desktop), 20px (mobile)
```

### Shadow & Depth

```json
// Subtle (cards, inputs)
{ "type": "shadow", "offsetX": 0, "offsetY": 1, "blur": 3, "spread": 0, "color": "rgba(0,0,0,0.05)" }

// Medium (dropdowns, popovers)
{ "type": "shadow", "offsetX": 0, "offsetY": 4, "blur": 12, "spread": 0, "color": "rgba(0,0,0,0.08)" }

// Elevated (modals, floating elements)
{ "type": "shadow", "offsetX": 0, "offsetY": 8, "blur": 24, "spread": -4, "color": "rgba(0,0,0,0.12)" }
```

### Copywriting Rules

- **Headlines:** 2-6 words. Punchy, benefit-driven. Not "Welcome to Our Website".
- **Subtitles:** 1 sentence, max 15 words. Expand the headline.
- **Buttons:** 1-3 words. Action verbs: "Get Started", "Try Free", "Learn More".
- **Body text:** Short sentences. No filler. Real product language.
- **Never:** Lorem ipsum, placeholder gibberish, or emoji as icons.

---

## 6. Layout Engine Rules

### Flexbox Model

Frames with `layout: "vertical"` or `"horizontal"` auto-position children via gap, padding, justifyContent, alignItems.

**Critical rules:**
1. **NEVER set x/y on children inside layout containers** — the engine positions them
2. **Child width must be <= parent content area** (parent width - padding)
3. **Siblings must use the SAME width strategy** — all `fill_container` or all fixed px
4. **NEVER use `fill_container` on children of `fit_content` parent** — circular dependency
5. For nodes NOT inside a layout container, manually set x/y

### Sizing Decision Guide

```
"Should this element stretch to fill available space?"
  YES → width: "fill_container"
  NO  → "Should it shrink to wrap its content?"
    YES → width: "fit_content"
    NO  → width: 300 (fixed pixels)
```

**Common patterns:**
- Page root frame: `width: 1200, height: 0` (auto-height)
- Sections: `width: "fill_container", height: "fit_content"`
- Cards in a row: ALL `width: "fill_container", height: "fill_container"`
- Buttons: `width: "fit_content", height: "fit_content"`
- Text in cards: `width: "fill_container"` with `textGrowth: "fixed-width"`

### Design Type Detection

| Type | Width | Height | Layout |
|------|-------|--------|--------|
| Landing / marketing page | 1200 | 0 (auto) | vertical |
| Mobile app screen | 375 | 812 (fixed) | vertical |
| Dashboard / admin | 1200 | 0 (auto) | vertical |
| Component / widget | fit_content | fit_content | varies |

---

## 7. Layered Design Workflow (MCP)

For complex multi-section pages, use three stages via MCP tools:

### Stage 1 — `design_skeleton`

Create the structural layout with section placeholders:

```json
{
  "rootFrame": { "width": 1200, "layout": "vertical", "gap": 0 },
  "sections": [
    { "name": "Navigation", "role": "navbar" },
    { "name": "Hero", "role": "hero" },
    { "name": "Features", "role": "feature-grid" },
    { "name": "CTA", "role": "cta-section" },
    { "name": "Footer", "role": "footer" }
  ]
}
```

Returns section IDs and per-section content guidelines.

### Stage 2 — `design_content` (per section)

Populate each section independently:

```json
{
  "sectionId": "<id from skeleton>",
  "children": [ /* PenNode array */ ],
  "postProcess": true
}
```

### Stage 3 — `design_refine`

Full-tree validation and auto-fixes:

```json
{
  "rootId": "<root frame id>"
}
```

Applies: card row equalization, overflow fix, form consistency, text height estimation, emoji removal, unique ID enforcement.

---

## 8. Common Patterns

### Navbar

```json
{
  "type": "frame", "role": "navbar",
  "width": "fill_container", "height": 72,
  "layout": "horizontal", "padding": [0, 80],
  "justifyContent": "space_between", "alignItems": "center",
  "fill": [{ "type": "solid", "color": "#FFFFFF" }],
  "stroke": { "thickness": 1, "fill": [{ "type": "solid", "color": "#F3F4F6" }] },
  "children": [
    { "type": "text", "content": "Brand", "fontSize": 20, "fontWeight": 700, "fontFamily": "Space Grotesk" },
    { "type": "frame", "role": "nav-links", "layout": "horizontal", "gap": 32, "width": "fit_content", "height": "fit_content", "children": [
      { "type": "text", "role": "nav-link", "content": "Features", "fontSize": 15 },
      { "type": "text", "role": "nav-link", "content": "Pricing", "fontSize": 15 },
      { "type": "text", "role": "nav-link", "content": "Docs", "fontSize": 15 }
    ]},
    { "type": "rectangle", "role": "button", "padding": [10, 24], "cornerRadius": 8,
      "fill": [{ "type": "solid", "color": "#111111" }],
      "children": [{ "type": "text", "content": "Get Started", "fontSize": 14, "fontWeight": 600, "fill": [{ "type": "solid", "color": "#FFFFFF" }] }] }
  ]
}
```

### Hero Section

```json
{
  "type": "frame", "role": "hero",
  "width": "fill_container", "height": "fit_content",
  "layout": "vertical", "padding": [100, 80], "gap": 24, "alignItems": "center",
  "children": [
    { "type": "text", "role": "heading", "content": "Build something great",
      "fontSize": 56, "fontWeight": 700, "fontFamily": "Space Grotesk",
      "textAlign": "center", "letterSpacing": -1.5, "lineHeight": 1.1,
      "textGrowth": "fixed-width", "width": 800 },
    { "type": "text", "role": "subheading",
      "content": "The modern platform for teams who ship fast.",
      "fontSize": 18, "textAlign": "center", "lineHeight": 1.6,
      "textGrowth": "fixed-width", "width": 560,
      "fill": [{ "type": "solid", "color": "#6B7280" }] },
    { "type": "frame", "layout": "horizontal", "gap": 12, "width": "fit_content", "height": "fit_content", "children": [
      { "type": "rectangle", "role": "button", "padding": [14, 32], "cornerRadius": 10,
        "fill": [{ "type": "solid", "color": "#111111" }],
        "children": [{ "type": "text", "content": "Start Free", "fontSize": 16, "fontWeight": 600, "fill": [{ "type": "solid", "color": "#FFFFFF" }] }] },
      { "type": "rectangle", "role": "button", "padding": [14, 32], "cornerRadius": 10,
        "fill": [{ "type": "solid", "color": "#F3F4F6" }],
        "children": [{ "type": "text", "content": "View Demo", "fontSize": 16, "fontWeight": 600, "fill": [{ "type": "solid", "color": "#111111" }] }] }
    ]}
  ]
}
```

### Feature Card Grid

```json
{
  "type": "frame", "role": "feature-grid",
  "width": "fill_container", "layout": "horizontal", "gap": 24,
  "children": [
    {
      "type": "rectangle", "role": "feature-card",
      "width": "fill_container", "height": "fill_container",
      "layout": "vertical", "padding": 28, "gap": 16, "cornerRadius": 16,
      "fill": [{ "type": "solid", "color": "#F9FAFB" }],
      "children": [
        { "type": "path", "name": "ZapIcon", "width": 24, "height": 24, "fill": [{ "type": "solid", "color": "#111111" }] },
        { "type": "text", "content": "Lightning Fast", "fontSize": 20, "fontWeight": 600 },
        { "type": "text", "role": "body-text", "content": "Sub-second builds with smart caching.", "fontSize": 15, "lineHeight": 1.6, "fill": [{ "type": "solid", "color": "#6B7280" }] }
      ]
    }
  ]
}
```

Cards in a horizontal row MUST ALL use `width: "fill_container"` and `height: "fill_container"` for equal sizing.

### Form Input

```json
{
  "type": "frame", "role": "form-group", "layout": "vertical", "gap": 8,
  "width": "fill_container",
  "children": [
    { "type": "text", "role": "label", "content": "Email", "fontSize": 14, "fontWeight": 500 },
    { "type": "rectangle", "role": "form-input",
      "width": "fill_container", "height": 48,
      "cornerRadius": 8, "layout": "horizontal", "padding": [0, 16], "alignItems": "center",
      "fill": [{ "type": "solid", "color": "#FFFFFF" }],
      "stroke": { "thickness": 1, "fill": [{ "type": "solid", "color": "#D1D5DB" }] },
      "children": [
        { "type": "path", "name": "MailIcon", "width": 18, "height": 18, "fill": [{ "type": "solid", "color": "#9CA3AF" }] },
        { "type": "text", "content": "you@example.com", "fontSize": 15, "fill": [{ "type": "solid", "color": "#9CA3AF" }] }
      ]
    }
  ]
}
```

### Pricing Card

```json
{
  "type": "rectangle", "role": "pricing-card",
  "width": "fill_container", "height": "fill_container",
  "layout": "vertical", "padding": 32, "gap": 24, "cornerRadius": 16,
  "fill": [{ "type": "solid", "color": "#FFFFFF" }],
  "stroke": { "thickness": 1, "fill": [{ "type": "solid", "color": "#E5E7EB" }] },
  "children": [
    { "type": "text", "content": "Pro", "fontSize": 18, "fontWeight": 600 },
    { "type": "frame", "layout": "horizontal", "gap": 4, "alignItems": "end", "width": "fit_content", "height": "fit_content", "children": [
      { "type": "text", "content": "$29", "fontSize": 48, "fontWeight": 700, "letterSpacing": -1 },
      { "type": "text", "content": "/mo", "fontSize": 16, "fill": [{ "type": "solid", "color": "#6B7280" }] }
    ]},
    { "type": "line", "x2": 280, "y2": 0, "stroke": { "thickness": 1, "fill": [{ "type": "solid", "color": "#F3F4F6" }] } },
    { "type": "frame", "layout": "vertical", "gap": 12, "width": "fill_container", "children": [
      { "type": "frame", "layout": "horizontal", "gap": 10, "alignItems": "center", "width": "fill_container", "height": "fit_content", "children": [
        { "type": "path", "name": "CheckIcon", "width": 18, "height": 18, "fill": [{ "type": "solid", "color": "#10B981" }] },
        { "type": "text", "content": "Unlimited projects", "fontSize": 15 }
      ]}
    ]},
    { "type": "rectangle", "role": "button", "width": "fill_container", "padding": [14, 24], "cornerRadius": 10,
      "fill": [{ "type": "solid", "color": "#111111" }],
      "children": [{ "type": "text", "content": "Get Started", "fontSize": 15, "fontWeight": 600, "fill": [{ "type": "solid", "color": "#FFFFFF" }] }] }
  ]
}
```

### Stats Section

```json
{
  "type": "frame", "role": "stats-section",
  "width": "fill_container", "layout": "horizontal", "gap": 0,
  "padding": [60, 80], "justifyContent": "space_around",
  "fill": [{ "type": "solid", "color": "#111111" }],
  "children": [
    { "type": "frame", "layout": "vertical", "gap": 4, "alignItems": "center", "width": "fit_content", "height": "fit_content", "children": [
      { "type": "text", "content": "10M+", "fontSize": 40, "fontWeight": 700, "fill": [{ "type": "solid", "color": "#FFFFFF" }] },
      { "type": "text", "content": "Downloads", "fontSize": 15, "fill": [{ "type": "solid", "color": "#9CA3AF" }] }
    ]}
  ]
}
```

### Footer

```json
{
  "type": "frame", "role": "footer",
  "width": "fill_container", "height": "fit_content",
  "layout": "horizontal", "padding": [48, 80], "gap": 80,
  "fill": [{ "type": "solid", "color": "#F9FAFB" }],
  "children": [
    { "type": "frame", "layout": "vertical", "gap": 16, "width": 240, "children": [
      { "type": "text", "content": "Brand", "fontSize": 20, "fontWeight": 700, "fontFamily": "Space Grotesk" },
      { "type": "text", "content": "Building the future of design tooling.", "fontSize": 14, "lineHeight": 1.6, "fill": [{ "type": "solid", "color": "#6B7280" }] }
    ]},
    { "type": "frame", "layout": "vertical", "gap": 12, "width": "fit_content", "children": [
      { "type": "text", "content": "Product", "fontSize": 14, "fontWeight": 600 },
      { "type": "text", "content": "Features", "fontSize": 14, "fill": [{ "type": "solid", "color": "#6B7280" }] },
      { "type": "text", "content": "Pricing", "fontSize": 14, "fill": [{ "type": "solid", "color": "#6B7280" }] },
      { "type": "text", "content": "Changelog", "fontSize": 14, "fill": [{ "type": "solid", "color": "#6B7280" }] }
    ]},
    { "type": "frame", "layout": "vertical", "gap": 12, "width": "fit_content", "children": [
      { "type": "text", "content": "Company", "fontSize": 14, "fontWeight": 600 },
      { "type": "text", "content": "About", "fontSize": 14, "fill": [{ "type": "solid", "color": "#6B7280" }] },
      { "type": "text", "content": "Blog", "fontSize": 14, "fill": [{ "type": "solid", "color": "#6B7280" }] },
      { "type": "text", "content": "Careers", "fontSize": 14, "fill": [{ "type": "solid", "color": "#6B7280" }] }
    ]}
  ]
}
```

---

## 9. Checklist — Before Submitting a Design

- [ ] Root frame has `width: 1200` (desktop) or `375` (mobile), `height: 0`, `layout: "vertical"`
- [ ] No x/y on children inside layout containers
- [ ] Cards in horizontal rows all use `fill_container` for both width AND height
- [ ] Text headings use `fontFamily: "Space Grotesk"`, body uses `"Inter"`
- [ ] Body text has `textGrowth: "fixed-width"` and `width: "fill_container"`
- [ ] No emoji in text content — use path icons instead
- [ ] No lorem ipsum — use realistic, concise copy
- [ ] Headlines: 2-6 words. Subtitles: max 15 words. Buttons: 1-3 words
- [ ] Primary text is `#111111`, secondary is `#6B7280`, not pure black/gray
- [ ] Max 2 saturated accent colors
- [ ] Shadows use `rgba(0,0,0,0.05-0.12)`, not heavy drop shadows
- [ ] Section padding: `[60-100, 80]`, component gap: `16-24`, section gap: `48-80`
- [ ] `postProcess: true` is set (for MCP tools)
- [ ] CJK text uses `"Noto Sans SC/JP/KR"`, lineHeight >= 1.3, letterSpacing: 0

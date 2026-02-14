---
name: excalidraw
description: Create and edit Excalidraw diagrams for Obsidian. Use when the user asks to create diagrams, flowcharts, architecture diagrams, wireframes, or any visual drawings. Outputs .excalidraw.md files compatible with the Obsidian Excalidraw plugin.
---

# Excalidraw Diagram Creation for Obsidian

This skill creates diagrams as `.excalidraw.md` files compatible with the Obsidian Excalidraw plugin (obsidian-excalidraw-plugin).

## Obsidian File Format

Obsidian Excalidraw files are **markdown files** (not raw JSON). They have this structure:

```
---

excalidraw-plugin: parsed
tags: [excalidraw]

---

# Excalidraw Data

## Text Elements
<text content> ^<elementId>

## Drawing
```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [...],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "gridSize": null
  },
  "files": {}
}
```
%%
```

### Key Rules

1. **File extension**: Always `.excalidraw.md` (NOT `.excalidraw`)
2. **Frontmatter**: Must include `excalidraw-plugin: parsed`
3. **Text Elements section**: Extract ALL text from elements and list them with their IDs. Each text block ends with ` ^<elementId>` where the ID matches the element's `id` in the JSON. Separate each text element with a blank line.
4. **Drawing section**: Contains the full Excalidraw JSON in an uncompressed `json` code block. The plugin will compress it on next save.
5. **Closing**: The Drawing section ends with `%%`
6. **Blank lines in frontmatter**: Include blank lines around `excalidraw-plugin: parsed` in frontmatter (Obsidian YAML quirk)

### Text Elements Format

For each `text` type element in the JSON, add an entry:

```
Text content here ^elementId

Multi-line text
second line ^anotherId
```

- The `^id` must match the element's `id` field in the JSON
- Multi-line text stays together, with `^id` on the last line
- Separate entries with a blank line

## Quick Start - Creating Elements

Every element needs at minimum: `type`, `id`, `x`, `y`, and styling properties.

### Minimal Rectangle
```json
{
  "type": "rectangle",
  "id": "rect-1",
  "x": 100,
  "y": 100,
  "width": 200,
  "height": 100,
  "angle": 0,
  "strokeColor": "#1e1e1e",
  "backgroundColor": "transparent",
  "fillStyle": "solid",
  "strokeWidth": 2,
  "strokeStyle": "solid",
  "roughness": 1,
  "opacity": 100,
  "seed": 1234,
  "version": 1,
  "versionNonce": 1234,
  "isDeleted": false,
  "groupIds": [],
  "boundElements": null,
  "link": null,
  "locked": false,
  "roundness": { "type": 3 }
}
```

### Text Element
```json
{
  "type": "text",
  "id": "text-1",
  "x": 120,
  "y": 130,
  "width": 160,
  "height": 25,
  "text": "Hello World",
  "fontSize": 20,
  "fontFamily": 1,
  "textAlign": "center",
  "verticalAlign": "middle",
  "angle": 0,
  "strokeColor": "#1e1e1e",
  "backgroundColor": "transparent",
  "fillStyle": "solid",
  "strokeWidth": 2,
  "strokeStyle": "solid",
  "roughness": 1,
  "opacity": 100,
  "seed": 5678,
  "version": 1,
  "versionNonce": 5678,
  "isDeleted": false,
  "groupIds": [],
  "boundElements": null,
  "link": null,
  "locked": false,
  "containerId": null,
  "originalText": "Hello World",
  "autoResize": true,
  "lineHeight": 1.25
}
```

### Arrow Element
```json
{
  "type": "arrow",
  "id": "arrow-1",
  "x": 300,
  "y": 150,
  "width": 100,
  "height": 0,
  "points": [[0, 0], [100, 0]],
  "startArrowhead": null,
  "endArrowhead": "arrow",
  "startBinding": null,
  "endBinding": null,
  "angle": 0,
  "strokeColor": "#1e1e1e",
  "backgroundColor": "transparent",
  "fillStyle": "solid",
  "strokeWidth": 2,
  "strokeStyle": "solid",
  "roughness": 1,
  "opacity": 100,
  "seed": 9012,
  "version": 1,
  "versionNonce": 9012,
  "isDeleted": false,
  "groupIds": [],
  "boundElements": null,
  "link": null,
  "locked": false
}
```

## Complete Obsidian Example

Here is a minimal complete `.excalidraw.md` file with two boxes and an arrow:

```markdown
---

excalidraw-plugin: parsed
tags: [excalidraw]

---

# Excalidraw Data

## Text Elements
Client ^text-client

Server ^text-server

## Drawing
```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    {
      "type": "rectangle",
      "id": "rect-client",
      "x": 50,
      "y": 100,
      "width": 160,
      "height": 80,
      "angle": 0,
      "strokeColor": "#1971c2",
      "backgroundColor": "#a5d8ff",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "seed": 1001,
      "version": 1,
      "versionNonce": 1001,
      "isDeleted": false,
      "groupIds": [],
      "boundElements": [
        { "id": "text-client", "type": "text" },
        { "id": "arrow-1", "type": "arrow" }
      ],
      "link": null,
      "locked": false,
      "roundness": { "type": 3 }
    },
    {
      "type": "text",
      "id": "text-client",
      "x": 95,
      "y": 127,
      "width": 70,
      "height": 25,
      "text": "Client",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "angle": 0,
      "strokeColor": "#1971c2",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "seed": 1002,
      "version": 1,
      "versionNonce": 1002,
      "isDeleted": false,
      "groupIds": [],
      "boundElements": null,
      "link": null,
      "locked": false,
      "containerId": "rect-client",
      "originalText": "Client",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "id": "rect-server",
      "x": 350,
      "y": 100,
      "width": 160,
      "height": 80,
      "angle": 0,
      "strokeColor": "#2f9e44",
      "backgroundColor": "#b2f2bb",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "seed": 1003,
      "version": 1,
      "versionNonce": 1003,
      "isDeleted": false,
      "groupIds": [],
      "boundElements": [
        { "id": "text-server", "type": "text" },
        { "id": "arrow-1", "type": "arrow" }
      ],
      "link": null,
      "locked": false,
      "roundness": { "type": 3 }
    },
    {
      "type": "text",
      "id": "text-server",
      "x": 395,
      "y": 127,
      "width": 70,
      "height": 25,
      "text": "Server",
      "fontSize": 20,
      "fontFamily": 2,
      "textAlign": "center",
      "verticalAlign": "middle",
      "angle": 0,
      "strokeColor": "#2f9e44",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "seed": 1004,
      "version": 1,
      "versionNonce": 1004,
      "isDeleted": false,
      "groupIds": [],
      "boundElements": null,
      "link": null,
      "locked": false,
      "containerId": "rect-server",
      "originalText": "Server",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "id": "arrow-1",
      "x": 215,
      "y": 140,
      "width": 130,
      "height": 0,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "seed": 1005,
      "version": 1,
      "versionNonce": 1005,
      "isDeleted": false,
      "groupIds": [],
      "boundElements": null,
      "link": null,
      "locked": false,
      "points": [[0, 0], [130, 0]],
      "startArrowhead": null,
      "endArrowhead": "arrow",
      "startBinding": { "elementId": "rect-client", "focus": 0, "gap": 5 },
      "endBinding": { "elementId": "rect-server", "focus": 0, "gap": 5 }
    }
  ],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "gridSize": null
  },
  "files": {}
}
```
%%
```

## Element Types

| Type | Use For |
|------|---------|
| `rectangle` | Boxes, containers, cards |
| `ellipse` | Circles, ovals, nodes |
| `diamond` | Decision points, conditions |
| `text` | Labels, descriptions |
| `arrow` | Connections, flow direction |
| `line` | Connectors without arrowheads |
| `freedraw` | Hand-drawn paths |
| `image` | Embedded images |
| `frame` | Grouping/framing elements |

## Color Palette (Recommended)

### Backgrounds
- `#a5d8ff` - Light blue
- `#b2f2bb` - Light green
- `#ffec99` - Light yellow
- `#ffc9c9` - Light red/pink
- `#e9ecef` - Light gray
- `#d0bfff` - Light purple
- `#99e9f2` - Light cyan

### Strokes
- `#1e1e1e` - Default black
- `#1971c2` - Blue
- `#2f9e44` - Green
- `#f08c00` - Orange
- `#e03131` - Red
- `#9c36b5` - Purple

## Styling Properties

| Property | Values | Description |
|----------|--------|-------------|
| `fillStyle` | `"solid"`, `"hachure"`, `"cross-hatch"` | Fill pattern |
| `strokeWidth` | `1`, `2`, `4` | Line thickness |
| `strokeStyle` | `"solid"`, `"dashed"`, `"dotted"` | Line style |
| `roughness` | `0`, `1`, `2` | Hand-drawn feel (0=smooth, 2=rough) |
| `opacity` | `0-100` | Transparency |
| `roundness` | `{ "type": 3 }` | Rounded corners |

## Connecting Elements with Arrows

To connect shapes with arrows, use bindings:

```json
{
  "type": "arrow",
  "id": "arrow-1",
  "startBinding": {
    "elementId": "rect-1",
    "focus": 0,
    "gap": 5
  },
  "endBinding": {
    "elementId": "rect-2",
    "focus": 0,
    "gap": 5
  },
  "points": [[0, 0], [200, 0]]
}
```

Also add `boundElements` to the connected shapes:
```json
{
  "type": "rectangle",
  "id": "rect-1",
  "boundElements": [{ "id": "arrow-1", "type": "arrow" }]
}
```

## Font Families

| Value | Font |
|-------|------|
| `1` | Hand-drawn (Virgil) |
| `2` | Normal (Helvetica) |
| `3` | Code (Cascadia) |

## Best Practices

1. **Generate unique IDs** - Use descriptive prefixes like `rect-1`, `arrow-2`, `text-header`
2. **Use random seeds** - Each element needs a unique `seed` for consistent rendering
3. **Space elements well** - Leave 50-100px between elements
4. **Align elements** - Use consistent x/y coordinates for visual alignment
5. **Group related items** - Use matching `groupIds` arrays
6. **Use the color palette** - Stick to Excalidraw's built-in colors for consistency
7. **Text Elements section** - Always keep it in sync with text elements in the JSON
8. **File placement** - Place `.excalidraw.md` files in the user's Obsidian vault (typically in an Excalidraw folder)

## Obsidian-Specific Notes

- The plugin will **auto-compress** the JSON on next save — uncompressed output is fine
- After creating the file, the user should open it in Obsidian and switch to **Excalidraw View**
- Embedded images use `[[wikilinks]]` in the `## Embedded Files` section
- The `## Text Elements` section makes diagram text **searchable** in Obsidian
- Files can be placed anywhere in the vault but are commonly in an `Excalidraw/` folder

## Reference Documentation

- **[references/diagram-patterns.md](references/diagram-patterns.md)** - Professional patterns for each diagram type
- **[references/examples.md](references/examples.md)** - Complete working JSON examples as templates
- **[references/element-reference.md](references/element-reference.md)** - Full property reference for all element types
- **[references/best-practices.md](references/best-practices.md)** - Design tips, color theory, typography guidelines

## Quick Tips

1. **Use `roughness: 0`** for formal/technical diagrams, `roughness: 1` for friendly sketches
2. **Use `fontFamily: 2`** (Helvetica) for professional look, `fontFamily: 1` (Virgil) only when casual
3. **Consistent spacing** - 100px between major elements, 50px for related items
4. **Color semantics** - Blue for info/input, Green for success/data, Yellow for decisions, Red for errors
5. **Arrow conventions** - Solid for actions, Dashed for responses/async

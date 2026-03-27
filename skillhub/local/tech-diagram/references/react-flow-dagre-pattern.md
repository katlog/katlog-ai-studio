# React Flow + dagre Pattern

Use this pattern for complex HTML diagrams and animated technical walkthroughs.

## When To Use

Choose React Flow + dagre when the diagram has one or more of these properties:

- More than 4 nodes in a single flow
- Long node labels or subtitles
- Multiple stages or viewpoints
- Need for active node highlighting
- Need for animated edges or step playback
- Need for right-side details, payload samples, or code references

Use Mermaid instead when the diagram is small, static, and readable without interaction.

## Why This Pattern

React Flow solves rendering, controls, selection states, and animated edges.

dagre solves automatic layout, stable spacing, and predictable arrow routing.

Together they avoid the common problems of hand-written SVG flows:

- arrow size inconsistency
- uneven line lengths
- overflowing text
- brittle manual coordinates

## Recommended Structure

### 1. Node Data

```js
const nodes = [
  {
    id: 'api',
    type: 'info',
    data: {
      eyebrow: 'API',
      title: 'PluginToolManager.invoke',
      subtitle: 'build payload and forward request',
      hint: 'request assembly',
      theme: 'primary',
      width: 252,
      height: 116,
    },
  },
]
```

### 2. Edge Data

```js
const edges = [
  { id: 'e-api-daemon', source: 'api', target: 'daemon', label: 'HTTP invoke' },
]
```

### 3. Stage Data

```js
const stages = [
  {
    id: 'request',
    label: '请求视角',
    focusNodes: ['api', 'daemon'],
    focusEdges: ['e-api-daemon'],
    payload: '{ ... }',
    sources: ['path/to/file.ts'],
  },
]
```

### 4. Layout Function

```js
function layoutGraph(nodes, edges) {
  const dagreGraph = new dagre.graphlib.Graph()
  dagreGraph.setDefaultEdgeLabel(() => ({}))
  dagreGraph.setGraph({ rankdir: 'LR', ranksep: 96, nodesep: 40 })

  nodes.forEach((node) => {
    dagreGraph.setNode(node.id, {
      width: node.data.width || 252,
      height: node.data.height || 116,
    })
  })

  edges.forEach((edge) => dagreGraph.setEdge(edge.source, edge.target))
  dagre.layout(dagreGraph)

  return nodes.map((node) => {
    const width = node.data.width || 252
    const height = node.data.height || 116
    const position = dagreGraph.node(node.id)

    return {
      ...node,
      position: { x: position.x - width / 2, y: position.y - height / 2 },
      sourcePosition: Position.Right,
      targetPosition: Position.Left,
    }
  })
}
```

## Animation Style

Prefer state-based animation, not timeline-heavy SVG animation.

Good animation primitives:

- stage buttons
- active node highlighting
- animated edges
- right panel content switching
- optional auto-play through stage index

Avoid:

- manual packet movement unless strictly necessary
- custom arrow path math
- placing large payloads inside nodes

## Visual Rules

- Keep node width and height consistent within one diagram
- Use title + subtitle + hint structure inside nodes
- Put long explanations in side panels
- Keep edge labels short
- Use at most 3 to 4 visual themes in one diagram
- Default to desktop-first layout with a usable mobile fallback

## Validation Checklist

Before exporting the screenshot:

- Open the page in a browser
- Check the console for errors
- Switch every stage at least once
- Confirm no node label overflows
- Confirm edges do not overlap labels badly
- Export a screenshot from the most representative stage

## CDN Imports

Use a single React version across all imports:

```js
import * as React from 'https://esm.sh/react@18.2.0'
import { createRoot } from 'https://esm.sh/react-dom@18.2.0/client'
import dagre from 'https://esm.sh/dagre@0.8.5'
import {
  ReactFlow,
  Background,
  Controls,
  Handle,
  MarkerType,
  Position,
  useEdgesState,
  useNodesState,
} from 'https://esm.sh/@xyflow/react?deps=react@18.2.0,react-dom@18.2.0'
```

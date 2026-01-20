# HytaleGenerator Graphs

Learn how to configure graph-based world generation using `Server/HytaleGenerator/Graphs/`.

## Overview

Graphs define node-based structures for world generation. They're used for pathfinding, connections, and spatial relationships in world generation. Graphs connect positioned nodes with edges that define relationships and constraints.

## Example from Game Files

Graphs define node-based structures for world generation. They're used for pathfinding, connections, and spatial relationships in world generation. Graphs connect positioned nodes with edges that define relationships and constraints.

## Location

`Server/HytaleGenerator/Graphs/`

## Basic Graph Structure

`Server/HytaleGenerator/Graphs/ExampleGraph.json`:

```json
{
  "$Position": {
    "$x": 308,
    "$y": 269
  },
  "Type": "Static",
  "ExportAs": "ExampleGraph",
  "Skip": false,
  "Nodes": [
    {
      "$Position": {
        "$x": 987,
        "$y": 111
      },
      "Type": "Positioned",
      "Id": "A",
      "Position": {
        "X": 0,
        "Y": 0,
        "Z": 0
      },
      "Payloads": [
        {
          "Type": "Distance",
          "MaxDistance": 40
        }
      ]
    },
    {
      "$Position": {
        "$x": 976,
        "$y": 522
      },
      "Type": "Positioned",
      "Id": "B",
      "Position": {
        "X": 100,
        "Y": 0,
        "Z": 0
      },
      "Payloads": [
        {
          "Type": "Distance",
          "MaxDistance": 40
        }
      ]
    }
  ],
  "Edges": [
    {
      "$Position": {
        "$x": 969,
        "$y": 818
      },
      "Type": "Anchored",
      "Id": "AB",
      "Nodes": ["A", "B"],
      "Payloads": [
        {
          "Type": "Distance",
          "MaxDistance": 40
        }
      ]
    }
  ],
  "$WorkspaceID": "Worldgen GraphProvider"
}
```

## Graph Properties

### Type

Graph type:

```json
{
  "Type": "Static"
}
```

**Values:**
- **`"Static"`** - Static graph with fixed nodes and edges

### ExportAs

Export name for reference:

```json
{
  "ExportAs": "ExampleGraph"
}
```

### Skip

Whether to skip this graph:

```json
{
  "Skip": false
}
```

## Nodes

### Positioned Node

Node with a specific world position:

```json
{
  "Type": "Positioned",
  "Id": "A",
  "Position": {
    "X": 0,
    "Y": 0,
    "Z": 0
  },
  "Payloads": [
    {
      "Type": "Distance",
      "MaxDistance": 40
    }
  ]
}
```

**Properties:**
- **`Id`** - Unique node identifier
- **`Position`** - World position (X, Y, Z)
- **`Payloads`** - Array of payload configurations
  - **`Type`** - Payload type (e.g., `"Distance"`)
  - **`MaxDistance`** - Maximum distance for connections

### Node Payloads

#### Distance Payload

Defines maximum connection distance:

```json
{
  "Type": "Distance",
  "MaxDistance": 40
}
```

**Properties:**
- **`MaxDistance`** - Maximum distance for edge connections

## Edges

### Anchored Edge

Edge connecting two nodes:

```json
{
  "Type": "Anchored",
  "Id": "AB",
  "Nodes": ["A", "B"],
  "Payloads": [
    {
      "Type": "Distance",
      "MaxDistance": 40
    }
  ]
}
```

**Properties:**
- **`Id`** - Unique edge identifier
- **`Nodes`** - Array of node IDs to connect (typically 2)
- **`Payloads`** - Array of edge payloads
  - **`Type`** - Payload type (e.g., `"Distance"`)
  - **`MaxDistance`** - Maximum distance constraint

### Edge Types

- **`"Anchored"`** - Fixed edge between nodes
- Other types may exist for different connection methods

## Creating Custom Graphs

### Step 1: Create Graph File

Create `Server/HytaleGenerator/Graphs/MyCustom_Graph.json`:

```json
{
  "Type": "Static",
  "ExportAs": "MyCustom_Graph",
  "Skip": false,
  "Nodes": [
    {
      "Type": "Positioned",
      "Id": "Node1",
      "Position": {
        "X": 0,
        "Y": 0,
        "Z": 0
      },
      "Payloads": [
        {
          "Type": "Distance",
          "MaxDistance": 50
        }
      ]
    },
    {
      "Type": "Positioned",
      "Id": "Node2",
      "Position": {
        "X": 100,
        "Y": 0,
        "Z": 100
      },
      "Payloads": [
        {
          "Type": "Distance",
          "MaxDistance": 50
        }
      ]
    }
  ],
  "Edges": [
    {
      "Type": "Anchored",
      "Id": "Edge1",
      "Nodes": ["Node1", "Node2"],
      "Payloads": [
        {
          "Type": "Distance",
          "MaxDistance": 50
        }
      ]
    }
  ]
}
```

### Step 2: Reference in World Generation

Reference in world generation configs that use graph providers.

## Graph Use Cases

### Pathfinding Networks

Create node networks for NPC pathfinding:

```json
{
  "Nodes": [
    {
      "Type": "Positioned",
      "Id": "Village_Center",
      "Position": {"X": 0, "Y": 100, "Z": 0},
      "Payloads": [{"Type": "Distance", "MaxDistance": 100}]
    },
    {
      "Type": "Positioned",
      "Id": "Village_Entrance",
      "Position": {"X": 50, "Y": 100, "Z": 50},
      "Payloads": [{"Type": "Distance", "MaxDistance": 100}]
    }
  ],
  "Edges": [
    {
      "Type": "Anchored",
      "Id": "Path1",
      "Nodes": ["Village_Center", "Village_Entrance"],
      "Payloads": [{"Type": "Distance", "MaxDistance": 100}]
    }
  ]
}
```

### Structure Connections

Connect prefab structures:

```json
{
  "Nodes": [
    {
      "Type": "Positioned",
      "Id": "Building_A",
      "Position": {"X": 0, "Y": 100, "Z": 0}
    },
    {
      "Type": "Positioned",
      "Id": "Building_B",
      "Position": {"X": 50, "Y": 100, "Z": 0}
    }
  ],
  "Edges": [
    {
      "Type": "Anchored",
      "Id": "Connection_AB",
      "Nodes": ["Building_A", "Building_B"]
    }
  ]
}
```

## Tips

1. **Node IDs** - Use descriptive, unique IDs for nodes
2. **Position Accuracy** - Set precise world coordinates
3. **Distance Constraints** - Use `MaxDistance` to limit connection ranges
4. **Edge Connections** - Connect nodes that should be linked
5. **Export Names** - Export graphs you'll reference elsewhere
6. **Static Graphs** - Use `"Static"` type for fixed graphs
7. **Payload Configuration** - Configure payloads appropriately for your use case

---

**Previous:** [HytaleGenerator Density](164_HytaleGenerator_Density.md) | **Next:** [HytaleGenerator BlockMasks](166_HytaleGenerator_BlockMasks.md)

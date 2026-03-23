---
name: larkmap-dev
description: 'LarkMap open-source developer skill containing comprehensive documentation references and component code examples. Use when you need to write new code, fix bugs, or understand the architecture of the LarkMap repo. Includes guidelines for adding new layers, controls, demos, and working with core hooks and types. Triggers: "add layer", "create control", "how to write a demo", "LarkMap architecture".'
argument-hint: 'Describe what feature or bug fix you want to implement in LarkMap'
---

# LarkMap Developer Skill

LarkMap is a highly customizable React + TypeScript map component library built on `@antv/l7` and `@antv/l7-composite-layers`. 

This skill package provides you with the complete API documentation bundled as **references**, and typical component templates bundled as **examples**, allowing secondary developers or contributors to quickly catch up with the codebase style and project requirements.

## 📂 Bundled Resources

### References (Documentation)
The original `docs/` folder of the repository has been packaged here. Refer to these files when you need to understand component usage or design guidelines:
- **Design Guidelines**: [references/guide/design.md](./references/guide/design.md)
- **Common Layer Events**: [references/common/layer/base-common/event.md](./references/common/layer/base-common/event.md)
- **Component Specific Docs**: e.g., HeatmapLayer docs at `./references/common/layer/heatmap-layer/`
- **Demos Examples**: Interactive use-cases at `./references/examples/`

### Examples (Code Patterns)
We have selected core components from the `src/` directory to serve as standard implementation templates. **Always refer to these examples when writing new features** to match the codebase's conventions exactly.

1. **BaseLayer Implementation (`HeatmapLayer`)**
   Use this when implementing any new `BaseLayer` or `CompositeLayer`:
   - Main Component: [examples/HeatmapLayer/index.tsx](./examples/HeatmapLayer/index.tsx)
   - Types Extension: [examples/HeatmapLayer/types.ts](./examples/HeatmapLayer/types.ts)
   - Standard Demo File: [examples/HeatmapLayer/demos/default.tsx](./examples/HeatmapLayer/demos/default.tsx)

2. **Control Implementation (`CustomControl`)**
   Use this when creating map controls that produce React portal outputs:
   - Control Component: [examples/CustomControl/index.tsx](./examples/CustomControl/index.tsx)
   - Control Types: [examples/CustomControl/types.ts](./examples/CustomControl/types.ts)

3. **Core Layer Hooks**
   Understand layer lifecycle, instantiation, and event bindings:
   - `useCreateLayer`: [examples/layer-hooks/use-create-layer/index.ts](./examples/layer-hooks/use-create-layer/index.ts)
   - `useLayerEvent`: [examples/layer-hooks/use-layer-event/index.ts](./examples/layer-hooks/use-layer-event/index.ts)

4. **Global Types**
   - Core Type Definitions (`LayerEventProps`, etc.): [examples/types/layer.ts](./examples/types/layer.ts)

## 🛠️ Development Workflow

### Adding a New Layer
1. Decide if it's a `BaseLayer` or `CompositeLayer`.
2. Review the reference docs and [HeatmapLayer Example](./examples/HeatmapLayer/index.tsx).
3. Create `types.ts` extending `XxxLayerOptions` and `LayerCommonProps`.
4. Create `index.tsx` using `memo(forwardRef(...))`. Ensure to use `useCreateLayer` synchronously and return `null`.
5. Map specific L7 events leveraging `LayerEventMap`.
6. Add exports to `src/components/Layers/index.ts` maintaining alphabetical ordering.

### Adding a Control Component
1. Controls do not return `null`; they utilize instances via `useState`.
2. Look at the [CustomControl Example](./examples/CustomControl/index.tsx).
3. Extract properties safely before passing to the L7 control constructor using `omitBy(options, isUndefined)`.
4. Manage React nodes via hooks like `useL7ComponentUpdate` and `useL7ComponentEvent`.

### Best Practices & Rules
- **No Default Component Exports**: Always use named exports (e.g. `export const MyLayer`).
- **Type Exports**: Always use `export type { XxxProps } from './types'` to be safe with Babel's isolated modules.
- **Hook Lifecycle**: Ensure layers are created *synchronously* in initialization, not asynchronously within a `useEffect`, to prevent potential missing sync events prior to commit phase.
- **Demo Requirements**: Demo definitions (`demos/default.tsx`) must `export default () => { ... }` as an anonymous arrow function for `dumi` compatibility. Imports inside demos should appear to be from `@antv/larkmap`.

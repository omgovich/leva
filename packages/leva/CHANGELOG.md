# leva

## 0.9.5

### Patch Changes

- a2db0e6: Improve `buttonGroup` API.

  The label is now completely hidden when specifying key that only includes spaces. Previously the label was still rendered, but without text, this caused ugly spacing when using the `oneLineLabels` option on the `Leva` component.

  ```ts
  const [values, set] = useControls(() => ({
    Size: 1,
    ' ': buttonGroup({
      '0.25x': () => set({ Size: 0.25 }),
      '0.5x': () => set({ Size: 0.5 }),
      '1x': () => set({ Size: 1 }),
      '2x': () => set({ Size: 2 }),
      '3x': () => set({ Size: 3 }),
    }),
  }))
  ```

  It is now possible to set the `label` via the `buttonGroup` arguments by using the alternative API:

  ```ts
  const [values, set] = useControls(() => ({
    Width: 1,
    WidthPresets: buttonGroup({
      label: null,
      opts: {
        '0.25x': () => set({ Size: 0.25 }),
        '0.5x': () => set({ Size: 0.5 }),
        '1x': () => set({ Size: 1 }),
        '2x': () => set({ Size: 2 }),
        '3x': () => set({ Size: 3 }),
      },
    }),
    Height: 1,
    HeightPresets: buttonGroup({
      label: null,
      opts: {
        '0.25x': () => set({ Size: 0.25 }),
        '0.5x': () => set({ Size: 0.5 }),
        '1x': () => set({ Size: 1 }),
        '2x': () => set({ Size: 2 }),
        '3x': () => set({ Size: 3 }),
      },
    }),
  }))
  ```

  This also allows passing any JSX element as the label beside strings.

  It also helps avoiding a bunch of `` labels (where each new one contains one more space).

## 0.9.4

### Patch Changes

- 50e8096: fix: sanitize step should behave better.
  improvement: expand panel when filter changes.
- 09a1a38: Allow specifying the explicit input type via the `type` option. This is handy when you want to prevent your string value being casted to a color or number.

  ```tsx
  import { LevaInputs, useControls } from 'leva'

  useControls({
    color: {
      type: LevaInputs.STRING,
      value: '#f00',
    },
    number: {
      type: LevaInputs.STRING,
      value: '1',
    },
  })
  ```

## 0.9.3

### Patch Changes

- 7edbb69: chore: update dependencies.

## 0.9.2

### Patch Changes

- 34281d7: Add new value option `transient`. This allows opting out of the transient mode when having a `onChange` handler invoked.

  ```tsx
  const data = useControls({
    color: {
      value: '#7c3d3d',
      onChange: (value) => {
        console.log(value)
      },
      transient: false,
    },
  })

  console.log(data) // { color: '#7c3d3d' }
  ```

  ```tsx
  const data = useControls({
    color: {
      value: '#7c3d3d',
      onChange: (value) => {
        console.log(value)
      },
      transient: true,
    },
  })

  console.log(data) // {}
  ```

  This is handy if you want to use the `onChange` handler for triggering a save on a remote server, while still triggering a re-render with the value.

## 0.9.1

### Patch Changes

- 09ac7b1: chore: remove `clipboard-polyfill` dependency.

## 0.9.0

### Patch Changes

- 50d850a: BREAKING CHANGE: Replace `hideTitleBar` with `titleBar` option.

  For hiding the title bar the usages of `<Leva hideTitleBar />` must be replaced with `<Leva titleBar={false} />`.

  It is now possible to overwrite the six dots rendered as the title by default by providing a `title` option to the `titleBar` property.

  ```tsx
  <Leva
    titleBar={{
      title: 'Some Title',
    }}
  />
  ```

  Its is now possible to disable dragging of the panel via the `drag` option to the `titleBar` property.

  ```tsx
  <Leva
    titleBar={{
      drag: false,
    }}
  />
  ```

  It is now possible to enable or disable filtering of the panel values via the `filter` option on the `titleBar` property.

  ```tsx
  <Leva
    titleBar={{
      filter: true,
    }}
  />
  ```

## 0.8.4

### Patch Changes

- e0fdefc: types: fix beautifier union type.

## 0.8.3

### Patch Changes

- a7f9bf1: types: don't use union type when not using objects as plugin function args.

## 0.8.2

### Patch Changes

- 7fd9f92: feat: allow input options to be spread inside custom plugin.
- b4aa43d: Fix: add empty key warning.
- 7fd9f92: fix: correct onUpdate for a blurred input: previously bluring an input from a
  store while selecting a second store would commit the change on the second
  store.

  fix: return number previous value when field is empty.

  types: (internal) fix default useInputContext types.

- e21f2fe: fix: slider position overflowing with range input.

## 0.8.1

### Patch Changes

- c997410: Plugin: add the Bezier plugin

  ```js
  import { bezier } from '@leva-ui/plugin-bezier'
  useControls({ curve: bezier([0.25, 0.1, 0.25, 1]) })
  ```

## 0.8.0

### Minor Changes

- edc8847: Breaking: change how `leva/plugin` exports components.

  ```jsx
  // before
  import { Row, Label, String } from 'leva/plugin'

  // after
  import { Components } from 'leva/plugin'
  const { Row, Label, String } = Components
  ```

  Feat: add `useValue` / `useValues` hooks that let an input query other inputs values.

  Feat: `normalize` has additional arguments to its signature:

  ```ts
  /**
   * @path the path of the input
   * @data the data available in the store
   */
  const normalize = (input: Input, path: string, data: Data)
  ```

  Feat: `sanitize` has additional arguments to its signature:

  ```ts
  /**
   * @path the path of the input
   * @store the store
   */
  const sanitize = (
    value: any,
    settings: Settings,
    prevValue: any,
    path: string,
    store: StoreType
  )
  ```

  Styles: better feedback when dragging number from inner label.

  Plugin: add the Plot plugin 📈

  ```js
  import { plot } from '@leva-ui/plugin-plot'
  useControls({ y: plot({ expression: 'cos(x)', graph: true, boundsX: [-10, 10], boundsY: [0, 100] }) })
  ```

## 0.7.3

### Patch Changes

- f9f7f1e: - Have `Select` accept functions as value or options. Fixes #165.
  - Temporarily type the `set` function values with `any` when `useControls` is used with the function API. In the future, we should infer th value type from the sanitize function.

## 0.7.2

### Patch Changes

- d2eaf58: fix: properly resolves value-keyed objects for the `Select` input.

## 0.7.1

### Patch Changes

- 98984e1: types: fix `set` types in transient mode.

## 0.7.0

### Minor Changes

- 0b7e968: Add the `invertY` setting for the Vector2D joystick for inverting the y coordinate.

  **BREAKING**: The default behavior has been changed. If you want the same behavior as in previous versions you will have to set the `joystick` option to `'invertY'`.

  ```tsx
  const values = useControl({
    vector2d: {
      value: [0, 0],
      joystick: 'invertY',
    },
  })
  ```

### Patch Changes

- f323cfc: Feat: `onChange` callback for transient updates

  ```js
  useControls({ color: { value: 'red', onChange: (v) => console.log(v) } })
  ```

## 0.6.3

### Patch Changes

- fa909f0: Allow useControls to update settings and input options when dependency array changes. (See #124)

## 0.6.2

### Patch Changes

- 72bcebe: Reject null Vectors

## 0.6.1

### Patch Changes

- 66dcb79: Add id to root div so that we're sure to always use the same root `div`. This should fix #106.
- c6a69e3: Fix scrollbars flashing when collapsing a folder. Fixes #139.
- 9e32b2c: Prevent errors in node environments by aliasing stitches global identifier to \_global as global indentifier is reserved

## 0.6.0

### Minor Changes

- ce42683: Add `hideCopyButton` property for hiding the copy to clipboard button.

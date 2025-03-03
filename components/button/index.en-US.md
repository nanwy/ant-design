---
category: Components
title: Button
cover: https://mdn.alipayobjects.com/huamei_7uahnr/afts/img/A*BrFMQ5s7AAQAAAAAAAAAAAAADrJ8AQ/original
coverDark: https://mdn.alipayobjects.com/huamei_7uahnr/afts/img/A*Lp1kTYmSsgoAAAAAAAAAAAAADrJ8AQ/original
demo:
  cols: 2
group:
  title: General
  order: 1
---

To trigger an operation.

## When To Use

A button means an operation (or a series of operations). Clicking a button will trigger corresponding business logic.

In Ant Design we provide 5 types of button.

- Primary button: indicate the main action, one primary button at most in one section.
- Default button: indicate a series of actions without priority.
- Dashed button: commonly used for adding more actions.
- Text button: used for the most secondary action.
- Link button: used for external links.

And 4 other properties additionally.

- `danger`: used for actions of risk, like deletion or authorization.
- `ghost`: used in situations with complex background, home pages usually.
- `disabled`: when actions are not available.
- `loading`: add loading spinner in button, avoiding multiple submits too.

## Examples

<!-- prettier-ignore -->
<code src="./demo/basic.tsx">Type</code>
<code src="./demo/icon.tsx">Icon</code>
<code src="./demo/debug-icon.tsx" debug>Debug Icon</code>
<code src="./demo/debug-block.tsx" debug>Debug Block</code>
<code src="./demo/size.tsx">Size</code>
<code src="./demo/disabled.tsx">Disabled</code>
<code src="./demo/loading.tsx">Loading</code>
<code src="./demo/multiple.tsx">Multiple Buttons</code>
<code src="./demo/ghost.tsx">Ghost Button</code>
<code src="./demo/danger.tsx">Danger Buttons</code>
<code src="./demo/block.tsx">Block Button</code>
<code src="./demo/legacy-group.tsx" debug>Deprecated Button Group</code>
<code src="./demo/chinese-chars-loading.tsx" debug>Loading style bug</code>
<code src="./demo/component-token.tsx" debug>Component Token</code>

## API

Common props ref：[Common props](/docs/react/common-props)

Different button styles can be generated by setting Button properties. The recommended order is: `type` -> `shape` -> `size` -> `loading` -> `disabled`.

| Property | Description | Type | Default | Version |
| --- | --- | --- | --- | --- |
| block | Option to fit button width to its parent width | boolean | false |  |
| classNames | Semantic DOM class | Record<SemanticDOM, string> | - | 5.4.0 |
| danger | Set the danger status of button | boolean | false |  |
| disabled | Disabled state of button | boolean | false |  |
| ghost | Make background transparent and invert text and border colors | boolean | false |  |
| href | Redirect url of link button | string | - |  |
| htmlType | Set the original html `type` of `button`, see: [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button#attr-type) | string | `button` |  |
| icon | Set the icon component of button | ReactNode | - |  |
| loading | Set the loading status of button | boolean \| { delay: number } | false |  |
| shape | Can be set button shape | `default` \| `circle` \| `round` | `default` |  |
| size | Set the size of button | `large` \| `middle` \| `small` | `middle` |  |
| styles | Semantic DOM style | Record<SemanticDOM, CSSProperties> | - | 5.4.0 |
| target | Same as target attribute of a, works when href is specified | string | - |  |
| type | Set button type | `primary` \| `dashed` \| `link` \| `text` \| `default` | `default` |  |
| onClick | Set the handler to handle `click` event | (event: MouseEvent) => void | - |  |

It accepts all props which native buttons support.

### `styles` and `classNames` attribute

| Property | Description       | Version |
| -------- | ----------------- | ------- |
| icon     | set `icon`element | 5.5.0   |

## Design Token

<ComponentTokenTable component="Button"></ComponentTokenTable>

## FAQ

### How to remove space between 2 chinese characters?

Following the Ant Design specification, we will add one space between if Button (exclude Text button and Link button) contains two Chinese characters only. If you don't need that, you can use [ConfigProvider](/components/config-provider/#api) to set `autoInsertSpaceInButton` as `false`.

```tsx
<ConfigProvider autoInsertSpaceInButton={false}>
  <Button>按钮</Button>
</ConfigProvider>
```

<img src="https://gw.alipayobjects.com/zos/antfincdn/MY%26THAPZrW/38f06cb9-293a-4b42-b183-9f443e79ffea.png" width="100px" height="64px" style="box-shadow: none; margin: 0;" alt="Button with two Chinese characters" />

<style>
.site-button-ghost-wrapper {
  padding: 16px;
  background: rgb(190, 200, 200);
}
</style>

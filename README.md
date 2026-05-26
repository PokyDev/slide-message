# slide-message

<img src="assets/slideMessage-logo.png" alt="slide-message logo" width="120" align="right" />

**A lightweight React notification system with slide animations, variant support, and themeable CSS custom properties.**

![npm version](https://img.shields.io/npm/v/slide-message)
![license](https://img.shields.io/npm/l/slide-message)
![bundle size](https://img.shields.io/bundlephobia/minzip/slide-message)

---

## Features

- **Slide animations** — direction-aware enter/exit animations for 4 corner positions.
- **One message at a time** — no notification stacks; the next call is silently ignored while a message is active.
- **Variant system** — built-in message variants with icon, label, and default text. Easily extensible.
- **Themeable** — all colors, radii, spacing, and timing are CSS custom properties with `--sm-` prefix and sane dark-mode defaults.
- **Zero dependencies** — only `react` and `react-dom` as peer dependencies.
- **Accessible** — `role="status"`, `aria-live="polite"`, `aria-atomic="true"`, and `prefers-reduced-motion` support.

---

## Installation

```bash
# npm
npm install slide-message

# pnpm
pnpm add slide-message

# yarn
yarn add slide-message
```

---

## Quick Start

```jsx
import { SlideMessageProvider, useSlideMessage } from 'slide-message';
import 'slide-message/dist/slide-message.css';

function App() {
  return (
    <SlideMessageProvider>
      <MyButton />
    </SlideMessageProvider>
  );
}

function MyButton() {
  const { notify } = useSlideMessage();

  return (
    <button onClick={() => notify({ position: 'top-right' })}>
      Notify me
    </button>
  );
}
```

> **Important:** import `slide-message/dist/slide-message.css` once in your app's entry point. `<SlideMessageProvider>` must be mounted only once, at the root of your component tree.

---

## API Reference

### `<SlideMessageProvider>`

The context provider. Wrap your app with it **once** at the root.

| Prop | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `children` | `ReactNode` | Yes | — | Your application tree. |

### `useSlideMessage()`

Hook that exposes the `notify` function.

```js
const { notify } = useSlideMessage();
```

#### `notify(params)`

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `message` | `string` | `'emptyState'` | Variant key. See [Variants](#variants). |
| `text` | `string` | `undefined` | Custom text. If omitted, the variant's default text is used. |
| `position` | `string` | `'top-left'` | Corner position: `top-left`, `top-right`, `bottom-left`, `bottom-right`. |
| `duration` | `number` | `2000` | Visible time in ms (excluding enter/exit animations). |
| `offsetX` | `number` | `20` | Distance in px from the nearest horizontal edge. |
| `offsetY` | `number` | `20` | Distance in px from the nearest vertical edge. |

---

## Variants

Available variants in `v1.0.0`:

| Key | Label | Default text | Description |
|-----|-------|--------------|-------------|
| `emptyState` | En Desarrollo | Funcionalidad en Desarrollo | For unimplemented features or buttons. |

> Import `MESSAGE_VARIANTS` to inspect variant keys programmatically:
>
> ```js
> import { MESSAGE_VARIANTS } from 'slide-message';
> ```

---

## Theming

All visual tokens are CSS custom properties with the `--sm-` prefix and dark-mode defaults. Override them in `:root` or any parent selector:

```css
:root {
  --sm-bg-elevated: #ffffff;
  --sm-border-default: #e0e0e0;
  --sm-text-color: #111111;
  --sm-label-color: #666666;
  --sm-accent-color: #6366f1;
  --sm-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  --sm-progress-bg: #f0f0f0;
}
```

### Full variable reference

| Variable | Default | Description |
|----------|---------|-------------|
| `--sm-bg-elevated` | `#222222` | Card background color. |
| `--sm-border-default` | `#3D3D3D` | Card border color. |
| `--sm-shadow` | `0 4px 12px rgba(0,0,0,0.4)` | Card box-shadow. |
| `--sm-radius` | `6px` | Card border-radius. |
| `--sm-radius-sm` | `4px` | Icon border-radius. |
| `--sm-gap` | `12px` | Gap between icon and text body. |
| `--sm-padding-y` | `16px` | Card vertical padding. |
| `--sm-padding-x` | `20px` | Card horizontal padding. |
| `--sm-body-gap` | `4px` | Gap between label and text. |
| `--sm-font` | `system-ui, sans-serif` | Font family for label and text. |
| `--sm-label-color` | `#9A9A9A` | Label (uppercase) text color. |
| `--sm-text-color` | `#F0F0F0` | Body text color. |
| `--sm-progress-bg` | `#2E2E2E` | Progress bar track color. |
| `--sm-accent-color` | `#D4AF37` | Accent color (icon, label, progress bar). |
| `--sm-ease` | `cubic-bezier(0.16,1,0.3,1)` | Enter animation easing. |

---

## Behavior

Only **one message is shown at a time**. While a message is on screen (including its enter and exit animations), any calls to `notify()` are silently ignored. This is intentional to prevent notification accumulation.

---

## Accessibility

- The root container uses `role="status"` with `aria-live="polite"` and `aria-atomic="true"` so screen readers announce message changes.
- Animation-only elements (icon, progress bar) use `aria-hidden="true"`.
- Respects `prefers-reduced-motion`: when enabled, all animations are disabled and exiting messages immediately become transparent.

---

## License

MIT
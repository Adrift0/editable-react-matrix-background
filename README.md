# CodeBackground

A lightweight, interactive Matrix-style code-rain background for React. Made with care for Billy, and ready to make itself at home on any website.

![Animated CodeBackground preview](./demo/code-background.gif)

## Try the demo

Requires Node.js 18 or newer.

```bash
npm install
npm run dev
```

Open the local URL printed by Vite. The demo lets you preview the effect, change its color, adjust smoothness (FPS) and fall speed independently, pause the animation, and copy a starter snippet.

## Build the library

```bash
npm run typecheck
npm run build
```
 React is a peer dependency when the component is used by another project.

To build the static demo or recreate its animated presentation:

```bash
npm run build:demo
npm run demo:gif
```

`npm run build:demo` writes the site to `demo-dist/`. To install a local library build into another project, run `npm pack` here and install the resulting `.tgz` file there with `npm install <path-to-tarball>`.

## Add it to a React website

Install this package from its published registry or use the generated package tarball. Then place the component inside a container that establishes its size and positioning context:

```tsx
import { CodeBackground } from 'code-background';

export function Hero() {
  return (
    <section className="hero">
      <CodeBackground color="#b5f36c" pointerInfluence={0.8} />
      <div className="hero-content">
        <h1>Billynflex clothing shop</h1>
      </div>
    </section>
  );
}
```

```css
.hero {
  position: relative;
  min-height: 24rem;
  overflow: hidden;
  isolation: isolate;
}

.hero-content {
  position: relative;
  z-index: 1;
}
```

The canvas automatically fills its parent, watches it for size changes, and renders at the devices pixel ratio for sharp output on high-density screens. The parent controls the displayed size: use it whereever, shit works. Keep meaningful page content sommewhere above the background canvas.

The parent can be a full-screen hero, a narrow shop banner, a product card, or any other element with a size. Set its height with CSS (fixed, viewport-based, or responsive); the background follows width and height changes and stays sharp on high-density displays. This package is for React sites; a non-React site would need a small framework-specific wrapper.

## Component options

| Option | Default | Description |
| --- | --- | --- |
| `color` | `#00ff41` | Character color |
| `background` | `rgba(0, 0, 0, 0.05)` | Translucent frame fade; lower opacity leaves longer trails |
| `fontSize` | `14` | Character size and column spacing in CSS pixels |
| `fps` | `30` | Frame-rate cap for animation smoothness; does not control fall speed |
| `speed` | `1` | Fall-speed multiplier; `1` is the default speed, `0.5` is half speed, and `2` is double speed |
| `pointerInfluence` | `0.6` | Cursor interaction strength from `0` to `1` |
| `pointerRadius` | `120` | Cursor interaction radius in CSS pixels |
| `paused` | `false` | Pause the animation |
| `chars` | Latin letters, digits, katakana | Characters used for the rain |
| `className` | — | Class applied to the canvas |
| `style` | — | Inline canvas styles |
| `ariaLabel` | — | accessible name; otherwise the decorative canvas is hidden from assistive technology |

For awareness: 
The animation respects the systems reduced-motion preference. If reduced motion is enabled, it stays paused until the preference changes.

## Hook

For custom layouts, use the hook directly:

```tsx
import { useCodeBackground } from 'code-background';

function CustomRain() {
  const { canvasRef, state, setRunning } = useCodeBackground({
    color: '#7fe7d3',
    fps: 24,
    speed: 0.8,
  });

  return (
    <div style={{ position: 'relative', height: '20rem' }}>
      <canvas ref={canvasRef} aria-hidden="true" />
      <button onClick={() => setRunning(!state.isRunning)}>
        {state.isRunning ? 'Pause' : 'Resume'}
      </button>
    </div>
  );
}
```

The hook returns `canvasRef`, `state` (`isRunning`, `prefersReducedMotion`, `width`, `height`), and `setRunning`.

## License

MIT

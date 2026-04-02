# Two-Step Equation

Vite + React lesson for solving a **linear two-step equation** where the variable term is a **unit fraction of x** (e.g. `x/3 + b = c`). **Step 1** is a full drag-and-pointer interaction with Flexi copy and CSS-driven equation transforms; **Step 2** is currently a **placeholder** shell (title + Back/Next only).

**Public URL (Content Interactives):** [https://content-interactives.github.io/two-step-equation/](https://content-interactives.github.io/two-step-equation/)

---

## Stack

| Layer | Notes |
|--------|--------|
| Build | Vite 7, `@vitejs/plugin-react` |
| UI | React 19 |
| Styling | Tailwind CSS 3, PostCSS; feature CSS under `src/components/reused-animations/` (`strike`, `shift`, `simplify`, `fade`, `scale`, `glow`) |
| Lint | ESLint 9 |
| Deploy | `gh-pages -d dist` (`predeploy` runs `build`). `package.json` **`deploy`** script targets a **hard-coded** remote repo URL—verify before publishing. |

---

## `base` path vs live URL

`vite.config.js` sets:

```js
base: '/2step/'
```

The README link above uses the path **`/two-step-equation/`**. If assets 404 on GitHub Pages, **`base`** must match the **actual repository name** (or the Pages project path). Align `base`, the GitHub repo slug, and any CK-12 embed URL.

---

## Source layout

```
index.html
vite.config.js, tailwind.config.js, postcss.config.js, eslint.config.js
src/
  main.jsx              # createRoot → <App />
  App.jsx               # Renders <DistributiveProperty /> only
  distributive-property/
    DistributiveProperty.jsx   # Step router: expression state, reset, Step1 / Step2
    Step1.jsx           # Main interactive (large component)
    Step2.jsx           # Stub UI
    utils.js            # generateTwoStepEquation, legacy distributive helpers
  components/
    reused-ui/          # Container, GlowButton, FlexiText, Input, NavButtons
    reused-animations/  # Shared CSS animation classes
    ComponentsMaker.jsx # UI demo sandbox (not mounted by App.jsx)
  assets/All Flexi Poses/   # PNG (Step1) and SVG (ComponentsMaker)
```

The root component name **`DistributiveProperty`** and Step 2 title **"Distributive Property - Step 2"** are **legacy naming**; runtime focus is **two-step equations** from `generateTwoStepEquation`.

---

## Equation generation (`utils.js`)

- Picks random **`b`, `c`** in **[-10, 10]** (then **`c`** is adjusted for an integer **`x`**).
- Coefficient **`a`** is always **`1/n`** for **`n ∈ {2,…,10}`**, so the left side is **`x/n + b = adjustedC`**.
- Returns `{ a, b, c, x, denominator, expression, ... }` with a display string from **`formatTwoStepEquation`**.
- **`generateDistributiveExpression`** / **`calculateDistributiveProperty`** are unused by the current `App` flow but kept in the same module.

---

## Step flow

1. **`DistributiveProperty`** holds **`currentStep`** (1 or 2), **`expression`**, and **`handleReset`** (back to step 1 + new equation).
2. **`Step1`** receives **`expression`** and **`onNext` / `onReset`**. It implements guided messaging (**`flexiMessages`**), drag state for moving terms across the equation, denominator drag, staged “simplify” animations, and forward/back navigation through message indices with **`applyEquationState`**-style syncing (see component for full state machine).
3. **`Step2`** renders **`Container`** with empty main area and **GlowButton** Back/Next; **`onNext`** increments step (falls through to “Step not found” div if pressed again—edge case).

Pointer handling uses a shared **`getClient`** helper for mouse + touch.

---

## Dependencies

`package.json` includes **`react-arrow`** and **`react-xarrows`**, but there are **no imports** under `src/`. Safe to remove if you confirm no dynamic use.

---

## Scripts

| Command | Purpose |
|---------|---------|
| `npm install` | Install dependencies |
| `npm run dev` | Vite dev server |
| `npm run build` | Output to `dist/` |
| `npm run preview` | Serve production build locally |
| `npm run lint` | ESLint |
| `npm run deploy` | Build + push `dist` via gh-pages to configured remote |

---

## Embedding

- Step 1 expects substantial vertical space (Flexi + equation + controls inside **`Container`**).
- No LMS or `postMessage` integration documented.

---

## Prior curriculum notes (from legacy README)

Common Core references that were listed for this applet: **7.EE.B.4**, **8.EE.C.7**, **8.EE.C.7.a**, **8.EE.C.7.b**. CK-12 production/master links were marked pending; confirm current embeds in your CMS.

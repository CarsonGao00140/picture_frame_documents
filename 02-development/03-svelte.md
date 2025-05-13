### index.html (From ! template)

```diff
  ...
  <body>
+     <div id="app"></div>
+     <script type="module" renderer="/renderer/main.ts"></script>
  <body>
  ...

```

### renderer/main.ts

```typescript
import { mount } from 'svelte'
import './app.css'
import App from './App.svelte'

const app = mount(App, {
    target: document.getElementById('app')!,
})

export default app

```

### renderer/app.css

```css
:root {
    font-family: 'Inter', sans-serif;
}

* {
    margin: 0;
    padding: 0;
    border: none;
    font-family: inherit;
    user-select: none;
}

html {
    height: 100%;
    overflow: hidden;
}

```

### global.d.ts

```typescript
declare module 'svelte-tiny-router';

```

### renderer/App.svelte

```html
<script lang="ts">
    import { Router, Route } from 'svelte-tiny-router';
    import Settings from './pages/Settings.svelte'
    import Frame from './pages/Frame.svelte'
</script>

<main>
    <Router>
        <Route path="/" component={Settings} />
        <Route path="/frame" component={Frame} />
    </Router>
</main>

```

### renderer/components/Counter.svelte

```html
<script lang="ts">
    let count: number = $state(0)
    const increment = () => count += 1;
</script>

<button onclick={increment}>
    count is {count}
</button>

```

### renderer/pages/Settings.svelte

```html
<script lang="ts">
    import Counter from '../components/Counter.svelte';
</script>

<h1>Settings</h1>
<Counter />
<a href="/frame">Frame</a>

```

### renderer/pages/Frame.svelte

```html
<h1>Frame</h1>
<a href="/">Settings</a>

```

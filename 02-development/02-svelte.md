## VS Code Settings

### .vscode/extensions.json

```jsonc
{
    "recommendations": [
        "svelte.svelte-vscode",
        "ardenivanov.svelte-intellisense",
        "Chanzhaoyu.svelte-5-snippets"
    ]
}

```

## Dependencies
```bash
pnpm i -D svelte @sveltejs/vite-plugin-svelte svelte-tiny-router
```

## Configuration

### vite.svelte.config.ts

```typescript
import { defineConfig } from 'vite'
import { svelte } from '@sveltejs/vite-plugin-svelte'

export default defineConfig({
    root: 'renderer',
    build: {
        outDir: '../dist',
        target: 'esnext',
        modulePreload: {
            polyfill: false
        },
    },
    plugins: [
        svelte(),
    ]
})

```

### svelte.config.ts

```typescript
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

export default {
    preprocess: vitePreprocess(),
    compilerOptions: {
        runes: true
    }
}

```

### renderer/index.html (From ! template)

```diff
  <body>
+     <div id="app"></div>
+     <script type="module" renderer="/renderer/main.ts"></script>

```

### renderer/index.ts

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

### renderer/App.svelte

```svelte
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

### svelte/components/Counter.svelte

```html
<script lang="ts">
    let count: number = $state(0)
    const increment = () => count += 1;
</script>

<button onclick={increment}>
    count is {count}
</button>

```

### svelte/pages/Settings.svelte

```html
<script lang="ts">
    import Counter from '../components/Counter.svelte';
</script>

<h1>Settings</h1>
<Counter />
<a href="/frame">Frame</a>

```

### package.json

```diff
          "electron:dev": "vite build -w -c vite.electron.config -m development",
+ 		  "svelte:dev": "vite -c vite.svelte.config"
```

## Start

```bash
pnpm electron:dev
```

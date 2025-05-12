### index.html (From ! template)

```diff
  ...
  <body>
+     <div id="app"></div>
+     <script type="module" src="/src/main.ts"></script>
  <body>
  ...

```

### src/main.ts

```ts
import { mount } from 'svelte'
import './app.css'
import App from './App.svelte'

const app = mount(App, {
    target: document.getElementById('app')!,
})

export default app

```

### src/app.css

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

### src/App.svelte

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

### src/components/Counter.svelte

```html
<script lang="ts">
    let count: number = $state(0)
    const increment = () => {
        count += 1
    }
</script>

<button onclick={increment}>
    count is {count}
</button>

```

### src/pages/Settings.svelte

```html
<script lang="ts">
    import { navigate } from 'svelte-tiny-router';
    import Counter from '../components/Counter.svelte'
</script>

<h1>Settings</h1>
<Counter />
<button onclick={() => navigate('/frame')}>Frame</button>

```

### src/pages/Frame.svelte

```html
<script lang="ts">
    import { navigate } from 'svelte-tiny-router';
</script>

<h1>Frame</h1>
<button onclick={() => navigate('/')}>Settings</button>

```

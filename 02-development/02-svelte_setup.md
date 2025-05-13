## VS Code Settings

### .vscode/settings.json

```jsonc
{
    "typescript.tsdk": "node_modules/typescript/lib"
}

```

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
pnpm i -D typescript svelte svelte-tiny-router vite @sveltejs/vite-plugin-svelte concurrently wait-on

pnpm approve-builds

pnpm tsc --init
```

## Configuration

### package.json

```diff
      ...
      "scripts": {
-         "dev": "electron -r tsx ./main/index.ts ${SSH_CONNECTION:+--remote-debugging-port=9222 --remote-allow-origins=devtools://devtools}"
+         "dev": "concurrently --kill-others \"vite\" \"wait-on tcp:5173 && electron -r tsx ./main/index.ts ${SSH_CONNECTION:+--remote-debugging-port=9222 --remote-allow-origins=devtools://devtools}\""
      }
      ...

```

### tsconfig.json

```diff
  {
      ...
-     "target": "es2016",
+     "target": "ESNext",
      ...
-     "module": "commonjs",
+     "module": "NodeNext",
      ...
-     // "noEmit": true,
+     "noEmit": true,
      ...
-     // "noUnusedLocals": true,
-     // "noUnusedParameters": true,
-     // "exactOptionalPropertyTypes": true,
-     // "noImplicitReturns": true,
-     // "noFallthroughCasesInSwitch": true,
-     // "noUncheckedIndexedAccess": true,
-     // "noImplicitOverride": true,
-     // "noPropertyAccessFromIndexSignature": true,
+     "noUnusedLocals": true,
+     "noUnusedParameters": true,
+     "exactOptionalPropertyTypes": true,
+     "noImplicitReturns": true,
+     "noFallthroughCasesInSwitch": true,
+     "noUncheckedIndexedAccess": true,
+     "noImplicitOverride": true,
+     "noPropertyAccessFromIndexSignature": true,
      ...
  }

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

### vite.config.ts

```typescript
import { defineConfig } from 'vite'
import { svelte } from '@sveltejs/vite-plugin-svelte'

export default defineConfig({
    plugins: [
        svelte()
    ]
})

```

### main/index.ts

```diff
  import { app, BrowserWindow } from 'electron';
- import { cpus } from 'os';
+ import { cpus, platform } from 'os';
  ...
  app.whenReady().then(() => {
-     const window = new BrowserWindow({});
+     const window = new BrowserWindow({
+         kiosk: platform() === 'linux',
+     });

-     window.loadURL('chrome://gpu');
+     if (app.isPackaged) {
+         window.loadFile('dist/index.html');
+     } else {
+         window.loadURL('http://localhost:5173');
+         if (process.env.SSH_CONNECTION) return;
+         window.webContents.openDevTools();
+     }
  })
```

## VS Code Settings

### .vscode/launch.json

```jsonc
{
    "configurations": [
        {
            "name": "dev script",
            "type": "node",
            "request": "launch",
            "runtimeExecutable": "pnpm",
            "runtimeArgs": ["dev"]
        }
    ]
}

```

Reboot at least once after installing Node.js to enable debugging.

## Dependencies

```bash
pnpm i -D electron tsx

pnpm approve-builds
```

## Configuration

### .gitignore

```glob
.DS_Store
node_modules

```

### main/index.ts

```typescript
import { app, BrowserWindow } from 'electron';

app.whenReady().then(() => {
    const window = new BrowserWindow({});

    window.loadURL('chrome://gpu');
})

```

### package.json

```diff
+     "type": "module",
+     "scripts": {
+         "dev": "electron -r tsx ./main/index.ts ${SSH_CONNECTION:+--remote-debugging-port=9222 --remote-allow-origins=devtools://devtools}"
+     }
      ...

```

## Start

``F5``

## Troubleshoot GPU issues on Linux

Inspect the `GL_RENDERER field`. The presence of `SwiftShader` indicates that **GPU acceleration is not enabled**.

### main/index.ts

```diff
  ...
+ import { cpus } from 'os';

+ if (cpus()[0].model === 'Cortex-A55') {
+    app.commandLine.appendSwitch('use-gl', 'angle');
+    app.commandLine.appendSwitch('use-angle', 'gles-egl');
+ }
  ...

```

## Remote Debugging

**Get Page ID**  
http://localhost:9222/json

**Open in Chrome**  
devtools://devtools/bundled/inspector.html?ws=localhost:9222/devtools/page/`id`

**This is because Electron does not host `inspector.html`.**

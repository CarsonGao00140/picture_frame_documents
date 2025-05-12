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
pnpm i -D electron

pnpm approve-builds
```

## Configuration

### .gitignore

```glob
.DS_Store
node_modules

```

### electron.js

```javascript
import { app, BrowserWindow } from 'electron';

app.whenReady().then(() => {
    const window = new BrowserWindow({});
    
    window.webContents.openDevTools();
})

```

### package.json

```diff
  {
+     "type": "module",
+     "main": "electron.js",
      ...
  }

```

## Start

```bash
pnpm electron .
```

## Troubleshoot GPU issues on Linux

### Open the GPU Internals page in Devtools

```bash
location.href = 'chrome://gpu';
```

Inspect the `GL_RENDERER field`. The presence of `SwiftShader` indicates that **GPU acceleration is not enabled**.

### electron.js

```diff
  ...
+ import os from 'os';

+ if (os.cpus()[0].model === 'Cortex-A55') {
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

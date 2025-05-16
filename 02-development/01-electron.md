## VS Code Settings

### .vscode/settings.json

```jsonc
{
    "typescript.tsdk": "node_modules/typescript/lib"
}

```

## Dependencies

```bash
pnpm i -D typescript electron vite execa

pnpm approve-builds

pnpm tsc --init
```

## Configuration

### tsconfig.json

```diff
      ...
-     "target": "es2016",
+     // "target": "ESNext",
      ...
-     "module": "commonjs",
+     // "module": "ESNext",
      ...
-     // "moduleResolution": "node10",
+     "moduleResolution": "bundler",
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
```

### .gitignore

```glob
.DS_Store
node_modules
dist

```

### main/main.ts

```typescript
import { platform } from 'os';
import { app, BrowserWindow } from 'electron';

app.whenReady().then(() => {
    const window = new BrowserWindow({
        show: false,
        kiosk: platform() === 'linux',
    });

    if (app.isPackaged) {
        window.on('ready-to-show', window.show);
        window.loadFile('dist/index.html');
    } else {
        window.on('ready-to-show', window.showInactive);
        window.loadURL('chrome://gpu');
        if (process.env['SSH_CONNECTION']) return;
        window.webContents.openDevTools();
    };
})

```

### vite.electron.config.ts

```typescript
import { defineConfig } from 'vite';
import { execaCommand } from 'execa';
import { builtinModules } from 'module';

let electronProcess: ReturnType<typeof execaCommand> | null;

const electronDev = () => ({
    name: 'vite-plugin-electron-preview',
    buildStart() {
        electronProcess?.kill('SIGTERM');
    },
    writeBundle() {
        electronProcess =  execaCommand(
            'electron .' + (process.env['SSH_CONNECTION']
                ? ' --remote-debugging-port=9222 --remote-allow-origins=devtools://devtools'
                : ''),
            { stdio: 'inherit' }
        );
    }
})

export default defineConfig(({ mode }) => ({
    build: {
        lib: {
            entry: 'main/main.ts',
            formats: ['es'],
        },
        target: 'esnext',
        rollupOptions: {
            external: [
                'electron', 
                ...builtinModules.flatMap(module => [module, `node:${module}`])
            ]
        }
    },
    plugins: [
        mode === 'development' && electronDev(),
    ],
}))

```

### package.json

```diff
+ 	"type": "module",
+ 	"main": "dist/main.js",
+ 	"scripts": {
+ 		"electron:dev": "vite build -w -c vite.electron.config -m development"
+ 	},
  	...
```

## Start

```bash
pnpm electron:dev
```


## Troubleshoot GPU issues on Linux

Inspect the `GL_RENDERER` field. The presence of `SwiftShader` indicates that **GPU acceleration is not enabled**.

### electron/main.ts

```diff
- import { platform } from 'os';
+ import { cpus, platform } from 'os';

+ if (cpus()?.[0]?.model === 'Cortex-A55') {
+     app.commandLine.appendSwitch('use-gl', 'angle');
+     app.commandLine.appendSwitch('use-angle', 'gles-egl');
+ }
  ...

```

## Remote Debugging

**Get Page ID**  
http://localhost:9222/json

**Open in Chrome**  
devtools://devtools/bundled/inspector.html?ws=localhost:9222/devtools/page/`id`

**This is because Electron does not host `inspector.html`.**

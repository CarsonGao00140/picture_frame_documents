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

## Dependencies
```bash
pnpm i -D concurrently
```

## Configuration

### main/main.ts

```diff
-         window.loadURL('chrome://gpu');
+         window.loadURL('http://localhost:5173');
```

### package.json

```diff
  		"svelte:dev": "vite -c vite.svelte.config",
+ 		"dev": "concurrently --kill-others \"npm:svelte:dev\" \"npm:electron:dev\""
```

## Start

`F5`

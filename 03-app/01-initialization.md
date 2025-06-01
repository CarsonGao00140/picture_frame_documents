## Create

```bash
flutter create <NAME> -e --platforms=web
```

## Configuration

### .vscode/launch.json

```jsonc
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug",
            "request": "launch",
            "type": "dart",
            "flutterMode": "debug",
            "deviceId": "chrome"
        }
    ]
}

```

## "No Device" when using non-standard versions of Chrome

### \> Preferences > Open User Settings (JSON)

```diff
    ...
+   "dart.env": {
+     "CHROME_EXECUTABLE": "/Applications/<CHROME_EXECUTABLE>.app/Contents/MacOS/<CHROME_EXECUTABLE>"
+   },
```

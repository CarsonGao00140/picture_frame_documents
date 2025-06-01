## Install

https://github.com/sony/flutter-elinux/wiki/flutter-elinux-install

* Complete the step 1
* In the step 2, ignore the commands after `mv`
* Complete the step 3 and select the `Only when using DRM flutter runner` section
```bash
echo 'export PATH=$PATH:/opt/flutter-elinux/bin' >> ~/.bashrc

source ~/.bashrc
```

## Configuration
```bash
flutter-elinux config --enable-custom-devices

cat > ~/.config/flutter/custom_devices.json <<'EOF'
{
    "$schema": "file:///opt/flutter-elinux/flutter/packages/flutter_tools/static/custom-devices.schema.json",
    "custom-devices": [
        {
            "id": "elinux-gbm",
            "label": "eLinux",
            "sdkNameAndVersion": "Debian GNU/Linux 12 (bookworm) 6.1.99",
            "enabled": true,
            "backend": "gbm",
            "ping": [
                "bash", "-c", "test -e /dev/dri/card0"
            ],
            "install": [
                "bash", "-c", "cp -r $(dirname ${localPath})/elinux/arm64/debug/bundle /tmp/${appName}"
            ],
            "uninstall": [
                "bash", "-c", "rm -rf /tmp/${appName}"
            ],
            "runDebug": [
                "bash", "-c", "/tmp/${appName}/${appName} -b . -n -s 2"
            ],
            "stopApp": [
                "pkill", "-f", "/tmp/${appName}/${appName}"
            ]
        }
    ]
}
EOF
```

## Create

```bash
flutter-elinux create <NAME> -e --platforms=elinux
```

## Troubleshoot Scale Factor Issues

```bash
git clone https://github.com/sony/flutter-embedded-linux
```

### src/flutter/shell/platform/linux_embedded/window/elinux_window.h
```diff
    uint32_t GetCurrentWidth() const {
-     return view_properties_.width * current_scale_;
+     return view_properties_.width;
    ...
    uint32_t GetCurrentHeight() const {
-     return view_properties_.height * current_scale_;
+     return view_properties_.height;
```

https://github.com/sony/flutter-embedded-linux/releases

Download `elinux-<ARCH>-debug` and `elinux-<ARCH>-release`.

```bash
mkdir flutter-embedded-linux/build && cd $_

# Copy the elinux-<ARCH>-debug/libflutter_engine.so here.

sudo apt install libegl1-mesa-dev

cmake -DBUILD_ELINUX_SO=ON -DBACKEND_TYPE=DRM-GBM -DCMAKE_BUILD_TYPE=Debug ..

cmake --build .

mv libflutter_elinux_gbm.so /opt/flutter-elinux/flutter/bin/cache/artifacts/engine/elinux-<ARCH>-debug/libflutter_elinux_gbm.so

# Copy the elinux-<ARCH>-release/libflutter_engine.so here.

cmake -DBUILD_ELINUX_SO=ON -DBACKEND_TYPE=DRM-GBM -DCMAKE_BUILD_TYPE=Release ..

cmake --build .

mv libflutter_elinux_gbm.so /opt/flutter-elinux/flutter/bin/cache/artifacts/engine/elinux-<ARCH>-release/libflutter_elinux_gbm.so

cd ../.. && rm -rf flutter-embedded-linux
```

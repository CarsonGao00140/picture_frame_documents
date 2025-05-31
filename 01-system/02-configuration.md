## Update Software List

```bash
sudo apt update
```

## Troubleshoot GPU Issues
```bash
dpkg -l | grep libmali
# If the output includes g52-g*p0-*-gbm, replace it with the pure gbm driver.

sudo apt purge <DRIVER_NAME>
```
### Get g52 gbm driver  
https://github.com/tsukumijima/libmali-rockchip/releases

```bash
wget <LINK_TO_DRIVER>

sudo apt install ./<DRIVER_FILENAME> && rm $_
```

## Git Configuration

```bash
git config --list
# execute on the local system

git config --global user.name "<USERNAME>"

git config --global user.email <EMAIL>
```

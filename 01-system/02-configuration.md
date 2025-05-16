## Default Root Password

`pi`

## Passwordless SSH Login

```bash
ssh-keygen

ssh-copy-id pi@NanoPi-R5C
```

## Remote Connection in VS Code

Install [Remote - SSH](vscode:extension/ms-vscode-remote.remote-ssh) extension, Remote Explore > Remotes > Add

```bash
ssh pi@NanoPi-R5C
```

Some extensions need to be reinstalled on the remote host.

## Download Node.js

https://nodejs.org/en/download  
Get Node.js® `vxx.x.x (Current)` for `Linux` using `nvm` with `pnpm`

## Set Scale Fator

```bash
vim ~/.Xresources
```

### ~/.Xresources

```diff
Xft.dpi: 192

```

Takes effect after reboot.  
Corresponds to 200% scaling for local development.

## Git Configuration

### Git

```bash
git config --list
```

Execute on the local system to copy.

```bash
git config --global user.name "<USERNAME>"
git config --global user.email <EMAIL>

```

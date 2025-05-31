## Device Wiki

https://wiki.friendlyelec.com/wiki/index.php/NanoPi_R5C

## OS

`debian-bookworm-core`

## Default Root Password

`pi`

## Passwordless SSH Login

```bash
ssh-keygen
# if not generated yet

ssh-copy-id pi@NanoPi-R5C

# or, if hostname doesn't resolve:

hostname -I
# execute directly on the device

ssh-copy-id pi@<IP_ADDRESS>
```

## Remote Connection in VS Code

Install [Remote - SSH](vscode:extension/ms-vscode-remote.remote-ssh) extension, Remote Explore > Remotes > Add

```bash
ssh pi@NanoPi-R5C

# or, if hostname doesn't resolve:

ssh pi@<IP_ADDRESS>
```

Some extensions need to be reinstalled on the remote host.


Docker on windows running very slow because of difference of file systems. Docker containers runs on ext4 filesystem and windows urns on NTFS. - NTFS and ext4 handle **permissions, metadata, and file structure** differently.The conversion between these two formats adds extra **latency** for every file read/write operation.

## Install WSL command

```bash
wsl --install
```
## Change the default Linux distribution installed

```bash
wsl --install -d <Distribution Name>
```

To see a list of available Linux distributions available for download through the online store

```bash
wsl --list --online
```

https://learn.microsoft.com/en-us/windows/wsl/setup/environment


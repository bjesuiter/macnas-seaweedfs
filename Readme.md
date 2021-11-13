# MacNAS SeaweedFS

SeaweedFS Setup on mac mini server used as NAS for bjesuiter

## For Reference 

[SeaweedFS Wiki](https://github.com/chrislusf/seaweedfs/wiki)
[SeaweedFS Repo](https://github.com/chrislusf/seaweedfs)
[Official Docker-Compose File](https://raw.githubusercontent.com/chrislusf/seaweedfs/master/docker/seaweedfs-compose.yml)
[Getting Started with Docker-Compose](https://github.com/chrislusf/seaweedfs/wiki/Getting-Started#with-compose)

## Setup this repo 

`doppler setup`

=> Provides secrets for seaweedfs

## Run this repo 

`bonnie start`

## Weed Cheatsheet

Generate default config files 
```sh 
weed scaffold -config=master output="."
weed scaffold -config=filer output="."
```

## Debugging

[Official FAQ](https://github.com/chrislusf/seaweedfs/wiki/FAQ#how-to-access-the-server-dashboard)

- Master Server Dashboard at: <http://localhost:9333>
- Volume Server Dashboards at: <http://localhost:8080/ui/index.html>

### code 255 osxfuse

=> evtl. because virtual device limit of macos is reached (6 < macos 10.13, 10 afterwards)
=> restart or stop smb service for example "sudo kextunload -b com.apple.filesystems.smbfs"

### kext load failed: -603946989 

=> search for macfuse problem with that number
=> SOLVED: Problem was that fuse was too old

## Alternative Way 

If docker-compose does not work: 

- [How to run seaweedFS as a daemon](https://stackoverflow.com/questions/65704384/how-to-run-seaweedfs-as-a-daemon)
    - Systemd / Launchd on mac 
    - [Daemonize](http://software.clapper.org/daemonize/)
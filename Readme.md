# MacNAS SeaweedFS

SeaweedFS Setup on mac mini server used as NAS for bjesuiter

## For Reference 

- [SeaweedFS Wiki](https://github.com/chrislusf/seaweedfs/wiki)  
- [SeaweedFS Repo](https://github.com/chrislusf/seaweedfs)  
- [Official Docker-Compose File](https://raw.githubusercontent.com/chrislusf/seaweedfs/master/docker/seaweedfs-compose.yml)  
- [Getting Started with Docker-Compose](https://github.com/chrislusf/seaweedfs/wiki/Getting-Started#with-compose)  

## Setup this repo 

`doppler setup`

=> Provides secrets for seaweedfs

## Run this repo 

`bonnie start`

## Weed Cheatsheet

### Generate default/template config files 
```sh 
weed scaffold -config=master -output="."
weed scaffold -config=filer -output="."

# view content of these templates, by ommiting the output flag
```

### Getting many files into a Filer Store
The easiest way is just to use the OS-provided copy command from the command line. However, usually it is single-threaded.

The most efficient way is to use "weed filer.copy". It is multi-threaded and bypasses the FUSE layer.
```
# Example: 
weed filer.copy file_or_dir1 [file_or_dir2 file_or_dir3] http://localhost:8888/path/to/a/folder/
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

## Docker-Compose vs. Bare-Metal

If docker-compose does not work: 

- [How to run seaweedFS as a daemon](https://stackoverflow.com/questions/65704384/how-to-run-seaweedfs-as-a-daemon)
    - Systemd / Launchd on mac 
    - [Daemonize](http://software.clapper.org/daemonize/)

Bare-Metal: 
- faster access from filer to volumes

## Known Errors
### fuse Mount is not writeable through desktop icon on macOS 
My Github Issue: [[mount] Fuse Mount is not writeable through virtual drive icon on macOS desktop](https://github.com/chrislusf/seaweedfs/issues/2445)

Usable alternative: 
Mountpoint can be accessed normally via mountpoint path in filesystem. 
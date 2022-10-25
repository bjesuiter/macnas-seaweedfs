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

## Run this repo - Docker 

- `bonnie start`
- `bonnie start fuse`

=> Latest Speedtest (2022-10-25): 65 MB/s write, 65 MB/s read 
=> without direct volume access from fuse to volumes (proxied by filer server, bc volume servers are running in docker-compose!)
=> Ohhhhh shit! :/

=> running with direct access of fuse to volume servers by `bonnie start fusedirect`
=> possible in theory, but the volumes have to be accessible from outside the docker-compose network! 
=> See this Guide for running master and volumes on public internet!
   https://github.com/seaweedfs/seaweedfs/wiki/Run-Blob-Storage-on-Public-Internet

   (based on this issue thread: https://github.com/seaweedfs/seaweedfs/issues/2020)

## Run this repo - Baremetall MacOS 

- `cd baremetal`
- `bonnie start`
- `bonnie mount`

=> Latest Speedtest (2022-10-25): 615 MB/s write, 655 MB/s read 
=> mit Command: `weed mount -filer=localhost:8888 -dir=$(bonnie eval LOCAL_FUSE_MOUNTPOINT) -nonempty -volumeServerAccess=filerProxy -replication=000 -filer.path=/`
=> with direct volume access from fuse to volumes (not proxied by filer server)
=> Reicht nur nicht für 8K 60FPS, laut Blackmagic Disk Test :D 

=> without direct volume access (via filerProxy) - as counter-test to running filerProxy inside docker
write: 355 MB/s, read: 418 MB/s
=> way faster as with running inside docker!!!
=> mit command: `bonnie weed -v=4 mount -filer=localhost:8888 -dir=$(bonnie eval LOCAL_FUSE_MOUNTPOINT) -allowOthers -volumeServerAccess=filerProxy -nonempty -replication=000`

=> CAUTION! The fast mounting (> 600MB/s) command differs from the slower mounting command ( ~ 355 MB/s to 420 MB/s). 
=> Find out, which param makes for this speed difference!

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

## Nice Links 

- Setting up SeaweedFS Cluster with Ubuntu 20.04: 
  https://www.howtoforge.de/anleitung/so-installieren-und-konfigurieren-sie-seaweedfs-cluster-unter-ubuntu-20-04/
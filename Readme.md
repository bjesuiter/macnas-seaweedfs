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

## Alternative Way 

If docker-compose does not work: 

- [How to run seaweedFS as a daemon](https://stackoverflow.com/questions/65704384/how-to-run-seaweedfs-as-a-daemon)
    - Systemd / Launchd on mac 
    - [Daemonize](http://software.clapper.org/daemonize/)
# Self-Hosted Media Server and Aggregation

This is currently a work in progress. 
Make sure to review everything here and if you have any issues please submit it or suggest any edits. 
Also, checkout the [Servarr Docker Setup](https://wiki.servarr.com/docker-guide) for more details on installing the stack.


## Navigation
* [Storage](https://github.com/paulobh/homelab-server/tree/main/storage)
* [Media Server](https://github.com/paulobh/homelab-server/tree/main/media)

---
## Data Directory
### Folder Mapping

It's good practise to give all containers the same access to the same root directory or share. 
This is why all containers in the compose file have the bind volume mount ```/data:/data```. 
It makes everything easier, plus passing in two volumes such as the commonly suggested /tv, /movies, and /downloads makes them look like two different file systems, even if they are a single file system outside the container. 
See my current setup below. 
```
data
├── books
├── downloads
│   ├── qbittorrent
│   │   ├── completed
│   │   ├── incomplete
│   │   └── torrents
│   └── nzbget
│       ├── completed
│       ├── intermediate
│       ├── nzb
│       ├── queue
│       └── tmp
├── movies
├── music
├── shows
└── youtube
```

Easy command to create the download directory scheme.
```
mkdir -p downloads/qbittorrent/{completed,incomplete,torrents}
mkdir -p downloads/nzbget/{completed,intermediate,nzb,queue,tmp}
mkdir movies music shows books youtube
```

### Network Share
Before switching to zfs on Proxmox I used to use a network share from Unraid to store downloads and all my media. 
This was created by adding the share to the fstab file within my Docker server. 
Due note, the appliction `cifs-utils` is required for this method.
```
//10.0.0.90/data /media/data cifs uid=1000,gid=100,username=user,password=password,iocharset=utf8 0 0
```

Storing the user creditentials within this file it's the best idea. 
Check out [this question](https://unix.stackexchange.com/questions/178187/how-to-edit-etc-fstab-properly-for-network-drive) on Stack Exchange to learn more.

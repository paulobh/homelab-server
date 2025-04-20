# Storage and Backup
In this repo I will layout my storage and backup solutions for all of the services and platforms running on my homelab. 
Currently, I manage everything with Proxmox and Proxmox Backup Server. 
While soultions like Unraid and TrueNAS are awesome, I have found over the years the Proxmox is actually an amazing solution for managing storage, network shares, and backups.

## Navigation

---
## Proxmox as a NAS
My current setup involves a single server with x2 NVME drives and a bunch of harddrives in a ZFS configuration. 
These are combined into seperate ZFS pools for the HDDs (vault) and the SSDs (flash). 
Vault is used as a large data storage pool and Flash is used for containers and virtual machines disks. 
No mattery your configuratiuon you can follow this guide. 
However, I would recommend at least one NVME SSD for your boot drive, and at least 512gb if you don't have any other NVME SSDs and at least x2 HHDs for file storage.

### 1. Post Install Steps (optional)

#### Removing Proxmox Subscription Notice
https://community-scripts.github.io/ProxmoxVE/scripts?id=post-pve-install

#### Disable Enterprise Repositories
1. Node > Repositories. Disable the enterprise repositories.
2. Now click Add and enable the no subscription repository. Finally, go Updates > Refresh.
3. Upgrade your system.

#### [Proxmox VE Post Install](https://community-scripts.github.io/ProxmoxVE/scripts?id=post-pve-install)
This script provides options for managing Proxmox VE repositories, including disabling the Enterprise Repo, adding or correcting PVE sources, enabling the No-Subscription Repo, adding the test Repo, disabling the subscription nag, updating Proxmox VE, and rebooting the system.

#### Delete local-lvm and Resize local
My boot drive is small and I run all my containers and virtual machine disks on a seperate storage pool. 
So the lvm paritiion is not nessesary for me and goes unused. 
If you're running everything off the same boot drive for fast storage skips this. 
Also you should check out this [video](https://www.youtube.com/watch?v=czQuRgoBrmM).

1. Delete local-lvm manually from web interface.
2. Run the following commands
```
lvremove /dev/pve/data
lvresize -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root
```
3. Check to ensure your local storage partition is using all avalible space. Reassign storage for containers and VM if needed.


#### Ensure IOMMU is enabled
Enable IOMMU on in grub configuration
```
nano /etc/default/grub
```
You will see the line with `GRUB_CMDLINE_LINUX_DEFAULT="quiet"`, all you need to do is add `intel_iommu=on` or `amd_iommu=on` depending on your system.
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```
Next run the following commands and reboot your system.
```
update-grub
```
Now check to make sure everything is enabled.
```
dmesg | grep -e DMAR -e IOMMU
dmesg | grep 'remapping'
```
Learn about enabling PCI Passthrough [here](https://pve.proxmox.com/wiki/PCI_Passthrough)
 

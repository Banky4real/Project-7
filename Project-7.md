## **Documentation for Project 7**

### Entire View of partitioned Volume 
`lsblk`
![list-of-newly-partitioned-volume](./Images-NFS-Server/Partitioned-volume-view.png)

### LVM2 Installation
`sudo yum install lvm2`
![lVM2-Installation](./Images-NFS-Server/LVM-2-Installed.png)

### Partition Marked as Physical Volume
`sudo pvcreate /dev/xvdf1`
`sudo pvcreate /dev/xvdg1`
`sudo pvcreate /dev/xvdh1`
`sudo pvs`
![Marking-partition-as-physical-Volume](./Images-NFS-Server/Partitons-marked-as-physical-volumes.png)

### Creating a Volume Group
`sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1`
`sudo vgs`
![Volume-Group-Creation](./Images-NFS-Server/Volume-group-created.png)

### Creating Logical Volume
`sudo lvcreate -n lv-apps -L 9G webdata-vg`
`sudo lvcreate -n lv-logs -L 9G webdata-vg`
`sudo lvcreate -n lv-opt -L 9G webdata-vg`
`sudo lvs`
![Logical-Volume-Creation](./Images-NFS-Server/Logical-volumes-created.png)

### Formatting Logical Volume as xfs
`sudo mkfs -t xfs /dev/webdata-vg/lv-apps`
`sudo mkfs -t xfs /dev/webdata-vg/lv-logs`
`sudo mkfs -t xfs /dev/webdata-vg/lv-opt`
![Formatting-Logical-Volume](./Images-NFS-Server/Logical-volumes-formatted-as-xfs.png)

### Mounting our logical volumes on their respective directories
`sudo mount /dev/webdata-vg/lv-apps /mnt/apps`
`sudo mount /dev/webdata-vg/lv-logs /mnt/logs`
`sudo mount /dev/webdata-vg/lv-opt /mnt/opt`
![Mounting-logical-volumes](./Images-NFS-Server/Logical-volumes-mounted-on-their-respective-mount-directories.png)

## Preparing NFS Server
`sudo yum -y update`
`sudo yum install nfs-utils -y`
![NFS-Server-Installation](./Images-NFS-Server/NFS-Utils-Installed.png)

### NFS Server Running
`sudo systemctl start nfs-server.service`
`sudo systemctl enable nfs-server.service`
`sudo systemctl status nfs-server.service`
![NFS-Server-Running](./Images-NFS-Server/NFS-Server-up-and-running.png)

## Exporting our mounts for access by clients within the same subnet cidr
`sudo vi /etc/exports`
`sudo exportfs -arv`
![exporting-our-mounts](./Images-NFS-Server/exporting-our-mounts-for-access-by-clients-within-thesame-subnets-cidr.png)

## Opening TCP and UDP Ports for access by webservers under security Group
![Security-Group-Configuration](./Images-NFS-Server/TCP-and-UDP-Ports-opened-on-NFS-%20for-accesss-by-webservers.png)
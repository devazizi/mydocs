## Documents related to disks and partitioning in linux

### Disk info commands
| Command               | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `lsblk`               | Lists all block devices (disks and partitions) in a tree view.              |
| `fdisk -l`            | Lists all partitions and disk info.                                         |
| `blkid`               | Displays UUID and filesystem type of block devices.                         |
| `parted -l`           | Lists partition tables using GNU Parted.                                    |
| `df -h`               | Shows mounted filesystems with human-readable sizes.                        |
| `du -sh /path`        | Shows size of a directory.                                                  |
| `ls -lh /dev/disk/by-*` | Shows symbolic links to devices by UUID, label, etc.                      |


### Disk partition command

| Command                  | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| `fdisk /dev/sdX`         | CLI tool to create/delete partitions (MBR).                                 |
| `parted /dev/sdX`        | GPT and MBR-aware partitioning tool.                                       |
| `cfdisk /dev/sdX`        | User-friendly curses-based partition tool.                                 |
| `mkfs.ext4 /dev/sdX1`    | Creates an ext4 filesystem.                                                 |
| `mkfs.xfs /dev/sdX1`     | Creates an XFS filesystem.                                                  |
| `mkfs.vfat /dev/sdX1`    | Creates a FAT32 filesystem.                                                 |
| `wipefs -a /dev/sdX1`    | Wipes filesystem signatures.                                                |
| `blkdiscard -f /dev/sdX` | Zeros out SSD blocks (TRIM). ⚠ **Destructive!**                             |

## mount and unmount commands

| Command                  | Description                                  |
|--------------------------|----------------------------------------------|
| `mount /dev/sdX1 /mnt`   | Mounts a partition to `/mnt`.               |
| `umount /mnt`            | Unmounts a partition from `/mnt`.           |
| `mount -a`               | Mounts all filesystems listed in `/etc/fstab`. |
| `mount | grep sdX`       | Filters mounted partitions containing `sdX`. |




🧩 LVM (Logical Volume Manager)
Command	Description
pvcreate /dev/sdX1	Initializes a physical volume.
vgcreate my_vg /dev/sdX1	Creates a volume group.
lvcreate -L 10G -n my_lv my_vg	Creates a logical volume.
lvextend -L +5G /dev/my_vg/my_lv	Expands a logical volume.
lvdisplay, vgdisplay, pvdisplay	Show LVM info.


📦 Disk Usage Monitoring
Command	Description
df -h	Filesystem disk space usage.
du -sh *	Directory size in current path.
ncdu /	Interactive disk usage viewer (needs install).
iotop	Real-time disk I/O per process (needs install).


🧪 Disk Health & SMART
Command	Description
smartctl -a /dev/sdX	Show SMART data (requires smartmontools).
badblocks -v /dev/sdX	Checks disk for bad sectors. ⚠ Slow!
hdparm -I /dev/sdX	View detailed hardware info.
⚙️ Other Useful
Command	Description
ls -lh /dev/	View all device files.
sync	Flushes filesystem buffers.
e2label /dev/sdX1 mylabel	Set ext2/3/4 label.
xfs_admin -L mylabel /dev/sdX1	Set XFS label.




🏗️ ZFS Pool Management (zpool)
Command	Description
zpool create mypool /dev/sdX	Create a ZFS pool.
zpool status	Show health of pools.
zpool list	List pools with usage info.
zpool destroy mypool	Destroy a pool (⚠ irreversible).
zpool import	List importable pools.
zpool import mypool	Import a pool.
zpool export mypool	Export (detach) a pool (for moving/disconnecting).
zpool scrub mypool	Check data integrity (run in background).
zpool iostat -v	Detailed I/O usage.
zpool upgrade	Upgrade pool to latest feature flags.
📁 ZFS Dataset Management (zfs)
Command	Description
zfs list	Show all datasets and usage.
zfs create mypool/mydataset	Create a ZFS filesystem.
zfs destroy mypool/mydataset	Delete a dataset.
zfs rename mypool/oldname mypool/newname	Rename a dataset.
zfs set compression=lz4 mypool/mydataset	Enable compression.
zfs get all mypool/mydataset	List all properties.
zfs set quota=10G mypool/mydataset	Set size quota.
zfs mount mypool/mydataset	Mount a dataset.
zfs unmount mypool/mydataset	Unmount a dataset.
zfs list -t snapshot	List snapshots.
📸 Snapshots & Clones
Command	Description
zfs snapshot mypool/mydataset@snap1	Create snapshot.
zfs destroy mypool/mydataset@snap1	Delete snapshot.
zfs rollback mypool/mydataset@snap1	Rollback to a snapshot.
zfs clone mypool/mydataset@snap1 mypool/clone1	Clone a snapshot.
📦 Volumes (ZVOLs)
Command	Description
zfs create -V 10G mypool/myvolume	Create a block volume (ZVOL).
zfs set volblocksize=8K mypool/myvolume	Set block size (before use).
mkfs.ext4 /dev/zvol/mypool/myvolume	Format and use like a block device.
📊 ZFS Useful Monitoring
Command	Description
zfs get used,available,referenced mypool/mydataset	Storage info.
zpool get all mypool	Pool settings and status.
watch zpool iostat 1	Live disk I/O stats.
🔐 ZFS Encryption (if supported)
Command	Description
zfs create -o encryption=on -o keyformat=passphrase mypool/securedata	Create encrypted dataset.
zfs load-key mypool/securedata	Load key for encrypted dataset.
zfs unload-key mypool/securedata	Unload key (lock dataset).
🔄 Backup & Send/Receive
Command	Description
zfs send mypool/mydataset@snap1 > backup.zfs	Export snapshot to file.
zfs receive backup/mydataset < backup.zfs	Restore snapshot from file.
`zfs send -R mypool@autosnap	ssh user@remote zfs receive -F remotepool`
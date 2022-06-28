## storage management
Not exhaustive, and still in build, list of useful commands, options, etc.


### useful commands

| **command** | **description** |
|-------------|-----------------|
| lsblk | shows partition structure of the disk |
| blkid | shows info about storage devices |
| pvs | shows all physical volumes |
| vgs | shows all volume groups |
| lvs | shows all logical volumes |



### most used partition types

| **fdisk** | **gdisk** | **type** |
|-----------|-----------|----------|
| 20 | 8300 | linux filesystem |
| 19 | 8200 | linux swap |
| 31 | 8e00 | linux lvm |


### creating lvm

- create partitions in 'linux lvm' type
- create physical volumes
```sh
pvcreate /dev/partition1 /dev/partition2
```
- create volume group (-s 1M resizes default extent - 4M - to 1M)
```sh
vgcreate -s 1M vgname /dev/partition1 /dev/partition2
```
- create logical volume
```sh
lvcreate -n lvname -L 1G vgname
lvcreate -n lvname -l 80%FREE vgname
```
- create snapshot of given logical volume
```sh
lvcreate -s -n lvname-snap -l 10M /dev/vgname/lvname
```



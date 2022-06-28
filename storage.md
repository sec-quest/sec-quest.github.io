1. Create LVM volume group vgexp with a size of 1.5GB. The volume group should use three physical volumes that are created as partitions on the secondary disk.

gdisk /dev/sdb
creating three partitions with 8e00 type - Linux LVM
pvcreate /dev/sdb1
pvcreate /dev/sdb2
pvcreate /dev/sdb2
vgcreate vgexp /dev/sdb1 /dev/sdb2 /dev/sdb3


2. In the vgexp volume group, create a logical volume with the name lvexp. Format it with the ext4 file system, and mount it persistently on /exp.

lvcreate -n lvexp -l 80%FREE vgexp
mkfs.ext4 /dev/vgexp/lvexp
vim /etc/fstab
	/dev/vgexam/lvexam	/exam	ext4	defaults	0 0
mount -a

OR

cp /usr/lib/systemd/system/tmp.mount /etc/systemd/system/exam.mount
vim /etc/systemd/system/exam.mount

[Unit]
Description=exam mount
Documentation=doc.file
Conflicts=umount.target
Before=local-fs.target umount.target

[Mount]
What=/dev/vgexam/lvexam
Where=/exam
Type=ext4
Options=defaults

[Install]
WantedBy=local-fs.target 

systemctl enable --now exam.mount


3. Copy all files with the size greater than 1MB from the /etc directory to the new volume which is mounted on /exam.

find /etc -type f -size +1M -exec cp {} /exam \;

4. Create a snapshot of the lvexam volume with the name lvexam-snap, and with size of 100MB.

lvcreate -s -n lvexam-snap -l 10M /dev/vgexam/lvexam
ls -l /dev/mapper
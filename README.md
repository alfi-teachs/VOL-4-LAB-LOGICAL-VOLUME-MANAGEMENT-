# VOL-4-LAB-LOGICAL-VOLUME-MANAGEMENT-
Advanced Storage Management Layer (LVM)

LVM sits between your physical disks and the filesystem. It gives flexibility to resize, extend, and manage storage without downtime in many cases.
# Components:

1. Physical Volume (PV)

Actual disk or partition used for LVM
Example: /dev/xvdb

2. Volume Group (VG)
   
Pool created by combining PVs
Example: my_vg

3. Logical Volume (LV)
   
Usable storage from VG (like a partition)
Example: my_lv

# step 1 create ec2 instances 
lvm
keypair
configure storage
add volume create ebs volume
launch 

# step 2
connect to ec2 instance

step 3
sudo su
lsblk
blkid
# step 3
install lvm 
```bash 
sudo yum install lvm2 -y     # Amazon Linux / RHEL
```
```bash

lvm version
```

# step 4

create physical volume

sudo pvcreate /dev/xvdf

display physical volume

sudo pvdisplay

# step 5

create volume group

sudo vgcreate group name phycial volume name
```bash
vgcreate my-vg /dev/sdb

```
# show group 

vgdisplay 


# step 6
crewte logical volume

lvcreate -n name of volume -L size volume group

lvcreate -n vol_1 -L 5g my-vg

lsblk

u can see type as lvm

# step 7

create volume 2

lvcreate -n vol_2 -L 7G my-vg 

lsblk 

# step 8

reduce the size of voulme
```bash

lvreduce -L 4G /dev/my-vg/vol_1
```
```bash 
lsblk 
```
you can see reduced volume size 

# step 9 

create dir 

mkdir /data

# step 10 

create file system 
 ```bash

mkfs.ext4 /dev/my_vg/vol-1

blkid

# step 11

mount /dev/my-vg/vol-1  /data

cd /data

touch  f1 f2 f3












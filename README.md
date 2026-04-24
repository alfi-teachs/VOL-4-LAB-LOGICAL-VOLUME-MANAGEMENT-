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




# LVM LAB – AWS EC2 (Step-by-Step)

# Step 1: Launch EC2 Instance

Create EC2 instance (Amazon Linux / RHEL)
Select key pair
Add EBS volume (extra disk, e.g., 10–20 GB)
Launch instance

# Step 2: Connect to Instance

ssh -i key.pem ec2-user@<public-ip>

# Step 3: Check Disks & Switch to Root

sudo su

lsblk

blkid

# Step 4: Install LVM

yum install lvm2 -y

lvm version

# Step 5: Create Physical Volume (PV)

(Use your attached disk name from lsblk, e.g., /dev/xvdf)

```bash

pvcreate /dev/xvdf

```
```bash

pvdisplay

```

# Step 6: Create Volume Group (VG)
vgcreate my_vg /dev/xvdf

vgdisplay

# Step 7: Create Logical Volumes (LV)

Create Volume 1

lvcreate -n vol1 -L 5G my_vg

Create Volume 2
lvcreate -n vol2 -L 7G my_vg

lsblk


(You will see type as lvm)

# Step 8: Create Filesystem

mkfs.ext4 /dev/my_vg/vol1

mkfs.ext4 /dev/my_vg/vol2

blkid

# Step 9: Create Mount Directory

mkdir /data

mkdir /backup

# Step 10: Mount Logical Volumes

mount /dev/my_vg/vol1 /data
mount /dev/my_vg/vol2 /backup

df -h

# Step 11: Test
cd /data
touch file1 file2 file3

# Step 12: Reduce Logical Volume (Important Fix)

You must unmount and check filesystem before reducing, otherwise data corruption can happen.

umount /data
e2fsck -f /dev/my_vg/vol1
resize2fs /dev/my_vg/vol1 4G
lvreduce -L 4G /dev/my_vg/vol1

mount /dev/my_vg/vol1 /data
lsblk

Optional (Recommended): Permanent Mount

Edit fstab:

vim /etc/fstab


Add:

/dev/my_vg/vol1   /data    ext4    defaults    0 0
/dev/my_vg/vol2   /backup  ext4    defaults    0 0


Test:

mount -a

Key Corrections I Made
Fixed device naming (my_vg instead of mixed my-vg)
Corrected path (/dev/my_vg/vol1, not vol-1)
Added filesystem before mount (missing earlier)
Added safe reduction steps (very important)
Added second mount point for clarity









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












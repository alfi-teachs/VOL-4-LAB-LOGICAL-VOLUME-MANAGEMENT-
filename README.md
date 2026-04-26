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

t2.micro

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
```bash

yum install lvm2 -y
```
```bash
lvm version
```

# Step 5: Create Physical Volume (PV)

(Use your attached disk name from lsblk, e.g., /dev/xvdf)

```bash

pvcreate /dev/xvdf

```
```bash

pvdisplay

```

# Step 6: Create Volume Group (VG)
```bash
vgcreate my_vg /dev/xvdf
```
```bash
vgdisplay
```

# Step 7: Create Logical Volumes (LV)

Create Volume 1

```bash

lvcreate -n vol1 -L 5G my_vg

```

Create Volume 2

```bash
lvcreate -n vol2 -L 7G my_vg

```
```bash 
lsblk
```


(You will see type as lvm)

# Step 8: Create Filesystem
```bash 
mkfs.ext4 /dev/my_vg/vol1
```
```bash
mkfs.ext4 /dev/my_vg/vol2
```
```bash
blkid
```

# Step 9: Create Mount Directory
```bash
mkdir /data
```
```bash
mkdir /backup
```

# Step 10: Mount Logical Volumes
```bash
mount /dev/my_vg/vol1 /data
```

```bash
mount /dev/my_vg/vol2 /backup
```
```bash
df -h

```

# Step 11: Test

```bash
cd /data

```
```bash
touch file1 file2 file3

```

# Step 12: Reduce Logical Volume (Important Fix)

You must unmount and check filesystem before reducing, otherwise data corruption can happen.
```bash
umount /data
```
```bash
e2fsck -f /dev/my_vg/vol1
```
```bash
resize2fs /dev/my_vg/vol1 4G
```
```bash
lvreduce -L 4G /dev/my_vg/vol1
```

```bash

mount /dev/my_vg/vol1 /data
```

```bash
lsblk
```

Optional (Recommended): Permanent Mount

Edit fstab:

```bash 

vim /etc/fstab

```


Add:

```bash

/dev/my_vg/vol1   /data    ext4    defaults    0 0

```
```bash
/dev/my_vg/vol2   /backup  ext4    defaults    0 0

```


Test:

```bash
mount -a
```
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












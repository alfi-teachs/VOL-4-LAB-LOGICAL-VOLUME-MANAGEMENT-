# VOL-4-LAB-LOGICAL-VOLUME-MANAGEMENT-
Advanced Storage Management Layer (LVM)

LVM sits between your physical disks and the filesystem. It gives flexibility to resize, extend, and manage storage without downtime in many cases.
# Components:

1. Physical Volume (PV)

Actual disk or partition used for LVM
Example: /dev/xvdb

3. Volume Group (VG)
   
Pool created by combining PVs
Example: my_vg

5. Logical Volume (LV)
   
Usable storage from VG (like a partition)
Example: my_lv

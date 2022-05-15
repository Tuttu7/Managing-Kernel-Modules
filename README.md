#### lsmod command is used to display the status of modules in the Linux kernel. It results in a list of loaded modules. lsmod is a trivial program which nicely formats the contents of the /proc/modules, showing what kernel modules are currently loaded.

```
root@ip-172-31-22-143 ~]# lsmod
Module                  Size  Used by
sunrpc                667648  1
dm_mirror              28672  0
dm_region_hash         24576  1 dm_mirror
dm_log                 20480  2 dm_region_hash,dm_mirror
dm_mod                159744  2 dm_log,dm_mirror
dax                   106496  1 dm_mod
crc32_pclmul           16384  0
ghash_clmulni_intel    16384  0
aesni_intel           372736  0
crypto_simd            16384  1 aesni_intel
cryptd                 28672  2 crypto_simd,ghash_clmulni_intel
glue_helper            16384  1 aesni_intel
mousedev               24576  0
psmouse                36864  0
button                 24576  0
ena                   139264  0
ata_piix               40960  0
libata                303104  1 ata_piix
scsi_mod              270336  1 libata
crc32c_intel           24576  0
```

#### Loading a kernel module

```
[root@ip-172-31-22-143 ~]# modinfo st
filename:       /lib/modules/5.10.109-104.500.amzn2.x86_64/kernel/drivers/scsi/st.ko
alias:          scsi:t-0x01*
alias:          char-major-9-*
license:        GPL
description:    SCSI tape (st) driver
author:         Kai Makisara
srcversion:     5BFFEFFBAC1E157DCA4B2FE
depends:        scsi_mod
retpoline:      Y
intree:         Y
name:           st
```


#### Displaying parameters a module supports 

```
[root@ip-172-31-22-143 ~]# modinfo -p st 
buffer_kbs:Default driver buffer size for fixed block mode (KB; 32) (int)
max_sg_segs:Maximum number of scatter/gather segments to use (256) (int)
try_direct_io:Try direct I/O between user buffer and tape drive (1) (int)
debug_flag:Enable DEBUG, same as setting debugging=1 (int)
try_rdio:Try direct read i/o when possible (int)
try_wdio:Try direct write i/o when possible (int)
```


#### Loading the kernel module 

```
[root@ip-172-31-22-143 ~]# modprobe st
[root@ip-172-31-22-143 ~]# lsmod| grep st
st                     69632  0
scsi_mod              270336  2 st,libata
```

#### Unloading the kernel module 

```
[root@ip-172-31-22-143 ~]# modprobe -r st
[root@ip-172-31-22-143 ~]# lsmod| grep st
[root@ip-172-31-22-143 ~]# 
```

#### To make changes to the kernel module parameters

```
modprobe -v st buffer_kbs=64
insmod /lib/modules/5.10.109-104.500.amzn2.x86_64/kernel/drivers/scsi/st.ko buffer_kbs=64
```

#### To make changes to the values permanent while loading the Module

```
cd /etc/modeprobe.d/
Create a file named module.conf
root@ip-172-31-22-143 modprobe.d]# cat module.conf 
options st buffer_kbs=64  max_sg_segs=512

[root@ip-172-31-22-143 modprobe.d]# modprobe -v st
insmod /lib/modules/5.10.109-104.500.amzn2.x86_64/kernel/drivers/scsi/st.ko buffer_kbs=64  max_sg_segs=512 

[root@ip-172-31-22-143 modprobe.d]# lsmod | grep st
st                     69632  0
```

```
-v --verbose

Print messages about what the program is doing. Usually modprobe only prints messages if something goes wrong.
	
-r --remove
This option causes modprobe to remove rather than insert a module

 -p, --parameters
	      Display the typed parameters that a module may support.

```





-v --verbose

Print messages about what the program is doing. Usually modprobe only prints messages if something goes wrong.
	
-r --remove
This option causes modprobe to remove rather than insert a module


 -p, --parameters 
	      Display the typed parameters that a module may support.

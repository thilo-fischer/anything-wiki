<!--
Copyright (c) 2014  Thilo Fischer.
This work is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.
-->

# Scope

My setup of a thrifty computer that runs at my home for various applications.

# Use Cases

## NAS

One of the main applications of the home server is to provide network attached storage.

The described setup provides much greater flexibility than a regular NAS system can provide. While specific features for a regular NAS must be provided by the original NAS firmware or by a plugin for that firmware, the approach of using a regular computer instead make much much more software available. Also, the software of an open source Linux distribution seems a little more trustworthy than proprietary closed-source firmware and plugin software of regular NAS systems.

The drives used to provide the storage capacity shall store the data encrypted (such that the data cannot be read by someone who might steal the physical drive). Using a regular computer allows a much more flexible setup of drive encryption than provided by regular NAS systems.

The NAS shall be used

- for storing all relevant data in a central place
- for sharing files between multiple computers
- for backup of workstations
- syncronization of folders shared among workstations (syncthing)

## NextCloud Server

## GitLab Server

~~It seems the officially encouraged way to install GitLab on Debian (["Omnibus package installation"](https://about.gitlab.com/install/#debian)) does not (yet) support Debian Buster (as of today, 2019-07-10). As I don't need GitLab urgently I will just way some time for the package to be ported to Debian Buster.~~

After installing GitLab using the ["Omnibus package installation"](https://about.gitlab.com/install/#debian) I realized that GitLab by default provides a HTTP server at port 80 and that it is at least not trivial to have a GitLab server and a regular HTTP server (Apache here) running in parallel on the same PC. I want to have another HTTP server independent from GitLab running on the PC at least to provide WebDAV access to some directories. I don't want to put too much effort in finding out how to configure this. I also realized that GitLab was performing a little slow on the 2GB RAM Intel Atom PC. So I finally decided to not install a GitLab server on my Home Server but use a regular [free account on GitLab.com](https://about.gitlab.com/pricing/) instead. (You can create private repositories there where no other users have access unless you explicitly allow it. This seems resonable secure and reasonable confidential for my current requierements.)

## ...

# Hardware

## Requirements

Depending on your performance demands, a gigabit ethernet and USB 3.0 (to connect external hard disc drives) should be considered.

If you want encryption of your data on the hard disc(s) (recommended), your system should support hardware acceleration for (AES) encryption. Note that e.g. the raspberry pi does not support hardware acceleration of AES encryption (even though the processor supports it in theory, but it would require additional license fees and thus is not available on the raspberry pi AFAIK).

## Atom Based Mini PC

I bought this device https://www.amazon.de/gp/product/B07HC7M87W/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1 and it looks as if it was a good choice. One drawback of that device is that it has only two USB ports; a great advantage over many comparable devices is that it has two USB 3.0 ports. (Many have one 3.0 and three 2.0 ports.) It is also one of the cheapest devices I found on amazon and one could easily buy a USB hub for the money saved when choosing this device over one of its competitiors ;)

### System Details of the Chosen Device

```
root@homesrv:~# cat /proc/cpuinfo 
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 76
model name	: Intel(R) Atom(TM) x5-Z8350  CPU @ 1.44GHz
stepping	: 4
microcode	: 0x40e
cpu MHz		: 480.008
cache size	: 1024 KB
physical id	: 0
siblings	: 4
core id		: 0
cpu cores	: 4
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology tsc_reliable nonstop_tsc cpuid aperfmperf tsc_known_freq pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4_1 sse4_2 movbe popcnt tsc_deadline_timer aes rdrand lahf_lm 3dnowprefetch epb pti tpr_shadow vnmi flexpriority ept vpid tsc_adjust smep erms dtherm ida arat
bugs		: cpu_meltdown spectre_v1 spectre_v2 mds msbds_only
bogomips	: 2880.00
clflush size	: 64
cache_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:

processor	: 1
vendor_id	: GenuineIntel
cpu family	: 6
model		: 76
model name	: Intel(R) Atom(TM) x5-Z8350  CPU @ 1.44GHz
stepping	: 4
microcode	: 0x40e
cpu MHz		: 479.869
cache size	: 1024 KB
physical id	: 0
siblings	: 4
core id		: 1
cpu cores	: 4
apicid		: 2
initial apicid	: 2
fpu		: yes
fpu_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology tsc_reliable nonstop_tsc cpuid aperfmperf tsc_known_freq pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4_1 sse4_2 movbe popcnt tsc_deadline_timer aes rdrand lahf_lm 3dnowprefetch epb pti tpr_shadow vnmi flexpriority ept vpid tsc_adjust smep erms dtherm ida arat
bugs		: cpu_meltdown spectre_v1 spectre_v2 mds msbds_only
bogomips	: 2880.00
clflush size	: 64
cache_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:

processor	: 2
vendor_id	: GenuineIntel
cpu family	: 6
model		: 76
model name	: Intel(R) Atom(TM) x5-Z8350  CPU @ 1.44GHz
stepping	: 4
microcode	: 0x40e
cpu MHz		: 480.000
cache size	: 1024 KB
physical id	: 0
siblings	: 4
core id		: 2
cpu cores	: 4
apicid		: 4
initial apicid	: 4
fpu		: yes
fpu_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology tsc_reliable nonstop_tsc cpuid aperfmperf tsc_known_freq pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4_1 sse4_2 movbe popcnt tsc_deadline_timer aes rdrand lahf_lm 3dnowprefetch epb pti tpr_shadow vnmi flexpriority ept vpid tsc_adjust smep erms dtherm ida arat
bugs		: cpu_meltdown spectre_v1 spectre_v2 mds msbds_only
bogomips	: 2880.00
clflush size	: 64
cache_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:

processor	: 3
vendor_id	: GenuineIntel
cpu family	: 6
model		: 76
model name	: Intel(R) Atom(TM) x5-Z8350  CPU @ 1.44GHz
stepping	: 4
microcode	: 0x40e
cpu MHz		: 480.000
cache size	: 1024 KB
physical id	: 0
siblings	: 4
core id		: 3
cpu cores	: 4
apicid		: 6
initial apicid	: 6
fpu		: yes
fpu_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology tsc_reliable nonstop_tsc cpuid aperfmperf tsc_known_freq pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4_1 sse4_2 movbe popcnt tsc_deadline_timer aes rdrand lahf_lm 3dnowprefetch epb pti tpr_shadow vnmi flexpriority ept vpid tsc_adjust smep erms dtherm ida arat
bugs		: cpu_meltdown spectre_v1 spectre_v2 mds msbds_only
bogomips	: 2880.00
clflush size	: 64
cache_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:

root@homesrv:~# cryptsetup benchmark
# Tests are approximate using memory only (no storage IO).
PBKDF2-sha1       364088 iterations per second for 256-bit key
PBKDF2-sha256     463971 iterations per second for 256-bit key
PBKDF2-sha512     305529 iterations per second for 256-bit key
PBKDF2-ripemd160  253524 iterations per second for 256-bit key
PBKDF2-whirlpool  160430 iterations per second for 256-bit key
argon2i       4 iterations, 363392 memory, 4 parallel threads (CPUs) for 256-bit key (requested 2000 ms time)
argon2id      4 iterations, 364888 memory, 4 parallel threads (CPUs) for 256-bit key (requested 2000 ms time)
#     Algorithm |       Key |      Encryption |      Decryption
        aes-cbc        128b       252.5 MiB/s       347.1 MiB/s
    serpent-cbc        128b        31.8 MiB/s        80.2 MiB/s
    twofish-cbc        128b        73.5 MiB/s        73.1 MiB/s
        aes-cbc        256b       192.9 MiB/s       272.8 MiB/s
    serpent-cbc        256b        31.7 MiB/s        80.2 MiB/s
    twofish-cbc        256b        73.4 MiB/s        73.1 MiB/s
        aes-xts        256b       281.7 MiB/s       282.8 MiB/s
    serpent-xts        256b        75.9 MiB/s        75.1 MiB/s
    twofish-xts        256b        69.5 MiB/s        69.1 MiB/s
        aes-xts        512b       230.3 MiB/s       231.5 MiB/s
    serpent-xts        512b        75.9 MiB/s        75.1 MiB/s
    twofish-xts        512b        69.3 MiB/s        69.0 MiB/s
root@homesrv:~# gocryptfs -speed
AES-GCM-256-OpenSSL 	  84.88 MB/s	
AES-GCM-256-Go      	 114.95 MB/s	(selected in auto mode)
AES-SIV-512-Go      	  51.31 MB/s	
```

## ARM Based Single Board Computers

Rock Pi 4 supports USB 3.0, Gigabit Ethernet and AES hardware acceleration (AFAIK).

# Setup

## Operating System

I installed a Debian Buster RC1 for amd64. I use the entire eMMC storage for the installation with the partition all-on-one partition scheme proposed by the debian installer (with just one deviation, I chose btrfs as file system for the root partition). I did a default installation of an LXDE environment and also selected samba, ssh and the other network services.

### Users

The default user of the system is `master` with UID 1000.

For each user accessing services of the system there is a separate user account, these UIDs start from 1001.

(Envisaged future enhancement: For each such user accont an associated account exists that has slightly reduced rights compared to the original account, e.g. read-only access to the same files the original account has read-write access to. These accounts start with UID 1501.)

User acconts shared by several users (e.g. accounts associated with specific access rights a user shoud not have with his default login but only when logging in specifically to get extended or restricted access rights to do certain operations) start with UID 3001.

Dummy users for specific applications (i.e. the user that shall be used to run Syncthing) are created starting with UID 4001.

### Groups

As it is the default for Debian (and many other distributions) there will be a group created for every user account created, e.g. when creating a user `master` with UID 1000, a group `master` with GID 1000 will be created, when creating user `syncthing` with UID 4001, a group `syncthing` with GID 4001 will be created. (This is done automatically by the `adduser` resp. `useradd` command.)

Additional groups shall have GID >= 5000.

Groups that just represent a certain group of users shall start with GID 5001.

## External Storage

I attached a huge external USB 3.0 HDD. I chose a 3.5 inch device as these devices usually come with its own power supply. I chose the disk where I got the highest storage capacity per Euro ratio and bought this one https://www.amazon.de/gp/product/B01IAD5ZC6/ref=ppx_yo_dt_b_asin_title_o03_s02?ie=UTF8&psc=1

A great benefit of this device is the integrated (2 port) USB 3.0 hub which compenstates a bit the little number of USB ports of the Mini PC I chose.

### Encryption and Partitioning

I evaluated different approches of how to set up the file system of the external storage.

#### Encryption With Stacked Filesystem

Create an unencrypted file system (encouraged: btrfs) and use e.g. gocryptfs to set up encrypted directories. Great advantage: You can have some unencryted data on the file system and several directories encrypted with different keys/passphrases and the storage capacity will be shared among all encrypted and unencrypted files.

#### Block Device Encryption With One Partition

Have one luks encryted file system (encouraged: btrfs).

Advantages: Easy setup. Also you will get the most benefits from the features a modern file system (like btrfs) provides, e.g. copy-on-write and deduplication.

Drawbacks: All data will be encrypted with the same key/passphrase. If you want specific data to be not encrypted at all, there is no way to do this. If you want specific data to be encrypted with a different key/passphrase, there is no way to do this. You can however protect data with an additional key/passphrase by using e.g. gocryptfs within the encrypted partition, but this requires then the CPU to process encryption/decryption twice for this data.

#### Block Device Encryption With Several Partitions

This allows to have an unencrypted and some encrypted partitions (and set up different keys/passphrases for different encrypted partitions). The main drawback is that you have to define the partition sizes upfront, i.e. you need to know in advance how much storage to reserve for each key/passphrase.

#### Conclusion

I decided to use block device encryption with one partition as I don't want to settle with fixed partition sizes and I assume the deduplication features of btrfs and alike will be very good to have available. Access to the drive will be done via network, so access to specific data can be restricted on operating system level and it is not strictly necessary to restrict it through encryption with different keys/passphrases. Data that shall be encrypted with different/additional encryption key/passphrase might be shared on the network in an encrypted format and the client that accesses that data uses gocryptfs (or cppcryptfs) to decrypt it locally -- this makes the dublicated en-/decryption seems like an additional security feature ;)

### Disk Encryption

```
cryptsetup luksFormat --type luks --iter-time 5000 --use-random /dev/sda1
```
(LUKS2 is rather new, it is supported since cryptsetup version 2.0. Debian Buster includes cryptsetup version 2.1.0 and thus supports LUKS2, but Debian Stretch currently includes only version 1.7.3 and thus does not yet support LUKS2. As Buster is not yet officially released, and as LUKS2 does not bring crucial enhancements compared to LUKS1, I will stay with the more conservative option of using LUKS1 for greater compatibility for now.)

```
root@homesrv:~# cryptsetup --version
cryptsetup 2.1.0
root@homesrv:~# cryptsetup luksFormat --type luks --iter-time 5000 --use-random /dev/sda1
WARNING: Device /dev/sda1 already contains a 'btrfs' superblock signature.

WARNING!
========
This will overwrite data on /dev/sda1 irrevocably.

Are you sure? (Type uppercase yes): YES
Enter passphrase for /dev/sda1: 
Verify passphrase: 
root@homesrv:~# cryptsetup luksOpen /dev/sda1 storage01
Enter passphrase for /dev/sda1: 
root@homesrv:~# mkfs.btrfs --label storage01 /dev/mapper/storage01 
btrfs-progs v4.20.1 
See http://btrfs.wiki.kernel.org for more information.

Label:              storage01
UUID:               a4e019df-be26-4741-8582-3871f3609a5f
Node size:          16384
Sector size:        4096
Filesystem size:    7.28TiB
Block group profiles:
  Data:             single            8.00MiB
  Metadata:         DUP               1.00GiB
  System:           DUP               8.00MiB
SSD detected:       no
Incompat features:  extref, skinny-metadata
Number of devices:  1
Devices:
   ID        SIZE  PATH
    1     7.28TiB  /dev/mapper/storage01

```

```
root@homesrv:~# blkid /dev/sda1
/dev/sda1: UUID="207b0219-6186-4c59-926c-d84fa9228381" TYPE="crypto_LUKS" PARTLABEL="main" PARTUUID="776d23ad-59a7-4ab5-9e93-44ad5843abea"
root@homesrv:~# echo storage01 UUID=207b0219-6186-4c59-926c-d84fa9228381 none luks >> /etc/crypttab
root@homesrv:~# echo LABEL=storage01 /mnt/storage01 btrfs relatime 0 2 >> /etc/fstab
root@homesrv:~# mkdir /mnt/storage01
root@homesrv:~# mount -a
root@homesrv:~# mount
[...]
/dev/mapper/storage01 on /mnt/storage01 type btrfs (rw,relatime,space_cache,subvolid=5,subvol=/)
```

### Power Saving

My specific device by default does not spin down after a certain timeout.

(Here)[https://www.htpcguides.com/spin-down-and-manage-hard-drive-power-on-raspberry-pi/] is a nice blog article about different approaches to get HDDs to spin down.

It turns out `hdparm` does not work for my device (`SG_IO: bad/missing sense data, ...`).

I found (here)[https://forums.fedoraforum.org/showthread.php?305579-Seagate-external-hdd-doesn-t-spin-down&p=1737849#post1737849] that it might help to change HDD device firmware settings using a Windows PC and a tool from Seagate. This seems the easiest solution to me for now, so I will try this apporach first.

I can spin down the device with `sg_start` (so `sdparm` should work as well). Seems worth a try if `blktool` can spin down the HDD. But all these tools can just spin down the device immediately and can not configure a spin down timeout, so it would require some extra scripting to cause the devices to spin down only after not having been accessed for a while. (Here)[https://lists.debian.org/debian-user/2015/08/msg00125.html] is a description how to configure a spin down timeout using sdparm. It seems resonable to try `hd-idle` first before creating scripts around `sdparm --command=stop` and alike as this already provides this timeout functionality.

## Software

### Syncthing

#### Installation on Debian Buster
```
root@homesrv:~# apt-get install syncthing
```

(Installs version 1.0.0-ds1 or newer.)

#### Automatic Startup

"Set up as system servic" according to this guide: https://docs.syncthing.net/users/autostart.html#using-systemd

1. Create user
   ```
root@homesrv:~# adduser --uid 4001 --disabled-login syncthing
Adding user `syncthing' ...
Adding new group `syncthing' (4001) ...
Adding new user `syncthing' (4001) with group `syncthing' ...
Creating home directory `/home/syncthing' ...
Copying files from `/etc/skel' ...
Changing the user information for syncthing
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: Dummy user account to run Syncthing.                                    
Is the information correct? [Y/n] y
```
2. Copy systemd service file -> not necessary on Debian Buster, file is already in place.
   ```
root@homesrv:~# ls /usr/lib/systemd/system/syncthing@.service 
/usr/lib/systemd/system/syncthing@.service
```
3. Enable service
   ```
root@homesrv:~# systemctl enable syncthing@syncthing.service
Created symlink /etc/systemd/system/multi-user.target.wants/syncthing@syncthing.service → /lib/systemd/system/syncthing@.service.
```
4. Reboot
   Starting the service directly fails of an unknown reason.
   ```
root@homesrv:~# systemctl start syncthing@syncthing.service
Failed to start syncthing@syncthing.service: Unit -.mount is masked.
```
   I simply rebooted an the service was started successfully then.  
5. Check service status
   ```
systemctl status syncthing@syncthing.service
```

#### Configuration

http://localhost:8384

Actions -> Settings
* Device Name
* Minimum Free Disk Space: 10%

### KeePass XC

apt-get install keepassxc

### cryptsetup (LUKS)

apt-get install cryptsetup

### gocryptfs

apt-get install gocryptfs

### samba

apt-get install samba

### TigerVNC

apt-get install tigervnc-xorg-extension

### X11vnc

apt-get install x11vnc

## User Accounts and Access Rights

### User Accounts

Let's assume we have 4 users: Alice, Bob, Carol and Dave.

```
adduser --uid 1001 --disabled-password alice
adduser --uid 1002 --disabled-password bob
adduser --uid 1003 --disabled-password carol
adduser --uid 1004 --disabled-password dave
```

```
usermod -G alice,bob,carol,dave syncthing
```

```
addgroup --gid 5001 alice_bob
addgroup --gid 5002 alice_bob_carol_dave
```

### User Directories

Each user directory has the following subdirectories (using username _alice_ as placeholder):

* `mixed` (`rwxr-xr-x alice:alice`)
    * `sync` (`rwxrwsr-x alice:alice`)
        note the setgid bit!
        * ...
            further subdirectories with `rwxrwsr-x alice:alice` or `rwxrwsr-x syncthing:alice`, the first for directories created manually by the user, the latter for directiories created by syncthing
* `private` (`rwxr-x--- alice:alice`)
* `share` (`rwxr-xr-x alice:alice`)
    * `public` (`rwxr-xr-x alice:alice`)
    * `bob` (`rwxr-s--- alice:bob`)
        may contain symbolic links to ../../mixed directories that shall be shared with bob
    * `carol`
    * `dave`
    * `bob+carol` (`rwxr-s--- alice:alboca`)
    * `bob+dave`
    * `carol+dave`
    * `bob+carol+dave`

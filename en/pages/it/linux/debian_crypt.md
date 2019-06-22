<!--
Copyright (c) 2014  Thilo Fischer.
This work is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.
-->

Some notes about setting up full disk encryption for linux systems in general and for Debian 9.4 (Stretch) specifically. (_Just a collection of notes, not a How-To yet._)

The Arch Linux wiki has some great pages with information on disk encryption in linux and full encryption of linux systems. I found especially useful:
- Overview on disk encryption with linux in general: https://wiki.archlinux.org/index.php/disk_encryption

The system setup I aimed for should have full system encryption for home, root and boot partition. I wanted to have the home partition in a GPT partition separate form the root partition (not as lvm partitions in one GPT partition); reason for this is that I want to "share" the home partition among different linux distribution installed on the same machine. The common way to set up encrypted system with debian is to chose the appropriate option in the installer, but this would create one crypto container with a lvm inside that contains lvm root, home and swap partitions; it also requires to delete the drive and use all space for the debian setup.

I had an already installed, unencrypted Debian system which I wanted to transform into a fully encrypted system.

I found a lot of very helpful information in the Arch Linux wiki.

I struggled to create an appropriate initramfs which allows to decrypt the root partition. In the unencrypted Debian system, you need to edit `/etc/fstab` and `/etc/crypttab` to have the same content they would have on the encrypted system you want to create. Then run `update-initramfs`. `lsinitramfs | grep crypt` should then list e.g. the cryptroot script.

I finally found very useful information on this topic in `/usr/share/doc/cryptsetup/README.initramfs.gz`.

It seems that luksOpen tries all key slots in order. If you have a key slot has a great number of PBKDF2 iterations and it takes a lot of time to luksOpen the device with that key, luksOpening the device with a key in a successive slot will take even more time than this, even if the key slot itself requires only very few iterations.

# Set up "recovery passphrase"

Require a very high number of PBKDF2 iterations for increased security (`-i 10000` means as many iterations as your computer can do in 10000ms) and write to the last slot (`-S 7`, last of 8 slots start counting from 0)
`cryptsetup luksAddKey -S 7 -i 10000 /dev/sdXn`

# Set up key files

If keyfiles provide good entropy (i.e. long enough and created form random numbers) security will not increase with increased number of PBKDF2 iterations (according to LUKS manuale -> TODO cite, reference).

Set keyfile for key slot 3 with few iterations:
`cryptsetup luksAddKey -S 3 -i 10 /dev/sdXn /path/to/keyfile`

# Grub requires *root* partition password

Grub will ask you for the boot partition password before showing you the boot menu. If you do the setup as described here, it will (prabably) ask you for the *root* partition password additionally (just after you entered the boot paration PW) which is quite anoying. The reason is that grub wants to load the backgrund image and font from the root partition.
Add
```
  GRUB_FONT=/boot/grub/unicode.pf2
```
to `/etc/default/grub` and run
```
  cp /usr/share/images/desktop-base/desktop-grub.png /boot/grub/
  cp /usr/share/grub/unicode.pf2 /boot/grub/
  update-grub
```
. (`GRUB_BACKGROUND=...` is not mandatory in `/etc/default/grub` as `update-grub` will just use the first image file found in `/boot/grub` if found.)
The `cryptmount` commands in `00_header` and `05_debian_theme sections` of `/boot/grub/grub.cfg` should now change to mount the boot partition instead of the root partition.

# Few Iterantions for boot partition

When entering the password to decryt the boot partition in Grub, it takes way longer to decrypt and access the partition than it takes in the bootet Linux system. (I assume Grub does not implement hardware acceleration for en-/decryption.) For this reason, I suggest to set up the boot partition passwort with very few iterations. (I have about 5000 iterations on my i5-3380M machine.)

IMHO it is ok to have less iterations for the boot partition as it seems to be less crutial to have it cryptographically secured than it is with other partitions. Many approaches how to set up a fully encrypted Linux system do not encrypt the boot partition at all. The reason for me to encrypt the partition at all is to prevent someone having physical access to your computer to install a tainted kernel that introduces malicious functions such as backdoors to use to steal information from you. But for one, this is an attac which requires quite some effort and also the boot partition is not that big: You could easily create an USB stick to use instead of the boot partition or to check a hash of the boot partition at times when you suspect someone might have manipulated your boot partition. This is also a good idea because Grub itself is not encrypted and a tainted Grub could be used to steal your boot partition's password. (Side note: One sould also use TPM to further protect from tainted boot devices.)

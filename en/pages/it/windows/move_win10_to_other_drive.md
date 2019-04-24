# Move Windows 10 Installation To Another Drive

This describes how to move Windows 10 installations from one drive to another that shall replace the original drive in a already set up PC. With *drive* I mean a drive commonly used as a PCs main drive: HDD, SSD, possibly eMMC. In the following the first drive will be called the *original* drive, the latter the *replacement* drive.

A Linux live system is used to copy the partition image from one drive to another. Apart from Windows onboard tools and the Linux live system no other software is required.

# Disclaimer

I have used the described procedure several times successfully and not yet preceived any issues or problems with this approach. Anyhow there is no warrenty this procedure will work in your case.

# Example Use Case

Say you have a PC in use with an already set up Windows 10 installation with several programs installed, user accouts configured, user data stored on the main drive and so on. The PC uses an HDD and that drive shall be replaced by a SSD.

# Step By Step Guide

## Create Backup Of Original Drive's Content

Just in case anything goes wrong you want to have a backup of your data.

## Disable BitLocker

If you use BitLocker on the drive disable it and wait until Windows has finished to unencrypt the drive. You can reactivate BitLocker after the Windows installation has been migrated to the replacement drive.

(I have not tested whether the procedure works with BitLocker encrypted drives. I have doubts this will work, but it might be possible. If you give it a try: Let me know about the results ;) )

## Shrink Windows System Partition To Fit On The Replacement Drive

### Why?

You need to ensure the Windows System Partition fits onto the replcement drive. If the replacement drive is smaller than the original drive (in our example of replacing a HDD with a SSD this is a common case) you should shrink the Windows system partition on the original drive to a size that will easily fit on the replacement drive.

If your original drive is smaller than the replacement drive you can usually skip the step of shrinking the system partition.

### How?

You can usually use Windows tool `diskmgmt.msc` to shrink the partition.

`diskmgmt.msc` usually allows to shrink the drive to some extend, but only until some "unmovable files" get in the way. I've even seen drives where `diskmgmt.msc` could not actually shrink the partition by a single byte due to "unmovable files". If you cannot shrink the drive sufficiently, either skip shrinking the partition now and use [`ntfsresize`](https://en.wikipedia.org/wiki/Ntfsresize) from you Linux live system later or see https://superuser.com/questions/1017764/how-can-i-shrink-a-windows-10-partition for other ways to deal with this issue.

### How much?

Be generous and shrink rather to a size too small than the perfectly fitting size. We will enlarge the partition again at the replacement drive.

It is fine to reduce the partition size as much as possible (and as much as `diskmgmt` allows); but keep about a GiB or so of free space on the partition such that Windows can still work properly on that partition. A small partition on the original drive will speed up the copying process from the original to the replacement drive (esp. if copying is done via a slower interface like USB 2.0 or such).

Keep in mind that we cannot use the full size of the replacement drive for the system partition: We will need some space for Windows recovery partition and such. Also keep in mind `diskmgmt` operates in GiB (though showing GB as unit) while the manufacture of your replacement drive specifies the drive's size in GB (see https://en.wikipedia.org/wiki/Gibibyte).

Rule of thumb: Your system partition should be smaller than the size of the replacement drive minus 1.5 GiB; better minus 5 GiB to be on the safe side. The smaller the better as long as there is still enough free space on the partition for Windows to work properly.

## Get a Linux Live System Boot Device

Download e.g. http://www.knoppix.org/ and create a bootable CD, DVD or USB stick from it.

## Shut Down Windows Without Fast Startup

Windows' Fast Startup feature may get into your way when accessing the Windows' drives file system from the Linux live system. To ensure the Fast Startup feature is (temporarily) non-functional just *restart* your Windows and shut down the PC when it starts to reboot (at the BIOS/UEFI stage).

(Windows does use the Fast Startup feature only when being *shut down*, Fast Startup is not used when *restarting* -- AFAIK at the time of writing.)

## Replace Original Drive With Replacement Drive

Change the drives in hardware.

## Set Up Fresh Windows 10 Installation On Replacement Drive

Use the same verison (32/64 bit etc.) as installed on the original drive.

Remove any preexisting partitions, make Windows installer use the full (empty) drive.

## Shut Down Fresh Windows Installation Without Fast Startup

This step might not be mandatory. But it won't harm to do so ;)

## Connect Original Drive To PC

If you can connect the drive via hat-pluggable interface like USB, preferably do so after booting the Linux live system. If you connect via internal SATA interface it might be good to connect before booting the Linux life system. (Do internal SATA controllers support hot-plugging reliably?).

## Boot Linux Live System

## Identify The Drives

The graphical partition tool gparted is good to get an overview about the installed drives and the associated device file names. Run `sudo gparted` to start it.

## Shrink System Partition

If it is necessary to shrink the original drive and this was not done previously using `diskmgmt.msc`, this is the perfect time to do it using `ntfsresize`/`gparted`.

## Copy From Original To Replacement Drive

`dd if=/dev/sdxI of=/dev/sdyJ`

with `xI` being replaced by the according letter and number for the original and `yJ` by those for the replacement drive.

## Shut Down Live Linux

## Unplug Original Drive

## Boot PC

The Windows installation known from the original drive should now boot from the replacement drive.

## Enlarge System Partition's File System

Size of *file system* on system partition will be the size of the partition on the original drive at the time of copying the partition, the partition itself will have the size Windows used when creating that partition. Use `diskmgmt.msc` to shrink the partition by 1 MB, afterwards enlarge it again by 1 MB. This should align the size of the file system to that of the partition.

## Enable BitLocker

Optional, but recommended.


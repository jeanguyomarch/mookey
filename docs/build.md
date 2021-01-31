---
title: Build
index: 40
---

# Building a mookey

Please make sure you have read the [motivations][1] behind `mookey` and
understand its [threat model][2] as use outside of its original scope may
compromise personal data.

`mookey` *is provided “as is”, without warranty of any kind, express or
implied, including but not limited to the warranties of merchantability,
fitness for a particular purpose and non infringement. In no event shall the
authors be liable for any claim, damages or other liability, whether in an
action of contract, tort or otherwise, arising from, out of or in connection
with mookey*.


## Flashing a Linux Live Image

If you want your `mookey` to act as a Linux live USB, which can come handy
sometimes, please follow this section. Otherwise, you can skip this step
and continue to the next section.

You must download a Linux live ISO image. You can for instance get an [Arch
Linux][3], an [Ubuntu][4] or any other GNU/Linux flavor, whatever suits you
best. It is assumed it you downloaded the file `<linux-live.iso>`, and that
you checked its integrity.

Then, use the `dd` command to flash the ISO image on your USB key. It is
assumed it is named `</dev/sdX>`. **Please double-check this path!
Misidentification of the USB key could yield to irremediable data loss!**
Depending on your system configuration, you may need to query administrator
privileges with `sudo`:

```
$ sudo dd if=<linux-live.iso> of=</dev/sdX> bs=4M
$ sudo sync
```

After a while, these two commands shall complete, and you end up with a USB key
that is able to boot a Linux. But we are not done yet, as we are missing the
*encrypted partition* and the *transfer partition*.


## Adding Encrypted and Transfer Partitions

After completing the previous step, let's create two additional partitions.
Here, we will create a 2GB encrypted partition whereas the remaining will
be used by the transfer partition. Using `fdisk`:

```
$ fdisk </dev/sdX>

Command (m for help): n [enter]
Partition number (4-248, default 4): [enter]
First sector (1413720-31309760, default 1415168): [enter]
Last sector, +/-sectors or +/-size{K,M,G,T,P} (1415168-31309760, default 31309760): +2G [enter]
Created a new partition 4 of type 'Linux filesystem' and of size 2 GiB.

Command (m for help): n  [enter]
Partition number (5-248, default 5): [enter]
First sector (1413720-31309760, default 5609472): [enter]
Last sector, +/-sectors or +/-size{K,M,G,T,P} (5609472-31309760, default 31309760): [enter]

Created a new partition 5 of type 'Linux filesystem' and of size 12.3 GiB.

Command (m for help): p [enter]
Disk /dev/sdb: 14.93 GiB, 16030629888 bytes, 31309824 sectors
Disk model: USB Flash Drive
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 31323032-3130-4130-B030-303931383536

Device       Start      End  Sectors  Size Type
/dev/sdb1       64  1288191  1288128  629M Linux filesystem
/dev/sdb2  1288192  1413119   124928   61M EFI System
/dev/sdb3  1413120  1413719      600  300K Microsoft basic data
/dev/sdb4  1415168  5609471  4194304    2G Linux filesystem
/dev/sdb5  5609472 31309760 25700289 12.3G Linux filesystem

Command (m for help): w [enter]
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

In further sections, we refer to the *encrypted partition* as `</dev/sdX4>` and
to the transfer partition as `</dev/sdX5>` (to reflect the log of `fdisk`
above). Note however that the *encrypted* partition is not yet encrypted!


## Setup the Transfer Partition

The most difficult task here is to select a filesystem type, that can be
understood by the various operating systems you expect to work with. Assuming
they are linux-only, you can safely select `ext4`:

```
$ sudo mkfs.ext4 </dev/sdX5>
mke2fs 1.45.6 (20-Mar-2020)
Creating filesystem with 3212536 4k blocks and 804672 inodes
Filesystem UUID: a9ac0669-9e1a-4afa-9f5d-78b1b4b0ae58
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
```


## Setup the Encrypted Partition

We will use [dm-crypt][5]:

```
$ sudo cryptsetup luksFormat </dev/sdX4>

WARNING!
========
This will overwrite data on /dev/sdb4 irrevocably.

Are you sure? (Type 'yes' in capital letters): YES [enter]
Enter passphrase for /dev/sdb4: [enter your passphrase here]
Verify passphrase: [enter your passphrase here]
```

Now that the partition has been encrypted, you must open it to bootstrap
the filesystem:

```
$ sudo cryptsetup open </dev/sdX4> mookey
Enter passphrase for /dev/sdb4: [enter your passphrase here]
```

This should result in the file `/dev/mapper/mookey` to be created. You can
now create an `ext4` filesystem:

```
$ sudo mkfs.ext4 /dev/mapper/mookey
mke2fs 1.45.6 (20-Mar-2020)
Creating filesystem with 520192 4k blocks and 130048 inodes
Filesystem UUID: 2ce02fee-76bc-44e5-bdf3-9d85b356515c
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done
```

Before going further, let's see how to lock the encrypted storage:

```
$ sudo cryptsetup close mookey
```

You now have successfully bootstrapped your `mookey`! But it does not quite
handle passwords yet!


[1]: {% link motivations.md %}
[2]: {% link threat_model.md %}
[3]: https://archlinux.org/download/
[4]: https://ubuntu.com/download/desktop
[5]: https://wiki.archlinux.org/index.php/Dm-crypt

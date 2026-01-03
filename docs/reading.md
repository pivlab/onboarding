# Suggested reading material

## General

* The importance of stupidity in scientific research [link](https://fermatslibrary.com/s/the-importance-of-stupidity-in-scientific-research)

## Reproducible research
* The Turing Way [link](https://github.com/the-turing-way/the-turing-way)

## Medicine

* Robbins & Kumar Basic Pathology, Eleventh Edition [link](https://www.clinicalkey.com/#!/browse/book/3-s2.0-C20190021577)
  * It has a nice introduction to Pathology, for example.

## Genetic studies

* 15 years of GWAS discovery: Realizing the promise [link](https://doi.org/10.1016/j.ajhg.2022.12.011)
* An Owner's Guide to the Human Genome: an introduction to human population genetics, variation and disease, by Jonathan Pritchard [link](https://web.stanford.edu/group/pritchardlab/HGbook.html)

## Deep learning

* https://deeplearning.neuromatch.io/index.html

## Technical

### Unlock encrypted partition remotely at boot time
* Main article: [How to unlock LUKS using Dropbear SSH keys remotely in Linux](https://www.cyberciti.biz/security/how-to-unlock-luks-using-dropbear-ssh-keys-remotely-in-linux/)
* In the office at AHSB:
  * Use a static IP address as mentioned in the article above.
  * Add a script in `/etc/initramfs-tools/scripts/init-bottom/deconfigure-interfaces` (remember to `chmod +x` it, and replace the interface/file name accordingly):
    ```
    #!/bin/sh

    rm -f /run/netplan/enp3s0.yaml
    ip -f inet address flush dev enp3s0
    ```
  * `sudo update-initramfs -u`

### Install Pop!_OS 24.04 with two disks using LVM and LUKS (encryption)

This assumes the computer has two disks.

First, on a new computer, make a clean install ("erases all").

Then, login into the newly installed system and run the following commands.

Identify disks:
```bash
lsblk -o NAME,SIZE,MODEL,SERIAL,TYPE
```

The following assumes that the new disk is in `/dev/nvme1n1`.

Wipe old signatures:

```bash
sudo wipefs -af /dev/nvme1n1
sudo parted -s /dev/nvme1n1 mklabel gpt
sudo parted -s /dev/nvme1n1 mkpart primary 1MiB 100%
sudo parted -s /dev/nvme1n1 set 1 lvm on

# re-read partition tables
# you should have /dev/nvme1n1p1
sudo partprobe
lsblk
```

Encrypt new partition with LUKS:

```bash
sudo cryptsetup luksFormat /dev/nvme1n1p1
sudo cryptsetup open /dev/nvme1n1p1 cryptdata1
```

Build LVM on top of the decrypted devices:

```bash
sudo pvcreate /dev/mapper/cryptdata1
# verify
sudo pvs
sudo lvmdiskscan -l
sudo vgs
sudo lvdisplay
```

Add newly created physical volume (pv) to volume group (vg) named `data`:

```bash
sudo vgextend data /dev/mapper/cryptdata1
sudo lvm lvextend -l +100%FREE /dev/data/root

# but df -h still not showing right size:
df -h
# so you have to also run:
sudo resize2fs -p /dev/mapper/data-root
# check again
df -h
```

Now, BEFORE YOU RESTART, it's very important to do update `/etc/crypttab` (if you fail to do this, you will have to boot from a USB drive, chroot, update-initramfs, etc).

```bash
# get uuid (change partitions accordingly)
sudo blkid /dev/nvme0n1p3 /dev/nvme1n1p1
```

Open `/etc/crypttab` and make sure both pv (that are encrypted with LUKS) are included with correct UUID got before:

```
cryptdata UUID=... crypt_disks luks,keyscript=decrypt_keyctl
cryptdata1 UUID=... crypt_disks luks,keyscript=decrypt_keyctl
[...]
```

The `decrypt_keyctl` options assume you used the same passphrase for both volumes (so you enter it only once at boot time).

Finally, and very important:

```bash
# decrypt_keyctl is included in the keyutils package
sudo apt install keyutils
```

```bash
sudo update-initramfs -u -k all
```

Reboot. Make sure everything works and you enter the passphrase only once.

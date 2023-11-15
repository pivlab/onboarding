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


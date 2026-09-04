# Born2beRoot - Writeup by ***aria***
***frodrig2** - September 2026 - Project Version 5.2*

## Debian GNU/Linux (amd64)

This document is *immensely* supported by the documentation provided by Debian:

- [Debian GNU/Linux Installation Guide for 64-bit PC (amd64)](https://www.debian.org/releases/stable/amd64/install.en.pdf)
- [DebianInstaller FAQ](https://wiki.debian.org/DebianInstaller/FAQ)
- [Debian Wiki - DebianInstaller](https://wiki.debian.org/DebianInstaller)

### 1. Download the installation image

Download the installation ISO image file for the [latest stable version of Debian
GNU/Linux](https://www.debian.org/releases/stable/).

*As of this writeup: **Debian GNU/Linux 13.6, "trixie"***

- [Download Debian](https://www.debian.org/distrib/)
- [Installing Debian](https://www.debian.org/releases/stable/debian-installer/)

A [network install](https://www.debian.org/CD/netinst/) is recommended: the
*"netinst"* image contains just the minimal amount of software to install the
base system and fetch the remaining packages over the Internet.

- [Get latest release's `netinst` ISO image](https://get.debian.org/images/release/current/amd64/iso-cd/)

    > Look for the file named `debian-x.x.x-amd64-netinst.iso`


#### 1.1 Verify authenticity of the image file

Use the available checksum files to confirm that the downloaded image is the one
created and released by Debian and has not been corruped or tampered with.

- [Get latest release's checksum files](https://get.debian.org/images/release/current/amd64/iso-cd/)

    > Look for the files named `SHA512SUMS` and `SHA512SUMS.sign`

    Place the files in the same directory as the ISO file.

##### Verify the SHA-512 checksum

Run `sha512sum` against the ISO and compare it with the contents of the checksum
file.

- The following command uses the `-c, --check` and `--ignore-missing` flags
to automatically check only the matching items from the given checksum file:

    ```bash
    sha512sum -c SHA512SUMS --ignore-missing
    ```

    A passing result looks like:

    ```bash
    debian-x.x.x-amd64-netinst.iso: OK
    ```

##### Verify signing key of the checksum file

Confirm that the checksum file used to validate the ISO is signed by Debian.

- [Get the latest public signing key from Debian](https://www.debian.org/CD/verify)

    - At the bottom of the page, search for the most recent "Debian CD signing key"
      (not Debian Testing)

        > As of this writeup: [key-DA87E80D6294BE9B.txt](https://www.debian.org/CD/key-DA87E80D6294BE9B.txt) (2011-01-05)

    - Right-click the key's highlighted link and "Save link as..."

    - Import the key with `gpg --import key-XXXXXXXXXXXXXXXX.txt`

- Verify the checksum file against its signature file using the imported public
  key:

  ```bash
  gpg --verify SHA512SUMS.sign SHA512SUMS
  ```

  A passing result looks something like:

  ```bash
  gpg: Signature made Sat Jul 11 21:25:53 2026 BST
  gpg:                using RSA key DF9B9C49EAA9298432589D76DA87E80D6294BE9B
  gpg: Good signature from "Debian CD signing key <debian-cd@lists.debian.org>" [unknown]
  gpg: WARNING: This key is not certified with a trusted signature!
  gpg:          There is no indication that the signature belongs to the owner.
  Primary key fingerprint: DF9B 9C49 EAA9 2984 3258  9D76 DA87 E80D 6294 BE9B
  ```

  > If you want to double-check, you can compare the `Primary key fingerprint`
  > value with the fingerprint of the key you imported from
  > [Debian](https://www.debian.org/CD/verify)


---
category: en
type: paper
hastr: true
layout: paper
tags: linux, systemd, ecryptfs
title: How to encrypt home directory. For dummies
short: ecnryption-home-directory
---
<figure class="img">![single-door](/resources/papers/single-door.jpg)</figure>This
paper is about encryption home directory using ecryptfs and automount settins
using systemd and key on flash card.

<!--more-->

## <a href="#preparation" class="anchor" id="preparation"><span class="octicon octicon-link"></span></a>Step 0: Preparation

1. Logout as user.
2. Login as root on tty. The following actions should be done as root.
3. Move your home directory and create empty directory (`s/$USER/user name/`):

    ```bash
    mv /home/{$USER,$USER-org}
    mkdir /home/$USER
    chmod 700 /home/$USER
    chown $USER:users /home/$USER
    ```

## <a href="#step1" class="anchor" id="step1"><span class="octicon octicon-link"></span></a>Step 1: Encryption

The widespread solution in the Internet is to use automatic utilities to do it.
However in our case they are not suitable, since we need to import key /
password signature, which is not possible in this case.

The encryption can be done by the following command (lol):

```bash
mount -t ecryptfs /home/$USER /home/$USER
```

While process it asks some question (I suggest to do first mounting in the
interactive mode). The answers may be like following (see the comments),
please note that if you change something, it will be changed in some lines below
too:

```bash
# key or certificate. The second one is more reliable while you don't lose it %)
Select key type to use for newly created files:
 1) passphrase
 2) openssl
Selection: 1
# password
Passphrase:
# cipher, select default
Select cipher:
 1) aes: blocksize = 16; min keysize = 16; max keysize = 32
 2) blowfish: blocksize = 8; min keysize = 16; max keysize = 56
 3) des3_ede: blocksize = 8; min keysize = 24; max keysize = 24
 4) twofish: blocksize = 16; min keysize = 16; max keysize = 32
 5) cast6: blocksize = 16; min keysize = 16; max keysize = 32
 6) cast5: blocksize = 8; min keysize = 5; max keysize = 16
Selection [aes]: 1
# key size, select default
Select key bytes:
 1) 16
 2) 32
 3) 24
Selection [16]: 1
# enable reading/writing to the non-encrypted files
Enable plaintext passthrough (y/n) [n]: n
# enable filename encryption
Enable filename encryption (y/n) [n]: y
Filename Encryption Key (FNEK) Signature [XXXXX]:
# toolongdontread
Attempting to mount with the following options:
  ecryptfs_unlink_sigs
  ecryptfs_fnek_sig=XXXXX
  ecryptfs_key_bytes=16
  ecryptfs_cipher=aes
  ecryptfs_sig=XXXXX
WARNING: Based on the contents of [/root/.ecryptfs/sig-cache.txt],
it looks like you have never mounted with this key
before. This could mean that you have typed your
passphrase wrong.

# accept, quit
Would you like to proceed with the mount (yes/no)? : yes
Would you like to append sig [XXXXX] to
[/root/.ecryptfs/sig-cache.txt]
in order to avoid this warning in the future (yes/no)? : yes
Successfully appended new sig to user sig cache file
Mounted eCryptfs
```

Then copy files from home directory to encrypted one:

```bash
cp -a /home/$USER-org/. /home/$USER
```

## <a href="#step2" class="anchor" id="step2"><span class="octicon octicon-link"></span></a>Step 2: systemd automounting

Create file on flash card (I've used microSD) with the following text (you
should insert your password):

```bash
passphrase_passwd=someverystronguniqpassword
```

Add card automount (mount point is `/mnt/key`) to `fstab` with option `ro`, for
example:

```bash
UUID=dc3ecb41-bc40-400a-b6bf-65c5beeb01d7    /mnt/key ext2     ro,defaults       0 0
```

Let's configure home directory mounting. The mount options can be found in the
following output:

```bash
mount | grep ecryptfs
```

I should note that there are not all options there, you need add `key`,
`no_sig_cache`, `ecryptfs_passthrough` too. Thus systemd mount-unit should be
like the following (if you are systemd-hater you can write the own daemon,
because it doesn't work over `fstab` without modification (see below)).

```bash
# cat /etc/systemd/system/home-$USER.mount
[Unit]
Before=local-fs.target
After=mnt-key.mount

[Mount]
What=/home/$USER
Where=/home/$USER
Type=ecryptfs
Options=rw,nosuid,nodev,relatime,key=passphrase:passphrase_passwd_file=/mnt/key/keyfile,no_sig_cache,ecryptfs_fnek_sig=XXXXX,ecryptfs_sig=XXXXX,ecryptfs_cipher=aes,ecryptfs_key_bytes=16,ecryptfs_passthrough=n,ecryptfs_unlink_sigs

[Install]
WantedBy=local-fs.target
```

`XXXXX` should be replaced to signature from options with which directory are
currently mounting. Also you need to insert user name and edit path to file with
password (and unit name) if it is needed. Autoload:

```bash
systemctl enable home-$USER.mount
```

Here is a service to unmount flash card when it will be unneeded:

```bash
# cat /etc/systemd/system/umount-key.service
[Unit]
Description=Unmount key card
Before=local-fs.target
After=home-arcanis.mount

[Service]
Type=oneshot
ExecStart=/usr/bin/umount /mnt/key

[Install]
WantedBy=local-fs.target
```

Enable:

```bash
systemctl enable umount-key.service
```

Reboot. Remove backups if all is ok. If not then you did a mistake, resurrect
system from emergency mode.

## <a href="#whynotfstab" class="anchor" id="whynotfstab"><span class="octicon octicon-link"></span></a>Why not fstab?

In my case I could not to make flash mounting before home decryption. Thus I saw
emergency mode on load in which I should just continue loading. There are two
solutions in the Internet:

* Create entry with noauto option and then mount using the special command in
`rc.local`.
* Create entry with nofail option and then remount all partitions in `rc.local`.

In my opinion both of them are workarounds too much.

## <a href="#whynotpam" class="anchor" id="whynotpam"><span class="octicon octicon-link"></span></a>Why not pam?

Other solution is to mount using pam entry. In my case I have authentication
without password on fingerprint so it doesn't work for me.

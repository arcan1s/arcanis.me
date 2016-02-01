---
category: en
type: paper
layout: paper
hastr: true
tags: site, github pages
title: Site changes
short: site-changes
---
I decided to change my site. You may find short list of changes below.

<!--more-->

## <a href="#list" class="anchor" id="list"><span class="octicon octicon-link"></span></a>The list of changes:
* I rented a `arcanis.me` domain. Now I have a normal address, as well as all
normal people have it. Small description of how to do it. Firstly, you should
rent domain and activate DNS editing (it is called
[DNS-master](//www.nic.ru/dns/service/dns_hosting/ "Service page") for
Ru-center). I pay about $30 in year. Then you should create CNAME file in your
repository; this file has line with your domain name. And finally you should
create two DNS records for your domain:

    ```bash
    @    A   192.30.252.153
    @    A   192.30.252.154
    # redirection from www.*
    www CNAME   @
    ```

    (Symbol `@` means you root domain.) And next wait for two hours. You may
find out the result as follows:

    ```bash
    $ dig domain.name +nostats +nocomments +nocmd
    ; <<>> DiG 9.9.2-P2 <<>> domain.name +nostats +nocomments +nocmd
    ;; global options: +cmd
    ;domain.name.                  IN      A
    domain.name.           912     IN      A       192.30.252.153
    domain.name.           912     IN      A       192.30.252.154
    ...
    ```

* Also I've created [my own repo](ftp://repo.arcanis.me/repo "Repository"),
which will contain some AUR packages that I'm using. Support of both
architectures is planned.
* Since the repo requires ftp protocol, I've changed samba shared folders to
ftp. The problem of access has been resolved by using mount options:

    ```bash
    # only read rights
    /home/arcanis/music                         /srv/ftp/music  ext4    defaults,bind,ro   0 0
    /home/arcanis/arch/repo                     /srv/ftp/repo   ext4    defaults,bind,ro   0 0
    # read and write rights (the file has size 2 Gb)
    /home/arcanis/share.fs                      /srv/ftp/share  ext4    defaults,rw   0 0
    ```

    Login on special user and option `anon_world_readable_only=YES` are used for
prevent access to the music directory. Also here is my `/etc/vsftpd.conf`
configuration file:

    ```bash
    anonymous_enable=YES
    anon_root=/srv/ftp
    local_enable=YES
    write_enable=YES
    local_umask=022
    anon_upload_enable=YES
    anon_mkdir_write_enable=YES
    anon_other_write_enable=YES
    anon_world_readable_only=YES
    dirmessage_enable=YES
    xferlog_enable=YES
    connect_from_port_20=YES
    nopriv_user=music
    ascii_upload_enable=YES
    ftpd_banner=Welcome to arcanis
    chroot_local_user=YES
    local_root=/srv/ftp/music
    listen=YES
    ```

    Now let's add redirection from `repo.arcanis.me` to the needed IP address.
To do this, add the following entry in DNS:

    ```bash
    repo   A   89.249.170.38
    ```

* Also there are plans to buy a server for compiling packages and hosting the
repository, filesharing and backups.

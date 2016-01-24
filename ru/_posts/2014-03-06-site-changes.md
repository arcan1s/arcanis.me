---
category: ru
type: paper
hastr: true
layout: paper
tags: сайт, github pages
title: Изменения сайта
short: site-changes
---
Решил немного поиграться с сайтом. Краткий список изменений ниже.

<!--more-->

## <a href="#list" class="anchor" id="list"><span class="octicon octicon-link"></span></a>Cписок изменений:

* Арендовал домен `arcanis.me`. Теперь, как и все белые люди, имею нормальный адрес. Небольшое описание (мне, как человеку далекому от интернет-технологий, пришлось немного попариться на эту тему). Арендуем домен, подключаем услугу редактирования DNS (для Ru-center [DNS-master](//www.nic.ru/dns/service/dns_hosting/ "Сервис")) - суммарно мне обошлось около 1100 рублей/год. Кладем в наш репозиторий с сайтом файл CNAME, содержащий имя желаемого домена. Идем и добавляем две записи в DNS для нашего домена:

    ```bash
    @ A 192.30.252.153
    @ A 192.30.252.154
    # перенаправление с www.*
    www CNAME @
    ```

    (`@` значит наш корневой домен.) Ждем пару часов. Результат можно узнать примерно так:

    ```bash
    $ dig domain.name +nostats +nocomments +nocmd
    ; <<>> DiG 9.9.2-P2 <<>> domain.name +nostats +nocomments +nocmd
    ;; global options: +cmd
    ;domain.name. IN A
    domain.name. 912 IN A 192.30.252.153
    domain.name. 912 IN A 192.30.252.154
    ...
    ```

* На радостях создал [собственный репозиторий](ftp://repo.arcanis.me/repo "Репозиторий"), в котором будут лежать некоторые пакеты из AUR, которые я использую. Планируется поддержка обеих архитектур.
* Поскольку репозиторий требует ftp, то перевел samba на ftp. Проблему доступа решил опциями монтирования:

    ```bash
    # только чтение
    /home/arcanis/music /srv/ftp/music ext4 defaults,bind,ro 0 0
    /home/arcanis/arch/repo /srv/ftp/repo ext4 defaults,bind,ro 0 0
    # чтение и запись (файл ограничен 2 Гб)
    /home/arcanis/share.fs /srv/ftp/share ext4 defaults,rw 0 0
    ```

    Для отсутствия доступа извне к директории с музыкой, используется логин под специальным пользователем и ограничение `anon_world_readable_only=YES`. Также привожу свой файл настроек `/etc/vsftpd.conf`:

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

    Теперь добавим переадресацию с `repo.arcanis.me` на нужный IP адрес. Для этого внесем следующие записи в DNS:

    ```bash
    repo A 89.249.170.38
    ```

*   В ближайшее время (как дойду до магазина с деньгами) планируется приобретение небольшого сервера для работы на постоянной основе (компиляция пакетов, репозиторий, файлообмен, бэкапы).

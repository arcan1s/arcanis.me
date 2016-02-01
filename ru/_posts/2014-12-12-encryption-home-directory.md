---
category: ru
type: paper
hastr: true
layout: paper
tags: linux, systemd, ecryptfs
title: Как зашифровать хомяк и не об%$#аться. For dummies
short: ecnryption-home-directory
---
<figure class="img">![single-door](/resources/papers/single-door.jpg)</figure> Статья
посвящена шифрованию домашнего каталога с использованием ecryptfs и настройке
автомонтирования посредством systemd с использованием ключа на флешке.

<!--more-->

## <a href="#preparation" class="anchor" id="preparation"><span class="octicon octicon-link"></span></a>Шаг 0: Подготовка

1.  Разлогинились пользователем. То есть совсем-совсем.
2.  Зашли под root в tty. Дальнейшие действия описаны от него.
3.  Передвинули наш хомяк и создали пустую директорию (`s/$USER/имя пользователя/`):

    ```bash
    mv /home/{$USER,$USER-org}
    mkdir /home/$USER
    chmod 700 /home/$USER
    chown $USER:users /home/$USER
    ```

## <a href="#step1" class="anchor" id="step1"><span class="octicon octicon-link"></span></a>Шаг 1: Шифрование

Самое распространенное решение в интернетах - воспользоваться автоматическими
тулзами. Однако в нашем случае они не подходят, так как нам необходимо
импортировать сигнатуру ключа / пароля, что при данном решении невозможно (или
у автора руки кривые).

Делается шифрование следующим образом (lol):

```bash
mount -t ecryptfs /home/$USER /home/$USER
```

В процессе он у нас задаст несколько вопросов (я предлагаю первое монтирование
делать в интерактивном режиме). Ответы можно взять примерно такие (в
комментариях показано, что эти опции делают), обратите внимание, что если вы что
то измените, то изменится и некоторые строчки далее:

```bash
# ключ или сертификат. Второе надежнее, но до тех пор пока не потеряете %)
Select key type to use for newly created files:
 1) passphrase
 2) openssl
Selection: 1
# пароль
Passphrase:
# шифрование, ставим берем дефолт
Select cipher:
 1) aes: blocksize = 16; min keysize = 16; max keysize = 32
 2) blowfish: blocksize = 8; min keysize = 16; max keysize = 56
 3) des3_ede: blocksize = 8; min keysize = 24; max keysize = 24
 4) twofish: blocksize = 16; min keysize = 16; max keysize = 32
 5) cast6: blocksize = 16; min keysize = 16; max keysize = 32
 6) cast5: blocksize = 8; min keysize = 5; max keysize = 16
Selection [aes]: 1
# размер ключа, берем дефолт
Select key bytes:
 1) 16
 2) 32
 3) 24
Selection [16]: 1
# разрешать читать/писать в нешифрованные файлы в точке монтирования
Enable plaintext passthrough (y/n) [n]: n
# включить шифрование имен файлов
Enable filename encryption (y/n) [n]: y
Filename Encryption Key (FNEK) Signature [360d0573e701851e]:
# многабукафниасилил
Attempting to mount with the following options:
  ecryptfs_unlink_sigs
  ecryptfs_fnek_sig=360d0573e701851e
  ecryptfs_key_bytes=16
  ecryptfs_cipher=aes
  ecryptfs_sig=360d0573e701851e
WARNING: Based on the contents of [/root/.ecryptfs/sig-cache.txt],
it looks like you have never mounted with this key
before. This could mean that you have typed your
passphrase wrong.

# подтверждаем, выходим
Would you like to proceed with the mount (yes/no)? : yes
Would you like to append sig [360d0573e701851e] to
[/root/.ecryptfs/sig-cache.txt]
in order to avoid this warning in the future (yes/no)? : yes
Successfully appended new sig to user sig cache file
Mounted eCryptfs
```

Далее просто копируем файлы из родного хомяка:

```bash
cp -a /home/$USER-org/. /home/$USER
```

## <a href="#step2" class="anchor" id="step2"><span class="octicon octicon-link"></span></a>Шаг 2: Автомонтирование с systemd

Создадим файл на флешке (я использовал microSD) со следующим содержанием (пароль
только поставьте свой):

```bash
passphrase_passwd=someverystronguniqpassword
```

Добавим автомонтирование флешки (направление `/mnt/key`) в `fstab` с опцией `ro`,
например так:

```bash
UUID=dc3ecb41-bc40-400a-b6bf-65c5beeb01d7    /mnt/key ext2     ro,defaults                            0 0
```

Теперь настроим монтирование хомяка. Опции монтирования можно подглядеть как то
так:

```bash
mount | grep ecryptfs
```

Однако замечу, что там указаны не все опции, необходимо добавить также `key`,
`no_sig_cache`, `ecryptfs_passthrough`. Таким образом, для systemd mount-юнит
выглядит примерно так (любители shell-простыней смогут написать свой демон,
потому что через `fstab` не сработает просто так (см. ниже)).

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

`XXXXX` нужно заменить на сигнатуру из опций, с которыми сейчас смонтирована
директория. Также нужно вставить имя пользователя и отредактировать путь к файлу
с паролем (и имя mount-юнита), если это необходимо. Автозагрука:

```bash
systemctl enable home-$USER.mount
```

Сервис для отмонтирования флешки, когда она не нужна будет:

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

Включаем:

```bash
systemctl enable umount-key.service
```

Перезагружаемся, если все ок, удаляем бекап. Если нет - значит что то где то
неправильно сделали, восстанавливаем из режима восстановления.

## <a href="#whynotfstab" class="anchor" id="whynotfstab"><span class="octicon octicon-link"></span></a>Почему не fstab?

В моем случае, мне не получилось заставить флешку монтироваться раньше. Таким
образом на загрузке я попадал в консоль восстановления из которой нужно было
просто продолжить загрузку. Существующие в интернете методы предлагают два
возможных варианта:

* Создать запись с опцией noauto, потом монтировать через специальную запись в
`rc.local`.
* Создать запись с опцией nofail, потом перемонтировать все разделы через
`rc.local`.

Оба варианта меня не устроили в виду их костыльности.

## <a href="#whynotpam" class="anchor" id="whynotpam"><span class="octicon octicon-link"></span></a>Почему не pam?

Другое распространенное предложение - монтировать через запись в настройках pam.
Мне этот вариант не подходит, так как у меня авторизация беспарольная по
отпечатку пальца.

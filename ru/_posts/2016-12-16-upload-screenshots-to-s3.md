---
category: ru
type: paper
hastr: true
layout: paper
tags: linux, s3
title: Как загрузить скриншот в S3 с помощью linux
short: upload-screenshots-to-s3
---
По ряду причин я вынужден загружать скриншоты в S3 вместо доступных публичных
ресурсов. Статья описывает простое решение, которое возволяет загрузить скриншоты
в S3 и возвращает ссылку на него, которое может быть использовано напрямую из
приложения для скриншотов.

<!--more-->

## Требования

1. Скриншотилка, которая позволяет использовать произвольные приложения для
открытия изображения (я использую [Spectacle](//www.kde.org/applications/graphics/spectacle/)).
2. Установленный и настроенный [s3cmd](//s3tools.org/s3cmd).
3. Установленный [xsel](//linux.die.net/man/1/xsel) для копирования ссылки в
буфер обмена.
4. Текстовый редактор.

## Desktop файл

Создайте desktop файл `$HOME/.local/share/applications/s3cmd.desktop` следующего
содержания

```ini
[Desktop Entry]
Name=s3cmd
Exec=s3cmd put --acl-public %U "s3://bucket-name/path/to/screenshots/`uuidgen`.png" | grep "Public URL" | cut -d ':' -f 2- | xsel -bi
Icon=image
Type=Application
Terminal=true
Categories=Graphics;
MimeType=image/png;
```

Основная магия в параметре `Exec=`. Он использует путь к временному файлу (`%U`),
загружает картинку в S3, используя `s3cmd` и возвращает ссылку с использованием
небольшой магии на bash.

## Использование

Просто сделайте скриншот и откройте его с помощью `s3cmd`.

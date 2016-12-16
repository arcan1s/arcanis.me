---
category: en
type: paper
hastr: true
layout: paper
tags: linux, s3
title: How to upload screenshots to S3 from linux
short: upload-screenshots-to-s3
---
For some reasons I'm forced to upload screenshots to S3 instead of public resourses.
This paper describes simple solution which allows to upload screenshots and get
link to it directly from screenshot tool.

<!--more-->

## Requirements

1. Screenshot tool which allows to use custom applications to open image (I use
[Spectacle](//www.kde.org/applications/graphics/spectacle/)).
2. Installed and configured [s3cmd](//s3tools.org/s3cmd).
3. Installed [xsel](//linux.die.net/man/1/xsel) to copy URL to clipboard.
4. Text editor.

## Desktop file

Create desktop file in `$HOME/.local/share/applications/s3cmd.desktop` with the
following content

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

The magic is in `Exec=` parameter. It uses path to temporary created file (`%U`),
uploads image to S3 by using `s3cmd` and gets URL from it with some bash magic.

## Usage

Just do screenshot and open it with `s3cmd`.

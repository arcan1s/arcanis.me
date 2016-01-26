---
category: ru
hastr: true
layout: project
title: git-etc
short: git-etc
tags: git, python, qt, linux, system, демон
hasgui: true
hasdocs: false
developers:
    - Evgeniy Alekseev
license: GPLv3
links:
    - Пакет в <a href="//aur.archlinux.org/packages/git-etc" title="AUR">AUR</a>
---
<!-- info block -->
## <a href="#info" class="anchor" id="info"><span class="octicon octicon-link"></span></a>Информация

Простой демон, который создает git репозиторий в указанной директории и создает коммит в указанный промежуток времени.

```bash
$ git-etc --help
Simple daemon written on BASH for monitoring changes in files

Usage: git-etc [ -c | --config /etc/git-etc.conf ] [ -h | --help ] [ -v | --version ]

Parametrs:
  -c  --config      - path to configuration file
  -h  --help        - show this help and exit
  -v  --version     - show version and exit

See "man 1 git-etc" for more details
```

```bash
$ ctrlconf --help
GUI for git-etc daemon

Usage: ctrlconf [ --default ] [ -h | --help ] [ -v | --version ]

Additional parametrs:
      --default     - create default configuration file
  -h  --help        - show this help and exit
  -v  --version     - show version and exit

See "man 1 ctrlconf" for more details
```

### <a href="#devel" class="anchor" id="devel"><span class="octicon octicon-link"></span></a>Разработчики

{% for devel in page.developers %}
* {{ devel }}{% endfor %}

### <a href="#license" class="anchor" id="license"><span class="octicon octicon-link"></span></a>Лицензия

* {{ page.license }}

<!-- end of info block -->

<!-- install block -->
## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Установка

### <a href="#instruction" class="anchor" id="instruction"><span class="octicon octicon-link"></span></a>Инструкция

* Скачайте [архив](//github.com/arcan1s/git-etc/releases "GitHub") с актуальной версией исходных файлов.
* Извлеките из него файлы и установите приложение:

    ```bash
    ./install.sh "/путь/к/корню/"
    ```

    Если Вы хотите установить в `/`, Вы должны запустить это, как root:

    ```bash
    sudo ./install.sh
    ```

    Если путь не указан, пакет будет установлен в `/`.

### <a href="#dependencies" class="anchor" id="dependencies"><span class="octicon octicon-link"></span></a>Зависимости

Все было протестировано на последних версиях зависимостей.

* Bash (включая awk, grep, sed)
* git
* python2 *(make)*
* systemd *(опционально, service-файл)*
* python2-pyqt4 *(опционально, GUI)*
* xterm *(опционально, GUI)*

<!-- end of install block -->

<!-- howto block -->
## <a href="#howto" class="anchor" id="howto"><span class="octicon octicon-link"></span></a>Использование

Если Вы хотите запустить демон в `/etc`, просто запустите

```bash
systemctl start git-etc
```

Если Вы хотите включить автозагрузку демона, запутите

```bash
systemctl enable git-etc
```

Но Вы можете изменить путь к конфигурационному файлу или изменить параметры. Для этого, скопируйте (рекомендуется) исходный конфигурационный файл

```bash
cp /etc/git-etc.conf /новый/путь/к/git-etc.conf
```

и отредактируйте его. Затем скопируйте исходный service-файл в `/etc`:

```bash
cp /usr/lib/systemd/system/git-etc.service /etc/systemd/system/git-etc-my-profile.service
```

Замените следующую строку в этом файле:

```bash
ExecStart=/usr/bin/git-etc -c /etc/git-etc.conf
```

на

```bash
ExecStart=/usr/bin/git-etc -c /новый/путь/к/git-etc.conf
```
<!-- end of howto block -->

<!-- config block -->
## <a href="#config" class="anchor" id="config"><span class="octicon octicon-link"></span></a>Настройка

Все настройки хранятся в `/etc/git-etc.conf`. После редактирования, Вы должны перезапустить демон

```bash
systemctl restart git-etc
```

### <a href="#options" class="anchor" id="options"><span class="octicon octicon-link"></span></a>Опции

|       |       |
|-------|-------|
| DIRECTORY | Полный путь к рабочей директории с наблюдаемыми файлами. По умолчанию `/etc`. |
| TIMESLEEP | Промежуток времени между обновлениями, часы. Должно быть целым и >= 1\. По умолчанию `12`. |
| IGNORELIST | Список файлов, которые не будут наблюдаться. Разделитель ";;". Может быть пустым. |
| FORALL | `1` включит доступ для обычного пользователя. По умолчанию `1`. |
<!-- end of config block -->

<!-- gui block -->
## <a href="#gui" class="anchor" id="gui"><span class="octicon octicon-link"></span></a>Графический интерфейс

Control Config (`ctrlconf`) - графический интерфейс для `git-etc`, написанный на `Python2/PyQt4`. Данное приложение позволяет Вам увидеть список коммитов и изменения в файлах в данных коммитах. Также данное приложение позволит Вам откатиться на указанный коммит (все файлы, посредством `git reset --hard`, или только указанный, посредством `git diff && git apply`). Также Вы можете объединить старый и новый конфигурационные файлы (используются две
ветки репозитория master и experimental). Приложение может потребовать привелегии root, убедитесь, что пакет `sudo` установлен.

### <a href="#gui_configuration" class="anchor" id="gui_configuration"><span class="octicon octicon-link"></span></a>Настройка

Запустите приложение и откройте окно настроек из меню.

### <a href="#screenshots" class="anchor" id="screenshots"><span class="octicon octicon-link"></span></a>Скриншоты

<div class="thumbnails">
  {% assign scrdesc = "Основное окно" %}
  {% assign scrname = "git-etc_mainwindow" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Окно 'О программе'" %}
  {% assign scrname = "git-etc_aboutwindow" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Окно с просмотром изменений при коммите" %}
  {% assign scrname = "git-etc_commitwindow" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Окно объединения" %}
  {% assign scrname = "git-etc_mergingwindow" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Окно отката" %}
  {% assign scrname = "git-etc_rollbackwindow" %}
  {% include prj_scr.html %}
</div>
<!-- end of gui block -->

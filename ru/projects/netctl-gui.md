---
category: ru
hastr: true
layout: project
title: Netctl GUI
short: netctl-gui
tags: archlinux, c++, qt, сеть, kde, netctl, система, dbus, библиотека
hasgui: true
hasdocs: true
developers:
    - Evgeniy Alekseev
    - nosada (перевод на японский)
license: GPLv3
links:
    - Страница на <a href="//linux.softpedia.com/get/System/Networking/Netctl-GUI-103383.shtml" title="Softpedia">Softpedia</a>
    - Страница на <a href="//kde-apps.org/content/show.php?content=164490" title="kde-apps">kde-apps.org</a>
    - <a href="//aur.archlinux.org/pkgbase/netctl-gui/" title="AUR">Пакет в AUR</a>
    - <a href="/devs/netctl-gui-dbus-api.html" title="DBus API">Описание DBus API</a>
    - <a href="/devs/netctl-gui-security-notes.html" title="Security">Примечания о безопасности</a>
---
<!-- info block -->
## <a href="#info" class="anchor" id="info"><span class="octicon octicon-link"></span></a>Информация

Графическая оболочка для `netctl` (набор скриптов для поднятия сети в Arch'е). Написана на `C++` с использованием библиотеки `Qt`. На текущим момент умеет работать с профилями, в том числе создавать новые, а также умеет подключаться к WiFi. Также предоставляет библиотеку для взаимодействия с netctl и виджет и DataEngine для KDE.

**ВНИМАНИЕ:** [НУЖНЫ ПЕРЕВОДЧИКИ!](//github.com/arcan1s/netctl-gui/issues/3 "Тикет")

```bash
$ netctl-gui --help
Использование:
netctl-gui [ options ]
Опции:
 Открыть окно:
       --detached        - запустить открепленным от консоли
       --maximized       - запустить развернутым
       --minimized       - запустить свернутым в трей
       --about           - показать окно "О программе"
       --netctl-auto     - показать окно netctl-auto
       --settings        - показать окно настроек
 Функции:
   -e, --essid <arg>     - выбрать данный ESSID
   -o, --open <arg>      - открыть данный профиль
   -s, --select <arg>    - выбрать данный профиль
 Дополнительные флаги:
   -c, --config <arg>    - прочитать настройки из данного файла
   -d, --debug           - показать отладочную информацию
       --default         - запустить со стандартными настройками
       --set-opts <arg>  - установить опции для данного запуска, разделенные запятыми
   -t, --tab <arg>       - открыть вкладку с этим номером
 Показать сообщения:
   -v, --version         - показать версию и выход
   -i, --info            - показать информацию о сборке и выход
   -h, --help            - показать справку и выход
```

```bash
$ netctlgui-helper --help
Использование:
netctlgui-helper [ options ]
Опции:
   -c, --config <arg>    - прочитать настройки из данного файла
   -d, --debug           - показать отладочную информацию
       --nodaemon        - не запускать как демон
       --replace         - принудительно заменить существующую сессию
       --restore         - принудительно восстановить существующую сессию
       --system          - не считывать пользовательские настройки, только системные
 Показать сообщения:
   -v, --version         - показать версию и выход
   -i, --info            - показать информацию о сборке и выход
   -h, --help            - показать справку и выход
```

### <a href="#devel" class="anchor" id="devel"><span class="octicon octicon-link"></span></a>Разработчики

{% for devel in page.developers %}
* {{ devel }}{% endfor %}

### <a href="#license" class="anchor" id="license"><span class="octicon octicon-link"></span></a>Лицензия

* {{ page.license }}

### <a href="#changelog" class="anchor" id="changelog"><span class="octicon octicon-link"></span></a>Changelog

[CHANGELOG](//github.com/arcan1s/netctl-gui/blob/master/CHANGELOG "GitHub")

<!-- end of info block -->

<!-- install block -->
## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Установка

### <a href="#instruction" class="anchor" id="instruction"><span class="octicon octicon-link"></span></a>Инструкция

* Скачайте [архив](//github.com/arcan1s/netctl-gui/releases "GitHub") с актуальной версией исходных файлов.
* Извлеките из него файлы и установите приложение. Если Вы хотите установить в `/`, Вы должны запустить как root:

    ```bash
    cd /путь/к/распакованному/архиву
    mkdir build && cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release ../
    make
    sudo make install
    ```

    Доступные флаги cmake:

    * компоненты:
        * `-DBUILD_DATAENGINE:BOOL=0` - не собирать DataEngine
        * `-DBUILD_GUI:BOOL=0` - не собирать GUI
        * `-DBUILD_HELPER:BOOL=0` - не собирать хелпер
        * `-DBUILD_LIBRARY:BOOL=0` - не собирать библиотеку
        * `-DBUILD_PLASMOID:BOOL=0` - не собирать плазмоид
    * дополнительные компоненты:
        * `-DBUILD_DOCS:BOOL=0` - не собирать документацию разработчика
        * `-DBUILD_TEST:BOOL=1` - собирать авто тесты для библиотеки и хелпера
    * свойства проекта:
        * `-DDBUS_SYSTEMCONF_PATH=/etc/dbus-1/system.d/` - путь к системный файлам конфигурации DBus
        * `-DSYSTEMD_SERVICE_PATH=lib/systemd/system` - путь к сервису systemd
        * `-DUSE_CAPABILITIES:BOOL=0` - не использовать setcap, чтобы дать необходимые привилегии хелперу
        * `-DBUILD_KDE4:BOOL=1` - собирать виджет под KDE4 вместо KF5
        * `-DUSE_QT5:BOOL=0` - использовать Qt4 вместо Qt5

### <a href="#dependencies" class="anchor" id="dependencies"><span class="octicon octicon-link"></span></a>Зависимости

Все было протестировано на последних версиях зависимостей.

* netctl
* qt5-base *(если используется Qt5)* **или** qt4 *(если используется Qt4)*
* automoc4 *(make)*
* cmake *(make)*
* qt5-tools *(make, если используется Qt5)*
* kdebase-workspace *(опционально, KDE4 виджет)*
* plasma-frameworks *(опционально, KF5 виджет)*
* sudo *(опционально, поддержка sudo)*
* wpa_supplicant *(опционально, поддержка WiFi)*

<!-- end of install block -->

<!-- howto block -->
## <a href="#howto" class="anchor" id="howto"><span class="octicon octicon-link"></span></a>Использование

Просто запустите приложение `netctl-gui`. Если потребуется (и если Вы используете KDE), можете добавить виджет `netctl`, предоставляемый приложением.

<!-- end of howto block -->

<!-- config block -->
## <a href="#config" class="anchor" id="config"><span class="octicon octicon-link"></span></a>Настройка

Рекомендуется использовать графический интерфейс для настройки. Конфигурационные файлы:

* Графический интерфейс и хелпер
    * `$HOME/.config/netctl-gui.conf` - пользовательские настройки GUI/хелпера
    * `/etc/netctl-gui.conf` - системные настройки хелпера
* DataEngine (KDE4 версия)
    * `$KDEHOME/share/config/plasma-dataengine-netctl.conf` - пользовательские настройки DataEngine
    * `$KDESYSTEM/share/config/plasma-dataengine-netctl.conf` - системные настройки DataEngine
* DataEngine (KF5 версия)
    * `$HOME/.config/plasma-dataengine-netctl.conf` - пользовательские настройки DataEngine
    * `/etc/xdg/plasma-dataengine-netctl.conf` - системные настройки DataEngine

Для настройки виджета и DataEngine рекомендуется использовать графический интерфейс. Все настройки графического интерфейса хранятся в `$HOME/.config/netctl-gui.conf`. Для редактирования настоятельно рекомендуется использовать графический интерфейс.

<!-- end of config block -->

<!-- gui block -->
## <a href="#gui" class="anchor" id="gui"><span class="octicon octicon-link"></span></a>Графический интерфейс

Графический интерфейс предоставляется приложением `netctl-gui`.

### <a name="screenshots" class="anchor" href="#screenshots"><span class="octicon octicon-link"></span></a>Скриншоты

<div class="thumbnails">
  {% assign scrdesc = "DataEngine" %}
  {% assign scrname = "netctl-gui_dataengine" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Виджет" %}
  {% assign scrname = "netctl-gui_plasmoid" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Настройки виджета" %}
  {% assign scrname = "netctl-gui_plasmoid_conf_01" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Настройки виджета" %}
  {% assign scrname = "netctl-gui_plasmoid_conf_02" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Настройки виджета" %}
  {% assign scrname = "netctl-gui_plasmoid_conf_03" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Главное окно" %}
  {% assign scrname = "netctl-gui_main" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Окно создания профиля" %}
  {% assign scrname = "netctl-gui_profile" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "WiFi меню" %}
  {% assign scrname = "netctl-gui_wifi" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Окно 'О программе'" %}
  {% assign scrname = "netctl-gui_about" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Окно netctl-auto" %}
  {% assign scrname = "netctl-gui_netctl-auto" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Окно настроек" %}
  {% assign scrname = "netctl-gui_settings" %}
  {% include prj_scr.html %}
</div>
<!-- end of gui block -->

---
permalink: projects/netctl-gui
hastr: true
layout: project
title: Netctl GUI
short: netctl-gui
tags: archlinux, c++, qt, network, kde, netctl, system, dbus, library
hasgui: true
hasdocs: true
developers:
    - Evgeniy Alekseev
    - nosada (Japanese translation)
license: GPLv3
links:
    - Page on <a href="//linux.softpedia.com/get/System/Networking/Netctl-GUI-103383.shtml" title="Softpedia">Softpedia</a>
    - Page on <a href="//kde-apps.org/content/show.php?content=164490" title="kde-apps">kde-apps.org</a>
    - <a href="//aur.archlinux.org/pkgbase/netctl-gui/" title="AUR">AUR package</a>
    - <a href="/devs/netctl-gui-dbus-api.html" title="DBus API">DBus API reference</a>
    - <a href="/devs/netctl-gui-security-notes.html" title="Security">Security notes</a>
---
<!-- info block -->

Graphical interface for `netctl` (several scripts for work with network connection
in Archlinux). It is written on `C++` using `Qt` library. Now it may work with
profiles and may create new profiles. Also it may create a connection to WiFi.
Moreover, it provides a Qt library for interaction with netctl and widget and
DataEngine for KDE4/KF5.

<!--more-->

**NOTE:** [LOOKING FOR TRANSLATORS!](//github.com/arcan1s/netctl-gui/issues/3
"Ticket")

```bash
$ netctl-gui --help
Usage:
netctl-gui [ options ]
Options:
 Open window:
       --detached        - start detached from console
       --maximized       - start maximized
       --minimized       - start minimized to tray
       --about           - show about window
       --netctl-auto     - show netctl-auto window
       --settings        - show settings window
 Functions:
   -e, --essid <arg>     - select this ESSID
   -o, --open <arg>      - open this profile
   -s, --select <arg>    - select this profile
 Additional flags:
   -c, --config <arg>    - read configuration from this file
   -d, --debug           - print debug information
       --default         - start with default settings
       --set-opts <arg>  - set options for this run, comma separated
   -t, --tab <arg>       - open a tab with this number
 Show messages:
   -v, --version         - show version and exit
   -i, --info            - show build information and exit
   -h, --help            - show this help and exit
```

```bash
$ netctlgui-helper --help
Usage:
netctlgui-helper [ options ]
Options:
   -c, --config <arg>    - read configuration from this file
   -d, --debug           - print debug information
       --nodaemon        - do not start as daemon
       --replace         - force replace the existing session
       --restore         - force restore the existing session
       --system          - do not read user configuration, system-wide only
 Show messages:
   -v, --version         - show version and exit
   -i, --info            - show build information and exit
   -h, --help            - show this help and exit
```

### <a href="#devel" class="anchor" id="devel"><span class="octicon octicon-link"></span></a>Developers and contributors

{% for devel in page.developers %}
* {{ devel }}{% endfor %}

### <a href="#license" class="anchor" id="license"><span class="octicon octicon-link"></span></a>License

* {{ page.license }}

### <a href="#changelog" class="anchor" id="changelog"><span class="octicon octicon-link"></span></a>Changelog

[CHANGELOG](//github.com/arcan1s/netctl-gui/blob/master/CHANGELOG "GitHub")

<!-- end of info block -->

<!-- install block -->
## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Installation

### <a href="#instruction" class="anchor" id="instruction"><span class="octicon octicon-link"></span></a>Instruction

* Download an [archive](//github.com/arcan1s/netctl-gui/releases "GitHub") with
latest version of source files.
* Extract it and install the application. If you want install it into `/`, you
should run as root following commands:

    ```bash
    cd /path/to/extracted/archive
    mkdir build && cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release ../
    make
    sudo make install
    ```

    Available cmake flags are:

    * components:
        * `-DBUILD_DATAENGINE:BOOL=0` - do not build DataEngine
        * `-DBUILD_GUI:BOOL=0` - do not build GUI
        * `-DBUILD_HELPER:BOOL=0` - do not build helper daemon
        * `-DBUILD_LIBRARY:BOOL=0` - do not build library
        * `-DBUILD_PLASMOID:BOOL=0` - do not build Plasmoid
    * additional components:
        * `-DBUILD_DOCS:BOOL=0` - do not build developer documentation
        * `-DBUILD_TEST:BOOL=1` - build auto tests for the library and the helper
    * project properties:
        * `-DDBUS_SYSTEMCONF_PATH=/etc/dbus-1/system.d/` - path to DBus system configuration files
        * `-DSYSTEMD_SERVICE_PATH=lib/systemd/system` - path to systemd services
        * `-DUSE_CAPABILITIES:BOOL=0` - do not use setcap to get privileges to the helper
        * `-DBUILD_KDE4:BOOL=1` - build plasmoid under KDE4 instead of KF5
        * `-DUSE_QT5:BOOL=0` - use Qt4 instead of Qt5 for GUI

### <a href="#dependencies" class="anchor" id="dependencies"><span class="octicon octicon-link"></span></a>Dependencies

I want note that all were tested on latest version of dependencies.

* netctl
* qt5-base *(if Qt5 is used)* **or** qt4 *(if Qt4 is used)*
* automoc4 *(make)*
* cmake *(make)*
* qt5-tools *(make, if Qt5 is used)*
* kdebase-workspace *(optional, KDE4 widget)*
* plasma-frameworks *(optional, KF5 widget)*
* sudo *(optional, sudo support)*
* wpa_supplicant *(optional, WiFi support)*

<!-- end of install block -->

<!-- howto block -->
## <a href="#howto" class="anchor" id="howto"><span class="octicon octicon-link"></span></a>How to use

Just run application `netctl-gui`. If it is needed (and if you use KDE), you may
add widget `netctl`, which provides by the application.

<!-- end of howto block -->

<!-- config block -->
## <a href="#config" class="anchor" id="config"><span class="octicon octicon-link"></span></a>Configuration

It is recommended to use graphical interface for configuration. Configuration
files are:

* UI and helper
    * `$HOME/.config/netctl-gui.conf` - GUI/helper user configuration
    * `/etc/netctl-gui.conf` - helper system-wide configuration
* DataEngine (KDE4 version)
    * `$KDEHOME/share/config/plasma-dataengine-netctl.conf` - DataEngine user
    configuration
    * `$KDESYSTEM/share/config/plasma-dataengine-netctl.conf` - DataEngine
    system-wide configuration
* DataEngine (KF5 version)
    * `$HOME/.config/plasma-dataengine-netctl.conf` - DataEngine user configuration
    * `/etc/xdg/plasma-dataengine-netctl.conf` - DataEngine system-wide configuration

<!-- end of config block -->

<!-- gui block -->
## <a href="#gui" class="anchor" id="gui"><span class="octicon octicon-link"></span></a>Graphical user interface

Graphical interface provides by `netctl-gui` application.

### <a name="screenshots" class="anchor" href="#screenshots"><span class="octicon octicon-link"></span></a>Screenshots

<div class="thumbnails">
  {% assign scrdesc = "DataEngine" %}
  {% assign scrname = "netctl-gui_dataengine" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Widget" %}
  {% assign scrname = "netctl-gui_plasmoid" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Widget settings window" %}
  {% assign scrname = "netctl-gui_plasmoid_conf_01" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Widget settings window" %}
  {% assign scrname = "netctl-gui_plasmoid_conf_02" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Widget settings window" %}
  {% assign scrname = "netctl-gui_plasmoid_conf_03" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Main window" %}
  {% assign scrname = "netctl-gui_main" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Profile window" %}
  {% assign scrname = "netctl-gui_profile" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "WiFi window" %}
  {% assign scrname = "netctl-gui_wifi" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "About window" %}
  {% assign scrname = "netctl-gui_about" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "netctl-auto window" %}
  {% assign scrname = "netctl-gui_netctl-auto" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Settings window" %}
  {% assign scrname = "netctl-gui_settings" %}
  {% include prj_scr.html %}
</div>
<!-- end of gui block -->

---
permalink: projects/oblikuestrategies
hastr: true
layout: project
title: Oblikue strategies
short: oblikuestrategies
tags: qt, c++, kde, linux, fun
hasgui: true
hasdocs: false
developers:
    - Evgeniy Alekseev
license: GPL
links:
    - Page on <a href="//kde-look.org/content/show.php/oblikue-strategies?content=160503", title="kde-look">kde-look.org</a>
    - Archlinux <a href="//aur.archlinux.org/packages/kdeplasma-applets-oblikuestrategies", title="AUR">AUR package</a>
---
<!-- info block -->

Plasmoid written on `CPP` that displays a random draw from Brian Eno and Peter Schmidt's [Oblique Strategies](//en.wikipedia.org/wiki/Oblique_strategies "Wiki"). It is [GNOME applet](//gnome-look.org/content/show.php/Oblique+Strategies?content=78405 "gnome-look") fork with some of special features.

<!--more-->

### <a href="#devel" class="anchor" id="devel"><span class="octicon octicon-link"></span></a>Developers and contributors

{% for devel in page.developers %}
* {{ devel }}{% endfor %}

### <a href="#license" class="anchor" id="license"><span class="octicon octicon-link"></span></a>License

* {{ page.license }}

<!-- end of info block -->

<!-- install block -->
## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Installation

### <a href="#instruction" class="anchor" id="instruction"><span class="octicon octicon-link"></span></a>Instruction

* Download an [archive](//github.com/arcan1s/oblikuestrategies/releases "GitHub") with latest version of source files.
* Extract it and install the application. For global isntallation type:

    ```bash
    cd /where/is/applet/
    mkdir build && cd build
    cmake -DCMAKE_INSTALL_PREFIX=`kde4-config --prefix` -DCMAKE_BUILD_TYPE=Release ../
    make
    sudo make install
    ```

    For local installation type:

    ```bash
    cd /where/is/applet/
    mkdir build && cd build
    cmake -DCMAKE_INSTALL_PREFIX=`kde4-config --localprefix` -DCMAKE_BUILD_TYPE=Release ../
    make
    make install
    ```

* Restart plasma to load the applet:

    ```bash
    kquitapp plasma-desktop && sleep 2 && plasma-desktop
    ```

    Also you might need to run `kbuildsycoca4` in order to get the `*.desktop` file recognized:

    ```bash
    kbuildsycoca4 &> /dev/null
    ```

### <a href="#dependencies" class="anchor" id="dependencies"><span class="octicon octicon-link"></span></a>Dependencies

I want note that all were tested on latest version of dependencies.

* kdebase-workspace
* automoc4 *(make)*
* cmake *(make)*

<!-- end of install block -->

<!-- howto block -->
## <a href="#howto" class="anchor" id="howto"><span class="octicon octicon-link"></span></a>How to use

Open your Plasma widgets and select `Oblikue strategies`.

<!-- end of howto block -->

<!-- config block -->
## <a href="#config" class="anchor" id="config"><span class="octicon octicon-link"></span></a>Configuration

Right click on widget.

<!-- end of config block -->

<!-- gui block -->
## <a href="#gui" class="anchor" id="gui"><span class="octicon octicon-link"></span></a>Graphical user interface

### <a href="#screenshots" class="anchor" id="screenshots"><span class="octicon octicon-link"></span></a>Screenshots

<div class="thumbnails">
  {% assign scrdesc = "Widget" %}
  {% assign scrname = "oblikuestrategies_widget" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Configuration window" %}
  {% assign scrname = "oblikuestrategies_config" %}
  {% include prj_scr.html %}
</div>
<!-- end of gui block -->

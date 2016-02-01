---
permalink: ru/projects/oblikuestrategies
category: ru
hastr: true
layout: project
title: Oblikue strategies
short: oblikuestrategies
tags: qt, c++, kde, linux, досуг
hasgui: true
hasdocs: false
developers:
    - Evgeniy Alekseev
license: GPL
links:
    - Страница на <a href="//kde-look.org/content/show.php/oblikue-strategies?content=160503" title="kde-look">kde-look.org</a>
    - Пакет в <a href="//aur.archlinux.org/packages/kdeplasma-applets-oblikuestrategies" title="AUR">AUR</a>
---
<!-- info block -->

Плазмоид, написанный на `CPP` который показывает случайные карты из Brian Eno и
Peter Schmidt's [Oblique Strategies](//en.wikipedia.org/wiki/Oblique_strategies
"Wiki"). Это форк [апплета для GNOME]
(//gnome-look.org/content/show.php/Oblique+Strategies?content=78405 "gnome-look")
с некоторыми дополнительными фичами.

<!--more-->

### <a href="#devel" class="anchor" id="devel"><span class="octicon octicon-link"></span></a>Разработчики

{% for devel in page.developers %}
* {{ devel }}{% endfor %}

### <a href="#license" class="anchor" id="license"><span class="octicon octicon-link"></span></a>Лицензия

* {{ page.license }}

<!-- end of info block -->

<!-- install block -->
## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Установка

### <a href="#instruction" class="anchor" id="instruction"><span class="octicon octicon-link"></span></a>Инструкция

* Скачайте [архив](//github.com/arcan1s/oblikuestrategies/releases "GitHub") с
актуальной версией исходных файлов.
* Извлеките из него файлы и установите приложение. Для глобальной установки наберите:

    ```bash
    cd /путь/куда/распакован/архив
    mkdir build && cd build
    cmake -DCMAKE_INSTALL_PREFIX=`kde4-config --prefix` -DCMAKE_BUILD_TYPE=Release ../
    make
    sudo make install
    ```

    Для локальной:

    ```bash
    cd /where/your/applet/is/installed
    mkdir build && cd build
    cmake -DCMAKE_INSTALL_PREFIX=`kde4-config --localprefix` -DCMAKE_BUILD_TYPE=Release ../
    make
    make install
    ```

* Перезапустите plasma, чтобы загрузить апплет:

    ```bash
    kquitapp plasma-desktop && sleep 2 && plasma-desktop
    ```

    Также Вам может потребоваться запустить `kbuildsycoca4`, чтобы распознать
    `*.desktop` файл:

    ```bash
    kbuildsycoca4 &> /dev/null
    ```

### <a href="#dependencies" class="anchor" id="dependencies"><span class="octicon octicon-link"></span></a>Зависимости

Все было протестировано на последних версиях зависимостей.

* kdebase-workspace
* automoc4 *(make)*
* cmake *(make)*

<!-- end of install block -->

<!-- howto block -->
## <a href="#howto" class="anchor" id="howto"><span class="octicon octicon-link"></span></a>Использование

Откройте список виджетов Plasma и выберете `Oblikue strategies`.

<!-- end of howto block -->

<!-- config block -->
## <a href="#config" class="anchor" id="config"><span class="octicon octicon-link"></span></a>Настройка

Клик правой кнопкой по виджету.

<!-- end of config block -->

<!-- gui block -->
## <a href="#gui" class="anchor" id="gui"><span class="octicon octicon-link"></span></a>Графический интерфейс

### <a href="#screenshots" class="anchor" id="screenshots"><span class="octicon octicon-link"></span></a>Скриншоты

<div class="thumbnails">
  {% assign scrdesc = "Виджет" %}
  {% assign scrname = "oblikuestrategies_widget" %}
  {% include prj_scr.html %}
  {% assign scrdesc = "Окно настроек" %}
  {% assign scrname = "oblikuestrategies_config" %}
  {% include prj_scr.html %}
</div>
<!-- end of gui block -->

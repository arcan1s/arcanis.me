---
category: ru
type: paper
hastr: true
layout: paper
tags: archlinux, linux, сборка, qutim
title: Сборка Qutim с Qt5
short: building-qutim-using-qt5
---
Если кто-то не знает, [Qutim](//qutim.org "Домашняя страница Qutim") -
мультипротокольный кросс-платформенный месседжер. Написан он на `CPP` с
использованием библиотек Qt. Проект активно развивается. В этой статье речь
пойдет о реализации сборки данного пакета в Archlinux с использованием библиотек
Qt5 (а не Qt4, как это делают текущие пакеты в AUR).

<!--more-->

## <a href="#problems" class="anchor" id="problems"><span class="octicon octicon-link"></span></a>Что не так?

Да все так. Просто пакет использует для сборки систему [qbs]
(//qt-project.org/wiki/qbs "Wiki"), которая, на мой взгляд, немного странная.
Пакет, необходимый для сборки, [находится в AUR]
(//aur.archlinux.org/packages/qbs-git/ "AUR") (рекомендую git-версию). Когда я
спросил у Andrea Scarpino (который сопровождает все KDE и Qt пакеты в
официальные репозитории) по поводу переноса этого пакета в репозитории, он
ответил, что всему свое время. В принципе, я с ним согласен, так как проект,
судя по всему, еще немного сыроват.

## <a href="#prepare" class="anchor" id="prepare"><span class="octicon octicon-link"></span></a>Подготовка

Установим зависимости. Что-то может быть пропустил, зависимости сканировал с
использованием `namcap`:

```bash
pacman -Sy --asdeps clang git libc++abi qt5-quick1 qt5-x11extras
yaourt -S --asdeps jreen-git qbs-git
```

### <a href="#qbs" class="anchor" id="qbs"><span class="octicon octicon-link"></span></a>Настройка qbs

Желающие могут почитать документацию [по ссылке](//qt-project.org/wiki/qbs "Wiki")
или посмотреть примеры (включены в пакет). Загвоздка в том, что эта штука
использует файл настроек, который, во-первых, нужно сначала сгенерировать,
во-вторых, хранится в домашней директории (и только там). В теории, генерация
файла настроек (`~/.config/QtProject/qbs.conf`) происходит следующим образом:

```bash
qbs-setup-qt --detect
qbs-detect-toolchains
```

Сначала находим Qt для сборки, потом находим инструментарий (компиляторы,
например). Дальше вставляем инструментарий (например, нам для Qutim нужен
`clang`) в Qt, например, так:

```bash
sed 's/clang\\/qt-5-2-0\\/g' -i ~/.config/QtProject/qbs.conf
```

Альтернативные варианты - править файл вручную или воспользоваться `qbs-config-ui`
или `qbs-config` на Ваш выбор. Так или иначе, нужный файл мы сгенерировали,
сохраним его в будущей директории сборки:

```ini
[General]

[Qt]
filedialog=

[profiles]
clang\cpp\compilerName=clang++
clang\cpp\toolchainInstallPath=/usr/bin
clang\qbs\architecture=x86_64
clang\qbs\endianness=little
clang\qbs\toolchain=clang, llvm, gcc
gcc\cpp\compilerName=g++
gcc\cpp\toolchainInstallPath=/usr/bin
gcc\qbs\architecture=x86_64
gcc\qbs\endianness=little
gcc\qbs\toolchain=gcc
qutim\Qt\core\binPath=/usr/lib/qt/bin
qutim\Qt\core\buildVariant=release
qutim\Qt\core\config=shared, qpa, no_mocdepend, release, qt_no_framework
qutim\Qt\core\docPath=/usr/share/doc/qt
qutim\Qt\core\incPath=/usr/include/qt
qutim\Qt\core\libInfix=
qutim\Qt\core\libPath=/usr/lib
qutim\Qt\core\mkspecPath=/usr/lib/qt/mkspecs/linux-g++
qutim\Qt\core\namespace=
qutim\Qt\core\pluginPath=/usr/lib/qt/plugins
qutim\Qt\core\qtConfig=minimal-config, small-config, medium-config, large-config, full-config, gtk2, gtkstyle, fontconfig, libudev, evdev, xlib, xcb-glx, xcb-xlib, xcb-sm, xrender, accessibility-atspi-bridge, linuxfb, c++11, accessibility, opengl, shared, qpa, reduce_exports, reduce_relocations, clock-gettime, clock-monotonic, mremap, getaddrinfo, ipv6ifname, getifaddrs, inotify, eventfd, system-jpeg, system-png, png, system-freetype, no-harfbuzz, system-zlib, nis, cups, iconv, glib, dbus, dbus-linked, openssl-linked, xcb, xinput2, alsa, pulseaudio, icu, concurrent, audio-backend, release
qutim\Qt\core\version=5.2.0
qutim\cpp\compilerName=clang++
qutim\cpp\toolchainInstallPath=/usr/bin
qutim\qbs\architecture=x86_64
qutim\qbs\endianness=little
qutim\qbs\toolchain=clang, llvm, gcc
```

[qbs-qutim.conf](/resources/docs/qutim-qt5-git/qbs-qutim.conf "Файл")

### <a href="#patch" class="anchor" id="patch"><span class="octicon octicon-link"></span></a>Готовим патч для исходников

Первая проблема - `clang` (по крайней мере, в Archlinux):

```diff
diff -ruN qutim.orig/core/libqutim.qbs qutim/core/libqutim.qbs
--- qutim.orig/core/libqutim.qbs    2014-01-06 15:39:56.000000000 +0400
+++ qutim/core/libqutim.qbs 2014-01-06 15:44:54.502175067 +0400
@@ -75,7 +75,7 @@
         cpp.linkerFlags: {
             var flags = base;
             if (qbs.toolchain.contains("clang") && qbs.targetOS.contains("linux"))
-                flags = flags.concat("-stdlib=libc++ -lcxxrt");
+                flags = flags.concat("-lc++abi");
             return flags;
         }

```

И пофиксить сборку библиотеки для Vk:

```diff
diff -ruN qutim.orig/protocols/vkontakte/vreen/vreen.qbs qutim/protocols/vkontakte/vreen/vreen.qbs
--- qutim.orig/protocols/vkontakte/vreen/vreen.qbs  2014-01-06 15:41:42.000000000 +0400
+++ qutim/protocols/vkontakte/vreen/vreen.qbs   2014-01-06 15:46:47.142178486 +0400
@@ -5,6 +5,7 @@
     property string vreen_qml_path: "bin"
     property string vreen_lib_path: "lib"
     property string vreen_libexec_path: "lib"
+    property string lib_path: "lib"

     property string vreen_version_major:  1
     property string vreen_version_minor: 9
```

[qutim-qbs-1.1.patch](/resources/docs/qutim-qt5-git/qutim-qbs-1.1.patch "Файл")

### <a href="#sources" class="anchor" id="sources"><span class="octicon octicon-link"></span></a>Получаем исходники

```bash
# клонируем репозиторий
git clone https://github.com/euroelessar/qutim
# кладем файл настроек qbs
mkdir -p .config/QtProject
cp qbs-qutim.conf .config/QtProject/qbs.conf
# создаем директорию сборки
mkdir build
# обновляем подмодули
cd qutim
git submodule update --init --recursive
# патчим
cd ..
patch -p0 -i qutim-qbs-1.1.patch
```

## <a href="#build" class="anchor" id="build"><span class="octicon octicon-link"></span></a>Сборка

```bash
cd qutim
HOME=$(pwd) qbs -j $(nproc) -d ../build release profile:qutim
```

Я пытался сделать универсальный способ сборки пакета, поэтому такое странное
переназначение домашней директории. Флаг `-j` указывает число потоков сборки,
флаг `-d` директорию сборки, `release` тип сборки (debug, release), `profile`
используемый профиль, описанный в файле настроек.

## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Установка

```bash
HOME=$(pwd) sudo qbs install -d ../build --install-root "/usr" profile:qutim
```

Из нового - указание корневого каталога (`--install-root`). Без этого пакет будет
установлен в `/` (`/bin` и `/lib`).

## <a href="#pkgbuild" class="anchor" id="pkgbuild"><span class="octicon octicon-link"></span></a>PKGBUILD

```bash
pkgname=qutim-qt5-git
_gitname=qutim
pkgver=v0.3.1.967.gc56d61e
pkgrel=1
pkgdesc="Multiplatform instant messenger (qt5 build). Git version"
arch=('i686' 'x86_64')
url="http://qutim.org"
license=('LGPL' 'GPL3')
depends=('jreen-git' 'libc++abi' 'qt5-quick1' 'qt5-x11extras')
makedepends=('clang' 'qbs')
conflicts=(qutim-0.2_ru-git, qutim-0.3-git, qutim-stable, qutim-git)
source=("${_gitname}::git+https://github.com/euroelessar/qutim.git"
        "qutim-qbs-1.1.patch"
        "qbs-qutim.conf")
md5sums=('SKIP'
         '12c30176729a5230ff7e6effbb1b37f8'
         '40f096b269eb00b040035591fce8e259')

pkgver() {
  cd "${_gitname}"
  git describe --always | sed 's|-|.|g'
}

prepare() {
  # FIXME: dirty hack
  mkdir -p "${srcdir}/.config/QtProject"
  cp "${srcdir}/qbs-qutim.conf" "${srcdir}/.config/QtProject/qbs.conf"
  # create build directory
  if [[ -d ${srcdir}/build ]]; then
    rm -rf "${srcdir}/build"
  fi
  mkdir "${srcdir}/build"

  cd "${_gitname}"
  # update modules
  git submodule update --init --recursive
  # fix qbs build
  cd ..
  patch -p0 -i "${srcdir}/qutim-qbs-1.1.patch"
}

build() {
  cd "${_gitname}"
  HOME="${srcdir}" qbs -j $(nproc) -d ../build release profile:qutim
}

package() {
  cd "${srcdir}/${_gitname}"
  HOME="${srcdir}" qbs install -d ../build --install-root "${pkgdir}/usr" profile:qutim
}
```

[PKGBUILD](/resources/docs/qutim-qt5-git/PKGBUILD "Файл")

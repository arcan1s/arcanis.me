---
category: en
type: paper
layout: paper
hastr: true
tags: archlinux, linux, building, qutim
title: Building Qutim using Qt5
short: building-qutim-using-qt5
---
[Qutim](//qutim.org) is a multiprotocol and cross platform messenger. It is
written on `CPP` using Qt library. The project is actively developed. In this
paper I will say about building this package in Archlinux using Qt5 library
(instead of Qt4 which is used in current AUR packages).

<!--more-->

# <a href="#problems" class="anchor" id="problems"><span class="octicon octicon-link"></span></a>What's wrong?

This package uses [qbs](//qt-project.org/wiki/qbs "Wiki") for building, which is
a bit strange IMHO. A package, which is necessary for building, is [in
AUR](//aur.archlinux.org/packages/qbs-git/ "AUR"). I recommend to use git
version of the package. When I asked Andrea Scarpino (who maintains KDE and Qt
packages into the official repos) about qbs, he told me "we will support it in
time". And I agree with him, the project seems to be a little unstable.

# <a href="#prepare" class="anchor" id="prepare"><span class="octicon octicon-link"></span></a>Prepare

Install dependences. I had used `namcap`, so maybe I missed something:

```bash
pacman -Sy --asdeps clang git libc++abi qt5-quick1 qt5-x11extras
yaourt -S --asdeps jreen-git qbs-git
```

### <a href="#qbs" class="anchor" id="qbs"><span class="octicon octicon-link"></span></a>qbs settings

You may read about qbs [on the link](//qt-project.org/wiki/qbs "Wiki") or see
examples which are provides by the package. qbs uses configuration file that
firstly you must create and secondly it is stored in your home directory. In
theory a configuration file creating (`~/.config/QtProject/qbs.conf`) looks like
this:

```bash
qbs-setup-qt --detect
qbs-detect-toolchains
```

Firstly we find Qt libraries. Then we find toolchains (such as compilers). And
next we must insert a toolchain into Qt profile (for example, we need `clang`
toolchain):

```bash
sed 's/clang\\/qt-5-2-0\\/g' -i ~/.config/QtProject/qbs.conf
```

And there are other ways. You may edit the file manually or use  `qbs-config-ui`
or `qbs-config`. So, we have created the configuration file and put it into
build directory:

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
qutim\Qt\core\qtConfig=minimal-config, small-config, medium-config,large-config, full-config, gtk2, gtkstyle, fontconfig, libudev, evdev, xlib,xcb-glx, xcb-xlib, xcb-sm, xrender, accessibility-atspi-bridge, linuxfb, c++11,accessibility, opengl, shared, qpa, reduce_exports, reduce_relocations,clock-gettime, clock-monotonic, mremap, getaddrinfo, ipv6ifname, getifaddrs,inotify, eventfd, system-jpeg, system-png, png, system-freetype, no-harfbuzz,system-zlib, nis, cups, iconv, glib, dbus, dbus-linked, openssl-linked, xcb,xinput2, alsa, pulseaudio, icu, concurrent, audio-backend, release
qutim\Qt\core\version=5.2.0
qutim\cpp\compilerName=clang++
qutim\cpp\toolchainInstallPath=/usr/bin
qutim\qbs\architecture=x86_64
qutim\qbs\endianness=little
qutim\qbs\toolchain=clang, llvm, gcc
```

[qbs-qutim.conf](/resources/docs/qutim-qt5-git/qbs-qutim.conf "File")

### <a href="#patch" class="anchor" id="patch"><span class="octicon octicon-link"></span></a>Patch for sources

The first problem is `clang` (at least in Archlinux):

```diff
diff -ruN qutim.orig/core/libqutim.qbs qutim/core/libqutim.qbs
--- qutim.orig/core/libqutim.qbs    2014-01-06 15:39:56.000000000 +0400
+++ qutim/core/libqutim.qbs 2014-01-06 15:44:54.502175067 +0400
@@ -75,7 +75,7 @@
         cpp.linkerFlags: {
             var flags = base;
             if (qbs.toolchain.contains("clang") &&
qbs.targetOS.contains("linux"))
-                flags = flags.concat("-stdlib=libc++ -lcxxrt");
+                flags = flags.concat("-lc++abi");
             return flags;
         }

```

And the second one is Vk plugin:

```diff
diff -ruN qutim.orig/protocols/vkontakte/vreen/vreen.qbs
qutim/protocols/vkontakte/vreen/vreen.qbs
--- qutim.orig/protocols/vkontakte/vreen/vreen.qbs  2014-01-06
15:41:42.000000000 +0400
+++ qutim/protocols/vkontakte/vreen/vreen.qbs   2014-01-06 15:46:47.142178486
+0400
@@ -5,6 +5,7 @@
     property string vreen_qml_path: "bin"
     property string vreen_lib_path: "lib"
     property string vreen_libexec_path: "lib"
+    property string lib_path: "lib"

     property string vreen_version_major:  1
     property string vreen_version_minor: 9
```

[qutim-qbs-1.1.patch](/resources/docs/qutim-qt5-git/qutim-qbs-1.1.patch "File")

### <a href="#sources" class="anchor" id="sources"><span class="octicon octicon-link"></span></a>Get sources

```bash
# clone repo
git clone https://github.com/euroelessar/qutim
# save qbs.conf
mkdir -p .config/QtProject
cp qbs-qutim.conf .config/QtProject/qbs.conf
# create build directory
mkdir build
# update submodules
cd qutim
git submodule update --init --recursive
# patch
cd ..
patch -p0 -i qutim-qbs-1.1.patch
```

## <a href="#build" class="anchor" id="build"><span class="octicon octicon-link"></span></a>Building

```bash
cd qutim
HOME=$(pwd) qbs -j $(nproc) -d ../build release profile:qutim
```

I want to create a universal recipe for the building, thus we must set `$HOME`
directory. Flag `-j` means number of jobs, `-d` means build directory, `release`
means building type (debug, release), `profile` is used profile, which is
described in the configuration file.

## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Installation

```bash
HOME=$(pwd) sudo qbs install -d ../build --install-root "/usr" profile:qutim
```

We must set root directory (`--install-root`), because without this option the
package will be installed into `/` (`/bin` and `/lib`).

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

[PKGBUILD](/resources/docs/qutim-qt5-git/PKGBUILD "File")

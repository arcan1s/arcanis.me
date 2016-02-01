---
category: en
type: paper
layout: paper
hastr: true
tags: archlinux, configuration, linux
title: Creating own repository
short: creating-custom-repo
---
It is a short paper devoted to creation own ArchLinux repository.

<!--more-->

## <a href="#prepare" class="anchor" id="prepare"><span class="octicon octicon-link"></span></a>Prepare

First find a server and desire to have sex with it. It is recommended to use
Archlinux on it, but it is not necessarily - because you may create special root
for Archlinux. Also you need two packages, `devtools` and `pacman`:

```bash
pacman -Sy devtools
```

[devtools](//www.archlinux.org/packages/devtools/ "Archlinux package") is script
set for building automation in the clean chroot. I think most of Arch
maintainers use it.

Let's create working directories and set colors:

```bash
# colors
if [ ${USECOLOR} == "yes" ]; then
  bblue='\e[1;34m'
  bwhite='\e[1;37m'
  cclose='\e[0m'
fi
export USECOLOR
# directories
if [ ! -d "${PREPAREDIR}" ]; then
  [ -e "${PREPAREDIR}" ] && error_mes "file" "${PREPAREDIR}"
  echo -e "${bwhite}[II] ${bblue}Creating directory ${bwhite}'${PREPAREDIR}'${cclose}"
  mkdir -p "${PREPAREDIR}" || error_mes "unknown"
fi
if [ ! -d "${REPODIR}" ]; then
  [ -e "${REPODIR}" ] && error_mes "file" "${REPODIR}"
  echo -e "${bwhite}[II] ${bblue}Creating directory ${bwhite}'${REPODIR}'${cclose}"
  mkdir -p "${REPODIR}/"{i686,x86_64} || error_mes "unknown"
fi
if [ ! -d "${REPODIR}/i686" ]; then
  [ -e "${REPODIR}/i686" ] && error_mes "file" "${REPODIR}/i686"
  echo -e "${bwhite}[II] ${bblue}Creating directory ${bwhite}'${REPODIR}/i686'${cclose}"
  mkdir -p "${REPODIR}/i686" || error_mes "unknown"
fi
if [ ! -d "${REPODIR}/x86_64" ]; then
  [ -e "${REPODIR}/x86_64" ] && error_mes "file" "${REPODIR}/x86_64"
  echo -e "${bwhite}[II] ${bblue}Creating directory ${bwhite}'${REPODIR}/x86_64'${cclose}"
  mkdir -p "${REPODIR}/x86_64" || error_mes "unknown"
fi
if [ ! -d "${STAGINGDIR}" ]; then
  [ -e "${STAGINGDIR}" ] && error_mes "file" "${STAGINGDIR}"
  echo -e "${bwhite}[II] ${bblue}Creating directory ${bwhite}'${STAGINGDIR}'${cclose}"
  mkdir -p "${STAGINGDIR}" || error_mes "unknown"
fi
```

`${REPODIR}/{i686,x86_64}` are directories for repository, `${PREPAREDIR}` is
directory where compiled packages will be stored, `${STAGINGDIR}` is one where
packages will be built.

## <a href="#theory" class="anchor" id="theory"><span class="octicon octicon-link"></span></a>A bit of theory

Create directory, share it (using [ftp](/en/2014/03/06/site-changes/ "Site
changes paper"), for example). It has two subdirectories - `i686` and `x86_64` -
for each architecture respectively. And fill them with a set of packages.

Updating repository may be split into the following steps:

1. Creating PKGBUILDs (or updating them from AUR).
2. Packages building for each architecture in clean chroot.
3. Packages signing.
4. Creating the list of packages.
5. Repository update:
    1. Removal old packages from repository.
    2. Copying new packages
    3. Repository update.
6. Cleaning.

### <a href="#pkgbuild" class="anchor" id="pkgbuild"><span class="octicon octicon-link"></span></a>Creating PKGBUILDs

Download source tarballs from AUR:

```bash
cd "${STAGINGDIR}"
yaourt -G package-name
```

### <a href="#building" class="anchor" id="building"><span class="octicon octicon-link"></span></a>Packages building

Build each package automatically:

```bash
func_build() {
  if [ ${USECOLOR} == "yes" ]; then
    _bblue='\e[1;34m'
    _bwhite='\e[1;37m'
    _cclose='\e[0m'
  fi
  _PREPAREDIR="$1"
  _ROOTDIR="$2"
  eval $(/usr/bin/grep 'arch=' PKGBUILD)
  eval $(/usr/bin/grep 'pkgname=' PKGBUILD)
  echo -e "${_bwhite}[II] ${_bblue}=>${_cclose} Building ${_bwhite}${pkgname}${_cclose}"
  if echo ${arch} | /usr/bin/grep 'any' -q; then
    LC_MESSAGES=C /usr/bin/sudo /usr/bin/staging-i686-build -r "${_ROOTDIR}" -c
  else
    eval $(/usr/bin/grep 'pkgname=' PKGBUILD)
    if echo ${pkgname} | /usr/bin/grep lib32 -q; then
      LC_MESSAGES=C /usr/bin/sudo /usr/bin/multilib-staging-build -r "${_ROOTDIR}" -c
    else
      if /usr/bin/grep 'lib32' PKGBUILD -q; then
        LC_MESSAGES=C /usr/bin/sudo /usr/bin/staging-i686-build -r "${_ROOTDIR}" -c
        LC_MESSAGES=C /usr/bin/sudo /usr/bin/multilib-staging-build -r "${_ROOTDIR}" -c
      else
        LC_MESSAGES=C /usr/bin/sudo /usr/bin/staging-i686-build -r "${_ROOTDIR}" -c
        LC_MESSAGES=C /usr/bin/sudo /usr/bin/staging-x86_64-build -r "${_ROOTDIR}" -c
      fi
    fi
  fi
  /usr/bin/cp *.pkg.tar.xz "${_PREPAREDIR}"
}
export -f func_build

# building
echo -e "${bwhite}[II]${cclose} Building packages"
cd "${STAGINGDIR}"
/usr/bin/find -name 'PKGBUILD' -type f -execdir /usr/bin/bash -c "func_build "${PREPAREDIR}" "${ROOTDIR}"" \;
```

It is recommended to add the following lines to `/etc/sudoers`:

```bash
username ALL=NOPASSWD: /usr/bin/staging-i686-build
username ALL=NOPASSWD: /usr/bin/staging-x86_64-build
username ALL=NOPASSWD: /usr/bin/multilib-staging-build
```

### <a href="#signing" class="anchor" id="signing"><span class="octicon octicon-link"></span></a>Packages signing

```bash
# signing
if [ ${USEGPG} == "yes" ]; then
  echo -e "${bwhite}[II]${cclose} Signing"
  cd "${PREPAREDIR}"
  for PACKAGE in $(/usr/bin/find . -name '*.pkg.tar.xz'); do
    /usr/bin/gpg -b ${PACKAGE}
  done
fi
```

It is recommended to configure
[gpg-agent](//wiki.archlinux.org/index.php/GPG#gpg-agent "ArchWiki").

### <a href="#list" class="anchor" id="list"><span class="octicon octicon-link"></span></a>Creating the list of packages

```bash
# creating packages list
cd "${PREPAREDIR}"
i686_PACKAGES=$(/usr/bin/find * -name '*-i686.pkg.tar.xz' -o -name
'*-any.pkg.tar.xz')
x86_64_PACKAGES=$(/usr/bin/find * -name '*-x86_64.pkg.tar.xz' -o -name
'*-any.pkg.tar.xz')
echo -e "${bwhite}[II] ${bblue}=>${cclose} i686 packages:
\n${bwhite}${i686_PACKAGES}${cclose}"
echo -e "${bwhite}[II] ${bblue}=>${cclose} x86_64 packages:
\n${bwhite}${x86_64_PACKAGES}${cclose}"
```

### <a href="#updating" class="anchor" id="updating"><span class="octicon octicon-link"></span></a>Repository update

Here is a function for removal packages from database and repository:

```bash
func_remove() {
  _PACKAGE="$1"
  /usr/bin/rm -f "${_PACKAGE}"{,.sig}
}
```

`i686` repository update:

```bash
# updating i686 repo
echo -e "${bwhite}[II]${cclose} Updating ${bwhite}i686${cclose} repo"
cd "${REPODIR}/i686"
for PACKAGE in ${i686_PACKAGES}; do
  PKGNAME=$(/usr/bin/package-query -p -f %n "${PREPAREDIR}/${PACKAGE}")
  for PKG in $(/usr/bin/find "${REPODIR}/i686" -name "${PKGNAME}"'*.pkg.tar.xz'); do
    _PKGNAME=$(/usr/bin/package-query -p -f %n "${PKG}")
    [ "${PKGNAME}" == "${_PKGNAME}" ] && func_remove "${PKG}"
  done
  /usr/bin/cp "${PREPAREDIR}/${PACKAGE}" .
  [ ${USEGPG} == "yes" ] && /usr/bin/cp "${PREPAREDIR}/${PACKAGE}.sig" .
  /usr/bin/repo-add ${DBNAME}.db.tar.gz "${PACKAGE}"
  /usr/bin/repo-add --files ${DBNAME}.files.tar.gz "${PACKAGE}"
done
```

`x86_64` repository update:

```bash
# updating x86_64 repo
echo -e "${bwhite}[II]${cclose} Updating ${bwhite}x86_64${cclose} repo"
cd "${REPODIR}/x86_64"
for PACKAGE in ${x86_64_PACKAGES}; do
  PKGNAME=$(/usr/bin/package-query -p -f %n "${PREPAREDIR}/${PACKAGE}")
  for PKG in $(/usr/bin/find "${REPODIR}/x86_64" -name "${PKGNAME}"'*.pkg.tar.xz'); do
    _PKGNAME=$(/usr/bin/package-query -p -f %n "${PKG}")
    [ "${PKGNAME}" == "${_PKGNAME}" ] && func_remove "${PKG}"
  done
  /usr/bin/cp "${PREPAREDIR}/${PACKAGE}" .
  [ ${USEGPG} == "yes" ] && /usr/bin/cp "${PREPAREDIR}/${PACKAGE}.sig" .
  /usr/bin/repo-add ${DBNAME}.db.tar.gz "${PACKAGE}"
  /usr/bin/repo-add --files ${DBNAME}.files.tar.gz "${PACKAGE}"
done
```

### <a href="#clear" class="anchor" id="clear"><span class="octicon octicon-link"></span></a>Cleaning

```bash
# clear
cd "${PREPAREDIR}"
/usr/bin/rm -rf *
cd "${STAGINGDIR}"
/usr/bin/rm -rf *
```

### <a href="#symlinks" class="anchor" id="symlinks"><span class="octicon octicon-link"></span></a>Creating symlinks

You may want to create a directory, which will contain symlinks on actual
packages with names, which does not contain version:

```bash
# creating symlinks
if [ ${SYMLINK} == "yes" ]; then
  echo -e "${bwhite}[II]${cclose} Creating symlinks"
  if [ ! -d "${REPODIR}/non-versioned" ]; then
    [ -e "${REPODIR}/non-versioned" ] && error_mes "file" "${REPODIR}/non-versioned"
    echo -e "${bwhite}[II] ${bblue}Creating directory ${bwhite}'${REPODIR}/non-versioned'${cclose}"
    mkdir -p "${REPODIR}/non-versioned" || error_mes "unknown"
  fi
  cd "${REPODIR}/non-versioned"
  for PACKAGE in ${i686_PACKAGES}; do
    PKGNAME=$(/usr/bin/package-query -p -f %n "${REPODIR}/i686/${PACKAGE}")
    /usr/bin/ln -sf "../i686/${PACKAGE}" "${PKGNAME}-i686.pkg.tar.xz"
  done
  for PACKAGE in ${x86_64_PACKAGES}; do
    PKGNAME=$(/usr/bin/package-query -p -f %n "${REPODIR}/x86_64/${PACKAGE}")
    /usr/bin/ln -sf "../x86_64/${PACKAGE}" "${PKGNAME}-x86_64.pkg.tar.xz"
  done
fi
```

### <a href="#file" class="anchor" id="file"><span class="octicon octicon-link"></span></a>File

Here is [the scripts](//github.com/arcan1s/repo-scripts "GitHub"). Download
source tarballs and run script (editing variables if it is necessary).

## <a href="#using" class="anchor" id="using"><span class="octicon octicon-link"></span></a>Repository usage

Just add following lines to `/etc/pacman.conf`:

```bash
[$REPONAME]
Server = ftp://$REPOADDRESS/repo/$arch
```

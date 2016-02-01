---
category: ru
type: paper
hastr: true
layout: paper
tags: archlinux, настройка, linux
title: Создание собственного репозитория
short: creating-custom-repo
---
Небольшая статья, посвященная созданию собственного репозитория для Archlinux.

<!--more-->

## <a href="#prepare" class="anchor" id="prepare"><span class="octicon octicon-link"></span></a>Подготовка

Для начала находим сервер и желание с ним заниматься сексом. Для простоты, лучше,
чтобы там стоял Archlinux, хотя, это и не совсем обязательно (можно создать
отдельный корень под Arch). Из пакетов, пожалуй, нам понадобится только два,
`devtools` и сам `pacman`:

```bash
pacman -Sy devtools
```

[devtools](//www.archlinux.org/packages/devtools/ "Пакет Archlinux") - набор
скриптов, предназначенный для автоматизации сборки пакетов в чистом чруте. Думаю,
большинство мейнтейнеров Arch'а пользуются им.

Создадим рабочие директории и установим цвета:

```bash
# цвета
if [ ${USECOLOR} == "yes" ]; then
  bblue='\e[1;34m'
  bwhite='\e[1;37m'
  cclose='\e[0m'
fi
export USECOLOR
# директории
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

Директории `${REPODIR}/{i686,x86_64}` для самого репозитория, `${PREPAREDIR}` -
директория, где будут лежать собранные пакеты, `${STAGINGDIR}` - директория,
откуда будут собираться пакеты.

## <a href="#theory" class="anchor" id="theory"><span class="octicon octicon-link"></span></a>Немного теории

Создаем директорию, расшариваем ее (например, по [ftp](/ru/2014/03/06/site-changes/
"Статья про изменения сайта")). В ней две субдиректории - `i686` и `x86_64`, для
каждого типа архитектур соответственно. И наполняем их набором пакетов по Вашему
усмотрению.

Процесс обновления репозитория можно разбить на следующие части:

1. Создание PKGBUILD'ов (обновление их из AUR'а).
2. Сборка пакетов для различных архитектур в чистом чруте.
3. Подписывание пакетов.
4. Создание списка пакетов.
5. Обновление репозиториев:
    1. Удаление старых пакетов из репозитория.
    2. Копирование новых пакетов.
    3. Обновление базы.
6. Очистка.

Теперь по шагам.

### <a href="#pkgbuild" class="anchor" id="pkgbuild"><span class="octicon octicon-link"></span></a>Создание PKGBUILD'ов

Скачаем исходники для всех нужных пакетов из AUR'а:

```bash
cd "${STAGINGDIR}"
yaourt -G package-name
```

### <a href="#building" class="anchor" id="building"><span class="octicon octicon-link"></span></a>Сборка пакетов

Автоматически соберем каждый пакет:

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

# сборка
echo -e "${bwhite}[II]${cclose} Building packages"
cd "${STAGINGDIR}"
/usr/bin/find -name 'PKGBUILD' -type f -execdir /usr/bin/bash -c "func_build "${PREPAREDIR}" "${ROOTDIR}"" \;
```

Для удобства рекомендую добавить в файл `/etc/sudoers` следующие строки:

```bash
username ALL=NOPASSWD: /usr/bin/staging-i686-build
username ALL=NOPASSWD: /usr/bin/staging-x86_64-build
username ALL=NOPASSWD: /usr/bin/multilib-staging-build
```

### <a href="#signing" class="anchor" id="signing"><span class="octicon octicon-link"></span></a>Подпись пакетов

```bash
# подпись
if [ ${USEGPG} == "yes" ]; then
  echo -e "${bwhite}[II]${cclose} Signing"
  cd "${PREPAREDIR}"
  for PACKAGE in $(/usr/bin/find . -name '*.pkg.tar.xz'); do
    /usr/bin/gpg -b ${PACKAGE}
  done
fi
```

Для удобства рекомендую настроить [gpg-agent]
(//wiki.archlinux.org/index.php/GPG#gpg-agent "ArchWiki").

### <a href="#list" class="anchor" id="list"><span class="octicon octicon-link"></span></a>Создание списка пакетов

```bash
# создание списка пакетов
cd "${PREPAREDIR}"
i686_PACKAGES=$(/usr/bin/find * -name '*-i686.pkg.tar.xz' -o -name '*-any.pkg.tar.xz')
x86_64_PACKAGES=$(/usr/bin/find * -name '*-x86_64.pkg.tar.xz' -o -name '*-any.pkg.tar.xz')
echo -e "${bwhite}[II] ${bblue}=>${cclose} i686 packages: \n${bwhite}${i686_PACKAGES}${cclose}"
echo -e "${bwhite}[II] ${bblue}=>${cclose} x86_64 packages: \n${bwhite}${x86_64_PACKAGES}${cclose}"
```

### <a href="#updating" class="anchor" id="updating"><span class="octicon octicon-link"></span></a>Обновление репозиториев

Функция для удаления пакетов из базы данных и из репозитория:

```bash
func_remove() {
  _PACKAGE="$1"
  /usr/bin/rm -f "${_PACKAGE}"{,.sig}
}
```

Обновление репозитория `i686`:

```bash
# обновление репозитория i686
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

Обновление репозитория `x86_64`:

```bash
# обновление репозитория x86_64
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

### <a href="#clear" class="anchor" id="clear"><span class="octicon octicon-link"></span></a>Очистка

```bash
# очистка
cd "${PREPAREDIR}"
/usr/bin/rm -rf *
cd "${STAGINGDIR}"
/usr/bin/rm -rf *
```

### <a href="#symlinks" class="anchor" id="symlinks"><span class="octicon octicon-link"></span></a>Создание симлинков

Вы можете захотеть создать директорию, которая будет содержать симлинки на
актуальные версии пакетов с именами, не содержащими версии:

```bash
# создание симлинков
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

### <a href="#file" class="anchor" id="file"><span class="octicon octicon-link"></span></a>Файл

[Скрипты](//github.com/arcan1s/repo-scripts "GitHub") целиком. Скачиваем и
сходники для пакетов, запускаем скрипт (при необходимости, редактируем переменные)
и радуемся жизни.

## <a href="#using" class="anchor" id="using"><span class="octicon octicon-link"></span></a>Использование репозитория

Просто добавляем в файл `/etc/pacman.conf` следующие строки:

```bash
[$REPONAME]
Server = ftp://$REPOADDRESS/repo/$arch
```

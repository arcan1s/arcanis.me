---
category: ru
type: paper
hastr: true
layout: paper
tags: zshrc, настройка, linux
title: О zshrc
short: about-zshrc
---
Это моя первая статья в блоге (я думаю, мне нужно что-нибудь для тестов =)).
Существует множество похожих статей и, я думаю, не буду отличаться от большинства.
Я просто хочу показать мой `.zshrc` и объяснить, что в нем есть и зачем оно нужно.
Также, любые комментарии или дополнения приветствуются. [Оригинал]
(//archlinux.org.ru/forum/topic/12752/ "Тема на форуме") статьи.

<!--more-->

## <a href="#prepare" class="anchor" id="prepare"><span class="octicon octicon-link"></span></a>Подготовка

Сначала установите необходимый минимум:

```bash
pacman -Sy pkgfile zsh zsh-completions zsh-syntax-highlighting
```

[pkgfile](//www.archlinux.org/packages/pkgfile/ "Пакет Archlinux") очень полезная
утилита. Данная команда также установит шелл, дополнения к нему и подсветку
синтаксиса.

## <a href="#configuration" class="anchor" id="configuration"><span class="octicon octicon-link"></span></a>Настройка шелла

Все доступные опции приведены [здесь](//zsh.sourceforge.net/Doc/Release/Options.html
"Документация zsh").

Указываем файл с историей, число команд хранящихся в кэше текущего сеанса и
число команд, хранящихся в файле:

```bash
# history
HISTFILE=~/.zsh_history
HISTSIZE=500000
SAVEHIST=500000
```

Я не могу запомнить все комбинации `Ctrl+`, поэтому я назначаю клавиши на их
стандартное использование:

```bash
# bindkeys
bindkey '^[[A'  up-line-or-search        # up arrow for back-history-search
bindkey '^[[B'  down-line-or-search      # down arrow for fwd-history-search
bindkey '\e[1~' beginning-of-line        # home
bindkey '\e[2~' overwrite-mode           # insert
bindkey '\e[3~' delete-char              # del
bindkey '\e[4~' end-of-line              # end
bindkey '\e[5~' up-line-or-history       # page-up
bindkey '\e[6~' down-line-or-history     # page-down
```

Но здесь важно, что стрелки `вверх`/`вниз` служат для навигации по истории с
учетом **уже введенной части** команды. А `PgUp`/`PgDown` **проигнорируют** уже
введенную часть команды.

Автодополнение команд:

```bash
# autocomplete
autoload -U compinit
compinit
zstyle ':completion:*' insert-tab false
zstyle ':completion:*' max-errors 2
```

Подключается полное автодополнение команд. `insert-tab false` включит
автодополнение для **невведенной** команды (не знаю, зачем). `max-errors`
устанавливает максимальное число опечаток, которые могут быть исправлены.

Приглашение:

```bash
# promptinit
autoload -U promptinit
promptinit
```

Включим цвета:

```bash
# colors
autoload -U colors
colors
```

Различные опции.
Смена директории без ввода `cd`:

```bash
# autocd
setopt autocd
```

Корректировка опечаток (и шаблон вопроса):

```bash
# correct
setopt CORRECT_ALL
SPROMPT="Correct '%R' to '%r' ? ([Y]es/[N]o/[E]dit/[A]bort) "
```

Отключаем е#$%ую пищалку:

```bash
# disable beeps
unsetopt beep
```

Включаем калькулятор:

```bash
# calc
autoload zcalc
```

Дополнение истории (**а не перезапись** файла):

```bash
# append history
setopt APPEND_HISTORY
```

Не сохранять дубликаты в историю:

```bash
# ignore dups in history
setopt HIST_IGNORE_ALL_DUPS
```

...и дополнительные пробелы:

```bash
# ignore spaces in history
setopt HIST_IGNORE_SPACE
```

...и пустые линии тоже:

```bash
# reduce blanks in history
setopt HIST_REDUCE_BLANKS
```

Включаем `pkgfile`:

```bash
# pkgfile
source /usr/share/doc/pkgfile/command-not-found.zsh
```

## <a href="#highlighting" class="anchor" id="highlighting"><span class="octicon octicon-link"></span></a>Подсветка синтаксиса

```bash
# highlighting
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
ZSH_HIGHLIGHT_HIGHLIGHTERS=(main brackets pattern)
# brackets
ZSH_HIGHLIGHT_STYLES[bracket-level-1]='fg=blue,bold'
ZSH_HIGHLIGHT_STYLES[bracket-level-2]='fg=red,bold'
ZSH_HIGHLIGHT_STYLES[bracket-level-3]='fg=yellow,bold'
ZSH_HIGHLIGHT_STYLES[bracket-level-4]='fg=magenta,bold'
# cursor
#ZSH_HIGHLIGHT_STYLES[cursor]='bg=blue'
# main
# default
ZSH_HIGHLIGHT_STYLES[default]='none'                                 # стандартный цвет
# unknown
ZSH_HIGHLIGHT_STYLES[unknown-token]='fg=red'                         # неизвестная команда
# command
ZSH_HIGHLIGHT_STYLES[reserved-word]='fg=magenta,bold'                # зарезервированное слово
ZSH_HIGHLIGHT_STYLES[alias]='fg=yellow,bold'                         # алиас
ZSH_HIGHLIGHT_STYLES[builtin]='fg=green,bold'                        # built-in функция (например, echo)
ZSH_HIGHLIGHT_STYLES[function]='fg=green,bold'                       # функция, определенная в шелле
ZSH_HIGHLIGHT_STYLES[command]='fg=green'                             # обычная команда
ZSH_HIGHLIGHT_STYLES[precommand]='fg=blue,bold'                      # пре-команда (например, sudo в sudo cp ...)
ZSH_HIGHLIGHT_STYLES[commandseparator]='fg=yellow'                   # разделитель команд, && || ;
ZSH_HIGHLIGHT_STYLES[hashed-command]='fg=green'                      # команда, найденная в путях (hashed)
ZSH_HIGHLIGHT_STYLES[single-hyphen-option]='fg=blue,bold'            # флаги типа -*
ZSH_HIGHLIGHT_STYLES[double-hyphen-option]='fg=blue,bold'            # флаги типа --*
# path
ZSH_HIGHLIGHT_STYLES[path]='fg=cyan,bold'                            # станлартный путь
ZSH_HIGHLIGHT_STYLES[path_prefix]='fg=cyan'                          # префикс пути
ZSH_HIGHLIGHT_STYLES[path_approx]='fg=cyan'                          # примерный путь
# shell
ZSH_HIGHLIGHT_STYLES[globbing]='fg=cyan'                             # шаблон (например, /dev/sda*)
ZSH_HIGHLIGHT_STYLES[history-expansion]='fg=blue'                    # подстановка из истории (команда, начинающаяся с !)
ZSH_HIGHLIGHT_STYLES[assign]='fg=magenta'                            # присвоение
ZSH_HIGHLIGHT_STYLES[dollar-double-quoted-argument]='fg=cyan'        # конструкции типа "$VARIABLE"
ZSH_HIGHLIGHT_STYLES[back-double-quoted-argument]='fg=cyan'          # конструкции типа \"
ZSH_HIGHLIGHT_STYLES[back-quoted-argument]='fg=blue'                 # конструкции типа `command`
# quotes
ZSH_HIGHLIGHT_STYLES[single-quoted-argument]='fg=yellow,underline'   # конструкции типа 'text'
ZSH_HIGHLIGHT_STYLES[double-quoted-argument]='fg=yellow'             # конструкции типа "text"
# pattern
#ZSH_HIGHLIGHT_PATTERNS+=('rm -rf *' 'fg=white,bold,bg=red')
# root
#ZSH_HIGHLIGHT_STYLES[root]='bg=red'
```

В первой строке включаем подсветку. Затем включаем основную подсветку, а также
подсветку скобок и шаблонов. Шаблоны указываются ниже (`rm -rf *` в примере).
Также может быть включена подсветка команд от `root` и курсора `cursor`.
Синтаксис настроек понятен, `fg` цвет шрифта, `bg` цвет фона.

## <a href="#prompt" class="anchor" id="prompt"><span class="octicon octicon-link"></span></a>$PROMPT и $RPROMPT

Я хочу использовать один файл `.zshrc` для рута и обычного пользователя:

```bash
# PROMPT && RPROMPT
if [[ $EUID == 0 ]]; then
# [root@host dir]#
  PROMPT="%{$fg_bold[white]%}[%{$reset_color%}\
%{$fg_bold[red]%}%n%{$reset_color%}\
%{$fg_bold[white]%}@%{$reset_color%}\
%{$fg_no_bold[red]%}%m %{$reset_color%}\
%{$fg_bold[yellow]%}%1/%{$reset_color%}\
%{$fg_bold[white]%}]# %{$reset_color%}"
else
# [user@host dir]$
  PROMPT="%{$fg_bold[white]%}[%{$reset_color%}\
%{$fg_bold[green]%}%n%{$reset_color%}\
%{$fg_bold[white]%}@%{$reset_color%}\
%{$fg_no_bold[green]%}%m %{$reset_color%}\
%{$fg_bold[yellow]%}%1/%{$reset_color%}\
%{$fg_bold[white]%}]$ %{$reset_color%}"
fi
```

`fg` цвет шрифта, `bg` цвет фона. `_bold` и `_no_bold` регулируют оттенок.
Команды должны быть обрамлены в `%{ ... %}`, чтобы не показывались. Доступные
цвета:

```bash
black
red
green
yellow
blue
magenta
cyan
white
```

Доступные переменные:

```bash
%n  - имя пользователя
%m  - хостнейм (выставляется только в начале сессии)
%M  - хостнейм
%l  - текущая tty
%?  - код возврата предыдущего приложения
%#  - # для рута и % для обычных пользователей
%T  - время (HH:MM)
%*  - время (HH:MM:SS)
%D  - дата (YY-MM-DD)
%d  - текущая директория
%~  - то же, домашняя директория будет заменена на ~
%1/ - то же, но только последняя директория
```

RPROMPT (необходим пакет `acpi`):

```bash
precmd () {
  # battery charge
  function batcharge {
    bat_perc=`acpi | awk {'print $4;'} | sed -e "s/\s//" -e "s/%.*//"`
    if [[ $bat_perc < 15 ]]; then
      col="%{$fg_bold[red]%}"
    elif [[ $bat_perc < 50 ]]; then
      col="%{$fg_bold[yellow]%}"
    else
      col="%{$fg_bold[green]%}"
    fi
    echo "%{$fg_bold[white]%}["$col$bat_perc"%{$fg_bold[white]%}%%]%{$reset_color%}"
  }
  # last command
  returncode="%(?.%{$fg[green]%}.%{$fg[red]%})%?%{$resetcolor%}"
  RPROMPT="%{$fg_bold[white]%}[%{$reset_color%}\
%{$fg_bold[cyan]%}%T%{$reset_color%}\
%{$fg_bold[white]%}] %{$reset_color%}"\
$(batcharge)\
"%{$fg_bold[white]%}[%{$reset_color%}"\
$returncode\
"%{$fg_bold[white]%}]%{$reset_color%}"
```

Мой RPROMPT показывает текущее время, заряд батареи и код возврата последнего
приложения. `precmd()` необходимо для автоматического обновления. Конструкция
`$(if.true.false)` является условным оператором в `zsh`.

## <a href="#aliases" class="anchor" id="aliases"><span class="octicon octicon-link"></span></a>Аллиасы

**Копируйте только те аллиасы, которые Вам необходимы.** Если какой-либо аллиас
использует приложение, которое не установлено, это приведет к сбою загрузки
конфигурационного файла.

Полезная (или не очень) функция:

```bash
show_which() {
  OUTPUT=$(which $1 | cut -d " " -f7-)
  echo "Running '$OUTPUT'" 1>&2
}
```

Первая группа аллиасов:

```bash
## alias
# цветной grep
alias grep='grep --colour=auto'
# замена top на htop
alias top='show_which top && htop'
# chromium с различными прокси серверами (i2p и tor в наличии)
alias chrommsu='show_which chrommsu && chromium --proxy-server=cache.msu:3128'
alias chromtor='show_which chromtor && chromium --proxy-server="socks://localhost:9050" --incognito'
alias chromi2p='show_which chromi2p && chromium --proxy-server="http=127.0.0.1:4444;https=127.0.0.1:4445" --incognito'
# человеческие df и du
alias df='show_which df && df -k --print-type --human-readable'
alias du='show_which du && du -k --total --human-readable'
# замена less и zless на vimpager
alias less='vimpager'
alias zless='vimpager'
```

ls аллиасы (смотри [man ls](//man7.org/linux/man-pages/man1/ls.1.html "Мануал")):

```bash
alias ls='show_which ls && ls --color=auto --group-directories-first'
alias ll='show_which ll && ls -l --human-readable'
alias lr='show_which lr && ls --recursive'
alias la='show_which la && ll --almost-all'
alias lx='show_which lx && ll -X --ignore-backups'
alias lz='show_which lz && ll -S --reverse'
alias lt='show_which lt && ll -t --reverse'
alias lm='show_which lm && la | more'
```

Аллиасы для быстрого просмотра файлов из консоли (просто набери имя файла!):

```bash
# alias -s
alias -s {avi,mpeg,mpg,mov,m2v,mkv}=mpv
alias -s {mp3,flac}=qmmp
alias -s {odt,doc,xls,ppt,docx,xlsx,pptx,csv}=libreoffice
alias -s {pdf}=okular
autoload -U pick-web-browser
alias -s {html,htm}=opera
```

"sudo" аллиасы:

```bash
# sudo alias
if [[ $EUID == 0 ]]; then
  alias fat32mnt='show_which fat32mnt && mount -t vfat -o codepage=866,iocharset=utf8,umask=000'
  alias synctime='show_which synctime && { ntpd -qg; hwclock -w; date; }'
else
  alias fat32mnt='show_which fat32mnt && sudo mount -t vfat -o codepage=866,iocharset=utf8,umask=000'
  alias umount='show_which umount && sudo umount'
  alias mount='show_which mount && sudo mount'
  alias netctl='show_which netctl && sudo netctl'
  alias synctime='show_which synctime && { sudo ntpd -qg; sudo hwclock -w; date; }'
  alias wifi-menu='show_which wifi-menu && sudo wifi-menu'
  alias dhcpcd='show_which dhcpcd && sudo dhcpcd'
  alias journalctl='show_which journalctl && sudo journalctl'
  alias systemctl='show_which systemctl && sudo systemctl'
  alias modprobe='show_which modprobe && sudo modprobe'
  alias rmmod='show_which rmmod && sudo rmmod'
  alias staging-i686-build='show_which staging-i686-build && sudo staging-i686-build'
  alias staging-x86_64-build='show_which staging-x86_64-build && sudo staging-x86_64-build'
fi
```

Некоторые глобальные аллиасы. Если они включены, команда `cat foo g bar` будет
эквивалентна `cat foo | grep bar`:

```bash
# global alias
alias -g g="| grep"
alias -g l="| less"
alias -g t="| tail"
alias -g h="| head"
alias -g dn="&> /dev/null &"
```

## <a href="#functions" class="anchor" id="functions"><span class="octicon octicon-link"></span></a>Функции

Специальная функция для `xrandr`:

```bash
# function to contorl xrandr
# EXAMPLE: projctl 1024x768
projctl () {
  if [ $1 ] ; then
    if [ $1 = "-h" ]; then
      echo "Usage:   projctl [ off/resolution ]"
      return
    fi
    if [ $1 = "off" ]; then
      echo "Disable VGA1"
      xrandr --output VGA1 --off --output LVDS1 --mode 1366x768
    else
      echo "Using resolution: $1"
      xrandr --output VGA1 --mode $1 --output LVDS1 --mode $1
    fi
  else
    echo "Using default resolution"
    xrandr --output VGA1 --mode 1366x768 --output LVDS1 --mode 1366x768
  fi
}
```

К сожалению, я не могу запомнить флаги `tar`, поэтому я использую специальные
функции:

```bash
# function to extract archives
# EXAMPLE: unpack file
unpack () {
  if [[ -f $1 ]]; then
    case $1 in
      *.tar.bz2)   tar xjfv $1                             ;;
      *.tar.gz)    tar xzfv $1                             ;;
      *.tar.xz)    tar xvJf $1                             ;;
      *.bz2)       bunzip2 $1                              ;;
      *.gz)        gunzip $1                               ;;
      *.rar)       unrar x $1                              ;;
      *.tar)       tar xf $1                               ;;
      *.tbz)       tar xjvf $1                             ;;
      *.tbz2)      tar xjf $1                              ;;
      *.tgz)       tar xzf $1                              ;;
      *.zip)       unzip $1                                ;;
      *.Z)         uncompress $1                           ;;
      *.7z)        7z x $1                                 ;;
      *)           echo "I don't know how to extract '$1'" ;;
    esac
  else
    case $1 in
      *help)       echo "Usage: unpack ARCHIVE_NAME"       ;;
      *)           echo "'$1' is not a valid file"         ;;
    esac
  fi
}
# function to create archives
# EXAMPLE: pack tar file
pack () {
  if [ $1 ]; then
    case $1 in
      tar.bz2)     tar -cjvf $2.tar.bz2 $2                 ;;
      tar.gz)      tar -czvf $2.tar.bz2 $2                 ;;
      tar.xz)      tar -cf - $2 | xz -9 -c - > $2.tar.xz   ;;
      bz2)         bzip $2                                 ;;
      gz)          gzip -c -9 -n $2 > $2.gz                ;;
      tar)         tar cpvf $2.tar  $2                     ;;
      tbz)         tar cjvf $2.tar.bz2 $2                  ;;
      tgz)         tar czvf $2.tar.gz  $2                  ;;
      zip)         zip -r $2.zip $2                        ;;
      7z)          7z a $2.7z $2                           ;;
      *help)       echo "Usage: pack TYPE FILES"           ;;
      *)           echo "'$1' cannot be packed via pack()" ;;
    esac
  else
    echo "'$1' is not a valid file"
  fi
}
```

Специальная функция для `su`:

```bash
su () {
  CHECKSU=0
  for FLAG in $*; do
    [[ $FLAG == "-" ]] && CHECKSU=1
    [[ $FLAG == "-l" ]] && CHECKSU=1
    [[ $FLAG == "--login" ]] && CHECKSU=1
  done
  if [[ $CHECKSU == 0 ]]; then
    echo "Use 'su -', Luke"
    /usr/bin/su - $*
  else
    /usr/bin/su $*
  fi
}
```

Функция, которая заменяет оригиналькую команду `rm`. Если Вы наберете `rm`, это
будет эквивалентно перемещению в корзину, также, Вы можете легко восстановить
удаленный файл:

```bash
rm () {
  # error check
  [ $# -eq 0 ] && { echo "Files are not set!"; return 1 }
  echo "$@" | grep -qe '-h\|--help' && { echo "Usage: rm FILE..."; return 0 }
  echo "$@" | grep -q "-" && echo "Warning: this function doesn't support any flags"
  # set trash path
  TRASHDIR="$HOME/.local/share/Trash"
  TRASHFILE="${TRASHDIR}/files"
  TRASHINFO="${TRASHDIR}/info"
  for DIRECTORY in "${TRASHDIR}" "${TRASHFILE}" "${TRASHINFO}"; do
    if [ -e "${DIRECTORY}" ]; then
      [ -d "${DIRECTORY}" ] || { echo "'${DIRECTORY}' is a file"; return 1 }
    else
      mkdir -p -m755 "${DIRECTORY}"
    fi
  done
  # confirm
  CONFIRM=""
  echo -n "You realy want to remove '$@'? [y/n] "; read -k1 CONFIRM; echo
  [[ ! $CONFIRM =~ [yY] ]] && return 1
  # move
  for FILE in "$@"; do
    DESTFILE="$(basename -- "${FILE}")"
    SUFFIX='';
    ITER=0;
    while [ -e "${TRASHFILE}/${DESTFILE}${SUFFIX}" ]; do
      SUFFIX="_${ITER}";
      ITER=$(expr ${ITER} + 1)
    done
    echo "Remove '${FILE}'"
    if [ "$(dirname -- "$(realpath -- "${FILE}")")" == "${TRASHFILE}" ]; then
      /usr/bin/rm -rf -- "${FILE}"
      /usr/bin/rm -rf -- "${TRASHINFO}/${DESTFILE}.trashinfo"
    else
      mv -- "${FILE}" "${TRASHFILE}/${DESTFILE}${SUFFIX}" || return 1
      echo "[Trash Info]\nPath=$(realpath -- "${FILE}")\nDeletionDate=$(date +%Y-%m-%dT%H:%M:%S)" > "${TRASHINFO}/${DESTFILE}${SUFFIX}.trashinfo" || return 1
    fi
  done
}
```

Функция для автоматических обновлений путей после установки пакетов:

```bash
pacman () {
  /usr/bin/sudo /usr/bin/pacman $* && echo "$*" | grep -q "S\|R\|U" && rehash
}
yaourt () {
  /usr/bin/yaourt $* && echo "$*" | grep -q "S\|R\|U" && rehash
}
# for testing repo
yatest () {
  /usr/bin/yaourt --config /etc/pactest.conf $* && echo "$*" | grep -q "S\|R\|U" && rehash
}
```

## <a href="#variables" class="anchor" id="variables"><span class="octicon octicon-link"></span></a>Переменные

Рекомендуется хранить свои переменные в `~/.zshenv`. Но я все храню в одном файле.

Пути, маска создаваемых файлов, редактор и пейджер:

```bash
# path
export PATH="$PATH:$HOME/.local/bin"
# umask
umask 022
# editor
export EDITOR="vim"
export PAGER="vimpager"
```

Хэши. Если они включены, команда `~global` будет эквивалентна команде `/mnt/global`:

```bash
# hash
hash -d global=/mnt/global
hash -d windows=/mnt/windows
hash -d iso=/mnt/iso
hash -d u1=/mnt/usbdev1
hash -d u2=/mnt/usbdev2
```

## <a href="#screenshot" class="anchor" id="screenshot"><span class="octicon octicon-link"></span></a>Скриншот

<div class="thumbnails">
  {% assign scrdesc = "Как оно выглядит" %}
  {% assign scrname = "zshrc_demo" %}
  {% include prj_scr.html %}
</div>

## <a href="#file" class="anchor" id="file"><span class="octicon octicon-link"></span></a>Файл

[Мой](//raw.github.com/arcan1s/dotfiles/master/zshrc "Github") `.zshrc`.

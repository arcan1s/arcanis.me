---
category: en
type: paper
layout: paper
hastr: true
tags: zshrc, configuration, linux
title: About zshrc
short: about-zshrc
---
It is first paper in my blog (I think I need something here for tests =)). There
are many similar articles, and I'll not be an exception. I just want to show my
`.zshrc` and explain what it does and why it is needed. Also any comments or
additions are welcome. It is a translated paper from Russian
([original](//archlinux.org.ru/forum/topic/12752/ "Forum thread")).

<!--more-->

## <a href="#prepare" class="anchor" id="prepare"><span class="octicon octicon-link"></span></a>Prepare

First install recommended minima:

```bash
pacman -Sy pkgfile zsh zsh-completions zsh-syntax-highlighting
```

[pkgfile](//www.archlinux.org/packages/pkgfile/ "Archlinux package") is a very
useful utility. Also this command will install shell, additional completion and
syntax highlighting.

## <a href="#configuration" class="anchor" id="configuration"><span class="octicon octicon-link"></span></a>Shell configuration

All options are avaible [here](//zsh.sourceforge.net/Doc/Release/Options.html
"zsh documentation").

Set history file and number of commands in cache of the current session and in
the history file:

```bash
# history
HISTFILE=~/.zsh_history
HISTSIZE=500000
SAVEHIST=500000
```

I can not remember all `Ctrl+` combinations so I bind keys to its default
usages:

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

But in this case `Up`/`Down` arrows are used to navigate through the history
based on **already entered part** of a command. And `PgUp`/`PgDown` **will
ignore** already entered part of a command.

Command autocomplete:

```bash
# autocomplete
autoload -U compinit
compinit
zstyle ':completion:*' insert-tab false
zstyle ':completion:*' max-errors 2
```

Full command autocomplete will be enabled. `insert-tab false` will enable
autocomplete for **non-entered** commands. `max-errors` sets maximum number of
errors that could be corrected.

Prompt:

```bash
# promptinit
autoload -U promptinit
promptinit
```

Enable colors:

```bash
# colors
autoload -U colors
colors
```

Here are some other options.
Change directory without `cd`:

```bash
# autocd
setopt autocd
```

Correcting of typos (and question template):

```bash
# correct
setopt CORRECT_ALL
SPROMPT="Correct '%R' to '%r' ? ([Y]es/[N]o/[E]dit/[A]bort) "
```

Disable f#$%ing beep:

```bash
# disable beeps
unsetopt beep
```

Enable calculator:

```bash
# calc
autoload zcalc
```

Append history (**do not recreate** the history file):

```bash
# append history
setopt APPEND_HISTORY
```

Do not save dups to history file:

```bash
# ignore spaces in history
setopt HIST_IGNORE_ALL_DUPS
```

...and additional spaces:

```bash
# ignore dups in history
setopt HIST_IGNORE_SPACE
```

...and blank lines too:

```bash
# reduce blanks in history
setopt HIST_REDUCE_BLANKS
```

Enable `pkgfile`:

```bash
# pkgfile
source /usr/share/doc/pkgfile/command-not-found.zsh
```

## <a href="#highlighting" class="anchor" id="highlighting"><span class="octicon octicon-link"></span></a>Syntax highlighting

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
ZSH_HIGHLIGHT_STYLES[default]='none'
# unknown
ZSH_HIGHLIGHT_STYLES[unknown-token]='fg=red'
# command
ZSH_HIGHLIGHT_STYLES[reserved-word]='fg=magenta,bold'
ZSH_HIGHLIGHT_STYLES[alias]='fg=yellow,bold'
ZSH_HIGHLIGHT_STYLES[builtin]='fg=green,bold'
ZSH_HIGHLIGHT_STYLES[function]='fg=green,bold'
ZSH_HIGHLIGHT_STYLES[command]='fg=green'
ZSH_HIGHLIGHT_STYLES[precommand]='fg=blue,bold'
ZSH_HIGHLIGHT_STYLES[commandseparator]='fg=yellow'
ZSH_HIGHLIGHT_STYLES[hashed-command]='fg=green'
ZSH_HIGHLIGHT_STYLES[single-hyphen-option]='fg=blue,bold'
ZSH_HIGHLIGHT_STYLES[double-hyphen-option]='fg=blue,bold'
# path
ZSH_HIGHLIGHT_STYLES[path]='fg=cyan,bold'
ZSH_HIGHLIGHT_STYLES[path_prefix]='fg=cyan'
ZSH_HIGHLIGHT_STYLES[path_approx]='fg=cyan'
# shell
ZSH_HIGHLIGHT_STYLES[globbing]='fg=cyan'
ZSH_HIGHLIGHT_STYLES[history-expansion]='fg=blue'
ZSH_HIGHLIGHT_STYLES[assign]='fg=magenta'
ZSH_HIGHLIGHT_STYLES[dollar-double-quoted-argument]='fg=cyan'
ZSH_HIGHLIGHT_STYLES[back-double-quoted-argument]='fg=cyan'
ZSH_HIGHLIGHT_STYLES[back-quoted-argument]='fg=blue'
# quotes
ZSH_HIGHLIGHT_STYLES[single-quoted-argument]='fg=yellow,underline'
ZSH_HIGHLIGHT_STYLES[double-quoted-argument]='fg=yellow'
# pattern example
#ZSH_HIGHLIGHT_PATTERNS+=('rm -rf *' 'fg=white,bold,bg=red')
# root example
#ZSH_HIGHLIGHT_STYLES[root]='bg=red'
```

In first line highlighting is turned on. Next main, brackets and pattern
highlighting are turned on. Patterns are set below (`rm -rf *` in the example).
Also `root` and `cursor` highlighting may be turned on. Colors syntax is
understandable, `fg` is font color, `bg` is background color.

## <a href="#prompt" class="anchor" id="prompt"><span class="octicon octicon-link"></span></a>$PROMPT and $RPROMPT

The general idea is the use single `.zshrc` for root and normal user:

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

`fg` is font color, `bg` is background color. `_bold` and `_no_bold` regulate
the tint. Commands should be in `%{ ... %}` so they do not appear. Avaible
colors are:

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

Avaible variables are:

```bash
%n  - the username
%m  - the computer's hostname (truncated to the first period)
%M  - the computer's hostname
%l  - the current tty
%?  - the return code of the last-run application.
%#  - the prompt based on user privileges (# for root and % for the rest)
%T  - system time(HH:MM)
%*  - system time(HH:MM:SS)
%D  - system date(YY-MM-DD)
%d  - the current working directory
%~  - the same as %d but if in $HOME, this will be replaced by ~
%1/ - the same as %d but only last directory
```

RPROMPT (`acpi` package is necessary):

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

My RPROMPT shows current time, battery change and last returned code. `precmd()`
is necessary for automatic updating. The construct `$(if.true.false)` is
conditional statement in `zsh`.

## <a href="#aliases" class="anchor" id="aliases"><span class="octicon octicon-link"></span></a>Aliases

**Copy only those aliases that you need.** If any alias uses application that is
not installed it will leads to fail of loading of configuration file.

Small useful (or maybe not) function:

```bash
show_which() {
  OUTPUT=$(which $1 | cut -d " " -f7-)
  echo "Running '$OUTPUT'" 1>&2
}
```

Here is the first group of aliases:

```bash
## alias
# colored grep
alias grep='grep --colour=auto'
# change top to htop
alias top='show_which top && htop'
# chromium with different proxy servers (i2p and tor included)
alias chrommsu='show_which chrommsu && chromium --proxy-server=cache.msu:3128'
alias chromtor='show_which chromtor && chromium --proxy-server="socks://localhost:9050" --incognito'
alias chromi2p='show_which chromi2p && chromium --proxy-server="http=127.0.0.1:4444;https=127.0.0.1:4445" --incognito'
# human-readable df and du
alias df='show_which df && df -k --print-type --human-readable'
alias du='show_which du && du -k --total --human-readable'
# change less and zless to vimpager
alias less='vimpager'
alias zless='vimpager'
```

Here are ls aliases (see [man ls](//man7.org/linux/man-pages/man1/ls.1.html "Man
page")):

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

Here are aliases to quick file view from console (just type a file name!):

```bash
# alias -s
alias -s {avi,mpeg,mpg,mov,m2v,mkv}=mpv
alias -s {mp3,flac}=qmmp
alias -s {odt,doc,xls,ppt,docx,xlsx,pptx,csv}=libreoffice
alias -s {pdf}=okular
autoload -U pick-web-browser
alias -s {html,htm}=opera
```

Here are "sudo" aliases:

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

Here are global aliases. If they are enable the command `cat foo g bar` will be
equivalent the command `cat foo | grep bar`:

```bash
# global alias
alias -g g="| grep"
alias -g l="| less"
alias -g t="| tail"
alias -g h="| head"
alias -g dn="&> /dev/null &"
```

## <a href="#functions" class="anchor" id="functions"><span class="octicon octicon-link"></span></a>Functions

Here is a special function for `xrandr`:

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

Unfortunately I can not remember `tar` flags thus I use special functions:

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

Here is a special function for `su`:

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

Function that replaces original `rm` command. If you type `rm` it will be
equivalent moving to trash an you can easily restore a file:

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

Functions with automatic rehash after installing/removing packages are:

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

## <a href="#variables" class="anchor" id="variables"><span class="octicon octicon-link"></span></a>Variables

It is recommended to set own variables in `~/.zshenv`. But I have everything
stored in the single file.

Here are path, mask of new files, editor and pager:

```bash
# path
export PATH="$PATH:$HOME/.local/bin"
# umask
umask 022
# editor
export EDITOR="vim"
export PAGER="vimpager"
```

Here is hashes. If they are enable the command `~global` will be equivalent the
command `/mnt/global`:

```bash
# hash
hash -d global=/mnt/global
hash -d windows=/mnt/windows
hash -d iso=/mnt/iso
hash -d u1=/mnt/usbdev1
hash -d u2=/mnt/usbdev
```

## <a href="#screenshot" class="anchor" id="screenshot"><span class="octicon octicon-link"></span></a>Screenshot

<div class="thumbnails">
  {% assign scrdesc = "Zsh demonstation" %}
  {% assign scrname = "zshrc_demo" %}
  {% include prj_scr.html %}
</div>

## <a href="#file" class="anchor" id="file"><span class="octicon octicon-link"></span></a>File

[Here is](//raw.github.com/arcan1s/dotfiles/master/zshrc "GitHub") my `.zshrc`.

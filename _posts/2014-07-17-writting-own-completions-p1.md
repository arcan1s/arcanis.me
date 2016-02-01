---
category: en
type: paper
hastr: true
layout: paper
tags: linux, development
title: Writting own Shell completions. Zsh
short: writting-own-completions-p1
---
<figure class="img">![bash_completion](/resources/papers/zsh_completion.png)</figure>Some
basics of creating a completion files for own application are described in these
articles.

<!--more-->

## <a href="#preamble" class="anchor" id="preamble"><span class="octicon octicon-link"></span></a>Preamble

While developing [one of my projects](/ru/projects/netctl-gui "Netctl-gui
project page") I have wanted to add completion files. I have already tried to
create these files, but I was too lazy to read some manuals about it.

## <a href="#introduction" class="anchor" id="introduction"><span class="octicon octicon-link"></span></a>Introduction

There are some possible ways to create zsh completion file. In this article I
will describe only one of them, which provides a lot of opportunities, but does
not require a lot of costs (such as regular expressions).

Lets consider the example of my application, which has a part of help message
that looks like this:

```bash
netctl-gui [ -h | --help ] [ -e ESSID | --essid ESSID ] [ -с FILE | --config FILE ]
           [ -o PROFILE | --open PROFILE ] [ -t NUM | --tab NUM ] [ --set-opts OPTIONS ]
```

Here is a flag list:
* flags `-h` and `--help` do not require any arguments;
* flags `-e` and `--essid` require a string argument without completion;
* flags `-c` and `--config` require a string argument, which is a file;
* flags `-o` and `--open` require a string argument, there is a completion from
files in the specified directory;
* flags `-t` and `--tab` require a string argument, there is a completion from
the specified array;
* flag `--set-opts` requires a string argument, there is a completion from the
specified array comma separated;

## <a href="#file" class="anchor" id="file"><span class="octicon octicon-link"></span></a>The file pattern

It must be specified in the header that it is a completion file and application
for which it will complete (may be string if this file provides completions for
several applications):

```bash
#compdef netctl-gui
```

Next there is flags, additional functions and variables declarations. It should
be noted that all functions and variables, which will be used for completions,
**should return arrays**. In my case this scheme looks like this (I left empty
these functions in this chapter):

```bash
# variables
_netctl_gui_arglist=()
_netctl_gui_settings=()
_netctl_gui_tabs=()
_netctl_profiles() {}
```

Then there are main functions, which will be called for completion of specific
application. In my case this there is only one applications, so there is only
one function:

```bash
# work block
_netctl-gui() {}
```

And finally **without isolation in a separate function** there is a small
shamanism, which declares a dependence "application-function":

```bash
case "$service" in
    netctl-gui)
        _netctl-gui "$@" && return 0
        ;;
esac
```

## <a href="#flags" class="anchor" id="flags"><span class="octicon octicon-link"></span></a>Flags

As it was said above, there are some different ways to create these files. In
particular they differ in the flag declaration and their further processing. In
my case I will use `_arguments` command, which require a specific format of
variables: `FLAG[description]:MESSAGE:ACTION`. The last two fields are not
required and, as you will see below, are not needed in some cases. If you want
to add two flags for an action (short and long format), then the format is a
little bit complicated:
`{(FLAG_2)FLAG_1,(FLAG_1)FLAG_2}[description]:MESSAGE:ACTION`. It should be
noted that if you want to create completions for two flags but some flags have
not a second format. you will should to add following line:
`{FLAG,FLAG}[description]:MESSAGE:ACTION`. `MESSAGE` is a message which will be
shown, `ACTION` is an action which will be performed after this flag. In this
tutorial `ACTION` will be following: `->STATE`.

So, according to our requirements, flags declaration will be following:

```bash
_netctl_gui_arglist=(
    {'(--help)-h','(-h)--help'}'[show help and exit]'
    {'(--essid)-e','(-e)--essid'}'[select ESSID]:type ESSID:->essid'
    {'(--config)-c','(-c)--config'}'[read configuration from this file]:select file:->files'
    {'(--open)-o','(-o)--open'}'[open profile]:select profile:->profiles'
    {'(--tab)-t','(-t)--tab'}'[open a tab with specified number]:select tab:->tab'
    {'--set-opts','--set-opts'}'[set options for this run, comma separated]:comma separated:->settings'
)
```

## <a href="#variables" class="anchor" id="variables"><span class="octicon octicon-link"></span></a>Arrays of variables

In my case there are two static arrays (which will not be changed):

```bash
_netctl_gui_settings=(
    'CTRL_DIR'
    'CTRL_GROUP'
)

_netctl_gui_tabs=(
    '1'
    '2'
)
```

And there is a dynamic array, which should be generated each time. In my case it
is a list of files in the specified directory (by the way it may be done by
means of zsh):

```bash
_netctl_profiles() {
    print $(find /etc/netctl -maxdepth 1 -type f -printf "%f\n")
}
```

## <a href="#body" class="anchor" id="body"><span class="octicon octicon-link"></span></a>Function

Remember, there was something about a state above? It is stored in the variable
`$state` and in this function we will check what it is to choose the appropriate
action. At the beginning of the function we should call `_arguments` with our
flags.

```bash
_netctl-gui() {
    _arguments $_netctl_gui_arglist
    case "$state" in
        essid)
            # do not completion, wait for string
            ;;
        files)
            # completion from files in this directory
            _files
            ;;
        profiles)
            # completion from function
            # first variable is a description
            # second variable is a completion array
            _values 'profiles' $(_netctl_profiles)
            ;;
        tab)
            # completion from array
            _values 'tab' $_netctl_gui_tabs
            ;;
        settings)
            # completion from array
            # flag -s sets separator and enables multi-selection
            _values -s ',' 'settings' $_netctl_gui_settings
            ;;
    esac
}
```

## <a href="#conclusion" class="anchor" id="conclusion"><span class="octicon octicon-link"></span></a>Conclusion

File should be places to `/usr/share/zsh/site-functions/` with any name (it is
recommended to set prefix to `_`). You may found the example [in my
repository](//raw.githubusercontent.com/arcan1s/netctl-gui/master/sources/gui/zsh-completions
"File").

The additional information may be found in
[zsh-completions](//github.com/zsh-users/zsh-completions "GitHub") repository.
For example there is this
[How-To](//github.com/zsh-users/zsh-completions/blob/master/zsh-completions-howto.org
"Tutorial"). And also there are a lot of examples.

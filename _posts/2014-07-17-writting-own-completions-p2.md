---
category: en
type: paper
hastr: true
layout: paper
tags: linux, development
title: Writting own Shell completions. Bash
short: writting-own-completions-p2
---
<figure class="img">![bash_completion](/resources/papers/bash_completion.png)</figure>Some
basics of creating a completion files for own application are described in these
articles.

<!--more-->

## <a href="#preamble" class="anchor" id="preamble"><span class="octicon octicon-link"></span></a>Preamble

While developing [one of my projects](/ru/projects/netctl-gui "Netctl-gui
project page") I have wanted to add completion files. I have already tried to
create these files, but I was too lazy to read some manuals about it.

## <a href="#introduction" class="anchor" id="introduction"><span class="octicon octicon-link"></span></a>Introduction

Bash, [unlike zsh](/ru/2014/07/17/writting-own-completions-p1 "Zsh completions
paper"), demands some dirty workarounds for completions. Cursory have Googled, I
have not found a more or less normal tutorials, so it is based on the available
`pacman` completion files in my system.

Lets consider the example of the same my application. I remind you that a part
of help message is as follows:

```bash
netctl-gui [ -h | --help ] [ -e ESSID | --essid ESSID ] [ -с FILE | --config FILE ]
           [ -o PROFILE | --open PROFILE ] [ -t NUM | --tab NUM ] [ --set-opts OPTIONS ]
```

Here is a flag list:
* flags `-h` and `--help` do not require any arguments;
* flags `-e` and `--essid` require a string argument without completion;
* flags `-c` and `--config` require a string argument, which is a file;
* flags `-o` and `--open` require a string argument, there is a completion from files in the specified directory;
* flags `-t` and `--tab` require a string argument, there is a completion from the specified array;
* flag `--set-opts` requires a string argument, there is a completion from the specified array comma separated;

## <a href="#file" class="anchor" id="file"><span class="octicon octicon-link"></span></a>The file pattern

Here **all** variables must return an array. And there no specific formats.
First we declare the flags and then we describe all other variables. As I am not
going to describe the functions in more detail below I remind you that
`_netctl_profiles()` should be generated each time:

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

And finally again **without isolation in a separate function** we create a
dependence "function-application":

```bash
complete -F _netctl_gui netctl-gui
```

## <a href="#flags" class="anchor" id="flags"><span class="octicon octicon-link"></span></a>Flags

As it was said above there is no specific format, so all available flags declare
by array:

```bash
_netctl_gui_arglist=(
    '-h'
    '--help'
    '-e'
    '--essid'
    '-c'
    '--config'
    '-o'
    '--open'
    '-t'
    '--tab'
    '--set-opts'
)
```

## <a href="#variables" class="anchor" id="variables"><span class="octicon octicon-link"></span></a>Arrays of variables

I just give a function that looked like this in zsh:

```bash
_netctl_profiles() {
    print $(find /etc/netctl -maxdepth 1 -type f -printf "%f\n")
}
```

Bash does not allow to do so, so this function should be a little changed:

```bash
_netctl_profiles() {
    echo $(find /etc/netctl -maxdepth 1 -type f -printf "%f\n")
}
```

## <a href="#body" class="anchor" id="body"><span class="octicon octicon-link"></span></a>Function

The variable `COMPREPLY` responds for completion in Bash. To keep track of the
current state function `_get_comp_words_by_ref` must be called with parameters
`cur` (current flag) and `prev` (previous flag, it is the state). Also some
point for case are needed (variables `want*`). Function `compgen` is used for
completion generation. A list of words is given after flag `-W`. (Also there is
flag `-F` which requires a function as argument, but it gives warning for me.)
The last argument is a current string to which you want to generate completion.

So, here is our function:

```bash
_netctl_gui() {
    COMPREPLY=()
    wantfiles='-@(c|-config)'
    wantprofiles='-@(o|-open|s|-select)'
    wantsettings='-@(-set-opts)'

    wanttabs='-@(t|-tab)'
    _get_comp_words_by_ref cur prev

    if [[ $prev = $wantstring ]]; then
        # do not completion, wait for string
        COMPREPLY=()
    elif [[ $prev = $wantfiles ]]; then
        # completion from files in this directory
        _filedir
    elif [[ $prev = $wantprofiles ]]; then
        # completion from function
        COMPREPLY=($(compgen -W '${_netctl_profiles[@]}' -- "$cur"))
    elif [[ $prev = $wanttabs ]]; then
        # completion from array
        COMPREPLY=($(compgen -W '${_netctl_gui_tabs[@]}' -- "$cur"))
    elif [[ $prev = $wantsettings ]]; then
        # completion from array
        # flag -S add a comma after completion, but does not enable multi-selection =(
        COMPREPLY=($(compgen -S ',' -W '${_netctl_gui_settings[@]}' -- "$cur"))
    else
        # show all available flags
        COMPREPLY=($(compgen -W '${_netctl_gui_arglist[@]}' -- "$cur"))
    fi

    true
}
```

## <a href="#conclusion" class="anchor" id="conclusion"><span class="octicon octicon-link"></span></a>Conclusion

File should be places to `/usr/share/bash-completion/completions/` with any
name. You may found the example [in my
repository](//raw.githubusercontent.com/arcan1s/netctl-gui/master/sources/gui/bash-completions
"File").

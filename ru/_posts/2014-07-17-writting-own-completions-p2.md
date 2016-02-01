---
category: ru
type: paper
hastr: true
layout: paper
tags: linux, разработка
title: Написание своих дополнений для Shell. Bash
short: writting-own-completions-p2
---
<figure class="img">![bash_completion](/resources/papers/bash_completion.png)</figure> В
данных статьях описываются некоторые основы создания файлов дополнений для
собственной программы.

<!--more-->

## <a href="#preamble" class="anchor" id="preamble"><span class="octicon octicon-link"></span></a>Преамбула

В процессе разработки [одного своего проекта](/ru/projects/netctl-gui
"Страница netctl-gui") возникло желание добавить также файлы дополнений (только
не спрашивайте зачем). Благо я как-то уже брался за написание подобных вещей, но
читать что-либо тогда мне было лень, и так и не осилил.

## <a href="#introduction" class="anchor" id="introduction"><span class="octicon octicon-link"></span></a>Введение

Bash, в [отличие от zsh](/ru/2014/07/17/writting-own-completions-p1
"Статья о дополнениях zsh"), требует к себе некоторого велосипедостроения в
отношении дополнений. Бегло погуглив, я не нашел более-менее нормальных
туториалов, потому за основу были взяты имеющиеся в системе файлы дополнений для
`pacman`.

Рассмотрим на примере все того же моего приложения. Я напомню, что часть справки
к которому выглядит таким образом:

```bash
netctl-gui [ -h | --help ] [ -e ESSID | --essid ESSID ] [ -с FILE | --config FILE ]
           [ -o PROFILE | --open PROFILE ] [ -t NUM | --tab NUM ] [ --set-opts OPTIONS ]
```

Список флагов:

* флаги `-h` и `--help` не требуют аргументов;
* флаги `-e` и `--essid` требуют аргумента в виде строки, без дополнения;
* флаги `-c` и `--config` требуют аргумента в виде строки, файл с произвольной
локацией;
* флаги `-o` и `--open` требуют аргумента в виде строки, дополнение по файлам из
определенной директории;
* флаги `-t` и `--tab` требуют аргумента в виде строки, дополнение из указанного
массива;
* флаг `--set-opts` требует аргумента в виде строки, дополнение из указанного
массива, разделены запятыми;

## <a href="#file" class="anchor" id="file"><span class="octicon octicon-link"></span></a>Структура файла

Здесь **все** переменные должны возвращать массив. Каких-либо особых форматов тут
уже нет. Сначала опишем флаги, потом уже все остальные переменные. Я напомню
(так как ниже я уже не буду приводить функции более подробно), что
`_netctl_profiles()`, в отличие от других переменных, должна возвращать
актуальный на данный момент массив:

```bash
# variables
_netctl_gui_arglist=()
_netctl_gui_settings=()
_netctl_gui_tabs=()
_netctl_profiles() {}
```

Затем идут основные функции, которые будут вызываться для дополнения для
определенной команды. В моем случае команда одна, и функция одна:

```bash
# work block
_netctl-gui() {}
```

Далее, опять, **без выделения в отдельную функцию** делаем соответствие
"функция-команда":

```bash
complete -F _netctl_gui netctl-gui
```

## <a href="#flags" class="anchor" id="flags"><span class="octicon octicon-link"></span></a>Флаги

Как было сказано выше, особого формата тут нет, доступные флаги располагаются
просто массивом:

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

## <a href="#variables" class="anchor" id="variables"><span class="octicon octicon-link"></span></a>Массивы переменных

Приведу только функцию, которая в zsh выглядела таким образом:

```bash
_netctl_profiles() {
    print $(find /etc/netctl -maxdepth 1 -type f -printf "%f\n")
}
```

В bash так не получится, пришлось чуть-чуть изменить:

```bash
_netctl_profiles() {
    echo $(find /etc/netctl -maxdepth 1 -type f -printf "%f\n")
}
```

## <a href="#body" class="anchor" id="body"><span class="octicon octicon-link"></span></a>Тело функции

За дополнение в bash отвечает переменная `COMPREPLY`. Для отслеживания текущего
состояния нужно вызвать функцию `_get_comp_words_by_ref` с параметрами `cur`
(текущая опция) и `prev` (предыдущая, собственно состояние). Ну и нужно
несколько точек, на которых сворачивать в определенную часть case (переменные
`want*`). Для генерации дополнения используется `compgen`. После флага `-W` ему
подается список слов. (Есть еще флаг `-F`, который вызывает функцию, но у меня
он помимо этого еще и ворнинг выдает.) Последним аргументом идет текущая строка,
к которой и нужно генерировать дополнение.

Таким образом, наша функция выглядит так:

```bash
_netctl_gui() {
    COMPREPLY=()
    wantfiles='-@(c|-config)'
    wantprofiles='-@(o|-open|s|-select)'
    wantsettings='-@(-set-opts)'

    wanttabs='-@(t|-tab)'
    _get_comp_words_by_ref cur prev

    if [[ $prev = $wantstring ]]; then
        # не делать дополнения, ждать введенной строки
        COMPREPLY=()
    elif [[ $prev = $wantfiles ]]; then
        # дополнение по существующим файлам
        _filedir
    elif [[ $prev = $wantprofiles ]]; then
        # дополнение из функции
        COMPREPLY=($(compgen -W '${_netctl_profiles[@]}' -- "$cur"))
    elif [[ $prev = $wanttabs ]]; then
        # дополнение из массива
        COMPREPLY=($(compgen -W '${_netctl_gui_tabs[@]}' -- "$cur"))
    elif [[ $prev = $wantsettings ]]; then
        # дополнение из массива
        # -S вставит запятую после, но вот мультивыбор не включил =(
        COMPREPLY=($(compgen -S ',' -W '${_netctl_gui_settings[@]}' -- "$cur"))
    else
        # вывести доступные аргументы
        COMPREPLY=($(compgen -W '${_netctl_gui_arglist[@]}' -- "$cur"))
    fi

    true
}
```

## <a href="#conclusion" class="anchor" id="conclusion"><span class="octicon octicon-link"></span></a>Заключение

Файл хранится в директории `/usr/share/bash-completion/completions/` с
произвольным именем. Файл примера полностью может быть найден [в моем репозитории]
(//raw.githubusercontent.com/arcan1s/netctl-gui/master/sources/gui/bash-completions "Файл").

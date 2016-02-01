---
category: ru
type: paper
hastr: true
layout: paper
tags: разработка, c++, cmake
title: Добавляем cppcheck и clang-format для проекта на cmake
short: cppcheck-and-clang-format
---
Небольшое How-To посвященное прикручиванию автоматической проверки стиля, а
также статического анализатора к проекту на `C++`, который использует в качестве
системы сборки `cmake`.

<!--more-->

## <a href="#project" class="anchor" id="project"><span class="octicon octicon-link"></span></a>Проект

Наш проект имеет следующую структуру:

```bash
sources/
|- CMakeLists.txt
|- 3rdparty/
|- first_component/
|- second_component/
```

**3rdparty** - директория с различными дополнительными библиотеками, которую
надо исключить из проверок (в дальнейшем соответствует переменной cmake
`PROJECT_TRDPARTY_DIR`). Дополнительно допустим, что у нас, помимо обычных
файлов исходного кода (`*.cpp`, `*.h`) есть еще какие-либо (например, `*.qml`).

Дополнительно используемые ниже команды можно вставить в pre-commit hook и
невозбранно тролить коллег по ынтырпрайзу, не давая им закоммитить ничего, пока
они не научатся читать `CONTRIBUTING.md`.

## <a href="#cppcheck" class="anchor" id="cppcheck"><span class="octicon octicon-link"></span></a>cppcheck

Коль скоро нормальных (из коробки) статических анализаторов не завезли в open
source будем использовать то, что имеется. Знатоки говорят, что
[cppcheck](//cppcheck.sourceforge.net/ "Сайт cppcheck") при должной конфигурации
будет лучше, чем любой аналог, но конфигурация его для достаточно большого
проекта похожа больше на написание нового проекта. Суть добавления cppheck к
проекту сводится к указанию очевидных недоработок в коде и тыканью в <del>лужу</del>
них.

### <a href="#cppcheck-run" class="anchor" id="cppcheck-run"><span class="octicon octicon-link"></span></a>Общий пример запуска

Тут все, казалось бы, очень просто:

```bash
cppcheck --enable=warning,performance,portability,information,missingInclude
--std=c++11 --library=qt.cfg --template="[{severity}][{id}] {message}
{callstack} (On {file}:{line})" --verbose --quiet
path/to/source/files/or/directory
```

* `--enable` говорит о том, какие уведомления надо включить. Я выключил `style`
(для этого ниже мы заведем `clang-format`), `unusedFunction` - выдает
false-positive для некоторых мест.
* `--std` говорит об используемом стандарте.
* `--library=qt.cfg` некий файл настроек, который говорит о том, что и как надо
обрабатывать. Добрые разработчики предлагаю почитать на эту тему
[мануал](//cppcheck.sourceforge.net/manual.pdf "cppcheck мануал"). В данном
случае я использовал шаблон из `/usr/share/cppcheck/cfg/`.
* `--template` - шаблон строки уведомления.
* `---verbose --quiet` две противоречащие друг другу опции. Первая включает
более информативные сообщения, вторая выключает отчет о прогрессе.

### <a href="#cppcheck-cmake" class="anchor" id="cppcheck-cmake"><span class="octicon octicon-link"></span></a>Интеграция с cmake

Файл `cppcheck.cmake` в корне проекта:

```cmake
# additional target to perform cppcheck run, requires cppcheck

# get all project files
# HACK this workaround is required to avoid qml files checking ^_^
file(GLOB_RECURSE ALL_SOURCE_FILES *.cpp *.h)
foreach (SOURCE_FILE ${ALL_SOURCE_FILES})
    string(FIND ${SOURCE_FILE} ${PROJECT_TRDPARTY_DIR} PROJECT_TRDPARTY_DIR_FOUND)
    if (NOT ${PROJECT_TRDPARTY_DIR_FOUND} EQUAL -1)
        list(REMOVE_ITEM ALL_SOURCE_FILES ${SOURCE_FILE})
    endif ()
endforeach ()

add_custom_target(
        cppcheck
        COMMAND /usr/bin/cppcheck
        --enable=warning,performance,portability,information,missingInclude
        --std=c++11
        --library=qt.cfg
        --template="[{severity}][{id}] {message} {callstack} \(On {file}:{line}\)"
        --verbose
        --quiet
        ${ALL_SOURCE_FILES}
)
```

`cppcheck` умеет рекурсивно директории проверять, однако, на моем примере, мне
нужно было пропустить проверку qml-файлов, потому что open source проект, в
лучших традициях, сегфолтился на некоторых из них - именно для этого
используется поиск исходных файлов с дальнейшим выбрасыванием из них файлов,
которые не должны проверяться.

Включаем в проект (`CMakeLists.txt`)...

```cmake
include(cppcheck.cmake)
```

...и запускаем:

```bash
cmake
make cppcheck
```

Дальше руками вносим необходимые исправления.

### <a href="#cppcheck-adds" class="anchor" id="cppcheck-adds"><span class="octicon octicon-link"></span></a>Дополнительно

* Можно добавить свои директории для поиска хидеров, используя опцию `-I dir`.
* Можно вычеркнуть файлы и/или директории из проверки, используя опцию `-i
path/to/file/or/directory`.

## <a href="#clang" class="anchor" id="clang"><span class="octicon octicon-link"></span></a>clang-format

[clang-format](//clang.llvm.org/docs/ClangFormat.html "Сайт clang-format")
предназначен для автоматического подгона стиля под желаемый или требуемый. Среди
аналогов стоит выделить [astyle](//astyle.sourceforge.net/ "Сайт astyle"),
который имеет очень скромные возможности, и
[uncrustify](//uncrustify.sourceforge.net/ "Сайт uncrustify"), который,
наоборот, имеет слишком много опций.

### <a href="#clang-run" class="anchor" id="clang-run"><span class="octicon octicon-link"></span></a>Общий пример запуска

```bash
clang-format -i -style=LLVM /path/to/source/files
```

(К сожалению, он **не умеет** в рекурсивный обход директории.)

* `-i` включает автозамену файлов (в противном случае, результат будет
печататься в stdout).
* `-style` выбор определенного стиля либо из предустановленных, либо из файла
(`file`), см. ниже.

### <a href="#clang-cmake" class="anchor" id="clang-cmake"><span class="octicon octicon-link"></span></a>Интеграция с cmake

Файл `clang-format.cmake` в корне проекта:

```cmake
# additional target to perform clang-format run, requires clang-format

# get all project files
file(GLOB_RECURSE ALL_SOURCE_FILES *.cpp *.h)
foreach (SOURCE_FILE ${ALL_SOURCE_FILES})
    string(FIND ${SOURCE_FILE} ${PROJECT_TRDPARTY_DIR} PROJECT_TRDPARTY_DIR_FOUND)
    if (NOT ${PROJECT_TRDPARTY_DIR_FOUND} EQUAL -1)
        list(REMOVE_ITEM ALL_SOURCE_FILES ${SOURCE_FILE})
    endif ()
endforeach ()

add_custom_target(
        clangformat
        COMMAND /usr/bin/clang-format
        -style=LLVM
        -i
        ${ALL_SOURCE_FILES}
)
```

Аналогичных способ поиска исходных файлов, как и для `cppcheck`, поскольку
`clang-format` не умеет в рекурсию.

Включаем в проект (`CMakeLists.txt`)...

```cmake
include(clang-format.cmake)
```

...и запускаем:

```bash
cmake
make clangformat
```

Никаких дополнительных действий не требуется.

### <a href="#clang-adds" class="anchor" id="clang-adds"><span class="octicon octicon-link"></span></a>Дополнительно

*   Настройка. Можно почитать опции на [официальном
сайте](//clang.llvm.org/docs/ClangFormat.html "Сайт clang-format"). Также можно
воспользоваться [интерактивной утилитой](//clangformat.com/ "Сайт") для
просмотра опций. Для использования уже готового стиля за базу используем
следующую команду:

    ```bash
    clang-format -style=LLVM -dump-config > .clang-format
    ```

    Далее редактируется полученный файл `.clang-format`. Для включения его
необходимо передать опцию `-style=file`, файл должен находиться в одной из
родительских директории для каждого файла (например, в корне проекта). Также,
можно передать нужные опции прямо в командной строке, например
`-style="{BasedOnStyle: llvm, IndentWidth: 8}"`.

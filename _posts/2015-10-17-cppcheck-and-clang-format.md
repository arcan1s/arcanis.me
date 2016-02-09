---
category: en
type: paper
hastr: true
layout: paper
tags: development, c++, cmake
title: Add cppcheck and clang-format for a cmake project
short: cppcheck-and-clang-format
---
A small How-To which describes how to add automatic code style checking and
static analyser to a project on `C++` which uses `cmake` as a build system.

<!--more-->

## <a href="#project" class="anchor" id="project"><span class="octicon octicon-link"></span></a>Project

The project has the following structure:

```bash
sources/
|- CMakeLists.txt
|- 3rdparty/
|- first_component/
|- second_component/
```

**3rdparty** is a directory which contains additional libraries and which should
be excluded from checking (`PROJECT_TRDPARTY_DIR` cmake variable is used to
indicate path to this directory). Also let's assume that we have additional
files (e.g. `*.qml`) in addition to common source files (`*.cpp`, `*.h`).

In addition the described below commands may be inserted to pre-commit hook; it
allows us to troll colleagues which will be able to commit nothing till they
read `CONTRIBUTING.md`.

## <a href="#cppcheck" class="anchor" id="cppcheck"><span class="octicon octicon-link"></span></a>cppcheck

As far as there are no good (out-of-box) static analysers in open source we will
use it. Knowledgeable people say that [cppcheck](//cppcheck.sourceforge.net/
"cppcheck site") in case of good configuration it is better than the any
alternative, but its configuration is the same that the new project creation.
`cppcheck` shows obvious errors and recommend to fix them.

### <a href="#cppcheck-run" class="anchor" id="cppcheck-run"><span class="octicon octicon-link"></span></a>Example of run

Here it is:

```bash
cppcheck --enable=warning,performance,portability,information,missingInclude \
         --std=c++11 --library=qt.cfg --verbose --quiet \
         --template="[{severity}][{id}] {message} {callstack} (On {file}:{line})" \
         path/to/source/files/or/directory
```

* `--enable` says which notifications should be enabled. I've disabled `style`
(we will use `clang-format` to do it), `unusedFunction` which shows
false-positive for some methods.
* `--std` says which standard should be used.
* `--library=qt.cfg` a configuration file, which describes how to check files.
The developers recommend to read the following
[manual](//cppcheck.sourceforge.net/manual.pdf "cppcheck manual"). I've used the
ready template from `/usr/share/cppcheck/cfg/`.
* `--template` is notification template.
* `---verbose --quiet` are two "conflicting" options. The first one enables more
verbose messages, the second one disables progress reports.

### <a href="#cppcheck-cmake" class="anchor" id="cppcheck-cmake"><span class="octicon octicon-link"></span></a>cmake integration

`cppcheck.cmake` file in the project root:

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

`cppcheck` may work with directories recursive, but I need to skip qml-files
checking in my example, because this `cppcheck` will segfault on some of them.
To do it source files search is used followed by the ejection of unnecessary
files.

Include to the project (`CMakeLists.txt`)...

```cmake
include(cppcheck.cmake)
```

...and run:

```bash
cmake
make cppcheck
```

Then edit files to avoid warnings in the future.

### <a href="#cppcheck-adds" class="anchor" id="cppcheck-adds"><span class="octicon octicon-link"></span></a>Adds

* You may add own directories to includes search, using `-I dir` option
* You may drop files and/or directories from checking by using `-i
path/to/file/or/directory` option.

## <a href="#clang" class="anchor" id="clang"><span class="octicon octicon-link"></span></a>clang-format

[clang-format](//clang.llvm.org/docs/ClangFormat.html "clang-format site") is
used to automatic code style checking and correction.
[astyle](//astyle.sourceforge.net/ "astyle site"), which has a very modest
capabilities, and [uncrustify](//uncrustify.sourceforge.net/ "uncrustify site"),
which on the contrary has too many options, should be mentioned from analogues.

### <a href="#clang-run" class="anchor" id="clang-run"><span class="octicon octicon-link"></span></a>Example of run

```bash
clang-format -i -style=LLVM /path/to/source/files
```

(Unfortunately it **could not** work with directories recursive.)

* `-i` enables files auto replace (otherwise the result will be printed to
stdout).
* `-style` is a style preset selection or from file (`file`), see below.

### <a href="#clang-cmake" class="anchor" id="clang-cmake"><span class="octicon octicon-link"></span></a>cmake integration

`clang-format.cmake` file in the project root:

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

There is the same method to get source files list as for `cppcheck`, because
`clang-format` doesn't support recursive directory search.

Include to the project (`CMakeLists.txt`)...

```cmake
include(clang-format.cmake)
```

...and run:

```bash
cmake
make clangformat
```

No other actions required.

### <a href="#clang-adds" class="anchor" id="clang-adds"><span class="octicon octicon-link"></span></a>Adds

*   Configuration. You may see all options on the [official
site](//clang.llvm.org/docs/ClangFormat.html "clang-format site"). Also you may
use [interactive tool](//clangformat.com/ "Site") to search for required
options. To use the preset for your style use the following command:

    ```bash
    clang-format -style=LLVM -dump-config > .clang-format
    ```

    Then edit generated file `.clang-format`. To enable it you need set
`-style=file` option, file should be placed to the any parent directory of each
source file (e.g., to the project root). Also you may send required options to
the command line directly, e.g. `-style="{BasedOnStyle: llvm, IndentWidth: 8}"`

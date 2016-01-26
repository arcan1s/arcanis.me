---
hastr: true
layout: project
title: Report a Bug
short: reportabug
tags: qt, c++, library, development
hasgui: false
hasdocs: true
developers:
    - Evgeniy Alekseev
license: LGPLv3
links:
---
<!-- info block -->
## <a href="#info" class="anchor" id="info"><span class="octicon octicon-link"></span></a>Information

Qt application/library which allows users to create an issue for projects which are hosted on GitHub. It may work over [GitHub](//github.com "GitHub") or [GitReport](//gitreports.com/ "GitReports"). It works fine, but it was created as proof-of-concept.

### <a href="#devel" class="anchor" id="devel"><span class="octicon octicon-link"></span></a>Developers and contributors

{% for devel in page.developers %}
* {{ devel }}{% endfor %}

### <a href="#license" class="anchor" id="license"><span class="octicon octicon-link"></span></a>License

* {{ page.license }}

<!-- end of info block -->

<!-- install block -->
## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Installation

### <a href="#instruction" class="anchor" id="instruction"><span class="octicon octicon-link"></span></a>Instruction

#### <a href="#singleapp" class="anchor" id="singleapp"><span class="octicon octicon-link"></span></a>Build as a standalone application

* Download the actual source [tarball](//github.com/arcan1s/reportabug/releases "GitHub").
* Extract it and set up your configuration.
* Install the application:

    ```bash
    cd /path/to/extracted/archive
    mkdir build && cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release -DBUILD_AS_LIBRARY:BOOL=0 ../
    make
    sudo make install
    ```

#### <a href="#aslibrary" class="anchor" id="aslibrary"><span class="octicon octicon-link"></span></a>Build as a library in your project

* Download the actual source [tarball](//github.com/arcan1s/reportabug/releases "GitHub").
* Extract it and set up your configuration.
* include it into your project. For example if you use `cmake`:

    ```cmake
    add_subdirectory (reportabug)
    ```

* Declare class in you sources. For example:

    ```cpp
    Reportabug *reportWindow = new Reportabug(parent=this, debugCmd=false, params=0);
    reportWindow->showWindow();
    ```

* Link your application with this library.

#### <a href="#cmakeflags" class="anchor" id="cmakeflags"><span class="octicon octicon-link"></span></a>Available cmake flags:

* `-DBUILD_AS_LIBRARY=0` - build the application but not a library.
* `-DBUILD_DOCS=1` - build developer documentation.
* `-DBUILD_SHARED_LIBRARY=1` - build the shared library instead of static one.
* `-DENABLE_GITHUB=0` - disable GitHub module.
* `-DENABLE_GITREPORT=0` - disable GitReport module.
* `-DOWN_GITHUB_TOKEN=STRING` - use STRING as own GitHub token.
* `-DUSE_QT5=0` - use Qt4 instead of Qt5.

### <a href="#dependencies" class="anchor" id="dependencies"><span class="octicon octicon-link"></span></a>Dependencies

I want note that all were tested on latest version of dependencies.

* qt5-base *(if Qt5 is used)* **or** qt4 *(if Qt4 is used)*
* qt5-network (if Qt5 is used)
* automoc4 *(make)*
* cmake *(make)*
* doxygen *(make, documentation)*
* qt5-webkit (if Qt5 is used) **or** qtwebkit (if Qt4 is used) *(optional, required by GitReports module)*

<!-- end of install block -->

<!-- howto block -->
## <a href="#howto" class="anchor" id="howto"><span class="octicon octicon-link"></span></a>How to use

### <a href="#github" class="anchor" id="github"><span class="octicon octicon-link"></span></a>GitHub module

This module creates an issue over GitHub. [GitHub API](//developer.github.com/v3/issues/ "Documentation") is used for creating issue. User should type own username and password. The typical POST request is:

```bash
curl -X POST -u user:pass -d '{"title":"A new bug","body":"Some error occurs"}' \
     //api.github.com/repos/owner/repo/issues
```

To disable this module use `-DENABLE_GITHUB=0` cmake flag.

This module may send request using developer's token too. Please visit [this page](//github.com/settings/applications "Settings") and generate a new one. Needed scopes are `public_repo` (or `repo` if you will use it for a private repository).

**Please keep in mind that passing the token in the clear, you may discredit your account.**

The typical POST request is:

```bash
curl -X POST -H "Authorization: token token" -d '{"title":"A new bug","body":"Some error occurs"}' \
     //api.github.com/repos/owner/repo/issues
```

To enable this module set up your token using `-DOWN_GITHUB_TOKEN=0` cmake flag.

This module requires `QtNetwork` module.

### <a href="#gitreports" class="anchor" id="gitreports"><span class="octicon octicon-link"></span></a>GitReports module

This module creates issue over [GitReports](//gitreports.com/about "GitReports"). Please visit [this page](//gitreports.com/ "GitReports") and set up it for your repository.

To disable this module use `-DENABLE_GITREPORT=0` cmake flag. This module requires `QtNetwork` and `QtWebKit` modules.

<!-- end of howto block -->

<!-- config block -->
## <a href="#config" class="anchor" id="config"><span class="octicon octicon-link"></span></a>Configuration

For the developer configuration please use `config.h` header. Also you may load parametrs dynamically using `params` array (needed keys is the same as for the header

### <a href="#mainconfig" class="anchor" id="mainconfig"><span class="octicon octicon-link"></span></a>Main configuration

* `OWNER` - the owner of the source repository.
* `PROJECT` - the project name.
* `TAG_BODY` - default body of an issue. It may be used for both modules.
* `TAG_TITLE` - default title of an issue. It may be used only for GitHub module.
* `TAG_ASSIGNEE` - assign an issue to this account. It may be used only for GitHub module. This tag will work only if user has push access. If it will be empty, it will be ignored.
* `TAG_LABELS` - set these labels to an issue. Labels should be comma separated. It may be used only for GitHub module. This tag will work only if user has push access. If it will be empty, it will be ignored.
* `TAG_MILESTONE` - set this milestone to an issue. It may be used only for GitHub module. This tag will work only if user has push access. If it will be empty, it will be ignored.

### <a href="#githubconfig" class="anchor" id="githubconfig"><span class="octicon octicon-link"></span></a>GitHub module settings

* `GITHUB_COMBOBOX` - text of this module into comboBox.
* `ISSUES_URL` - issues url, in the most cases do not touch it. Default is `//api.github.com/repos/$OWNER/$PROJECT/issues`. Available tags here are `$PROJECT`, `$OWNER`.

### <a href="#gitreportsconfig" class="anchor" id="gitreportsconfig"><span class="octicon octicon-link"></span></a>GitReports module settings

* `CAPTCHA_URL` - captcha url, in the most cases do not touch it. Default is `//gitreports.com/simple_captcha?code=`.
* `GITREPORT_COMBOBOX` - text of this module into comboBox.
* `PUBLIC_URL` - issues url, in the most cases do not touch it. Default is `//gitreports.com/issue/$OWNER/$PROJECT`. Available tags here are `$PROJECT`, `$OWNER`.

<!-- end of config block -->

<!-- gui block -->
<!-- end of gui block -->

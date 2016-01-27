---
permalink: ru/projects/reportabug
category: ru
hastr: true
layout: project
title: Report a Bug
short: reportabug
tags: qt, c++, библиотека, разработка
hasgui: false
hasdocs: true
developers:
    - Evgeniy Alekseev
license: LGPLv3
links:
---
<!-- info block -->

Приложение/библиотека, написанное на Qt, которое позволяет пользователям отправлять багрепорт для проектов, расположенных на GitHub. Оно может работать как через [GitHub](//github.com "GitHub"), так и через [GitReports](//gitreports.com "GitReports"). Работает нормально, однако данное приложение было создано as proof-of-concept.

<!--more-->

### <a href="#devel" class="anchor" id="devel"><span class="octicon octicon-link"></span></a>Разработчики

{% for devel in page.developers %}
* {{ devel }}{% endfor %}

### <a href="#license" class="anchor" id="license"><span class="octicon octicon-link"></span></a>Лицензия

* {{ page.license }}

<!-- end of info block -->

<!-- install block -->
## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Установка

### <a href="#instruction" class="anchor" id="instruction"><span class="octicon octicon-link"></span></a>Инструкция
### <a href="#singleapp" class="anchor" id="singleapp"><span class="octicon octicon-link"></span></a>Сборка, как отдельное приложение

* Скачайте [архив](//github.com/arcan1s/reportabug/releases "GitHub") с актуальной версией исходных файлов.
* Извлеките из него файлы и настройте под себя.
* Установите приложение:

    ```bash
    cd /path/to/extracted/archive
    mkdir build && cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release -DBUILD_AS_LIBRARY:BOOL=0 ../
    make
    sudo make install
    ```

### <a href="#aslibrary" class="anchor" id="aslibrary"><span class="octicon octicon-link"></span></a>Сборка, как библиотека в проекте

* Скачайте [архив](//github.com/arcan1s/reportabug/releases "GitHub") с актуальной версией исходных файлов.
* Извлеките из него файлы и настройте под себя.
* Включите библиотеку в Ваш проект. Например, если Вы используете `cmake`:

    ```cmake
    add_subdirectory (reportabug)
    ```

* Объявите класс в Вашем приложении, например:

    ```cpp
    Reportabug *reportWindow = new Reportabug(parent=this, debugCmd=false, params=0);
    reportWindow->showWindow();
    ```

* Слинкуйте Ваше приложение с библиотекой.

### <a href="#cmakeflags" class="anchor" id="cmakeflags"><span class="octicon octicon-link"></span></a>Доступные флаги cmake

* `-DBUILD_AS_LIBRARY=0` - собирать отдельное приложение, а не библиотеку.
* `-DBUILD_DOCS=1` - собирать документацию для разработчиков.
* `-DBUILD_SHARED_LIBRARY=1` - собирать библиотеку общего доступа, а не статическую.
* `-DENABLE_GITHUB=0` - отключить модуль GitHub.
* `-DENABLE_GITREPORT=0` - отключить модуль GitReports
* `-DOWN_GITHUB_TOKEN=STRING` - использовать STRING, как свой GitHub токен.
* `-DUSE_QT5=0` - использовать Qt4 вместо Qt5.

### <a href="#dependencies" class="anchor" id="dependencies"><span class="octicon octicon-link"></span></a>Зависимости

Все было протестировано на последних версиях зависимостей.

* qt5-base *(если используется Qt5)* **или** qt4 *(если используется Qt4)*
* qt5-network (если используется Qt5)
* automoc4 *(make)*
* cmake *(make)*
* doxygen *(make, документация)*
* qt5-webkit (если используется Qt5) **или** qtwebkit (если используется Qt4) *(опционально, требуется для модуля GitReports)*

<!-- end of install block -->

<!-- howto block -->
## <a href="#howto" class="anchor" id="howto"><span class="octicon octicon-link"></span></a>Использование

### <a href="#github" class="anchor" id="github"><span class="octicon octicon-link"></span></a>Модуль GitHub

Данный модуль создает тикет, используя [GitHub API](//developer.github.com/v3/issues/ "Документация"). Данный модуль требует авторизации пользователя. Типичный POST запрос выглядит так:

```bash
curl -X POST -u user:pass -d '{"title":"A new bug","body":"Some error occurs"}' \
     //api.github.com/repos/owner/repo/issues
```

Для того, чтобы отключить данный модуль, используйте `-DENABLE_GITHUB=0` флаг cmake.

Также данный модуль может отправлять запросы, используя токен разработчика. Пожалуйста, посетите [данную страницу](//github.com/settings/applications "Настройки") и сгенерируйте токен. Требуемые права для токена - **public_repo** (или **repo**, если Вы используете для приватных репозиториев).

**Имейте в виду, что передача токена в открытом виде может скомпрометировать его!**

Типичный POST запрос выглядит так:

```bash
curl -X POST -H "Authorization: token token" -d '{"title":"A new bug","body":"Some error occurs"}' \
     //api.github.com/repos/owner/repo/issues
```

Для того, чтобы включить данный модуль, используйте `-DOWN_GITHUB_TOKEN=STRING` флаг cmake.

Данный модуль требует наличия в системе `QtNetwork`.

### <a href="#gitreports" class="anchor" id="gitreports"><span class="octicon octicon-link"></span></a>Модуль GitReports

Данный модуль создает тикет, используя возможности [GitReports](//gitreports.com/about "GitReports"). Пожалуйста, посетите [данную страницу](//gitreports.com/ "GitReports") и настройте под Ваши репозитории.

Для того, чтобы отключить данный модуль, используйте `-DENABLE_GITREPORT=0` флаг cmake. Данный модуль требует наличия в системе `QtNetwork` и `QtWebKit`.

<!-- end of howto block -->

<!-- config block -->
## <a href="#config" class="anchor" id="config"><span class="octicon octicon-link"></span></a>Настройка

Для настройки перед компиляцией отредактируйте хидер `src/config.h`. Также Вы можете подгрузить параметры автоматически, используя массив `params` (необходимые ключи такие же, как и для хидера).

### <a href="#mainconfig" class="anchor" id="mainconfig"><span class="octicon octicon-link"></span></a>Основные настройки

* `OWNER` - владелец репозитория.
* `PROJECT` - имя проекта.
* `TAG_BODY` - тело тикета по умолчанию. Может быть использовано в обоих модулях.
* `TAG_TITLE` - имя тикета по умолчанию. Может быть использовано только в модуле GitHub.
* `TAG_ASSIGNEE` - прикрепить тикет к данному аккаунту. Может быть использовано только в модуле GitHub. Данный тег будет работать, только если пользователь имеет права на запись. Если будет пустым, будет проигнорировано.
* `TAG_LABELS` - установить данные метки тикету. Метки должны быть разделены запятыми. Может быть использовано только в модуле GitHub. Данный тег будет работать, только если пользователь имеет права на запись. Если будет пустым, будет проигнорировано.
* `TAG_MILESTONE` - установить данную веху тикету. Может быть использовано только в модуле GitHub. Данный тег будет работать, только если пользователь имеет права на запись. Если будет пустым, будет проигнорировано.

### <a href="#githubconfig" class="anchor" id="githubconfig"><span class="octicon octicon-link"></span></a>Настройки модуля GitHub

* `GITHUB_COMBOBOX` - текст модуля в ComboBox.
* `ISSUES_URL` - URL, в большинстве случаев, не редактируйте его. По умолчанию `//api.github.com/repos/$OWNER/$PROJECT/issues`. Доступные теги `$PROJECT`, `$OWNER`.

### <a href="#gitreportsconfig" class="anchor" id="gitreportsconfig"><span class="octicon octicon-link"></span></a>Настройки модуля GitReports

* `CAPTCHA_URL` - URL капчи, в большинстве случаев, не редактируйте его. По умолчанию `//gitreports.com/simple_captcha?code=`.
* `GITREPORT_COMBOBOX` - текст модуля в ComboBox.
* `PUBLIC_URL` - URL, в большинстве случаев, не редактируйте его. По умолчанию `//gitreports.com/issue/$OWNER/$PROJECT`. Доступные теги `$PROJECT`, `$OWNER`.

<!-- end of config block -->

<!-- gui block -->
<!-- end of gui block -->

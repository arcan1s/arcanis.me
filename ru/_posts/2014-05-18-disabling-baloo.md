---
category: ru
type: paper
hastr: true
layout: paper
tags: linux, archlinux, сборка
title: Отключение baloo, gentoo-way
short: disabling-baloo
---
Пока ононимные онолитеги ЛОР'а ноют на тему baloo, я предпочел потратить 15 минут на то, чтобы отвязать приложения от этого чуда человеческой мысли.

<!--more-->

## <a href="#disclaimer" class="anchor" id="disclaimer"><span class="octicon octicon-link"></span></a>Дисклеймер

Сам этим я не пользуюсь, поскольку предпочитаю менее деструктивные методы. Однако, судя по всему, все работает без проблем, поскольку жалоб нет. Так как патч делался действительно за несколько минут, то он просто выкорчевывает все вызовы baloo из исходников (возможно, когда-нибудь я сделаю нормальный патч).

С другой стороны, я настоятельно рекомендую людям, которым по каким-либо причинам baloo не нужен, отключить его из меню настроек (добавили пункт в 4.13.1), либо воспользоваться этой [статьей](//blog.andreascarpino.it/disabling-baloo-the-arch-way/ "Блог Скарпино").

## <a href="#intro" class="anchor" id="intro"><span class="octicon octicon-link"></span></a>Введение

В Archlinux, на текущий момент (2014-05-18) от baloo, помимо **baloo-widgets**, зависит **gwenview** и **kdepim**. В версии 4.13.0, почему то, **kactivities** тоже зависел от baloo, однако, эта зависимость не требовалась явно (таким образом, достаточно было просто пересобрать его, удалив baloo из списка зависимостей).

## <a href="#gwenview" class="anchor" id="gwenview"><span class="octicon octicon-link"></span></a>gwenview

Тут все довольно просто. Разработчики сами позаботились за нас о возможных пожеланиях простых пользователей и добавили специальный флаг:

```cmake
//Semantic info backend for Gwenview (Baloo/Fake/None)
GWENVIEW_SEMANTICINFO_BACKEND:STRING=Baloo
```
Таким образом, в сценарий сборки к cmake добавляем нужный флаг:

```bash
cmake ../gwenview-${pkgver} \
    -DCMAKE_BUILD_TYPE=Release \
    -DKDE4_BUILD_TESTS=OFF \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DGWENVIEW_SEMANTICINFO_BACKEND=None
```

## <a href="#kdepim" class="anchor" id="kdepim"><span class="octicon octicon-link"></span></a>kdepim

Так как делалось все на скорую руку, то я предпочел пробежаться по исходникам с помощью grep и найти все упоминания baloo. Нужные строки (а это указания на baloo в файлах CMakeLists.txt, вызовы функций из его библиотек, объявления заголовочных файлов) просто закомментировал (в исходном коде местами пришлось добавить фейковые вызовы). Патч полностью здесь приводить не буду (он, к тому же, немного большой), а дам [ссылку на него](//gist.github.com/arcan1s/b698bb586faef627b3bb "Gist") (4.13.3). Далее просто требуется применить этот патч к исходникам и пересобрать kdepim.

## <a href="#packages" class="anchor" id="packages"><span class="octicon octicon-link"></span></a>Пакеты

Все пакеты для Archlinux для обеих архитектур доступны [в моем репозитории](//wiki.archlinux.org/index.php/Unofficial_user_repositories#arcanisrepo "ArchWiki").

---
category: ru
type: paper
hastr: true
layout: paper
tags: настройка, linux, archlinux
title: Приложения, которые я использую
short: my-desktop
---
Небольшая статья, посвященная набору приложений и расширений, которые я
использую в повседневной жизни на моем домашнем компьютере.

<!--more-->

## <a href="#apps" class="anchor" id="apps"><span class="octicon octicon-link"></span></a>Приложения

* **Shell** - zshrc без вариантов. Некоторое описание моих настроек шелла могут
быть найдены [тут](/ru/2014/01/14/about-zshrc/ "Статья о zshrc"). Сами настройки
хранятся [тут](//raw.githubusercontent.com/arcan1s/dotfiles/master/zshrc "Файл")
или [тут](//raw.githubusercontent.com/arcan1s/dotfiles/master/zshrc_server
"Файл").
* **DE** - KDE со всеми вытекающими (поэтому набор приложений, преимущественно,
Qt-based). Некоторые подробности приготовления KDE будут даны ниже.
* **Работа с изображениями** -
[gwenview](//kde.org/applications/graphics/gwenview/ "Домашняя страница
Gwenview") для просмотра и быстрого редактирования,
[kolourpaint](//kde.org/applications/graphics/kolourpaint/ "Домашняя страница
Kolourpaint") для простого редактирования стандартных форматов пиксельной
графики, [gimp](//www.gimp.org/ "Домашняя страница Gimp") (без плагинов, ибо не
было необходимости) для более сурового редактирования и
[inkskape](//www.inkscape.org/ "Домашняя страница Inkskape") для работы с
векторной графикой.
* **Браузер** - Firefox, ранее Qupzilla, еще ранее Opera. Некоторый набор
настроек Firefox будет дан ниже. Дополнительный браузер - Chromium. Консольный -
elinks.
* **IM клиент** - [qutIM](//qutim.org "Домашняя страница Qutim").
Кроссплатформенный, мультипротокольный, с необходимым набором фич.
[Kopete](//kde.org/applications/internet/kopete/ "Домашняя страница Kopete"),
который использовался ранее, часто падал, работал как хотел и вообще не дружил с
кодировкой. Раньше еще был какой то консольный, но сейчас его нет. Для таких
случаев предпочитаю использовать клиент с планшета. Skype для скайпа, очевидно.
* **Почтовый клиент** - [kmail](//kde.org/applications/internet/kmail/ "Домашняя
страница Kmail"). Много фич, большая часть из которых мною используется,
симпатично выглядит и удобный. Еще бы был DE-независимый, цены бы ему не было.
* **IRC клиент** - [konversation](//konversation.kde.org/ "Домашняя страница
Konversation"). Самый обычный IRC-клиент. Хотя, если мне не изменяет память,
qutIM тоже поддерживает IRC протокол, лично мне удобнее использовать отдельный
клиент для этого.
* **Torrent клиент** - [transmission](//www.transmissionbt.com/ "Домашняя
страница Transmission") с Qt5 интерфейсом (gtk тоже имеется). Для сервера он же,
но без GUI.
* **Видео плеер** - [mpv](//mpv.io/ "Домашняя страница mpv"). Mplayer умер, а
mplayer2 родился мертворожденным. Ах да, графические надстройки сверху ненужны.
* **Аудио плеер** - [qmmp](//qmmp.ylsoftware.com/ "Домашняя страница Qmmp").
Хороший, годный плеер с закосом под winamp. Легким движением руки делаем ему
человеческий интерфейс aka simpleui.
* **Работа с аудио/видео** -
[kdenlive](//kde-apps.org/content/show.php?content=29024 "Домашняя страница
kdenlive") для работы с видео,
[soundkonverter](//kde-apps.org/content/show.php?content=29024 "Домашняя
страница Soundkonverter") для работы с аудио,
[easytag](//wiki.gnome.org/Apps/EasyTAG "Домашняя страница Easytag") для работы
с аудио тегами (gtk, но зато единственный, чья функциональность меня устроила).
Ну и командная строка и небольшие скрипты на bash.
* **Офис** - [Kingsoft Office](//wps-community.org/ "Домашняя страница KO") в
качестве замены Microsoft Office; в общем то ничем не примечательный, разве что
не так ущербно смотрится, как стандартные офисы, Qt-based и, говорят, с хорошей
поддержкой стандартных форматов. Версия под линукс находится в состоянии альфы.
[Kile](//kile.sourceforge.net/ "Домашняя страница Kile") в качестве фронтенда к
LaTeX. [Okular](//kde.org/applications/graphics/okular/ "Домашняя страница
Okular"), как просмотрщик всего. [GoldenDict](//goldendict.org/ "Домашняя
страница GoldenDict") в качестве словаря.
* **Редакторы** - [kwrite](//www.kde.org/applications/utilities/kwrite/
"Домашняя страница Kwrite") в качестве легковесного редактора,
[kate](//www.kde.org/applications/utilities/kate/ "Домашняя страница Kate") (с
плагином [cpp-helper](//zaufi.github.io/kate-cpp-helper-plugin.html "Домашняя
страница плагина")) для более суровых вещей. Ну и, конечно, vim для консоли.
* **Научный софт**. Визуализаторы химические -
[vmd](//www.ks.uiuc.edu/Research/vmd/ "Домашняя страница VMD"),
[chimera](//www.cgl.ucsf.edu/chimera/ "Домашняя страница Chimera") и
[pymol](//pymol.org/ "Домашняя страница Pymol"). Физический симулятор
[step](//kde.org/applications/education/step/ "Домашняя страница Step").
Калькулятор [kalgebra](//kde.org/applications/education/kalgebra/ "Домашняя
страница Kalgebra") и консольный [ipython](//ipython.org/ "Домашняя страница
ipython"). Рисовалка графиков и анализ [qtiplot](//qtiplot.com/ "Домашняя
страница Qtiplot") (его форк scidavis, к сожалению, полумертв), только рисовалка -
[grace](//plasma-gate.weizmann.ac.il/Grace/ "Домашняя страница Grace").
[Chemtool](//ruby.chemie.uni-freiburg.de/~martin/chemtool/chemtool.html
"Домашняя страница Chemtool") в качестве замены ChemDraw.
* **Системное**. Файловый менеджер
[dolphin](//kde.org/applications/system/dolphin/ "Домашняя страница Dolphin"),
[doublecmd](//doublecmd.sourceforge.net/ "Домашняя страница Doublecmd") как
двухпанельный менеджер. Эмуляторы терминала - [yakuake](//yakuake.kde.org/
"Домашняя страница Yakuake") и
[urxvt](//software.schmorp.de/pkg/rxvt-unicode.html "Домашняя страница urxvt") в
качестве оконного. Графический интерфейс для архиваторов
[ark](//kde.org/applications/utilities/ark/ "Домашняя страница Ark").

## <a href="#kde" class="anchor" id="kde"><span class="octicon octicon-link"></span></a>Настройка KDE

<div class="thumbnails">
  {% assign scrdesc = "Нотариально заверенный скриншот" %}
  {% assign scrname = "kde" %}
  {% include prj_scr.html %}
</div>

В качестве стиля Qt используется QtCurve, настройки могут быть найдены
[здесь](//github.com/arcan1s/dotfiles/tree/master/qtcurve "GitHub"), оформление
окон оттуда же. Курсор
[ecliz-small](//kde-look.org/content/show.php/Ecliz?content=110340 "Домашняя
страница курсоров"). Тема плазмы
[volatile](//kde-look.org/content/show.php/Volatile?content=128110 "Домашняя
страница темы"). Значки [compass](//nitrux.in/ "Домашняя страница Nitrux").
Шрифты на базе Liberation.

**Используемые виджеты** (слева направо, сверху вниз):
[menubar](//launchpad.net/plasma-widget-menubar "Домашняя страница виджета"),
[homerun](//userbase.kde.org/Homerun "Домашняя страница виджета") с прозрачной
иконкой, [icontask](//kde-apps.org/content/show.php?content=144808 "Домашняя
страница виджета"), [netctl](/ru/projects/netctl-gui/ "Домашняя страница
виджета"), стандартный трей от KDE, [colibri](//agateau.com/projects/colibri/
"Домашняя страница виджета") в качестве уведомлений, [Awesome
Widgets](/ru/projects/awesome-widgets "Домашняя страница виджета").

В качестве бонусного материала - яркие цвета в консоли (для
[konsole](//github.com/arcan1s/dotfiles/blob/master/themes/yakuake/My%20color.colorscheme
"GitHub")):

<div class="thumbnails">
  {% assign scrdesc = "Как оно выглядит" %}
  {% assign scrname = "zshrc_demo" %}
  {% include prj_scr.html %}
</div>

## <a href="#firefox" class="anchor" id="firefox"><span class="octicon octicon-link"></span></a>Настройка Firefox

В самих настройках ничего интересного нет, я просто напишу список аддонов. Дико
радует, что для того, чтобы интерфейс был минималистичным (и удобным), нужно
поставить кучу плагинов.

* **Adblock plus** - куда же без него.
* **Add to search bar** - для кастомных поисков.
* **Auto Refresh** - автоматическое обновление страниц.
* **Clone tab** - добавляет функцию "Дублировать вкладку".
* **Close tab by double click** - понятно, короче.
* **New scrollbars** используется для кастомизации скроллбаров, потому что
оригинальные смотрятся ущербно в Qt окружении.
* **NoScript** используется, например, для I2P и Tor.
* **PrivateTab** - добавляет приватную вкладку (а не окно).
* **Proxy Selector** добавляет возможность использовать несколько
прокси-серверов.
* **QuickJava** используется примерно с той же целью, что и NoScript.
* **RSS иконка в строке адреса** - очевидно.
* **Словари для проверки орфографии** (eng/rus).
* **Space Next** - на нажатие на пробел внизу страницы реагирует, как на нажатие
кнопки "Далее".
* **Speed Dial** - простая экспресс-панель.
* **Status-4-Evar** - нормальная строка состояния.
* **tab delabelifier** - сворачивает неиспользуемые вкладки.
* **Tab Scope + Tab Scope Tweaker** - всплывающая подсказка у вкладок.
* **accessKey** - пока не работает. Вообще служит для удобной навигации
(opera-like) с клавиатуры.
* **FXOpera** - нормальный минималистичный вид.

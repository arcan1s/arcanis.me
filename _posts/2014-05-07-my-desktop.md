---
category: en
type: paper
layout: paper
hastr: true
tags: настройка, linux, archlinux
title: Apps which I use
short: my-desktop
---
Here is a short paper devoted to the set of applications and extensions that I use everyday on my home computer.

<!--more-->

## <a href="#apps" class="anchor" id="apps"><span class="octicon octicon-link"></span></a>Applications

* **Shell** is zshrc and nothing else. You may find a small description of my settings [here](/en/2014/01/14/about-zshrc/ "About zshrc paper"). It is stored [here](//raw.githubusercontent.com/arcan1s/dotfiles/master/zshrc "File") (or [here](//raw.githubusercontent.com/arcan1s/dotfiles/master/zshrc_server "File")).
* **DE** - I use KDE as Desktop Environment. And that's why most of apps are qt-based. Some KDE settings are below.
* **Graphic editors** - [gwenview](//kde.org/applications/graphics/gwenview/ "Gwenview Homepage") is used for viewing images, [kolourpaint](//kde.org/applications/graphics/kolourpaint/ "Kolourpaint Homepage") is used for simple editing pixel images, [gimp](//www.gimp.org/ "Gimp Homepage") (without plugins, since they are not needed for me) - for editing and [inkskape](//www.inkscape.org/ "Inkskape Homepage") is used as editor of vector graphics.
* **Browser** - I use Firefox. Some Firefox settings are below. Chromium is used as additional browser, elinks is used as console browser.
* **IM client** is [qutIM](//qutim.org "Qutim Homepage"). It is a cross-platform, multiprotocol and full featured client. [Kopete](//kde.org/applications/internet/kopete/ "Kopete Homepage"), which I used before it, crashes, does not work correctly and does not work normally with codepage. Also I don't use a console client since I use a tablet IM. And I use Skype for skype obviously.
* **Mail client** is [kmail](//kde.org/applications/internet/kmail/ "Kmail Homepage"). It is a full featured client (and I use most of them), looks pretty and it is easy to use. If it will be DE-undepended it will be better.
* **IRC client** is [konversation](//konversation.kde.org/ "Konversation Homepage"). It is a simple IRC client. Though as far as I remember qutIM also supports IRC protocol, I prefre to use a special IRC client.
* **Torrent client** is [transmission](//www.transmissionbt.com/ "Transmission Homepage") with Qt5 interface (it has gtk interface too). It is also used for server but without GUI.
* **Video player** is [mpv](//mpv.io/ "Mpv Homepage"), since mplayer died and mplayer2 was born deadborn. Graphical frontend are not needed.
* **Audio player** is [qmmp](//qmmp.ylsoftware.com/ "Qmmp Homepage"). It is a good winamp-like player. Flick of the wrist you may make a handy interface for it (simpleui).
* **Audio/video editors**: [kdenlive](//kde-apps.org/content/show.php?content=29024 "Kdenlive Homepage") is used as video editor, [soundkonverter](//kde-apps.org/content/show.php?content=29024) is used as audio editor, [easytag](//wiki.gnome.org/Apps/EasyTAG "Easytag Homepage") is used for editing audio tags (unfortunately, it is a gtk-based, but I didn't find a better tool for it). And command line and scripts written on bash are used too.
* **Office**: [Kingsoft Office](//wps-community.org/ "KO Homepage") is used as alternative of Microsoft Office; it has no any feature, but it looks normally, it is qt-based and it is said that it has a good support for standart formats. (Linux version has an alfa stage.) [Kile](//kile.sourceforge.net/ "Kile Homepage") is used as LaTeX frontend. [Okular](//kde.org/applications/graphics/okular/ "Okular Homepage") is used as document viewer. And I use [GoldenDict](//goldendict.org/ "GoldenDict Homepage") as dictionary.
* **Editors**: [kwrite](//www.kde.org/applications/utilities/kwrite/ "Kwrite Homepage") is used as a simple text editor, [kate](//www.kde.org/applications/utilities/kate/ "Kate Homepage") (and [cpp-helper](//zaufi.github.io/kate-cpp-helper-plugin.html "Plugin Homepage") plugin) is used as advanced text editor. And of course I use vim in console.
* **Scientific soft**. Chemical visualizers are [vmd](//www.ks.uiuc.edu/Research/vmd/ "VMD Homepage"), [chimera](//www.cgl.ucsf.edu/chimera/ "Chimera Homepage") and [pymol](//pymol.org/ "Pymol Homepage"). Physics simulator is [step](//kde.org/applications/education/step/ "Step Homepage"). Calculator is [kalgebra](//kde.org/applications/education/kalgebra/ "Kalgebra Homepage") and console [ipython](//ipython.org/ "ipython Homepage"). [Qtiplot](//qtiplot.com/ "Qtiplot Homepage") is used for drawing graphs and data analysis (scidavis, which is its fork, unfortunately, is half-dead), [grace](//plasma-gate.weizmann.ac.il/Grace/ "Grace Homepage") is used for only drawing graphs. [Chemtool](//ruby.chemie.uni-freiburg.de/~martin/chemtool/chemtool.html "Chemtool Homepage") is used as alternative of ChemDraw.
* **System applications**. File manager is [dolphin](//kde.org/applications/system/dolphin/ "Dolphin Homepage"), [doublecmd](//doublecmd.sourceforge.net/ "Doublecmd Homepage") is used as twin-panel manager. Terminal emulators are [yakuake](//yakuake.kde.org/ "Yakuake Homepage") and [urxvt](//software.schmorp.de/pkg/rxvt-unicode.html "Urxvt Homepage") (as windowed emulator). Archiver graphical interface is [ark](//kde.org/applications/utilities/ark/ "Ark Homepage").

## <a href="#kde" class="anchor" id="kde"><span class="octicon octicon-link"></span></a>KDE settings

<div class="thumbnails">
  {% assign scrdesc = "KDE screenshot" %}
  {% assign scrname = "kde" %}
  {% include prj_scr.html %}
</div>

QtCurve is used as Qt style, its settings may be found [here](//github.com/arcan1s/dotfiles/tree/master/qtcurve "GitHub"), window decorations are presented by QtCurve too. Cursor theme is [ecliz-small](//kde-look.org/content/show.php/Ecliz?content=110340 "Cursor Homepage"). Plasma theme is [volatile](//kde-look.org/content/show.php/Volatile?content=128110 "Theme Homepage"). Icon pack is [compass](//nitrux.in/ "Nitrux Homepage"). I use fonts which are based on Liberation.

**Used widgets** (from left to right, top to bottom) are: [menubar](//launchpad.net/plasma-widget-menubar "Widget Homepage"), [homerun](//userbase.kde.org/Homerun "Widget Homepage") with transparent icon, [icontask](//kde-apps.org/content/show.php?content=144808 "Widget Homepage"), [netctl](/projects/netctl-gui/ "Widget Homepage"), default KDE tray, [colibri](//agateau.com/projects/colibri/ "Widget Homepage") for notifications, [Awesome Widgets](/projects/awesome-widgets "Widget Homepage").

As a bonus material [here](//github.com/arcan1s/dotfiles/blob/master/themes/yakuake/My%20color.colorscheme "GitHub") is a settings for konsole bright colors.

<div class="thumbnails">
  {% assign scrdesc = "Zsh demonstation" %}
  {% assign scrname = "zshrc_demo" %}
  {% include prj_scr.html %}
</div>

## <a href="#firefox" class="anchor" id="firefox"><span class="octicon octicon-link"></span></a>Firefox settings

I do not use a special settings, thus I get you a list of used add-ons:

* **Adblock plus**.
* **Add to search bar** is used for custom searchs.
* **Auto Refresh** is used for auto update pages.
* **Clone tab** adds "Clone tab" function.
* **Close tab by double click**.
* **New scrollbars** is used for customizing scrollbars, because original ones look horrible in Qt environment.
* **NoScript** is used for I2P and Tor, for example.
* **PrivateTab** adds private tab (not the window).
* **Proxy Selector** adds an ability to use multiple proxies.
* **QuickJava** is used with the same goal as NoScript.
* **RSS icon in url bar**.
* **Dictionaries for spellchecking** (eng/rus).
* **Space Next**. If I tap a space at the bottom of a page, it will be perceived as pushing the "Next" button.
* **Speed Dial** is a simple express panel.
* **Status-4-Evar** is a normal status bar.
* **tab delabelifier** minimizes inactive tabs.
* **Tab Scope + Tab Scope Tweaker** is tab tooltip.
* **accessKey** does not work now. But it is needed for easy navigation from keyboard (opera-like).
* **FXOpera** is a normal minimalistic appearance.


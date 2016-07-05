---
category: en
type: paper
hastr: true
layout: paper
tags: awesome-widgets, pytextmonitor
title: Awesome Widgets - bells and whistles
short: aw-v21-bells-and-whistles
---
The paper deals with settings of a custom scripts and graphical bars in the new
version of Awesome Widgets (2.1).

<!--more-->

## <a href="#intro" class="anchor" id="intro"><span class="octicon octicon-link"></span></a>Introduction

For a start it is highly recommended copy file `$HOME/.kde4/share/config/extsysmon.conf`
after widget update before you open widget settings, because old and new script
settings are incompatible. Also I should note that these features can be
configured from graphical interface, but I will describe how it can be done by
simply editing the desktop file.

## <a href="#general" class="anchor" id="general"><span class="octicon octicon-link"></span></a>General

Items are stored in the two directories: `/usr/share/awesomewidgets/%TYPE%/` and
`$HOME/.local/share/awesomewidgets/%TYPE%/` (path may be differ in depend from
your distro). Settings in the home directory have a higher priority that global
ones. Common settings for all extensions are:

| Field              | Required | Value                            | Default    |
| -------------------|----------|----------------------------------|------------|
| Name               | yes      | name                             | none       |
| Comment            | no       | comment                          | empty      |
| X-AW-Active        | no       | whether or not the item is active, N/A for bars | true |
| X-AW-ApiVersion    | yes      | API compatibility index. Do not touch it if you don't know what does it mean | 0 |
| X-AW-Interval      | no       | update interval in standard widget intervals, N/A for bars | 1 |
| X-AW-Number        | yes      | unique number which will be associated with the item | random number which is less than 1000 |


## <a href="#bars" class="anchor" id="bars"><span class="octicon octicon-link"></span></a>Bars

Directory is `desktops`, configuration files have the following fields:

| Field              | Required | Value                            | Default    |
| -------------------|----------|----------------------------------|------------|
| X-AW-Custom        | yes      | does value have custom formula or not, boolean | false |
| X-AW-Value         | yes      | bar value. The following tags are available `cpu*`, `gpu`, `mem`, `swap`, `hdd*`, `bat`. In case of `X-AW-Custom=true` it uses syntax similar to [lambda functions](/projects/awesome-widgets/#lambda) | cpu |
| X-AW-Max           | no       | maximal possible value, float    | 100.0      |
| X-AW-Min           | no       | minimal possible value, float    | 0.0        |
| X-AW-Count         | no       | points count, used in case of `X-AW-Type=Graph` | 100 |
| X-AW-ActiveColor   | yes      | active part fill in format `R,G,B,A` or full path to image | 0,0,0,130 |
| X-AW-InactiveColor | yes      | inactive part fill in format `R,G,B,A` or full path to image | 255,255,255,130 |
| X-AW-Type          | yes      | bar type. The following types are supported `Horizontal`, `Vertical`, `Circle`, `Graph` | Horizontal |
| X-AW-Direction     | yes      | the fill direction. The following variants are supported `LeftToRight`, `RightToLeft` | LeftToRight |
| X-AW-Height        | yes      | height, pixels                   | 100        |
| X-AW-Width         | yes      | width, pixels                    | 100        |

## <a href="#quotes" class="anchor" id="quotes"><span class="octicon octicon-link"></span></a>Quotes

To provide these data to users [Yahoo! Finance](//finance.yahoo.com/ "Yahoo!
Finance site") API iis used. Directory is `quotes`, configuration files have the
following fields:

| Field | Required | Value | Default |
|-------|----------|-------|---------|
| X-AW-Ticker | yes | ticker from Yahoo! Finance system | EURUSD=X |

## <a href="#scripts" class="anchor" id="scripts"><span class="octicon octicon-link"></span></a>Scripts

Directory is `scripts`, configuration files have the following fields:

| Field | Required | Value | Default |
|-------|----------|-------|---------|
| Exec | yes | path to executable file | /usr/bin/true |
| X-AW-Prefix | no | prefix to executable file. Usually it's not required, but in other you may want to specify interpreter for example |
| X-AW-Redirect | no | stream redirection. The following variants are available `stderr2stdout`, `nothing`, `stdout2stderr`, `swap`. stderr will be available, if you run application in the debug mode | nothing |
| X-AW-Filters | no | comma separated filters from `awesomewidgets-extscripts-filters.json` |

## <a href="#upgrade" class="anchor" id="upgrade"><span class="octicon octicon-link"></span></a>Upgrade

Directory is `upgrade`, configuration files have the following fields:

| Field | Required | Value | Default |
|-------|----------|-------|---------|
| Exec | yes | path to executable file | /usr/bin/true |
| X-AW-Filter | no | the regexp which will be applied to command output. If set `X-AW-Null` will be ignored |
| X-AW-Null | no | the number of lines which should be skipped in output | 0 |

## <a href="#weather" class="anchor" id="weather"><span class="octicon octicon-link"></span></a>Weather

The weather uses data and API from [Yahoo! Weather](//weather.yahoo.com
"Yahoo! Weather site"). Directory is `weather`, configuration files have the
following fields:

| Field | Required | Value | Default |
|-------|----------|-------|---------|
| X-AW-City | yes | city | London |
| X-AW-Country | yes | two-letter country code | uk |
| X-AW-Image | no | use images as weather icon or text. | false |
| X-AW-Provider | yes | weather provider. Supported ones are `Yahoo`, `OWM` | OWM |
| X-AW-TS | yes | time to which weather should be. `0` is a current weather, `1` - weather 3 hours later, etc | 0 |

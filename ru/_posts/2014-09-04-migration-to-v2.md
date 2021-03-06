---
category: ru
type: paper
hastr: true
layout: paper
tags: awesome-widgets, pytextmonitor
title: Миграция Awesome Widgets (ex-PyTextMonitor) на версию 2.0
short: migration-to-v2
---
<figure class="img">![broken-computer](/resources/papers/broken-computer.jpg)</figure> В
версии 2.0 произошел ряд значительных изменений (совершенно случайно, само по
себе, я тут не при чем) и было полностью переписано пользовательское API. Данная
статья призвана облегчить миграцию со старых версий PyTextMonitor (<1.11.0) на
новую (>2.0).

<!--more-->

## <a href="#new" class="anchor" id="new"><span class="octicon octicon-link"></span></a>Новые фичи

Во-первых, это ряд нововведений, среди которых:

* Новый виджет - **Desktop panel**. Показывает список рабочих столов, выделяя
активный. Умеет переключаться между ними по клику. Также умеет скрывать выбранные
панели по хоткею.
* Новые теги - `hddfreemb`, `hddfreegb`, `memusedmb`, `memusedgb`, `memfreemb`,
`memfreegb`, `swapfreemb`, `swapfreegb`. А также новые теги, связанные с новыми
возможностями - `desktop`, `ndesktop`, `tdesktops`.
* Новый графический тултип - батарея. Двухцветный, в зависимости от статуса
адаптора питания.

## <a href="#changes" class="anchor" id="changes"><span class="octicon octicon-link"></span></a>Значительные изменения

Во-вторых, и это главное - произошел ряд изменений, из-за которых старые настройки
**не будут** более работать. Среди пользовательских следует выделить:

* Переписка основного виджета на `С++`, что вызвало переименование проекта в
**Awesome Widgets**, а главного виджета в **Awesome Widget**
* Настройки файлов батареи и адаптора питания **вынесены в DataEngine**.
* **Убраны поля**. Теперь виджет представляет собой монолитное поле. Текст
настраивается в специальном браузере.
* В связи с удалением отдельных полей, тултип теперь **настраивается отдельно**.
* Настройка выравнивания текста теперь может быть осуществлена только с
использованием HTML тегов.
* В связи с объединением полей, несколько тегов были переименованы:
    *   `custom` (время) -> `ctime`
    *   `custom` (время работы) -> `cuptime`
    *   `time` (плеер) -> `duration`

По любым проблемам, связанным с миграцией, не стесняйтесь оставлять здесь
комментарий.

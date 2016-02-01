---
category: en
type: paper
hastr: true
layout: paper
tags: awesome-widgets, pytextmonitor
title: Migration Awesome Widgets (ex-PyTextMonitor) to version 2.0
short: migration-to-v2
---
<figure class="img">![broken-computer](/resources/papers/broken-computer.jpg)</figure> Some
significant changes occur in the version 2.0 (really, I didn't do anything
which can break your desktop!) and user API was rewritten. This paper should
help to migrate from older PyTextMonitor versions (<1.11.0) to new one (>2.0).

<!--more-->

## <a href="#new" class="anchor" id="new"><span class="octicon octicon-link"></span></a>New features

Firstly, a series of new features, including:

* New widget - **Desktop panel**. It shows desktop list and select the active
one. It can switch to the selected desktop by mouse clicking. Also it may set
selected panels hidden.
* New tags - `hddfreemb`, `hddfreegb`, `memusedmb`, `memusedgb`, `memfreemb`,
`memfreegb`, `swapfreemb`, `swapfreegb`. And there are new tags related to new
features - `desktop`, `ndesktop`, `tdesktops`.
* New graphical tooltip - battery. It is twin colour (the colour depends on AC
status).

## <a href="#changes" class="anchor" id="changes"><span class="octicon octicon-link"></span></a>Significant changes

Secondly, there are some changes because of which the old settings **will not**
more work. They are:

* The main widget was rewritten to `С++`, so the project was renamed to
**Awesome Widgets**, and the main widget was done to **Awesome Widget**
* Configuration of battery and AC files **was moved to DataEngine**.
* **The labels was removed**. Now the widget is a single label. You may set up
text in the special browser.
* According to removal of the label, tooltip **should be configured
separately**.
* Align of text now can be configured only by using HTML tags.
* According to fields combining several tags were renamed:
    * `custom` (time) -> `ctime`
    * `custom` (uptime) -> `cuptime`
    * `time` (player) -> `duration`

On any issues related to the migration, feel free to leave a comment here.

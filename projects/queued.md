---
hastr: true
layout: project
title: queued
short: queued
tags: linux, shell, daemon, system
hasgui: false
hasdocs: false
developers:
    - Evgeniy Alekseev
license: GPLv3
links:
---
<!-- info block -->
## <a href="#info" class="anchor" id="info"><span class="octicon octicon-link"></span></a>Information

Daemon for starting jobs to queue of calculations. It was written as proof-of-concept.

```bash
$ queued --help
Simple daemon written on BASH for starting jobs to queue of calculations

Usage: queued [ -c /etc/queued.conf ] [ -v | --version ] [ -h | --help ]
Parametrs:
  -c               PATH     - path to configuration file. Default is '/etc/queued.conf'

  -v   --version            - show version and exit
  -h   --help               - show this help and exit
```

```bash
$ add_queued --help
add_queued [ -c /etc/queued.conf ] [ -p NUM ] [ -u USER ] [ -h | --help ] /path/to/script

Parameters:
    -c               PATH     - path to configuration file. Default is '/etc/queued.conf'
    -p               NUM      - job priority
    -u               USER     - username
    -h   --help               - show this help and exit
```

### <a href="#devel" class="anchor" id="devel"><span class="octicon octicon-link"></span></a>Developers and contributors

{% for devel in page.developers %}
* {{ devel }}{% endfor %}

### <a href="#license" class="anchor" id="license"><span class="octicon octicon-link"></span></a>License

* {{ page.license }}

<!-- end of info block -->

<!-- install block -->
## <a href="#install" class="anchor" id="install"><span class="octicon octicon-link"></span></a>Installation

### <a href="#instruction" class="anchor" id="instruction"><span class="octicon octicon-link"></span></a>Instruction

* Download an [archive](//github.com/arcan1s/queued/releases "GitHub") with latest version of source files.
* Extract it and install the application:

    ```bash
    ./install.sh "/path/to/root/"
    ```

    If you want install it to `/` you must run it as root, e.g.:

    ```bash
    sudo ./install.sh
    ```

    If no path is specified it will be installed to `/` by default.

### <a href="#dependencies" class="anchor" id="dependencies"><span class="octicon octicon-link"></span></a>Dependencies

I want note that all were tested on latest version of dependencies.

* Bash (including awk, grep, sed)
* systemd *(optional, service file)*

<!-- end of install block -->

<!-- howto block -->
## <a href="#howto" class="anchor" id="howto"><span class="octicon octicon-link"></span></a>How to use

If you want to start the daemon just run

```bash
systemctl start queued
```

If you want to enable daemon autoload run

```bash
systemctl enable queued
```

But you may change path to configuration file or change parameters. To do it just copy (recommended) the source configuration file to new path

```bash
cp /etc/queued.conf /path/to/new/queued.conf
```

and edit it. Then copy the source service file to `/etc`:

```bash
cp /usr/lib/systemd/system/queued.service /etc/systemd/system/queued-my-profile.service
```

Replace following string in the file:

```bash
ExecStart=/usr/bin/queued
```

to

```bash
ExecStart=/usr/bin/queued -c /path/to/new/queued.conf
```

### <a href="#adding" class="anchor" id="adding"><span class="octicon octicon-link"></span></a>Adding a job

1. Create shell script with the command (e.g. it have a name `script.sh`).
2. Create priority file (`script.sh.pr`) with the job priority if it is needed.
3. Create user file (`script.sh.user`) with the job username if it is needed.
4. Copy files to `$WORKDIR`.

Also you may use `add_queued`.

## <a href="#configuration" class="anchor" id="configuration"><span class="octicon octicon-link"></span></a>Configuration

All settings are stored in `/etc/queued.conf`. After edit them you must restart daemon

```bash
systemctl restart queued
```
<!-- end of howto block -->

<!-- config block -->
### <a href="#options" class="anchor" id="options"><span class="octicon octicon-link"></span></a>Options

|         |         |
|---------|---------|
| WORKDIR | Full path to directory with source jobs. Default is `/var/lib/queued/work`. This directory must contain source scripts `script-name`, a priority file (it is not necessary) `script-name.pr` and a file with username (it is not necessary too) `script-name.user`. |
| JOBDIR | Full path to directory with running jobs. Default is `/var/lib/queued/job`. All job files will be moved here. |
| QUEUEFILE | Full path to file with queue list. Default is `/var/lib/queued/queue`. |
| PRIORITY | Default priority. Default is `0`. The higher the value, the higher the priority of the task. |
| SLEEPTIME | Time interval in minutes. Default is `5`. |
| STARTASUSER | Default user. Default is `root`. This user will own created files. |
<!-- end of config block -->

<!-- gui block -->
<!-- end of gui block -->

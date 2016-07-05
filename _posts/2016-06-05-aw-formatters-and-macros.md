---
category: en
type: paper
hastr: true
layout: paper
tags: awesome-widgets, pytextmonitor
title: Awesome Widgets - Introducing custom formatters and macros
short: aw-formatters-and-macros
---
This paper describes new major features of Awesome Widgets 3.2.0.

<!--more-->

# <a href="#formatters" class="anchor" id="formatters"><span class="octicon octicon-link"></span></a>Formatters

Actually it has own graphical interface for configuration, but let me describe
how to configure it by using your editor.

To apply formatters use `$HOME/.local/share/awesomewidgets/formatters/formatters.ini`
file. It has only one section named `[Formatters]` in which keys are AW keys,
values are related formatter names. For example,

```ini
[Formatters]
cpu=myformatter
```

means that formatter `myformatter` will be used for key `cpu`.

All formatters are stored in the same directory, one formatter per file, files
should have `.desktop` extension. Each formatter has the following configuration
fields inside `[Desktop Entry]` section:

| Field              | Required | Value                            | Default    |
| -------------------|----------|----------------------------------|------------|
| Name               | yes      | formatter name                   | none       |
| Comment            | no       | comment                          | empty      |
| X-AW-Type          | no       | formatter type. The following types are supported: `NoFormat`, `DateTime`, `Float`, `List`, `Script` | NoFormat   |

Each formatter type has own behaviour and own settings and they are described
below. Also there are system-wide settings which are stored in `/usr/share/awesomewidgets/formatters/`, system formatters will be overwritten by
user defined ones, but formatter settings (i.e. `formatters.ini`) will be appended.

## <a href="#formatter-noformat" class="anchor" id="formatter-noformat"><span class="octicon octicon-link"></span></a>`NoFormat` formatter

Just puts value as string directly. It has no any special settings.

## <a href="#formatter-datetime" class="anchor" id="formatter-datetime"><span class="octicon octicon-link"></span></a>`DateTime` formatter

Converts `QDateTime` object to string.

| Field              | Required | Value                            | Default    |
| -------------------|----------|----------------------------------|------------|
| X-AW-Format        | yes      | Qt specific format string        | (empty)    |

Actually it is the same as `$ctime` tag and has the same configuration.

## <a href="#formatter-float" class="anchor" id="formatter-float"><span class="octicon octicon-link"></span></a>`Float` formatter

Converts any number to string.

| Field              | Required | Value                            | Default    |
| -------------------|----------|----------------------------------|------------|
| X-AW-FillChar      | no       | char to fill number to `X-AW-Width` | (space) |
| X-AW-Format        | no       | Qt specific number format, supported values are `e`, `E`, `f`, `g`, `G` | `f` |
| X-AW-Multiplier    | no       | float to which value will be multiplied | 1.0 |
| X-AW-Precision     | no       | show this count of symbols after dot | -1 (as expected) |
| X-AW-Summand       | no       | float to which value will be increased  | 0.0 |
| X-AW-Width         | no       | width of the field               | 0 (do not limit) |

Please note that actual formula is `X-AW-Multiplier * value + X-AW-Summand`.

## <a href="#formatter-list" class="anchor" id="formatter-list"><span class="octicon octicon-link"></span></a>`List` formatter

Coverts list of string objects to string.

| Field              | Required | Value                            | Default    |
| -------------------|----------|----------------------------------|------------|
| X-AW-Filter        | no       | filter by this regular expression | (empty)   |
| X-AW-Separator     | no       | use this separator to join strings | (empty   |
| X-AW-Sort          | no       | boolean, sort or not list        | false      |

## <a href="#formatter-script" class="anchor" id="formatter-script"><span class="octicon octicon-link"></span></a>`Script` formatter

Uses javascript code to convert value to string. Value will be passed as argument
to fuction.

| Field              | Required | Value                            | Default    |
| -------------------|----------|----------------------------------|------------|
| X-AW-AppendCode    | no       | prepend code by `(function(value) {` and append `})` | true |
| X-AW-Code          | no       | code for use                     |            |
| X-AW-HasReturn     | no       | if false will append your code by `return output;`. Only works if `X-AW-AppendCode` is `true` | false |

Actually for example to covert download speed to kibibits on the fly you may use
the following:

```ini
X-AW-AppendCode=true
X-AW-Code="output=value/8.0;"
X-AW-HasReturn=false
```

The code will be expanded to:

```javascript
(function(value) {
  output = value / 8.0;
  return output;
})
```

# <a href="#macros" class="anchor" id="macros"><span class="octicon octicon-link"></span></a>Macros

Another new feature is macros. User may define any own function by using the following
construction `$aw_macro<my_macro_name,some_arg,another_arg>{{macro body here with $some_arg}}`.

The first argument is macros name, which is required. Another ones describe arguments
which will be passed to the macro call. Macro body may have any text (including templates,
lambdas, etc) and arguments which are defined by using `$`.

To put defined macro to your code use the following construction:
`$aw_macro_my_macro_name<$cpu,$cpucl>{{}}` (body will be ignored here). In this
example macro will be expanded to `macro body here with $cpu`.

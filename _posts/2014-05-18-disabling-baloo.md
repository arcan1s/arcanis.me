---
category: en
type: paper
layout: paper
hastr: true
tags: linux, archlinux, building
title: Disabling baloo, gentoo-way
short: disabling-baloo
---
Paper, which describes how to remove the dependency on baloo in your system.

<!--more-->

## <a href="#disclaimer" class="anchor" id="disclaimer"><span class="octicon octicon-link"></span></a>Disclaimer

I do not use this patch, since I prefer less destructive methods. However,
apparently all works fine, because there is no any claims. Since this patch was
created in a few minutes, it removes all baloo's calls from source files (maybe
I'll create a normal patch sometime).

On other hand, I highly recommend to people, who do not use baloo for some
reason, disable it from the settings menu (it was added it 4.13.1) or read this
[article](//blog.andreascarpino.it/disabling-baloo-the-arch-way/ "Scarpino's
blog").

## <a href="#intro" class="anchor" id="intro"><span class="octicon octicon-link"></span></a>Introduction

In Archlinux **gwenview** and **kdepim** (and **baloo-widgets**) depend on baloo
currently (2014-05-18). In the version 4.13.0 **kactivities** depends on baloo
too (and I don't know why); but this dependency was not required explicitly, so
it was enough just to rebuild the package by removing baloo from the list of
dependencies.

## <a href="#gwenview" class="anchor" id="gwenview"><span class="octicon octicon-link"></span></a>gwenview

It's all quite simple. Developers have taken care of the wishes of ordinary
users and added a special flag:

```cmake
//Semantic info backend for Gwenview (Baloo/Fake/None)
GWENVIEW_SEMANTICINFO_BACKEND:STRING=Baloo
```

Thus, we add requred cmake flag to the build script:

```bash
cmake ../gwenview-${pkgver} \
    -DCMAKE_BUILD_TYPE=Release \
    -DKDE4_BUILD_TESTS=OFF \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DGWENVIEW_SEMANTICINFO_BACKEND=None
```

## <a href="#kdepim" class="anchor" id="kdepim"><span class="octicon octicon-link"></span></a>kdepim

Since everything was done in a hurry, I prefer to look at the source code using
grep and to find all references to baloo. Needed strings (they are links to
ballo in CMakeLists.txt, baloo's function calls and header declarations) were
commented (I added some fake calls to the source code). You may find the patch
[here](//gist.github.com/arcan1s/b698bb586faef627b3bb "Gist") (4.13.3). Download
the patch, apply it to the source code and recompile kdepim.

## <a href="#packages" class="anchor" id="packages"><span class="octicon octicon-link"></span></a>Packages

All Archlinux packages for both architectures may be found in [my
repository](//wiki.archlinux.org/index.php/Unofficial_user_repositories#arcanisrepo "ArchWiki").

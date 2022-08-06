---
title: "A guide for Qt5/WebKit2 development setup for Nokia N9 on Ubuntu Linux"
date: 2012-04-11
---

As part of my daily activities on QtWebKit maintenance and development for Nokia devices, it is interesting to keep track of the latest developments in QtWebKit. Among these, a promising project of a Qt5/WebKit2-based browser called [Snowshoe](https://github.com/snowshoe/snowshoe) mainly developed by my fellow friends from [INdT](http://www.indt.org/?lang=en) which is open-source.

This browser requires the latest Qt5 and QtWebKit binaries and thus requires us to have a functional build system environment. There is a guide available on the [WebKit's wiki](https://trac.webkit.org/wiki/SettingUpDevelopmentEnvironmentForN9) which is very helpful but lacks some information about compilation issues found when following the setup steps, so I am basing this guide from that wiki page.

On this guide it is assumed the following:

- All commands are issued on a Linux console. I am not aware of how this guide would work on other systems.
- All commands are supposed to be issued inside base directory, unless expressly said otherwise (ie. cd `<QT5_DIR>`).
- You might want to check if you have *git* and *rsync* packages installed in your system.

## 1. Install Qt SDK

In order to build Qt5 and QtWebKit for Nokia N9, you need to set up a
cross-compiler. Thankfully, Qt SDK already comes with a working setup.
Please download the *online installer* from [Qt Downloads](http://qt.nokia.com/downloads/) section.

> **NOTE**: The offline installer comes with an outdated version of the [MADDE](http://wiki.maemo.org/MADDE)
target, which can be updated by running the script below and choosing "Update components" when asked:

```shell
$ ~/QtSDK/SDKMaintenanceTool
```

## 2. Directory setup

It is suggested (and actually required by some build scripts) to have a
base directory which holds Qt5, Qt Components and WebKit project
sources. The suggested base directory can be created by running:

```shell
$ mkdir -p ~/swork
```

> **Note**: You can actually choose another directory name, but so far it is
required by some scripts to have at least a symbolic link pointing to
`<HOME_DIR>/swork` (notice the `s` prefix).

## 3. Download convenience scripts

### 3.1. browser-scripts

```shell
$ git clone https://github.com/resworb/scripts.git browser-scripts
```

### 3.2. rsync-scripts

```shell
$ wget http://trac.webkit.org/attachment/wiki/SettingUpDevelopmentEnvironmentForN9/rsync-scripts.tar.gz?format=raw
$ tar xzf rsync-scripts.tar.gz
```

## 4. Download required sources

### 4.1. testfonts

```shell
$ git clone git://gitorious.org/qtwebkit/testfonts.git
```

### 4.2. Qt5, QtComponents and WebKit

The script below when run successfully will create `~/swork/qt5`,
`~/swork/qtcomponents` and `~/swork/webkit` directories:

```shell
$ browser-scripts/clone-sources.sh --no-ssh
```

**NOTE:** You can also manually download sources, but remember to stick with the directory names described above.

## 5. Pre-build hacks

### 5.1. Qt5 translations

Qt5 translations are not being properly handled by cross-platform toolchain. This happens mainly because the `lrelease` application is called to generate Qt message files, but due to it being an [ARMEL](https://linuxhint.com/about-arm64-armel-armhf/) binary your system is probably not capable of running it natively (unless you have a `misc_runner` kernel module properly set, then you can safely skip this step). In this case, you can use `lrelease` from your system's Qt binaries without any worries.

If you have a Scratchbox environment set, it is suggested for you to
stop its service first:

```shell
$ sudo service scratchbox-core stop
```

Now you can manually generate Qt message files by running this:

```shell
$ cd ~/swork/qt5/qttranslations/translations
$ for file in `ls *ts`; do lrelease $file -qm `echo "$file" | sed 's/ts$/qm/'`; done
```

### 5.2. Disable jsondb-client tool

The `QtJsonDB` module from Qt5 contains a tool called `jsondb-client`, which
depends on `libedit` (not available on MADDE target). It is safe to disable its compilation for now:

```shell
$ sed -i 's/jsondb-client//' ~/swork/qt5/qtjsondb/tools/tools.pro
```

### 5.3. Create missing symbolic links

Unfortunately the Qt5 build system is not robust enough to support our cross-compilation environment, so some symbolic links are required on MADDE to avoid compilation errors (where `<USER>` is your system user name):

```shell
$ ln -s ~/swork/qt5/qtbase/include ~/QtSDK/Madde/sysroots/harmattan_sysroot_10.2011.34-1_slim/home/<USER>/swork/qt5/qtbase
$ ln -s ~/swork/qt5/qtbase/mkspecs ~/QtSDK/Madde/sysroots/harmattan_sysroot_10.2011.34-1_slim/home/<USER>/swork/qt5/mkspecs
```

## 6. Build sources

You can execute the script that will build all sources using
cross-compilation setup:

```shell
$ browser-scripts/build-sources.sh --cross-compile
```

If everything went well, you now have the most up-to-date binaries for
Qt5/WebKit2 development for Nokia N9. Please have a look at the WebKit's
wiki for more information about how to update sources after a previous
build and information on how to keep files in sync with device. The
guide assumes PR1.1 firmware for N9 device, which is already outdated,
so I might come up next with updated instructions on how to safely sync
files to your PR1.2-enabled device.

That's all for now, I appreciate your comments and feedback!

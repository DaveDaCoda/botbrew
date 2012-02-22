# BotBrew - *nix tools and package management for Android

This project compiles various *nix tools and makes Opkg packages suitable for installation on ARM devices running Android.

## Prebuilt

If you just want to use prebuilt binaries, get a shell on your rooted Android device and run:

    wget http://botbrew.inportb.com/opkg/install.sh -O- | su

Otherwise, keep reading to roll your own.

## Prerequisites

The following Debian packages are required for using BotBrew.

- bash
- make
- git-core
- mercurial
- subversion
- build-essential
- autoconf
- libtool
- libglib2.0-dev
- python
- python-yaml
- python-xcbgen 1.7
- ruby1.9.1
- xsltproc
- file
- fakeroot
- flex
- bison
- gettext
- intltool

In addition, the Android NDK (r7 recommended) is required.

## Cookbook

BotBrew knows how to make

- binutils
- gcc
- bzip2
- curl
- opkg
- python
- ruby
- sqlite3
- vim
- dropbear
- weechat
- ... and more (have a look at the cookbook)

Many projects do not generate packages at all; instead, they provide libraries that allow other projects to be built. These may eventually be packaged as well, to facilitate on-board software development.

## Configuration

Create a new file `config.sh` to define a couple of variables

- G_MAINTAINER=your name and &lt;email address&gt; in RFC822 format
- G_NDKPATH=absolute path to the NDK

`config.sh` is sourced by the `botbrew` script during execution.

## Usage

First, navigate to the project directory of interest, within the cookbook.

    ../../botbrew recipe		# just shows basic information
    ../../botbrew recipe build		# builds the project, satisfying dependencies in the process
    ../../botbrew recipe package	# packages the project, building it first if necessary
    ../../botbrew recipe clean		# removes object files
    ../../botbrew recipe distclean	# removes object files, makefiles, and imports
    ../../botbrew recipe clobber	# removes everything

BotBrew could also generate a makefile. First, navigate to BotBrew's top-most directory.

    ./botbrew makefile > Makefile	# generates the makefile
    make <project>			# builds <project>
    make package-<project>		# packages <project>
    make clean-<project>		# cleans <project>
    make distclean-<project>		# distcleans <project>
    make clobber-<project>		# clobbers <project>
    make build-all			# builds all projects
    make package-all			# packages all projects

With a makefile, it is possible to specify multiple targets at once and avoid manually changing directories. Since the makefile is basically an index of the BotBrew cookbook, it should be re-generated when projects are added to or removed from BotBrew. Makefile usage is optional.

## Packaging

BotBrew takes cues from Arch Linux. Just as Arch's PKGBUILD scripts direct the assembly of its packages, BotBrew's recipes (`cookbook/*/recipe`) control how its projects are built, cleaned, and packaged. Each method is handled by a shell function. In most cases, the maintainer only needs to override the build function. Defaults are used whenever there is no override. BotBrew provides functions that assist in downloading/unpacking/patching source code, and functions that apply various fixes and make cross-compilation easier. The Opkg packages themselves are controlled by YAML files (`cookbook/*/package/*`). Please have a look at some of the projects for packaging hints.

## agcc.bash

agcc.bash is a wrapper around the NDK that behaves as gcc when invoked as agcc.bash or g++ when invoked as agcc-bash-g++. In the future, this wrapper may be removed in favor of the standalone toolchain generated by the NDK.

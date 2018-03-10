XP-qt: Qt5 GUI for XP
===============================

Build instructions
===================

Debian
-------

First, make sure that the required packages for Qt5 development of your
distribution are installed, these are

for Debian and Ubuntu  <= 16.04 :

```bash
sudo add-apt-repository ppa:bitcoin/bitcoin -y
sudo apt-get update
sudo apt-get install git build-essential libssl-dev libdb4.8-dev libdb4.8++-dev \
    libboost-all-dev libqrencode-dev libminiupnpc-dev qt5-default qttools5-dev-tools
```

then execute the following:

```bash
qmake
make
```

Alternatively, install Qt Creator and open the `XP-qt.pro` file.

An executable named `XP-qt` will be built.


Windows
-------

for Windows 10 over.

setup follows software: 

- Visual Studio 2017 Community
- [chocolatey](https://chocolatey.org/)
- Qt 5.10.1 & msvc 2015 32bit


prepare XP-Qt dependency libraries:

```bat
MSVC\prepare.bat (execute by Admin permission.)
MSVC\boost.bat
MSVC\openssl.bat
MSVC\berkeleydb.bat
MSVC\qrencode.bat
```

build berkeley db by using MSVC\db-4.8.30.NC\build_windowsBerkeley_DB.sln by yourself.

`XP-Qt` build:

```bash
call "C:\Program Files (x86)\Microsoft Visual　Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat" x86
cd \path\to\XPCoin
C:\Qt\5.10.1\msvc2015\bin\qmake.exe -spec win32-msvc
jom
C:\Qt\5.10.1\msvc2015\bin\windeployqt --release release\XP-Qt.exe
```


Mac OS X
--------

- Execute the following commands in a terminal to get the dependencies:

```bash
brew install autoconf automake libtool boost openssl pkg-config protobuf qt qrencode berkeley-db@4
`brew --prefix qt`/bin/qmake
make
`brew --prefix qt`/bin/macdeployqt XP-Qt.app -dmg
```

Build configuration options
============================

Berkeley DB transaction index
LevelDB transaction index
--------------------------

To use LevelDB for transaction index, pass the following argument to qmake:

```bash
qmake "USE_LEVELDB=1"
```

No additional external dependencies are required. If you're running this on your current sources tree then don't forget to run

```bash
make distclean
```

prior to running qmake.

Assembler implementation of scrypt hashing
------------------------------------------

To use optimized scrypt implementation instead of generic scrypt module, pass the following argument to qmake:

```bash
qmake "USE_ASM=1"
```

If you're using clang compiler then you need to unroll macroses before compiling. Following commands will do this for you:

```bash
cd src/
../contrib/clang/nomacro.pl
```

No additional external dependencies required. Note that only x86, x86_64 and ARM processors are supported.

Notification support for recent (k)ubuntu versions
---------------------------------------------------

To see desktop notifications on (k)ubuntu versions starting from 10.04, enable usage of the
FreeDesktop notification interface through DBUS using the following qmake option:

```bash
qmake "USE_DBUS=1"
```

Generation of QR codes
-----------------------

libqrencode is used to generate QRCode images for payment requests.
It can be downloaded from http://fukuchi.org/works/qrencode/index.html.en, or installed via your package manager.

Berkely DB version warning
==========================

A warning for people using the *static binary* version of XP on a Linux/UNIX-ish system (tl;dr: **Berkely DB databases are not forward compatible**).

The static binary version of XP is linked against libdb5.3.

If the globally installed development package of Berkely DB installed on your system is 5.X, for example, any source you
build yourself will be linked against that. The first time you run with a 5.X version the database will be upgraded,
and 4.X cannot open the new format. This means that you cannot go back to the old statically linked version without
significant hassle!

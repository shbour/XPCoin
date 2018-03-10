Mac OS X Build Instructions and Notes
====================================
This guide will show you how to build XPd(headless client) for OSX.

Notes
-----

* Tested on OS X 10.7 through 10.10 on 64-bit Intel processors only.

* All of the commands should be executed in a Terminal application. The
built-in one is located in `/Applications/Utilities`.

Preparation
-----------

You need to install XCode with all the options checked so that the compiler
and everything is available in /usr not just /Developer. XCode should be
available on your OS X installation media, but if not, you can get the
current version from https://developer.apple.com/xcode/. If you install
Xcode 4.3 or later, you'll need to install its command line tools. This can
be done in `Xcode > Preferences > Downloads > Components` and generally must
be re-done or updated every time Xcode is updated.

There's also an assumption that you already have `git` installed. If
not, it's the path of least resistance to install [Github for Mac](https://mac.github.com/)
(OS X 10.7+) or
[Git for OS X](https://code.google.com/p/git-osx-installer/). It is also
available via Homebrew.

You will also need to install [Homebrew](http://brew.sh) in order to install library
dependencies.

The installation of the actual dependencies is covered in the Instructions
sections below.

Instructions: Homebrew
----------------------

#### Install dependencies using Homebrew

```bash
brew install autoconf automake libtool boost openssl pkg-config protobuf qt qrencode berkeley-db@4 cmake
```

### Building `XPd`

1. Clone the github tree to get the source code and go into the directory.

```bash
git clone https://github.com/eXperiencePoints/XPCoin.git
cd XPCoin
```

2. Build XPd:

```bash
cd src
mkdir build
cd build
cmake ..
cmake --build .
```

3. Build XP-Qt application: 

```bash
`brew --prefix qt`/bin/qmake
make
```

Use Qt Creator as IDE
------------------------
You can use Qt Creator as IDE, for debugging and for manipulating forms, etc.
Download Qt Creator from http://www.qt.io/download/. Download the "community edition" and only install Qt Creator (uncheck the rest during the installation process).

1. Make sure you installed everything through homebrew mentioned above 
2. In Qt Creator do "File" -> "Open Project"
3. Select XP-qt.pro as project file.
4. In the "Projects" tab select "Manage Kits..."
5. Select the default "Desktop" kit and select "Clang (x86 64bit in /usr/bin)" as compiler
6. Select LLDB as debugger (you might need to set the path to your installtion)
7. Start debugging with Qt Creator

Creating a release build
------------------------
You can ignore this section if you are building `XPd` for your own use.

XPd binary isn't included in the XP-Qt.app bundle.

If you are building `XPd` or `XP-Qt` for others, your build machine should be set up
as follows for maximum compatibility:

Once dependencies are compiled, you can create the .dmg disk image:

```bash
`brew --prefix qt`/bin/macdeployqt XP-Qt.app -dmg
```

Running
-------

It's now available at `./XPd`, provided that you are still in the `src`
directory. We have to first create the RPC configuration file, though.

Run `./XPd` to get the filename where it should be put, or just try these
commands:

```bash
echo -e "rpcuser=XPrpc\nrpcpassword=$(xxd -l 16 -p /dev/urandom)" > "/Users/${USER}/Library/Application Support/XP/XP.conf"
chmod 600 "/Users/${USER}/Library/Application Support/XP/XP.conf"
```

The next time you run it, it will start downloading the blockchain, but it won't
output anything while it's doing this. This process may take several hours;
you can monitor its process by looking at the debug.log file, like this:

```bash
tail -f $HOME/Library/Application\ Support/XP/debug.log
```

Other commands:
-------

```bash
./XPd -daemon # to start the bitcoin daemon.
./XPd --help  # for a list of command-line options.
./XPd help    # When the daemon is running, to get a list of RPC commands
```
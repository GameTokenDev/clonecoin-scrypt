![image](https://github.com/crazysoldier/clonecoin-scrypt/blob/master/src/qt/res/images/splash2.jpg)

CloneCoin (CCN) - a 'faster' version of Litecoin which also uses scrypt
as a proof of work scheme and is intended for microtransactions.
 - 60 seconds block targets: beat that MinCoin! ;)
 - 42 000 000 total coins
 - only 1 Block Reward/subsidy within the first 3 days and after approximately 5 years;
    in between: 4 coins per generated block
 - difficulty retargets every 0.35 days
 - currently peers are looked up over IRC only
 - currently no block checkpoints are in the code (but could be easily
   added)
Other than that, this coin is exactly like Litecoin and should by no
means be used as a real cryptocurrency. All of the coin parameters
are chosen arbitrarily or at most with 'fairness' towards everyone in mind.

So actually, this 'new' coin exists for the following reasons:
 - CCN proves that really anyone(!) can start a Litecoin/Bitcoin based currency
    (just look at the changes I applied to the original Litecoin source,
     for genesis block generation look at main.cpp)
 - allows me to experiment with coin parameters (in a private network)

 Credits go to the original authors/communities that created Bitcoin and Litecoin.
 
 Wallets:

Headless (no gui) :
- Linux : https://github.com/crazysoldier/clonecoin-scrypt/blob/master/src/clonecoin (open on linux with './clonecoin &' and watch './clonecoin getinfo' or enable mining './clonecoin setgenerate true 50' (number of threads).
- Mac
- Windows

Qt-Wallet (gui) :
- Linux : https://github.com/crazysoldier/clonecoin-scrypt/blob/master/clonecoin-qt
- Mac
- Windows

Building from Source:

Mac OSX -Qt

I'm starting with this one simply to go inline with dependencies order above. In order to keep things tidy on my iMac I created a virtual machine loaded with OSX 10.6.8, Snow Leopard. This was pretty straight forward using VMWare Fusion. After install and software updating, I installed XCode 3.2.6, which contains a working non-llvm version of gcc and its free from Apple here: http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792 A simple install, no frills, make sure all the objects are checked for installation.

Next, I installed MacPorts this version: https://distfiles.macports.org/MacPorts/MacPorts-2.1.3-10.6-SnowLeopard.pkg and then the dependencies listed in the first section ala:

$ sudo port install boost db48 qt4-mac openssl miniupnpc git

After a bit of time, all goodies are installed so we'll clone the coin's software in the regular fashion:

$ git clone https://github.com/crazysoldier/clonecoin-scrypt.git
$ cd clonecoin
Now, something a tad different this time, we need to run qmake instead of make. Do that like so:

$ qmake "USE_UPNP=-" clonecoin-qt.pro

Yes, you need the " " around USE_UPNP=- and yes, this may produce some strange looking results, something like this:

Project MESSAGE: Building without UPNP support
Removed plural forms as the target language has less forms.
If this sounds wrong, possibly the target language is not set or recognized.
Now, lets build it:

$ make -f Makefile

Go, go, go, do not look back. After a bit you'll see it finish and an icon should appear in the clonecoin folder:


Now launch that and voila! A Mac clonecoin wallet:


Just like the Windows and Linux wallets, you may want to add addnode=x.x.x.x where the x.x.x.x is the IP of your seed node. This won't be needed after a few clients begin connecting to the network, eventually they will begin talking to each other via IRC.

Linux -Qt

This is by a long shot the easiest wallet to compile, but its hindered by two things for distribution: Linux has very small market share, though for a personal or club coin, what the hell right? and Most Linux users will compile their own software so you'll not likely get far distributing a Linux executable (as well you shouldn't). My example here is based on Debian and should equate to most Debian/Ubuntu flavors.

Now, since we already built a system and installed the dependencies in the first bit--wait, you didn't? You did it all on Windows? Nice. You should write a guide next time! Now, where were we...oh yes, you already have a working coin building system, so lets just stick with it. First things first:

cd ~/clonecoin
clonecoin% qmake "USE_UPNP=-"
Thinking, thinking, output:

Project MESSAGE: Building without UPNP support 
Removed plural forms as the target language has less forms.
If this sounds wrong, possibly the target language is not set or recognized.
Now, build it:

$ make

Yeah, seriously, that's it. Just 'make.' Ha--Debian is so beautiful, is it not? Ok now after a bit of churning and burning it will finish.

Windows -Qt

This is the trickiest one to crack of the GUI wallets. I am going to detail how I got this to work and offer you an easy way to get the dependencies in an attempt to make this work for you too. That said, it may not--and I've already said I won't do tech support. So here's the deal. I got this to work and then duplicated it on a second machine to ensure it wasn't a fluke! Most of the information needed to compile the basic coind.exe or GUI wallet is in this thread: https://bitcointalk.org/index.php?topic=149479.0 Unfortunately nothing is as easy as it seems, and although the MinGW and QT installs went fine, I couldn't compile it without a few tweaks to the .pro file.


Begin by installing MinGW32 from here: https://sourceforge.net/downloads/mingw Go ahead and install the whole bloody thing if you like, but at least the "C Compiler", "C++ Compiler" and "MSYS Basic System" components. Everything else leave stock, next, next, next kind of thing.

Next, install ActivePerl 32 or 64 bit from here: http://www.activestate.com/activeperl/downloads Again, standard install, next, next, next and so forth.

Now open the "MinGW System Shell" from Start - Programs and you'll basically have a Linux prompt:


Now make a /c/deps folder to keep our files in:

$ mkdir /c/deps
$ cd /c/deps
Now download the following files and put them in C:\Deps:

OpenSSL: http://www.openssl.org/source/openssl-1.0.1e.tar.gz
Install it like so:

$ tar xvfz openssl-1.0.1e.tar.gz
$ cd openssl-1.0.1e
$ ./config
$ make
Berkeley DB 4.8: http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz
Install it like so:

$ tar xvfz db-4.8.30.NC.tar.gz
$ cd db-4.8.30.NC/build_unix
$ ../dist/configure --disable-replication --enable-mingw --enable-cxx
Boost: http://sourceforge.net/projects/boost/files/boost/1.53.0/
For this one, open a regular command (CMD) window and do the following:

cd \deps\boost-1.53.0\
bootstrap.bat mingw
b2 --build-type=complete --with-chrono --with-filesystem --with-program_options --with-system --with-thread toolset=gcc stage
For simplicity's sake, my versions are simply named deps\boost; deps\ssl; etc. If you build your own, either rename the folders in \deps OR change the paths to suit your changes in the coin-qt.pro file. Remember to change the Boost suffix too to match the version you compile with!

At this point you're ready to build normal non-Qt coin wallets on windows. Go ahead and check the thread at the beginning of this section if you'd like to know how. We're making a GUI though:

Next, install the Qt-MiniGW32 4.8.4 Build from here: http://download.qt-project.org/official_releases/qt/4.8/4.8.4/qt-win-opensource-4.8.4-mingw.exe Again, all normal installation options, next next next...you know the drill. Once QT is installed, you will find a program in Start - All Programs - Qt by Digia - Qt Command Prompt:


Fire it up and it will look pretty much like a DOS box:


Now since we don't have git on this our Windows computer (you can install it if you want, Cygwin is a good way to do that) you must download the clonecoin-master.zip file from https://github.com/crazysoldier/clonecoin-scrypt and extract it to the PC. For this example, we'll put it in c:\. One last thing we need to do before we compile for Windows. We need to edit the "clonecoin-qt.pro" file to enable the Windows libs, includes, and correct ordering for some of the syntax:

clonecoin/clonecoin-qt.pro:

LINES 11-22, UNCOMMENT ALL OF THESE TO ENABLE WINDOWS BUILDS:
#windows:LIBS += -lshlwapi
#LIBS += $$join(BOOST_LIB_PATH,,-L,) $$join(BDB_LIB_PATH,,-L,) $$join(OPENSSL_LIB_PATH,,-L,) $$join(QRENCODE_LIB_PATH,,-L,)
#LIBS += -lssl -lcrypto -ldb_cxx$$BDB_LIB_SUFFIX
#windows:LIBS += -lws2_32 -lole32 -loleaut32 -luuid -lgdi32
#LIBS += -lboost_system-mgw46-mt-sd-1_53 -lboost_filesystem-mgw46-mt-sd-1_53 -lboost_program_options-mgw46-mt-sd-1_53 -lboost_thread-mgw46-mt-sd-1_53
#BOOST_LIB_SUFFIX=-mgw46-mt-sd-1_53
#BOOST_INCLUDE_PATH=C:/deps/boost
#BOOST_LIB_PATH=C:/deps/boost/stage/lib
#BDB_INCLUDE_PATH=c:/deps/db/build_unix
#BDB_LIB_PATH=c:/deps/db/build_unix
#OPENSSL_INCLUDE_PATH=c:/deps/ssl/include
#OPENSSL_LIB_PATH=c:/deps/ssl
IF YOU BUILT YOUR OWN dependencies, then also change the paths in the file above to suit their locations, use / instead of \, yea--its odd. Now go back to your Qt Command Shell window and build the same way we built on the other platforms:

c:\Qt-Builder> cd \clonecoin-master\src
c:\clonecoin-master\src> qmake "USE_UPNP=- clonecoin-qt.pro
c:\clonecoin-master\src> make -f Makefile.Release
Wait for a bit...and once its done, you'll find a folder called "release" under the main clonecoin-master folder containing the .exe and a .cpp file.


Development process
===================

Developers work in their own trees, then submit pull requests when
they think their feature or bug fix is ready.

The patch will be accepted if there is broad consensus that it is a
good thing.  Developers should expect to rework and resubmit patches
if they don't match the project's coding conventions (see coding.txt)
or are controversial.

The master branch is regularly built and tested, but is not guaranteed
to be completely stable. Tags are regularly created to indicate new
official, stable release versions of Litecoin.

Feature branches are created when there are major new features being
worked on by several people.

From time to time a pull request will become outdated. If this occurs, and
the pull is no longer automatically mergeable; a comment on the pull will
be used to issue a warning of closure. The pull will be closed 15 days
after the warning if action is not taken by the author. Pull requests closed
in this manner will have their corresponding issue labeled 'stagnant'.

Issues with no commits will be given a similar warning, and closed after
15 days from their last activity. Issues closed in this manner will be
labeled 'stale'.

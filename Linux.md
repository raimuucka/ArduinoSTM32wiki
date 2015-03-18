

DFU Util on most linux distro's won't work, possibly because its too old

You need to compile Version 0.8 from source

Here is are the instructions from @Ahull (on the Arduino forum) to download, build and install dfu-util



"I created a folder to work in. In my case this is a subfolder of my "Sandbox" folder where I tend to put stuff I need to compile and play with, and that is what I will use for this example, but you can use pretty much any folder. Change to your working folder with something like.."

cd ~/Sandbox

Next, clone the git repo, note the instruction for this on the  http://dfu-util.sourceforge.net/build.html page is incorrect, the correct command is below.

git clone https://gitorious.org/dfu-util/dfu-util.git

You should see something like this..

Cloning into 'dfu-util'...
remote: Counting objects: 1858, done.
remote: Compressing objects: 100% (892/892), done.
remote: Total 1858 (delta 1347), reused 1316 (delta 959)
Receiving objects: 100% (1858/1858), 386.64 KiB | 0 bytes/s, done.
Resolving deltas: 100% (1347/1347), done.
Checking connectivity... done.


This will have cloned the git repo in to a subfolder of the current folder, called dfu-util

Next install the required packages to compile...

sudo apt-get build-dep dfu-util

followed by

sudo apt-get install libusb-1.0-0-dev

If not already installed you will also need
autoconf   automake  autotools-dev  
so ...

sudo apt-get install autoconf   automake  autotools-dev

now cd to the new dfu-util folder

cd dfu-util

run the autogen.sh script

 ./autogen.sh

... output will be something like

autoreconf: Entering directory `.'
autoreconf: configure.ac: not using Gettext
autoreconf: running: aclocal
autoreconf: configure.ac: tracing
autoreconf: configure.ac: creating directory m4
autoreconf: configure.ac: not using Libtool
autoreconf: running: /usr/bin/autoconf
autoreconf: running: /usr/bin/autoheader
autoreconf: running: automake --add-missing --copy --no-force
configure.ac:14: installing 'm4/compile'
configure.ac:7: installing 'm4/install-sh'
configure.ac:7: installing 'm4/missing'
src/Makefile.am: installing 'm4/depcomp'
autoreconf: Leaving directory `.'

next configure the build

./configure

then make

make

(or perhaps sudo make, depending on how your system is configured).

now if all went well, if you look in the src folder you will see ...

dfu.c       dfu_file.o  dfu_load.h  dfu-prefix  dfuse_mem.c  dfuse.o     dfu_util.c  main.c    Makefile.am  prefix.c  quirks.h  suffix.o
dfu_file.c  dfu.h       dfu_load.o  dfuse.c     dfuse_mem.h  dfu-suffix  dfu_util.h  main.o    Makefile.in  prefix.o  quirks.o  usb_dfu.h
dfu_file.h  dfu_load.c  dfu.o       dfuse.h     dfuse_mem.o  dfu-util    dfu_util.o  Makefile  portable.h   quirks.c  suffix.c

We need to copy dfu-util dfu-suffix and dfu-prefix to the location of the current dfu-util

which dfu-util

will give you the current location

which dfu-util
/usr/bin/dfu-util

.. so  

sudo cp src/dfu-util /usr/bin/dfu-util
sudo cp src/dfu-suffix /usr/bin/dfu-suffix
sudo cp src/dfu-prefix /usr/bin/dfu-prefix

.. should do the trick.

now if you run

dfu-util -v

you should see..

dfu-util -v
dfu-util 0.8

Copyright 2005-2009 Weston Schmidt, Harald Welte and OpenMoko Inc.
Copyright 2010-2014 Tormod Volden and Stefan Schmidt
...
.. and you are good to go. 
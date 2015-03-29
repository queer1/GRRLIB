GRRLIB
======


Table of Contents
-----------------

#####Introduction
######...What is it?

#####Developing for the Wii
######...How do I even start?

#####Downloading GRRLIB
######...Where do I get it from?

#####Installing GRRLIB
######...How do I get it to a useable state?

#####Using GRRLIB
######...What essentials do I need to know to get going?

#####Upgrading to v4.1.0 From Previous Versions of GRRLIB
######...I upgraded and now my programs won't compile properly!?

#####Using SVN
######...What is this SVN thing that the L337 devs keep talking about?

#####Credits
######...Who do I thank for all this free stuff?

#####Licence
######...When you say "free" do you actually mean something else?


Introduction
------------

GRRLIB is a C/C++ 2D/3D graphics library for Wii application developers.  It is
essentially a wrapper which presents a friendly interface to the Nintendo GX
core.

This document is written to be viewed with equal clarity in either a web browser
or a text editor.

As of v4.1.0, GRRLIB is supplied as a standard C/C++ library (aka. archive)
called 'libgrrlib'.  Because GRRLIB processes JPEG and PNG images, it requires
the installation of the 'libjpeg' and 'libpngu' libraries.  'libpngu', in turn,
requires 'libpng' and 'libpng' requires 'libz'.  GRRLIB has FileIO functions
to allow real-time loading and saving of graphical data, and thus requires
'libfat'.  GRRLIB also has the possibility to use TrueType fonts, so
'libfreetype' is also required.

    libgrrlib          <- 2D/3D graphics library
    +-- libfat         <- File I/O
    +-- libjpeg        <- JPEG image processor
    +-- libpngu        <- Wii wrapper for libpng
        +-- libpng     <- PNG image processor
            +-- libz   <- Zip (lossless) compression (for PNG compression)
    +-- libfreetype    <- TrueType font processor


Developing for the Wii
----------------------

Do not progress until you have installed and configured devkitPro.  Guides are
and assistance are available at http://forums.devkitpro.org

If you have just performed a clean (re)install on your Windows PC, be sure to
reboot before you continue.


Downloading GRRLIB
------------------

You are invited to use "the latest SVN trunk version" of GRRLIB at all times.

The SVN repository is located at:   http://grrlib.googlecode.com/svn/

There is a simple guide to "Using SVN" later in this document.

This document will presume that you have downloaded "the latest SVN trunk
version" to a directory called  C:\grr\trunk

Installing GRRLIB
-----------------

This guide is for Windows.  If you are using Linux, I am going to presume you
are smart enough to convert these instructions.

GRRLIB      is supplied as source code
libjpeg     is supplied as source code
libpngu     is supplied as source code
libpng      is supplied as source code
libz        is supplied as source code
libfreetype is supplied as source code
libfat      is supplied with devkitpro (Ie. preinstalled)

The easy way is to install GRRLIB and all the required libraries in a single
command:
  c:
  cd \grr\trunk\GRRLIB
  make clean all install

This process may take some time depending on the speed of your PC.

If you used the method above the following steps are not required, GRRLIB is
installed and you are ready to start developing Wii homebrew games.

If you want, you could install the libz, libpng, libpngu, libjpeg and
libfreetype libraries in a single command:
```
  c:
  cd \grr\trunk\GRRLIB\lib 
  make clean all install
```

Each library could also be installed individually:

To install libz
```
  c:
  cd \grr\trunk\GRRLIB\lib\zlib
  make clean all install
```

To install libpng
```
  c:
  cd \grr\trunk\GRRLIB\lib\png
  make clean all install
```

To install libpngu
```
  c:
  cd \grr\trunk\GRRLIB\lib\pngu
  make clean all install
```

To install libjpeg
```
  c:
  cd \grr\trunk\GRRLIB\lib\jpeg
  make clean all install
```

To install libfreetype
```
  c:
  cd \grr\trunk\GRRLIB\lib\freetype
  make clean all install
```

To install libgrrlib:
```
  c:
  cd \grr\trunk\GRRLIB\GRRLIB
  make clean all install
```


Using GRRLIB
------------

After everything is installed, simply put
```c
    #include <grrlib.h>
```
at the top of your .c/.cpp file and use the functions as required

You will also need to add
```make
    -lgrrlib -lfreetype -lfat -ljpeg -lpngu -lpng -lz
```
to the libs line in your makefile
...Remember the order of the libraries is critical.  You may (need to) insert
other libraries in the middle of the list, you may need to add others to the
start, or even the end - but do _not_ change the order of the libraries shown
here.

You do NOT need to place /anything/ in your application directory.

If you would like to see a working example of this, you can look at the example
found in: C:\grr\trunk\examples\template\source


Upgrading to v4.1.0 From Previous Versions of GRRLIB
----------------------------------------------------

Older versions of GRRLIB, required a line such as:
```c
  #include "../../../GRRLIB/GRRLIB/GRRLIB.h"
```
...to be placed at the top of each C file which uses GRRLB.
Because GRRLIB is now installed as a system library, this must be replaced with:
```c
  #include <grrlib.h>
```

Older versions of GRRLIB required the 'GRRLIB.h' and 'GRRLIB.c" files to be
present in every project which uses GRRLIB.
Because GRRLIB is now installed as a system library, these files are no longer
required and must be erased.
*WARNING* Be careful if you have edited (either of) these files.

Older versions of GRRLIB "passed 'structs'" and therefore Textured Images were
defined with:
```c
  GRRLIB_texImg  tex1, tex2;
```
Because GRRLIB now "passes 'pointers'" these definitions should be changed to:
```c
  GRRLIB_texImg  *tex1, *tex2;
```

With older versions of GRRLIB if the programmer (you) required access to the
mode and frame information, you were required to add one or more of the
following three lines:
```c
  extern GXRModeObj  *rmode;
  extern void        *xfb[2];
  extern u32         fb;
```
Because GRRLIB now does this for you automatically, these lines must be removed
from your code.


Using SVN
---------

SVN stands for "SubVersioN" ...No it doesn't mean much to me either.

It allows the developers to submit changes to the code in such a way that
these changes can be easily monitored, quickly merged together with other
changes. and (if necessary) reverted.

It also allows the power-users to gain access to the latest (often "in-test")
features.

SVN is classically divided in to three chunks:
trunk    - The main development & release code.
branches - Sometimes a developer may spend a week-or-more making their changes,
           so (s)he will work in a copy of the code until the changes are
           approved by the project leader ...then the changes are "merged" back
           in to trunk.
tags     - These are just copies of the code at critical points, such as
           official releases.
GRRLIB conforms to this official guideline.

To obtain the "cutting edge" codebase (ie. the latest in SVN) you need an SVN
tool ...The same as: if you want to view a web page, you need a web browser

 * For Windows you will choose: TortoiseSVN
 * For Debian  you will choose: apt-get install subversion
 * For others  you will need to do a bit of research (I only use Debian & Windows)

Windows:
 1. Create a directory to hold the code (Eg. C:\grr)
 2. Right click it and choose "svn checkout"
 3. Enter the URL of the SVN 'repository':  http://grrlib.googlecode.com/svn/
 4. Click the [...] button and choose the trunk*
 5. Leave advanced options alone (Ie. fully recursive, head)
 6. Hit OK

Linux:
 1. Create a directory to hold the code (Eg. `mkdir -p /home/user/src/grr`)
 2. Change to that directory            (Eg. `cd /home/user/src/grr`)
 3. Type `svn checkout http://grrlib.googlecode.com/svn/trunk/ grrlib-read-only`*

> You may choose to check-out any part of the repository you wish, but if you venture outide 'trunk' you are likely to get old or broken code.

If you network connection dies half-way through the download
 * Windows: ...simply right-click the directory again and choose "SVN Update"
 * Linux:   ...Simply type `svn update`

You may also perform an "update" any time you like to get the latest & greatest
code changes.  But be warned, if you have edited the GRRLIB source code things
can (and often do) get messy.  The best help you can get about this is probably
here:  http://svnbook.red-bean.com/en/1.1/svn-book.html#svn-ch-3-sect-5.4


Credits
-------

#### Project Leader
* NoNameNo

#### Documentation
* Crayon
* BlueChip

#### Lead Coder
* NoNameNo

#### Support Coders
* Crayon
* Xane
* DragonMinded
* BlueChip
* elisherer

#### Advisors
* RedShade
* Jespa


Licence
-------

GRRLIB is released under the MIT Licence.  If we had chosen the GPL licence you
would be +forced+ (legally required) to release your source code.  But in the
spirit of "free as in FREE" we have left you with the +option+ to release your
source code.

We do +request+ that you tell others about us by naming our library (GRRLIB) in
the credits of your game/application.  And, if you +choose+ to do that, we
encourage you to use our logo to achieve it; You can find our logo here:
C:\grr\trunk\grrlib_logo.png
and here:
http://grrlib.santo.fr/wiki/images/logo.png

This is the official license text
```
Copyright (c) 2015 The GRRLIB Team

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
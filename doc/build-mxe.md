Copyright (c) 2009-2013 Bitcoin Developers
Distributed under the MIT/X11 software license, see the accompanying
file COPYING or http://www.opensource.org/licenses/mit-license.php.
This product includes software developed by the OpenSSL Project for use in the [OpenSSL Toolkit](http://www.openssl.org/). This product includes
cryptographic software written by Eric Young ([eay@cryptsoft.com](mailto:eay@cryptsoft.com)), and UPnP software written by Thomas Bernard.


CROSS-COMPILE BUILD NOTES(64bit Ubuntu 14.04)
===================

Dependencies
------------
Libraries you need to download separately and build:

	MXE(M cross evironment)           /mnt/mxe          http://mxe.cc
	Berkeley DB     /mnt/db-5.3.28          http://download.oracle.com/berkeley-db/db-5.3.28.tar.gz
	miniupnpc       /mnt/miniupnpc-1.6.20120509         http://miniupnp.free.fr/files/miniupnpc-1.6.20120509.tar.gz
	OpenSSL         you build with windows        http://www.openssl.org/source/

Their licenses:

	MXE            MIT license
	Berkeley DB    New BSD license with additional requirement that linked software must be free open source
	miniupnpc      New (3-clause) BSD license
	OpenSSL        Old BSD license with the problematic advertising requirement

Versions used in this release:

	MXE          build-2017-02-11
	Berkeley DB  5.3.28
	miniupnpc    1.6
	OpenSSL      1.0.1c

MXE
-------
Install mxe dependencies:

	sudo apt-get install p7zip-full autoconf automake autopoint bash bison bzip2 cmake flex gettext git g++ gperf intltool libffi-dev libtool libltdl-dev libssl-dev libxml-parser-perl make openssl patch perl pkg-config python ruby scons sed unzip wget xz-utils g++-multilib libc6-dev-i386 libgtk2.0-dev

Clone mxe github repo

	cd /mnt
	git clone https://github.com/mxe/mxe.git

Compile boost:

	cd /mnt/mxe
	make MXE_TARGETS="i686-w64-mingw32.static" boost

Compile qt4:

	make MXE_TARGETS="i686-w64-mingw32.static" qt

Add mxe bins to PATH:

	export PATH=/mnt/mxe/usr/bin:$PATH

Berkeley DB
-----------
Compiling berkley db:
Download and unpack berkeley db:

	cd /mnt
	wget http://download.oracle.com/berkeley-db/db-5.3.28.tar.gz
	tar zxvf db-5.3.28.tar.gz

Make bash script for compilation:

	cd /mnt/db-5.3.28
	touch compile-db.sh
	chmod ugo+x compile-db.sh

Content of compile-db.sh:

	#!/bin/bash
	MXE_PATH=/mnt/mxe
	sed -i "s/WinIoCtl.h/winioctl.h/g" src/dbinc/win_db.h
	mkdir build_mxe
	cd build_mxe

	RANLIB=$MXE_PATH/usr/bin/i686-w64-mingw32.static-ranlib \
	CC=$MXE_PATH/usr/bin/i686-w64-mingw32.static-gcc \
	CXX=$MXE_PATH/usr/bin/i686-w64-mingw32.static-g++ \
	../dist/configure \
		--disable-replication \
		--enable-mingw \
		--enable-cxx \
		--host x86 \
		--prefix=$MXE_PATH/usr/i686-w64-mingw32.static

	make

	make install

MiniUPnPc
---------
UPnP support is optional, make with `USE_UPNP=` to disable it.

Compiling miniupnpc:
Download and unpack miniupnpc:

	cd /mnt
	wget http://miniupnp.free.fr/files/miniupnpc-1.6.20120509.tar.gz
	tar zxvf miniupnpc-1.6.20120509.tar.gz

Make bash script for compilation:

	cd /mnt/miniupnpc-1.6.20120509
	touch compile-m.sh
	chmod ugo+x compile-m.sh

Content of compile-m.sh:

	#!/bin/bash
	MXE_PATH=/mnt/mxe

	CC=$MXE_PATH/usr/bin/i686-w64-mingw32.static-gcc \
	AR=$MXE_PATH/usr/bin/i686-w64-mingw32.static-ar \
	CFLAGS="-DSTATICLIB -I$MXE_PATH/usr/i686-w64-mingw32.static/include" \
	LDFLAGS="-L$MXE_PATH/usr/i686-w64-mingw32.static/lib" \
	make libminiupnpc.a

	mkdir $MXE_PATH/usr/i686-w64-mingw32.static/include/miniupnpc
	cp *.h $MXE_PATH/usr/i686-w64-mingw32.static/include/miniupnpc
	cp libminiupnpc.a $MXE_PATH/usr/i686-w64-mingw32.static/lib

OpenSSL
-------
Unfortunately OpenSSL libraries with mxe will happen compile error.

You should manually build with Windows and copy libraries to '/mnt/mxe/i686-w64-mingw32/lib' .

Bunnycoin
-------
Download and unpack bunnycoin sources:

	cd /mnt
	git clone https://github.com/hitobb/bunnycoin.git

Compile:

	cd /mnt/bunnycoin/src
	make -f makefile.linux-mingw

Bunnycoin-Qt
-------
Make bash script for compilation:

	cd /mnt/bunnycoin
	touch compile-bun.sh
	chmod ugo+x compile-bun.sh

Modified Boost header file (/mnt/mxe/usr/i686-w64-mingw32.static/include/boost/type_traits/detail/has_binary_operator.hpp):

add to the top:

	#ifndef Q_MOC_RUN

add to the bottom:

	#endif

Content of compile-bun.sh:

	#!/bin/bash
	MXE_INCLUDE_PATH=/mnt/mxe/usr/i686-w64-mingw32.static/include
	MXE_LIB_PATH=/mnt/mxe/usr/i686-w64-mingw32.static/lib

	i686-w64-mingw32.static-qmake-qt4 \
		BOOST_LIB_SUFFIX=-mt \
		BOOST_THREAD_LIB_SUFFIX=_win32-mt \
		BOOST_INCLUDE_PATH=$MXE_INCLUDE_PATH/boost \
		BOOST_LIB_PATH=$MXE_LIB_PATH \
		OPENSSL_INCLUDE_PATH=$MXE_INCLUDE_PATH/openssl \
		OPENSSL_LIB_PATH=$MXE_LIB_PATH \
		BDB_INCLUDE_PATH=$MXE_INCLUDE_PATH \
		BDB_LIB_PATH=$MXE_LIB_PATH \
		MINIUPNPC_INCLUDE_PATH=$MXE_INCLUDE_PATH \
		MINIUPNPC_LIB_PATH=$MXE_LIB_PATH \
		QMAKE_LRELEASE=/mnt/mxe/usr/i686-w64-mingw32.static/qt/bin/lrelease bunnycoin-qt.pro

	make -f Makefile.Release

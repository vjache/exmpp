Exmpp is an Erlang application which provides the modules to ease the
development of an XMPP/Jabber server or client.

# About this fork
This fork includes the following PR's from the original repository:

- [#16](https://github.com/processone/exmpp/pull/16) Adds Rebar
- [#14](https://github.com/processone/exmpp/pull/14) Updated README

# Table of contents #

 1. Requirements
 2. Creating Autotools files
 3. Build and install
 4. Building examples
 5. Using an alternate Erlang environment
 6. Using another C compiler

1. Requirements
---------------

*   Erlang/OTP (REQUIRED)  
    A full Erlang environment is recommended but only ERTS and erl_interface are required:
    - Minimum version: R12B-5
*   C compiler (REQUIRED)  
    Exmpp contains Erlang port drivers which are written in C. Tested C compilers include:
    -   GNU Compiler Collection (gcc)
    -   Intel C++ Compiler (icc)
    -   GCC frontend for LLVM (llvm-gcc)
    -   Microsoft Visual C++ (cl)
    
    C compilers known not to work:
    -   clang frontend for LLVM (ccc)
*   XML parsing library (REQUIRED)  
    Tested libraries are:
    -   Expat: recommended. Tested: 2.0.1
    -   LibXML2: only experimental support.
*   OpenSSL (optional)  
    It's the only TLS engine supported for now.
    -   Tested version: 0.9.8e
*   zlib (optional)  
    It's the only compression engine supported for now. 
    -   Tested version: 1.2.3
*   eunit (optional)  
    To be able to use the testsuite, this Erlang application is required.
    -   Tested version: 2.0

2. Creating Autotools files
---------------------------

If you already have ``configure`` and ``Makefile.in`` files, you can this
section and go directly to section 3.

However, if you work on a Subversion/Git checkout, you'll have to generate
some files such as the configure script and Makefile.in. For this, you'll
have to install the following autotools:

* autoconf 2.60 or higher (read below for 2.62 and 2.63)
* automake 1.9 or later
* libtool 1.5 or later

To generate the files, run:

    $ autoreconf -vif

After that, you can use the standard procedure:

    $ ./configure
    $ make

WARNING: Erlang support in old autoconf 2.62 and 2.63 was broken! If you
must stick to these versions, you must apply the following patch to
'erlang.m4' in 'share/autoconf-2.62/autoconf' (patch taken from official
autoconf GIT repository):

```diff
--- erlang.m4-broken    2009-01-14 17:54:41.000000000 +0100
+++ erlang.m4	2009-01-14 16:51:57.000000000 +0100
@@ -124,7 +124,7 @@
 m4_define([AC_LANG(Erlang)],
 [ac_ext=erl
 ac_compile='$ERLC $ERLCFLAGS -b beam conftest.$ac_ext >&AS_MESSAGE_LOG_FD'
-ac_link='$ERLC $ERLCFLAGS -b beam conftest.$ac_ext >&AS_MESSAGE_LOG_FD ; echo "#!/bin/sh" > conftest$ac_exeext ; AS_ECHO(["\"$ERL\" -run conftest start -run init stop -noshell"]) >> conftest$ac_exeext ; chmod +x conftest$ac_exeext'
+ac_link='$ERLC $ERLCFLAGS -b beam conftest.$ac_ext >&AS_MESSAGE_LOG_FD ; echo "[#]!/bin/sh" > conftest$ac_exeext ; AS_ECHO(["\"$ERL\" -run conftest start -run init stop -noshell"]) >> conftest$ac_exeext ; chmod +x conftest$ac_exeext'
 ])
```

After applying the patch, you must run the following commands:

    $ cd [...]/share/autoconf-2.62/autoconf
    $ autom4te --language=autoconf --freeze --output=autoconf.m4f

3. Build and install
--------------------

Exmpp uses the Autotools. Therefore the process is quite common:

    $ ./configure
    $ make
    $ sudo make install

Building outside of the source directory is supported:

    $ mkdir exmpp-build
    $ cd exmpp-build
    exmpp-build$ /path/to/exmpp-src/configure
    exmpp-build$ make
    exmpp-build$ sudo make install

By default, Exmpp is installed in Erlang lib directory. You may select
another directory with the --prefix argument to the configure script:

    $ ./configure --prefix=/install/exmpp/here

Exmpp will be installed in "/install/exmpp/here/exmpp-$VERSION".

4. Building examples
--------------------

You can find example code in the "examples" directory.

These modules may be built for you with the --enable-examples configure
argument:

    $ ./configure --enable-examples

For now, they're not installed.

5. Using an alternate Erlang environment
----------------------------------------

If Erlang cannot be found by the configure script of if you prefer to
use a specific Erlang environment, you may indicate an alternate Erlang
root directory with the --with-erlang argument:

    $ ./configure --with-erlang=/path/to/alternate/erlang

Another way is to set the following configure variables:

    $ ./configure ERL=/path/to/bin/erl ERLC=/path/to/bin/ERLC \
      ESCRIPT=/path/to/bin/escript

6. Using another C compiler
---------------------------

You may specify another C compiler to use at configure time. For
instance, to use GCC frontend for LLVM:

    $ ./configure CC=llvm-gcc
    $ make



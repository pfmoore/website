.. title: Building Unix utilities using msys2
.. slug: building-unix-utilities-using-msys2
.. date: 2019-05-21 18:19:53 UTC+01:00
.. tags: programming, C
.. category: Computing
.. link: 
.. description: 
.. type: text

Unix comes with a mass of tools available "out of the box". Things like sed,
grep, awk, etc. Sources for these are available (mostly from the Gnu project)
and are generally portable to Windows. But building them can be a challenge,
because they rely heavily on Unix build tools like autoconfig, which are *not*
typically portable.

The msys2 project provides a Unix compatible environment (based on cygwin)
that can be used to build *native* Windows tools. Essentially, it is a dual
system, providing a cross-compilation environment from Windows/msys to
Windows/native. Setting up that environment, and building tools, is not hard,
but the documentation on how to do it is not that easy to follow (a lot of
concepts are handled by directly referring to the documentation of the pacman
tool, which is the native package manager for Arch Linux).

Note: There is an alternative approach, which is to simply cross-compile for
Windows using the Linux Subsystem for Windows and the mingw cross-compiler
(available in apt). I'll try this another day.

Setting up msys2
================

The msys2 environment is completely portable (i.e., you can install it in a
directory of your choice, and *everything* needed is in that directory). You
can have multiple copies on one machine without issues. Installation is
basically as described on the `Msys2 homepage <https://www.msys2.org/>`_. It
does create start menu shortcuts, which is the one "non-local" change made to
your system, but these can be deleted.

Actually, on inspection, the installer also creates an entry in Add/Remove
programs. So to make the installation fully portable, you need to *copy* the
whole msys64 directory, then *uninstall* the original. This is a pain. There
is a "zip" build, I believe, but I didn't go looking for it when writing this
article.

Once installed, and brought up to date, msys64 is ready to use. Further
information is available on the `Msys2 wiki
<https://github.com/msys2/msys2/wiki>`_, and in particular for this exercise,
on the page `Creating Packages
<https://github.com/msys2/msys2/wiki/Creating-packages>`_.

Setting up a build environment
==============================

To set up a build environment, you need to decide whether you're building for
32-bit or 64-bit systems (or both). I will only be building for 64-bit, so the
packages we need to install are ``base-devel`` and
``mingw-w64-x86_64-toolchain``. Install these from a msys2 shell, using::

    pacman -S base-devel mingw-w64-x86_64-toolchain

Once you have that done, you should be ready for a build.

Building a package
==================

A package build script consists of a ``PKGBUILD`` file, possibly accompanied
by some patch files. There is a huge repository of such build scripts in the
`package source repository <https://github.com/msys2/MINGW-packages>`.

As an example, we'll build ``diffutils``::

    cp -r /e/Work/Projects/MINGW-packages/mingw-w64-diffutils ~/mingw-w64-diffutils
    cd ~/mingw-w64-diffutils
    MINGW_INSTALLS=mingw64 makepkg-mingw -sCLf

The ``MINGW_INSTALLS`` variable says what architecture to build (leaving it
out builds both), ``-s`` says to install missing dependencies, ``-C`` says to
clean before building, ``-L`` says log the build progress, and ``-f`` says to
overwrite any existing package file.

The build is fairly slow - the configure script is not very fast on Windows
(it runs a huge number of subprocesses, which is much more costly on Windows
than it is on Linux).

Once the build has finished, with luck you will have a package file, in this
case named ``mingw-w64-x86_64-diffutils-3.6-2-any.pkg.tar.xz``, in your build
directory. You can install this directly, using::

    pacman -U mingw-w64-x86_64-diffutils-3.6-2-any.pkg.tar.xz

But if you do this, be aware that all of the build tools are in your
``mingw64`` directory. If what you want is a "clean" runtime environment, you
may prefer to install in a brand new ``mingw64`` environment. On the other
hand, some of the build tools like ``objdump`` are quite useful, so there may
be an advantage in keeping them available.

Removing unwanted dependencies
==============================

Some packages depend on others, and in some cases we don't want to have the
dependencies installed. A common example is a tool that includes some
supporting Python scripts, which depends on the Python interpreter. But we
definitely *don't* want the mingw64 version of Python, as we have the official
Windows distribution. In that case we can uninstall it, using ``pacman -R``::

    pacman -R -dd mingw-w64-x86_64-python3

You need the ``-dd`` flag to force removal even though this leaves unresolved
dependencies. The weaker ``-d`` or ``--nodeps`` flag is not sufficient. You
should probably also check regularly that the dependencies haven't been
reinstalled, as I expect pacman will try to fix any missing dependencies.

Maybe there's a way of forcing a dependency to be permanently "blocked". But I
haven't found it yet.

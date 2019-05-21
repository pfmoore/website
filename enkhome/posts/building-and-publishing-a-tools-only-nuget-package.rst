.. title: Building and Publishing a (tools only) nuget package
.. slug: building-and-publishing-a-tools-only-nuget-package
.. date: 2019-05-14 09:27:14 UTC+01:00
.. tags: windows, nuget
.. category: Computing
.. link: 
.. description: 
.. type: text

The Python interpreter is available from the `Nuget <https://www.nuget.org/>`_
package repository, so you can download a local copy of Python just by doing
``nuget install Python``. This is really convenient, and it would potentially
be nice to be able to do it for my own tools.

The good news is that it's possible - and actually really easy!

Building a package
==================

The first step is to create a ``.nuspec`` file in your project directory. The
simplest way to do this is with the ``nuget spec MyProject`` command, which
will just create the ``MyProject.nuspec`` file, with placeholders for most of
what you need. The one key thing here is to remove the ``<dependencies>``
section - we're creating a simple "put these files somewhere" project, so we
don't need (or want) dependencies. (Translation: I haven't looked into how
they work, so I'm ignoring them {{% emoji slightly_smiling_face %}}).

The next step is to add a ``<files>`` section *after* the ``<metadata>``
section (not inside it, which is where the ``<dependencies>`` section was).

Here's a working example of a ``.nuspec`` file::

   <?xml version="1.0"?>
   <package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
     <metadata>
       <id>Foo</id>
       <version>0.0.1</version>
       <title>Foo - Test package</title>
       <authors>Paul Moore</authors>
       <owners>Paul Moore</owners>
       <projectUrl>https://github.com/pfmoore/foo</projectUrl>
       <requireLicenseAcceptance>false</requireLicenseAcceptance>
       <description>Foo is a demo</description>
       <summary>Foo is a demo</summary>
       <copyright>Copyright © Paul Moore 2019</copyright>
     </metadata>
     <files>
       <file src="bin\sumatrapdf.exe" target="tools" />
     </files>
   </package>

The ``<file .../>`` elements specify the files to package up, and where to put
them when unpacking (usually in the "tools" directory). You can use wildcards
here. There's also an ``init.ps1`` file which is special, in that if you
specify it, it will get run on install, to do post-install setup. (Note: I
haven't actually tried using this yet).

Once you have your ``.nuspec`` file, creating a Nuget package is as simple
as::

    nuget pack MyPackage.nuspec

This creates a ``.nupkg`` file, which bundles up everything you need to
install your package.

Package sources
===============

By default, Nuget installs packages from the "official" source at nuget.org
(that's where the Python package is published, for example). But a package
source can be nothing more than a local directory, formatted in a particular
way. To create such a local source, all you need to do is to add your package
to it::

    nuget add MyPackage.0.0.1.nupkg -source <LocalDir>

The ``source`` argument is the directory that will be your package source. It
does not need to exist in advance, nuget will create it for you if needed.

Installing from a local source is just as easy::

    nuget install MyPackage -source <LocalDir>

That's it!

There is a lot more to nuget, but if all you want to do is publish some tools,
this is *really* simple to get started.

References
==========

How to create a tools-only package:
http://www.marcusoft.net/2011/12/creating-tools-only-nuget-package.html

Local nuget feeds:
https://docs.microsoft.com/en-us/nuget/hosting-packages/local-feeds

.. title: Embedding graphviz diagrams in postings
.. slug: embedding-graphviz-diagrams-in-postings
.. date: 2019-05-09 10:03:49 UTC+01:00
.. tags: website, nikola
.. category: Computing
.. link: 
.. description: 
.. type: text

I just discovered, nikola has a plugin that supports Graphviz diagrams. To
install it, all you need to do is::

    nikola plugin -i graphviz

It needs graphviz (specifically, the ``dot`` executable) available on your
``PATH``, but only at site build time. The build process embeds the graphviz
output in the site, so nothing is needed when you host the site (which is
basically how a "static site generator" has to work, of course...)

Using the plugin is as simple as putting the diagram source in a ``graphviz``
directive::

    .. graphviz::

    digraph foo {
        "Idea" -> "tap tap tap" -> "Code";
    }

This gives a result looking like the following:

.. graphviz::

   digraph foo {
       "Idea" -> "tap tap tap" -> "Code";
   }

To do: Investigate what other plugins are available for nikola!

Note: While creating this post, I renamed my project directory. But I keep the
software (nikola, etc) in a virtual environment ``.venv`` in the project
directory, and after the rename, all of the script wrappers broke (because
they embed the absolute path to the Python interpreter, and aren't
relocatable). I need to think about a better way of managing virtual
environments (pew, pipenv, ...).

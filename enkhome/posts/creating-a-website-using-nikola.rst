.. title: Creating a website using Nikola
.. slug: creating-a-website-using-nikola
.. date: 2019-04-26 11:26:41 UTC+01:00
.. tags: website, nikola
.. category: Computing
.. link: 
.. description: 
.. type: text

This post documents the steps I went through to set up the initial version of
this website, using the Nikola static website generator. It's very much a
learning exercise, as I've never used any sort of website generator before.

Goals
=====

OK, to start with, I want to set some goals, so that I'm not just floundering
round "trying things out". I'll also note some explicit non-goals, for
completeness, and to set some boundaries.

Goals:

* A fully static website, with posts (usually) written in a markup language,
  either Markdown or reStructured Text.
* Site content version controlled using git.
* Ultimately, publishing to github pages with my own domain name, if that's
  possible.
* Both blog-style posts and longer static articles.
* Reasonable control over layout of the site, ideally with some way of
  gradually modifying things rather than having to go straight from a default
  template to a full "write your own in HTML" custom style.
* Ability to include Python code in the posts, with syntax highlighting.
* Ability to include maths in the posts. This is not essential, I just think
  it's cool.
* Emoji. Nice smilies, and things like that.
* Ability to post Jupyter notebooks - I do a reasonable amount of analysis
  using Jupyter, and it would be nice to be able to post it without rewriting.

Non-goals:

* At this stage, I am *not* insisting on stable URLs. I will be experimenting,
  and in doing so I'm fine with links breaking. Once I have things in a style
  I like, I can think about how I manage future overhauls, but initially it's
  "development mode".
* Comments. Maybe I'd like to add comments, but for right now I just want
  somewhere to publish my content.

Software
========

As noted, I'm using Nikola. I briefly looked at Pelican as an alternative, and
maybe it's fine, I didn't really look too hard. Mostly I didn't like its
default website look, and changing it seemed like something I didn't want to
dive into initially. Also Nikola is based on the Python doit library - totally
irrelevant to how I'll be using it, but I quite like the library and it makes
me happy to think someone has built a nice tool using it
{{% emoji slightly_smiling_face %}}

Don't read anything into my choice. Hopefully the above paragraph makes it
obvious that it's not based on any sort of actual review.

Initial Steps
=============

I created a project directory, and a virtual environment named ``.venv``
inside it. I installed Nikola into that environment.

To create the website structure I just ran::

    > .venv/Scripts/nikola.exe init enkhome

With that in place, I added a ``.gitignore`` file that looked like this::

   # General Python admin stuff  
   .venv                         
   __pycache__                   
   *.pyc                         
                                 
   # Nikola output               
   cache                         
   output                        
   .doit.db.*                    
                                 
   # IDEs                        
   .vscode                       

The important stuff here is the "Nikola output" section. I worked it out by
experimenting, but Nikola stores its generated files in ``cache`` and
``output``, so I excluded them. It also stores its state in ``.doit.db.*``. In
addition it compiles the ``conf.py`` file, but that's covered by the earlier
``__pycache__`` and ``*.pyc`` stuff.

With all that in place, I initialised git in the project directory, added the
files I'd just created, and committed everything. That's the baseline empty
layout.

Next step was to just get writing. It would be all too easy to spend forever
fiddling with admin, and have no content. So rather than do that, I created
this post. Go into the website subdirectory, run::

    > ../.venv/Scripts/nikola.exe new_post

(I'm going to abbreviate ``.../.venv/Scripts/nikola.exe`` to just ``nikola``
from now on. And I'll probably set Nikola up globally, too, so that will
actually be what I type.)

Give it a title, then open the file it creates in Vim, and start typing. Once
I have some content, commit it to git.

Once that's done, I'll preview the page::

    > nikola build
    > nikola serve -b


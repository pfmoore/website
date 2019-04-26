.. title: Using emoji in Nikola
.. slug: using-emoji-in-nikola
.. date: 2019-04-26 14:21:04 UTC+01:00
.. tags: website, nikola
.. category: Computing
.. link: 
.. description: 
.. type: text

Using emoji in Nikola actually isn't that hard. The manual explains the
process of using shortcodes `here
<https://getnikola.com/handbook.html#built-in-shortcodes>`_, and the ``emoji``
shortcode is the one you want. One mistake I made was to miss the ``%``
character off the terminating ``}}``. I think it's because Vim auto-completed
the ``}}``, but not the ``%``... This gives a really nasty traceback, rather
than a helpful message, and took me ages to find.

It's worth noting that the scary note about using shortcodes in
ReStructuredText doesn't seem to be relevant here. Just putting the
{{% raw %}} ``{{% ... %}}`` {{% /raw %}} construct inline seems to work
fine. (But actually entering the above text was an exercise in itself -
see the ``raw`` shortcode for details!)

There is also a bug on Windows -
``nikola\plugins\shortcode\emoji\__init__.py`` reads the JSON files using the
default text encoding, which on Windows is *not* UTF-8. So you get an encoding
error. It's easy to fix (just add an ``encoding`` argument to the ``open``
call). I'll raise a bug report in due course.

Finally, you need to work out how to show the emoji a bit more nicely - the
default font is not ideal. Usefully, Nikola adds an ``emoji`` CSS class to the
output, so it should be possible to do this with a bit of CSS. There's a nice
web font, Noto Color Emoji, available from Google at
https://www.google.com/get/noto/#emoji-zsye-color.

To put this all together:

1. Add a file in ``files\assets\css\custom.css`` with the content
   ::

       /* fonts*/
       @font-face {
           font-family: "color-emoji";
           src: local("Noto Color Emoji");
           src: url("../fonts/NotoColorEmoji.ttf");
       }
       .emoji {
           font-family: 'color-emoji';
       }

2. Download the font from the URL above, and extract the ``.ttf`` file to
   ``files\assets\fonts``.

Done. {{% emoji slightly_smiling_face %}}

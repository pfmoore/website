.. title: Typesetting Music
.. slug: typesetting-music
.. date: 2019-05-03 09:35:14 UTC+01:00
.. tags: programming, music, python
.. category: Music
.. link: 
.. description: 
.. type: text

When playing music, I use the app forScore on my iPad to avoid having
to carry around huge folders full of scores. This is great, but it
does rely on my having PDF files for all my scores. At the moment, I
do this by scanning in paper copies - but sometimes, the scans are
quite poor quality. Ideally, what I'd like is to be able to create my
own PDFs, from scratch.

Requirements
============

Most of my music (currently) is guitar music. To accompany a song on the
guitar, all I *really* need is the lyrics and chords - and many people do use
just this. Formats like ChordPro provide lyrics and chords very well. But
personally, I like to have a "lead sheet" - lyrics and chords, plus the melody,
in standard "sheet music" format. So for me, chord-only formats aren't enough.

For normal use, I only need PDF output. But there are apps that work better
with MusicXML output, allowing "on the fly" transposition, for example. So that
would be a bonus.

In terms of musical features, the songs I play aren't that complex - but they
do include such things as repeats (often with separate first and second time
endings), and multiple verses of lyrics, often with slight variations in the
music (optional ties, for example). So in practice, scoring can be quite
complex.

In terms of input, I would like text-based input. I'd rather be able to store
my scores in git, review diffs, all of the sorts of processes I'm used to from
a programming perspective (but which aren't always the norm for music score
writers!)

Available Software
==================

Graphical Tools
---------------

Unsurprisingly, there are plenty of options already available. Tools like
MuseScore, Sibelius, Notion, etc. are professional level score writing tools.
MuseScore is free, and Sibelius has a free tier, so price isn't even an issue.
However, these tools uniformly rely on graphical input, so entering a score is
a matter of clicking and typing in the GUI. While they seem very well tuned to
making data entry efficient, I would have to learn the UI, and it's unlikely I
would easily reach the level of efficiency I have with typing text. However,
they do have other useful features, which I will return to.

Lilypond
--------

The biggest name in text-based music typesetting (at least as far as I have
been able to find) is Lilypond. This is an *extremely* high quality tool, that
takes input in a text format reminiscent of, and I believe inspired by, TeX,
and generates high-quality PDF output. Unfortunately, it has a number of
drawbacks for my purposes.

1. The input language is very complex, and while that doesn't put me off, the
   core focus of the manual is on types of music I am not interested in - the
   coverage of chords and lyric handling is complete, but it's only a tiny
   part of the documentation - and the examples given of most of the features
   I am interested in (such as repeats) use musical styles that I will never
   need, and as a result tend to be hard for me to follow.

2. The only output format available is PDF. While that's what I need, not
   having MusicXML is a drawback.

3. The lack of MusicXML output also means that the whole Lilypond toolchain
   lives in a world of its own. There is no useful interoperability with other
   tools. If I were to choose Lilypond as a tool, I'd be effectively "locked
   in" to that choice.

4. The defaults in Lilypond are really annoying for me. The tool uses non-UK
   defaults for note names, for instance (there's no reason the authors should
   default to UK notation, they aren't from the UK after all, but it's a
   stumbling block for me). It also uses the "Jazz" style of a triangle to
   denote a major 7th chord, which I dislike and find hard to read while
   playing. While all of the defaults can be modified, this needs to be done
   in each score, which adds a chunk of boilerplate to deal with. To be fair,
   there may be a settings file somewhere - I haven't looked that closely, to
   be honest.

Overall, Lilypond would just feel too much like locking myself into a tool
that I'd struggle to use. And as a result, I'd simply never get round to
actually inputting scores, which is after all the point.

ChordPro
--------

The ChordPro format, which I mentioned above, is close to ideal - it's text
based, simple to use, and relatively clean. It feels very much like a musical
equivalent of something like Markdown - straightforward, a bit limited but not
in any important ways, and widely supported.

The major downside is that it doesn't include the melody. I have used ChordPro
tools to generate PDF chord sheets, and they are very nice, but I want the
melody score (mostly to have an indication of rhythm and timing). Also, if I
were to use ChordPro exclusively, I would *not* use forScore as my app. I'd
want to use a tool that handles ChordPro natively, and includes features like
on the fly transposition, capo support, etc.

Music21
-------

The final option here is not actually a tool at all, rather it's a library for
Python. It was written for use in the academic study of music theory, not as a
score production tool at all (and indeed, behind the scenes it relies on tools
like MuseScore and Lilypond for engraving). However, it has an immensely
powerful data model, which allows scores to be represented in code. It has the
ability to generate output in either MusicXML or Lilypond formats (as well as
many others, with an extensible framework if you want something extra). So
basically, while it's not itself a score production tool, it would allow me to
write one.

So that's typical. Rather than do the job I intended to, I'll write a tool to
do it. Sigh.

Honestly, though, the hard part of what I'd write would be converting a
convenient text input format into the music21 internal representation. And
that's the bit that I *have* to do anyway, as there is no existing text format
that suits me. So it's not actually like I'm adding much non-essential
overhead. Any the hard part of *any* data entry process for my music is the
sheer typing in of the lyrics - and if I can do that as part of "creating
sample input for the program I'm writing", I might be able to persuade myself
to do that work sooner rather than later.

More to follow in future posts on how I set up music21 and integrate it with
other tools.

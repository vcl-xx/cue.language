# cue.language #

## What? ##

cue.language is a small library of Java code and resources
that provides the following basic natural-language
processing capabilities:

* Tokenizing natural language text into individual words
* Tokenizing natural language text into sentences
* Tokenizing natural language text into n-grams (sequences of 2 or more words
  that appear next to each other in a sentence)
* Counting strings
* Detecting which script (alphabet, writing system) is required
  to represent a text
* Guessing what language a text is in
* Customizable "stop word" detection for a variety of languages

## Why? ##

This code grew out of the particular needs of the
[Wordle](http://www.wordle.net/) word cloud toy, 
but is potentially useful for
other simple natural language tasks.

## Who? ##

cue.language was written, and is currently maintained, by 
[Jonathan Feinberg](http://www.research.ibm.com/visual/jonathan.html).

The "cue" in "cue.language" refers to the
[Collaborative User Experience](http://domino.watson.ibm.com/cambridge/research.nsf/pages/cue.html) group,
the Cambridge, MA home of
[IBM Research](http://www.research.ibm.com/)'s 
[Visual Communication Lab](http://www.research.ibm.com/visual/).

## Supported languages ##

cue.language's stop word lists and language detection support the following
languages:

Arabic, Catalan, Croatian, Czech, Dutch, 
Danish, English, Esperanto, Farsi, Finnish, 
French, German, Greek, Hebrew, Hindi, Hungarian, 
Italian, Latin, Norwegian, Polish, Portuguese, 
Romanian, Russian, Slovenian, Slovak, Spanish,
Swedish, Turkish

To add support for your own language, please examine one or more of
the existing stop word lists as models, and construct such a list
with the most common and least interesting words from your language.
You can either [send me the list](mailto:jdf@us.ibm.com) or
fork cue.language, perform the integration yourself, and issue
a github pull request.

## Help needed! ##

We need unit tests.
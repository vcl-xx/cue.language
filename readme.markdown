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

## How? ##

In the following examples, the `String` `hound` contains the
Gutenberg e-text edition of Arthur Conan Doyle's
[The Hound of the Baskervilles](http://www.gutenberg.org/cache/epub/2852/pg2852.txt.utf8).

### Tokenizing: words

    for (final String word : new WordIterator(hound)) {
        System.out.println(word);
    }

### Tokenizing: sentences

    for (final String word : new SentenceIterator(hound, Locale.ENGLISH)) {
        System.out.println(word);
    }

### Tokenizing: n-grams

    // all 3-grams
    for (final String ngram : new NGramIterator(3, hound, Locale.ENGLISH)) {
        System.out.println(ngram);
    }

    // all 3-grams not containing stop words
    for (final String ngram : new NGramIterator(3, hound, Locale.ENGLISH, StopWords.English)) {
        System.out.println(ngram);
    }

### Counting

    // find the most common 3-grams of the Baskervilles 
    final Counter<String> ngrams = new Counter<String>();
    for (final String ngram : new NGramIterator(3, hound, Locale.ENGLISH, StopWords.English)) {
        ngrams.note(ngram.toLowerCase(Locale.ENGLISH));
    }
    for (final Entry<String, Integer> e : ngrams.getAllByFrequency().subList(0, 10)) {
        System.out.println(e.getKey() + ": " + e.getValue());
    }
    
    // count "Baskerville"
    final Counter<String> words = new Counter<String>();
    for (final String word : new WordIterator(hound)) {
        words.note(word);
    }
	System.out.println("Baskerville: " + words.getCount("Baskerville"));    
			
### Guessing script and language
    
    final String arabic = fetchURL("http://ar.wikipedia.org/wiki/مبارك_الصباح");
    System.out.println(BlockUtil.guessUnicodeBlock(arabic));
    System.out.println(StopWords.guess(arabic));
    
    final String farsi = fetchURL("http://fa.wikipedia.org/wiki/محمد_زکریای_رازی");
    System.out.println(BlockUtil.guessUnicodeBlock(farsi));
    System.out.println(StopWords.guess(farsi));
    
    final String hindi = fetchURL("http://hi.wikipedia.org/wiki/विकिपीडिया:निर्वाचित_लेख");
    System.out.println(BlockUtil.guessUnicodeBlock(hindi));
    System.out.println(StopWords.guess(hindi));
    
    final String slovenian = fetchURL("http://sl.wikipedia.org/wiki/Godfrey_Harold_Hardy");
    System.out.println(BlockUtil.guessUnicodeBlock(slovenian));
    System.out.println(StopWords.guess(slovenian));
    
    final String catalan = fetchURL("http://ca.wikipedia.org/wiki/Godfrey_Harold_Hardy");
    System.out.println(BlockUtil.guessUnicodeBlock(catalan));
    System.out.println(StopWords.guess(catalan));
    
    final String french = fetchURL("http://fr.wikipedia.org/wiki/Godfrey_Harold_Hardy");
    System.out.println(BlockUtil.guessUnicodeBlock(french));
    System.out.println(StopWords.guess(french));

### Stop words

    System.out.println(StopWords.English.isStopWord("the"));
    System.out.println(StopWords.English.isStopWord("ThE"));
    System.out.println(StopWords.Farsi.isStopWord("بیشتر"));
    System.out.println(StopWords.English.isStopWord("borborygmus"));
    for (final String word : new WordIterator(hound)) {
        if (StopWords.English.isStopWord(word)) {
            System.out.println(word);
        }
    }
    
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

## Known bugs and weaknesses ##

If your text is small, you're likely to get near misses on the
language guessing.

The SentenceIterator incorrectly breaks on "Mrs." and "Ms.", though it works
just fine with "Mr.". I have reported this bug to Sun, since my
SentenceIterator relies on the JDK BreakIterator.

## Help needed! ##

cue.language has exactly 0% test coverage. Fastidious programmers
with extra time on their hands would find fertile ground here.

## Planned ##

The tokenizers all operate on Strings, which makes this library
unsuitable for use on extremely large texts. I intend to modify
the tokenizers to work on Readers, so that they can keep memory
use small and constant. This would be easier in a language that
supports generators/coroutines!

## License ##

&copy; 2009 IBM Corp

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

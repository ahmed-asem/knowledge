TESSERACT
=========

[NAME](#NAME)
 [SYNOPSIS](#SYNOPSIS)
 [DESCRIPTION](#DESCRIPTION)
 [OPTIONS](#OPTIONS)
 [LANGUAGES](#LANGUAGES)
 [CONFIG FILES AND AUGMENTING WITH USER DATA](#CONFIG%20FILES%20AND%20AUGMENTING%20WITH%20USER%20DATA)
 [HISTORY](#HISTORY)
 [RESOURCES](#RESOURCES)
 [SEE ALSO](#SEE%20ALSO)
 [AUTHOR](#AUTHOR)
 [COPYING](#COPYING)

* * * * *

NAME
----

tesseract - command-line OCR engine

SYNOPSIS
--------

**tesseract** *imagename outbase*|*stdout* [*-l lang*] [*-psm N*] [-c configvar=value] [*configfile* ...]

DESCRIPTION
-----------

tesseract(1) is a commercial quality OCR engine originally developed at HP between 1985 and 1995. In 1995, this engine was among the top 3 evaluated by UNLV. It was open-sourced by HP and UNLV in 2005, and has been developed at Google since then.

OPTIONS
-------

*imagename*

The name of the input image. Most image file formats (anything readable by Leptonica) are supported.

*outbase*

The basename of the output file (to which the appropriate extension will be appended). By default the output will be named *outbase.txt*. When stdout is used as outbase, output will be sent to stdout.

*-l lang*

The language to use. If none is specified, English is assumed. Multiple languages may be specified, separated by plus characters. Tesseract uses 3-character ISO 639-2 language codes. (See LANGUAGES)

*-psm N*

Set Tesseract to only run a subset of layout analysis and assume a certain form of image. The options for **N** are:

0 = Orientation and script detection (OSD) only.
 1 = Automatic page segmentation with OSD.
 2 = Automatic page segmentation, but no OSD, or OCR.
 3 = Fully automatic page segmentation, but no OSD. (Default)
 4 = Assume a single column of text of variable sizes.
 5 = Assume a single uniform block of vertically aligned text.
 6 = Assume a single uniform block of text.
 7 = Treat the image as a single text line.
 8 = Treat the image as a single word.
 9 = Treat the image as a single word in a circle.
 10 = Treat the image as a single character.

*-c configvar=value*

Sets a configuration variable. Multiple options can be set by using -c multiple times, once for each option.

*-v*

Returns the current version of the tesseract(1) executable.

*configfile*

The name of a config to use. A config is a plaintext file which contains a list of variables and their values, one per line, with a space separating variable from value. Interesting config files include:

• hocr - Output in hOCR format instead of as a text file.

**Nota Bene:** The options *-l lang*, *-psm N* and *-c configvar=value* must occur before any *configfile*.

LANGUAGES
---------

There are currently language packs available for the following languages:

**ara** (Arabic), **aze** (Azerbauijani), **bul** (Bulgarian), **cat** (Catalan), **ces** (Czech), **chi\_sim** (Simplified Chinese), **chi\_tra** (Traditional Chinese), **chr** (Cherokee), **dan** (Danish), **dan-frak** (Danish (Fraktur)), **deu** (German), **ell** (Greek), **eng** (English), **enm** (Old English), **epo** (Esperanto), **est** (Estonian), **fin** (Finnish), **fra** (French), **frm** (Old French), **glg** (Galician), **heb** (Hebrew), **hin** (Hindi), **hrv** (Croation), **hun** (Hungarian), **ind** (Indonesian), **ita** (Italian), **jpn** (Japanese), **kor** (Korean), **lav** (Latvian), **lit** (Lithuanian), **nld** (Dutch), **nor** (Norwegian), **pol** (Polish), **por** (Portuguese), **ron** (Romanian), **rus** (Russian), **slk** (Slovakian), **slv** (Slovenian), **sqi** (Albanian), **spa** (Spanish), **srp** (Serbian), **swe** (Swedish), **tam** (Tamil), **tel** (Telugu), **tgl** (Tagalog), **tha** (Thai), **tur** (Turkish), **ukr** (Ukrainian), **vie** (Vietnamese)

To use a non-standard language pack named **foo.traineddata**, set the **TESSDATA\_PREFIX** environment variable so the file can be found at **TESSDATA\_PREFIX**/tessdata/**foo**.traineddata and give Tesseract the argument *-l foo*.

CONFIG FILES AND AUGMENTING WITH USER DATA
------------------------------------------

Tesseract config files consist of lines with variable-value pairs (space separated). The variables are documented as flags in the source code like the following one in tesseractclass.h:

STRING\_VAR\_H(tessedit\_char\_blacklist, "", "Blacklist of chars not to recognize");

These variables may enable or disable various features of the engine, and may cause it to load (or not load) various data. For instance, let's suppose you want to OCR in English, but suppress the normal dictionary and load an alternative word list and an alternative list of patterns — these two files are the most commonly used extra data files.

If your language pack is in /path/to/eng.traineddata and the hocr config is in /path/to/configs/hocr then create three new files:

/path/to/eng.user-words:

the
 quick
 brown
 fox
 jumped

/path/to/eng.user-patterns:

1-\\d\\d\\d-GOOG-411
 www.\\n\\\\\\\*.com

/path/to/configs/bazaar:

load\_system\_dawg F
 load\_freq\_dawg F
 user\_words\_suffix user-words
 user\_patterns\_suffix user-patterns

Now, if you pass the word *bazaar* as a trailing command line parameter to Tesseract, Tesseract will not bother loading the system dictionary nor the dictionary of frequent words and will load and use the eng.user-words and eng.user-patterns files you provided. The former is a simple word list, one per line. The format of the latter is documented in dict/trie.h on read\_pattern\_list().

HISTORY
-------

The engine was developed at Hewlett Packard Laboratories Bristol and at Hewlett Packard Co, Greeley Colorado between 1985 and 1994, with some more changes made in 1996 to port to Windows, and some C++izing in 1998. A lot of the code was written in C, and then some more was written in C++. The C\\++ code makes heavy use of a list system using macros. This predates stl, was portable before stl, and is more efficient than stl lists, but has the big negative that if you do get a segmentation violation, it is hard to debug.

Version 2.00 brought Unicode (UTF-8) support, six languages, and the ability to train Tesseract.

Tesseract was included in UNLV's Fourth Annual Test of OCR Accuracy. See <http://www.isri.unlv.edu/downloads/AT-1995.pdf>. With Tesseract 2.00, scripts are now included to allow anyone to reproduce some of these tests. See <http://code.google.com/p/tesseract-ocr/wiki/TestingTesseract> for more details.

Tesseract 3.00 adds a number of new languages, including Chinese, Japanese, and Korean. It also introduces a new, single-file based system of managing language data.

Tesseract 3.02 adds BiDirectional text support, the ability to recognize multiple languages in a single image, and improved layout analysis.

For further details, see the file ReleaseNotes included with the distribution.

RESOURCES
---------

Main web site: <http://code.google.com/p/tesseract-ocr/>

Information on training: <http://code.google.com/p/tesseract-ocr/wiki/TrainingTesseract3>

SEE ALSO
--------

ambiguous\_words(1), cntraining(1), combine\_tessdata(1), dawg2wordlist(1), shape\_training(1), mftraining(1), unicharambigs(5), unicharset(5), unicharset\_extractor(1), wordlist2dawg(1)

AUTHOR
------

Tesseract development was led at Hewlett-Packard and Google by Ray Smith. The development team has included:

Ahmad Abdulkader, Chris Newton, Dan Johnson, Dar-Shyang Lee, David Eger, Eric Wiseblatt, Faisal Shafait, Hiroshi Takenaka, Joe Liu, Joern Wanke, Mark Seaman, Mickey Namiki, Nicholas Beato, Oded Fuhrmann, Phil Cheatle, Pingping Xiu, Pong Eksombatchai (Chantat), Ranjith Unnikrishnan, Raquel Romano, Ray Smith, Rika Antonova, Robert Moss, Samuel Charron, Sheelagh Lloyd, Shobhit Saxena, and Thomas Kielbus.

COPYING
-------

Licensed under the Apache License, Version 2.0

* * * * *

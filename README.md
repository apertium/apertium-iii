Sichuan Yi: `apertium-iii`
===============================================================================

This is an Apertium monolingual language package for Sichuan Yi. What
you can use this language package for:

* Morphological analysis of Sichuan Yi
* Morphological generation of Sichuan Yi
* Part-of-speech tagging of Sichuan Yi

Requirements
-------------------------------------------------------------------------------

You will need the following software installed:

* autoconf
* automake
* pkg-config
* lttoolbox (>= 3.5.1)
* apertium (>= 3.6.1)
* vislcg3 (>= 1.3.1)
* hfst (>= 3.15.1)
* lexd (>= 0.0.1)

If this does not make any sense, we recommend you look at: apertium.org.

Compiling
-------------------------------------------------------------------------------

Given the requirements being installed, you should be able to just run:

```bash
$ autoreconf -fvi
$ ./configure
$ make
```

You can use `./autogen.sh` instead of `autoreconf` and `./configure` if you're compiling
from source.

If you're doing development, you don't have to install the data, you
can use it directly from this directory.

If you are installing this language package as a prerequisite for an
Apertium translation pair, then do (typically as root / with sudo):

```bash
$ make install
```

You can use a `--prefix` with `./configure` to install as a non-root user,
but make sure to use the same prefix when installing the translation
pair and any other language packages.

If any of this doesn't make sense or doesn't work, see https://wiki.apertium.org/wiki/Install_language_data_by_compiling

Testing
-------------------------------------------------------------------------------

If you are in the source directory after running make, the following
commands should work:

```console
$ echo "TODO: test sentence" | apertium -d . iii-morph
TODO: test analysis result

$ echo "TODO: test sentence" | apertium -d . iii-tagger
TODO: test tagger result
```

Files and data
-------------------------------------------------------------------------------


* [`apertium-iii.iii.lexd`](apertium-iii.iii.lexd) - Morphotactic dictionary
* [`apertium-iii.iii.twol`](apertium-iii.iii.twol) - Morphophonological rules
* [`apertium-iii.iii.rlx`](apertium-iii.iii.rlx) - Constraint Grammar disambiguation rules
* [`apertium-iii.post-iii.dix`](apertium-iii.post-iii.dix) - Post-generator
* [`iii.prob`](iii.prob) - Tagger model
* [`apertium-iii.iii.spellrelax`](apertium-iii.iii.spellrelax) - Typographical variance rules
* [`apertium-iii.iii.udx`](apertium-iii.iii.udx) - Mappings from Apertium tags to Universal Dependencies features
* [`modes.xml`](modes.xml) - Translation modes

For more information
-------------------------------------------------------------------------------

* https://wiki.apertium.org/wiki/Installation
* https://wiki.apertium.org/wiki/apertium-iii
* https://wiki.apertium.org/wiki/Using_an_lttoolbox_dictionary

Help and support
-------------------------------------------------------------------------------

If you need help using this language pair or data, you can contact:

* Mailing list: apertium-stuff@lists.sourceforge.net
* IRC: `#apertium` on irc.oftc.net (irc://irc.oftc.net/#apertium)

See also the file [`AUTHORS`](AUTHORS), included in this distribution.

More documentations
-------------------------------------------------------------------------------
Refer to our wiki page for more detailed documentations

Nuosu Main Page
https://wikis.swarthmore.edu/ling073/Nuosu

Grammar Documentation
https://wikis.swarthmore.edu/ling073/Nuosu/Grammar

Keyboard Layout Design
https://wikis.swarthmore.edu/ling073/Nuosu/Keyboard

Transducer
https://wikis.swarthmore.edu/ling073/Nuosu/Transducer

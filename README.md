xer
===

Compute useful evaluation metrics (CER, WER, SER, ...)

This tool is useful if you are working on Automatic Speech Recognition,
Handwriting Text Recognition, Optical Character Recognition or Machine
Translation. It computes different evaluation metrics typically used in those
areas, given a set of reference and hypotheses sentences.

The great thing about this tool, which many others lack of, is that you can
choose what is the character encoding set of four text. That way, the tool will
give you meaningful metrics, no matter which encoding uses your data (UTF-8,
ISO-8859-1, Windows-1251, etc). The default is UTF-8, but you can use any of
the encoding sets supported by Python
(https://docs.python.org/2/library/codecs.html#standard-encodings).

## Usage

```
usage: xer [-h] [-r REF] [-t HYP] [-i {-,str,file}] [-s SEP] [-e ENC]

Compute useful evaluation metrics (CER, WER, SER, ...)

optional arguments:
  -h, --help            show this help message and exit
  -r REF, --reference REF
                        reference sentence or file
  -t HYP, --transcription HYP
                        transcription sentence or file
  -i {-,str,file}, --input_source {-,str,file}
                        "-" reads parallel sentences from the standard input,
                        "str" interprets `-r' and `-t' as sentences, and
                        "file" interprets `-r' and `-t' as two parallel files,
                        with one sentence per line (default: -)
  -s SEP, --separator SEP
                        use this string to separate the reference and
                        transcription when reading from the standard input
                        (default: \t)
  -e ENC, --encoding ENC
                        character encoding of the reference and transcription
                        text (default: utf-8)
```

## Examples

### Evaluate a single sentence

Suppose that we want to evaluate an output sentence of our
transcription/translation system with the reference ground-truth. Then, we can
simply run this command:

```
$ xer -i str -r "I'm André" -t "I'm Andrew"
CER: 22.2222%, WER: 50%, SER: 100%
```

Notice that the Character Error Rate (CER) is sensible to the encoding of your
strings. In these example we made 2 character errors (we output "ew", instead
of "é") and the number of characters in the reference is 9, thus the CER is
2/9. However, if we are using UTF-8 which is a multibyte encoding, most
evaluation tools would output a CER of 2/10, because the reference is 10 bytes
long ("é" takes two bytes).

### Evaluate a file

We typically have the output of our system in a separate file, with one
sentence in each line. Then it is strightforward to evaluate the performance of
the sentences in that file, running this command:

```
$ xer -i file -r reference_file -t hypotheses_file
CER: 3.08351%, WER: 13.7931%, SER: 13.7931%
```

### Evaluate sentences from the standard input

If you have your sentences aligned with your reference data in a single file,
you can also directly evaluate that file with the following line:

```
$ cat parallel_file | xer -s '<>'
CER: 1.97002%, WER: 7.58621%, SER: 7.58621%
```

The input format is a file with a pair of sentences in each line, separated by
the string "<>". You can use any string as the separator, but keep in mind that
the separator that you choose **cannot** be present in any of the sentences as
a substring. The default separator is the tab character ("\t").
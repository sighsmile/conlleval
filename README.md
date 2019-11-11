# conlleval-python

**NOTE: This repository is currently not managed. Please check the Issues page for bug reports. I will try to fix it later. Sorry for any inconvenience.**

## Intro

This repository contains two scripts:

- **conlleval_perl.py**: the Python equivalent of the Perl script [`conlleval`](http://www.cnts.ua.ac.be/conll2000/chunking/conlleval.txt), which can be used for measuring the performance of a system that has processed the CoNLL-2000 shared task data. 

- **conlleval.py**: a slight modification on the above script, so that it can be imported and used elsewhere. You will find `import evaluate from conlleval` useful.

For more information on the original Perl script, see http://www.cnts.ua.ac.be/conll2000/chunking/output.html.

## Usage

Both scripts can be used to evaluate from a supported file.

Read from `output.txt` and print the results to the console: 
```
  python conlleval.py < output.txt
```   
or save the results in `result.txt`:
```
  python conlleval.py < output.txt > result.txt
```

And the result is:
```
   processed 961 tokens with 459 phrases; found: 539 phrases; correct: 371.
   accuracy:  84.08%; precision:  68.83%; recall:  80.83%; FB1:  74.35
                ADJP: precision:   0.00%; recall:   0.00%; FB1:   0.00
                ADVP: precision:  45.45%; recall:  62.50%; FB1:  52.63
                  NP: precision:  64.98%; recall:  78.63%; FB1:  71.16
                  PP: precision:  83.18%; recall:  98.89%; FB1:  90.36
                SBAR: precision:  66.67%; recall:  33.33%; FB1:  44.44
                  VP: precision:  69.00%; recall:  79.31%; FB1:  73.80
```         



Options for `conlleval_perl.py` (the same as the original Perl script):

+ -l: Generate output as part of a LaTeX table. The definition of the table can be found in an example document: [latex](http://www.cnts.ua.ac.be/conll2003/ner/example.tex) [ps](http://www.cnts.ua.ac.be/conll2003/ner/example.ps) [pdf](http://www.cnts.ua.ac.be/conll2003/ner/example.pdf)

+ -d char: On each line, use this character rather than whitespace (or tab) as delimiter between tokens
+ -r: Assume raw output tokens, that is without the prefixes B- and I-. In this case each word will be counted as one chunk.
+ -o token: Use token as output tag for items that are outside of chunks or other classes. This option only works when -r is used as well. The default value for the outside output tag is O.

Usage for `conlleval.py`:

```python
from conlleval import evaluate

# print out the table as above
evaluate(true_tags, pred_tags, verbose=True) 

# calculate overall metrics
prec, rec, f1 = evaluate(true_tags, pred_tags, verbose=False)
```

## Data format

**NOTE: This script can be used with IOB2 or IOBES tagging scheme. 
If you are using a different scheme, please convert to IOB2 or IOBES.**

**EDIT: There has been report that IOB2 support is broken (see Issues page). This has not been fixed yet.**

For an example of data format to be used with this script, check out the accompanied `output.txt` file in this repository, or the original source at http://www.cnts.ua.ac.be/conll2000/chunking/output.txt.gz.

Sentences are separated by empty lines. Each line contains at least three columns, seperated by whitespaces (or a character specified in `-d`). The second last column is the chunk tag according to the corpus, and the last column is the predicted chunk tag. The other columns are ignored in evaluation.

Example:

```
   Boeing NNP B-NP I-NP
   's POS B-NP B-NP
   747 CD I-NP I-NP
   jetliners NNS I-NP I-NP
   . . O O
   
   Rockwell NNP B-NP I-NP
   said VBD B-VP B-VP
   the DT B-NP B-NP
   agreement NN I-NP I-NP
```


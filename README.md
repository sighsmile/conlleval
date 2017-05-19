# conlleval-python

This is the Python equivalent of the Perl script [`conlleval`](http://www.cnts.ua.ac.be/conll2000/chunking/conlleval.txt), which can be used for measuring the performance of a system that has processed the CoNLL-2000 shared task data. 

For more information on the original Perl script, see http://www.cnts.ua.ac.be/conll2000/chunking/output.html.

## Usage


Options (the same as the original Perl script):

+ -l: Generate output as part of a LaTeX table. The definition of the table can be found in an example document: [latex](http://www.cnts.ua.ac.be/conll2003/ner/example.tex) [ps](http://www.cnts.ua.ac.be/conll2003/ner/example.ps) [pdf](http://www.cnts.ua.ac.be/conll2003/ner/example.pdf)

+ -d char: On each line, use char rather than whitespace as delimiter between tokens
+ -r: Assume raw output tokens, that is without the prefixes B- and I-. In this case each word will be counted as one chunk.
+ -o token: Use token as output tag for items that are outside of chunks or other classes. This option only works when -r is used as well. The default value for the outside output tag is O.


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

## Data format

For a stanford data format to be used with this script, check out the accompanied `output.txt` file in this repository, or the original source at http://www.cnts.ua.ac.be/conll2000/chunking/output.txt.gz.

Sentences are separated by empty lines. Each line contains at least three columns, seperated by whitespaces (or a character specified in `-d`). The second last column is the chunk tag according to the corpus, and the last column is the predicted chunk tag; in the code, they are referred to as `correct` and `guessed` respectively. The other columns are ignored in evaluation.

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


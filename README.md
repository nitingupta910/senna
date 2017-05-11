SENNA
=====

(version: 3.0)

A fork of SENNA NLP toolkit

homepage: http://ml.nec-labs.com/senna/

SENNA is a software distributed under a non-commercial license, which outputs a host of Natural Language Processing (NLP) predictions: part-of-speech (POS) tags, chunking (CHK), name entity recognition (NER), semantic role labeling (SRL) and syntactic parsing (PSG).

SENNA is fast because it uses a simple architecture, self-contained because it does not rely on the output of existing NLP system, and accurate because it offers state-of-the-art or near state-of-the-art performance.

SENNA is written in ANSI C, with about 3500 lines of code. It requires about 200MB of RAM and should run on any IEEE floating point computer.

Compilation
===========

Compiling SENNA is straightforward, as it is written in ANSI C and does not require external libraries. For speed it is however recommended to use the Intel MKL library.

Linux
=====

In Linux/Unix/MacOS X systems, use gcc compiler:

gcc -o senna -O3 -ffast-math *.c

You might want to add additional suitable optimization flags for your platform. SENNA also compiles fine with the Intel compiler (icc).

If speed is critical, we recommend to compile SENNA with the Intel MKL library, which provides a very efficient BLAS. Add the definition USE_MKL_BLAS, as well as correct MKL libraries and include path.

gcc -o senna -O3 -ffast-math *.c -DUSE_MKL_BLAS [...]

SENNA also compiles with ATLAS BLAS. On our platform, the handcrafted code compiled with the gcc command line shown above was faster. However, if you want to use it, you can compile it with:

gcc -o senna -O3 -ffast-math *.c -DUSE_ATLAS_BLAS [...]

This [link](COMPILING_TESTING_PERFORMANCE.md) provides more details on how to compile, test, and even compare performance against the library used in the build.

Mac OS X
========

Assuming you installed the XCode tools (which are provided on the Mac OS X DVD/CDs), simply compile with gcc:

gcc -o senna -O3 -ffast-math *.c -DUSE_APPLE_BLAS -framework Accelerate

This will compile against Apple BLAS libraries included in your system. As for Linux, it is recommended to use Intel MKL library instead of Apple BLAS libraries. The following command line can be invoked (replacing the dots by the correct library and include paths):

gcc -o senna -O3 -ffast-math *.c -DUSE_MKL_BLAS [...]

Sanity Check
============

Run in a console the following command:

senna < sanity-test-input.txt > sanity-test-result.txt

SENNA should create a file sanity-test-result.txt which should be identical to the provided sanity-test-output.txt file.

The file sanity-test-input.txt comes from the CoNLL 2000 chunking testing set. SENNA will output all tags for this file. It should run in about 90 seconds on a decent computer (using MKL).

Usage
=====

SENNA reads input sentences from the standard input and outputs tags into the standard output. The most likely command line usage for SENNA is therefore:

senna [options] < input.txt > output.txt

Of course you can run SENNA in an interactive mode without the "pipes" < and >.
Each input line is considered as a sentence. SENNA has its own tokenizer for separating words, which can be deactivated with the -usrtokens option.

SENNA outputs one line per "token", with all the corresponding tags (in IOBES format) on the same line. An empty line is inserted between each output sentence. The first column is the token. Tags for all task then follow by default (POS, CHK, NER and SRL). Tags for SRL are preceded by a column which indicates if SENNA considered the token as a SRL verb or not ("-"). Then, there is one column per SRL verb.

SENNA supports the following options:

-h
    Display an inline help.

-verbose
    Display model informations (on the standard error output, so it does not mess up the tag outputs).

-notokentags
    Do not output tokens (first output column).

-offsettags
    Output start/end character offset (in the sentence), for each token.

-iobtags
    Output IOB tags instead of IOBES.

-brackettags
    Output 'bracket' tags instead of IOBES.

-path <path>
    Specify the path to the SENNA data/ and hash/ directories, if you do not run SENNA in its original directory. The path must end by "/".

-usrtokens
    Use user's tokens (space separated) instead of SENNA tokenizer.

-posvbs
    Use verbs outputed by the POS tagger instead of SRL style verbs for SRL task. You might want to use this, as the SRL training task ignore some verbs (many "be" and "have") which might be not what you want.

-usrvbs <file>
    Use user's verbs (given in <file>) instead of SENNA verbs for SRL task. The file must contain one line per token, with an empty line between each sentence. A line which is not a "-" corresponds to a verb.

-pos

-chk

-ner

-srl

-psg

Instead of outputing tags for all tasks, SENNA will output tags for the specified (one or more) tasks.
Remarks

SENNA does not handle -LRB-, -RRB-, ... tokens. Please, replace these tokens in your input text by the appropriate (, ), .... Not replacing these tokens will have an impact on performance (for e.g., POS accuracy goes down, from 97.29% to 97.00%).

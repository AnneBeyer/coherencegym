# CoherenceGym

This repository contains the code for creating the CoherenceGym test suites introduced in our paper [Is Incoherence Surprising? Targeted Evaluation of Coherence Prediction from Language Models](https://www.aclweb.org/anthology/2021.naacl-main.328/) 

It is based on the [SynatxGym](https://cpllab.github.io/syntaxgym-core/) framework for evaluating pre-trained language models on syntactic phenomena and extends this line of work to notions of discourse and dialogue coherence.

## Requirements

Install SyntaxGym as described [here](https://cpllab.github.io/syntaxgym-core/) (requires Python>=3.7)

## Test suite generation

The core of our work is the collection and creation of test suites that target discourse and dialogue coherence phenomena.
We transform some existing test sets into the test suite format specified by SyntaxGym, and extend this set with three new suites.
Each test suite contains different items in two versions each, where one is designed to be more coherent than the other.
All test suites are based on existing corpora, from which we extract sentences or sentence pairs and introduce perturbations that a model
which is able to encode different notions of coherence should find more surprising than the original version.

### Sentence Shuffling

### ROCStories

### Winograd

### *new* Entity re-mention

### *new* Connectives

### *new* Speaker Commitment


## Model Evaluation

Models can be evaluated using the syntaxGym pipeline as follows

```
syntaxgym run gpt2 /path/to/test_suite > gpt2_test-suite.results
syntaxgym run DialoGPT-medium /path/to/test_suite > dialogpt_test-suite.results
```
where the .results files will contain a per-item evalaution of whether the prediction specified in the test suite was met (True or False).

Currently, only the evaluation of GPT-2 is supported in the official installation of syntaxgym. The pull request for DialoGPT is pending and we are still trying to fix some compatibility issues with other models in [`lm-zoo`](https://cpllab.github.io/lm-zoo/), on which SyntaxGym is based. Information on how to include DialoGPT locally can be found in models/Readme.md. 

We further plan to add more models to `lm-zoo` in order to evaluate the impact of different model sizes and architectures. 


## Citation
```
@inproceedings{beyer-etal-2021-incoherence,
    title = "Is Incoherence Surprising? Targeted Evaluation of Coherence Prediction from Language Models",
    author = "Beyer, Anne and Lo{\'a}iciga, Sharid and Schlangen, David",
    booktitle = "Proceedings of the 2021 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies",
    year = "2021",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://www.aclweb.org/anthology/2021.naacl-main.328",
    pages = "4164--4173"
}
```

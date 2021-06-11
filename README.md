# CoherenceGym

This repository contains the code for creating the CoherenceGym test suites introduced in our paper 

[Is Incoherence Surprising? Targeted Evaluation of Coherence Prediction from Language Models](https://www.aclweb.org/anthology/2021.naacl-main.328/) 

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
We include the commonly used sentence suffling approach to break coherence in an uncontrolled way.

#### Data
For discourse data we use the ROCStories corpus. Access can be aquired free of charge [here](https://www.cs.rochester.edu/nlp/rocstories/). We use the 2016 test set ```cloze_test_test__spring2016-cloze_test_ALL_test.tsv```

The PersonaChat corpus is used to represent dialogue data and can be found under ```ParlAI/data/Persona-Chat/personachat``` after [installing ParlAI](https://github.com/facebookresearch/ParlAI#installing-parlai). We use ```test_both_original.txt```.

#### Perturbation
```python de_coherify.py TODO```

### ROCStories
Another previously proposed task the requires a notion of coherence is the Story Cloze task of discriminating a wrong from a right ending for short (5 sentence) stories.
##### Data
For the Story Cloze test suite, we use the same ROCStories data as described above.
##### Perturbation
```python de_coherify.py TODO```

### Winograd
We re-use the Winograd sentence pairs used by Trinh and Le (2019) and Radford et al. (2019) and transform them into the test suite format.
##### Data
The original data can be found here under test_suites/original/wsc273.json
##### Perturbation
```python de_coherify.py TODO```

### *\*new\** Entity re-mention
We extract sentence pairs from the ARRAU corpus and perturb the second mention of coreferent entities from Pronoun to NP repetition.
##### Data
Access to the corpus has to be aquired through the [LDC](https://catalog.ldc.upenn.edu/LDC2013T22)
##### Perturbation
```python de_coherify.py TODO```

### *\*new\** Connectives
The connectives test suite contains manipulations of explicit connectives.
##### Data
The test suite is based on the Disco-Annotation corpus, which is available [here](https://www.idiap.ch/dataset/Disco-Annotation)
##### Perturbation
```python de_coherify.py TODO```

### *\*new\** Speaker Commitment
We construct a dialogue test suite on speaker commitment by combining two segments labeled as contradiction in the DialogueNLI dataset to test whether models are able to detect violations where same speakers contradict themselves vs. two different speakers uttering contradicting sequences.
##### Data
The Dataset is available at https://wellecks.github.io/dialogue_nli/. We use the human verified portion of the test set in ```dialogue_nli_verified_test.jsonl```.
##### Perturbation
```python de_coherify.py TODO```

## Model Evaluation

Models can be evaluated on the test suites using the syntaxGym pipeline as follows

```
syntaxgym run gpt2 /path/to/test_suite > gpt2_test-suite.results
syntaxgym run DialoGPT-medium /path/to/test_suite > dialogpt_test-suite.results
```
where the .results files will contain a per-item evaluation of whether the prediction specified in the test suite was met.

Currently, only the evaluation of GPT-2 is supported in the official installation of syntaxgym. The pull request for DialoGPT is pending and we are still trying to fix some compatibility issues with other models in [`lm-zoo`](https://cpllab.github.io/lm-zoo/), on which SyntaxGym is based. Information on how to include DialoGPT locally can be found in models/Readme.md. 

To evaluate on several models on all (or selected) test suites run
```python eval.py``` 
```TODO: Add more detailed description!```

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

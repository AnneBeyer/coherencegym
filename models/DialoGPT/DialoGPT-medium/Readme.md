This image is adapted from 
```
github.com/cpllab/lm-zoo/tree/master/models/transformers-base
```

## Model: DialoGPT-medium
DialoGPT-medium extends GPT-2-medium by fine tuning on Reddit data in order to model dialogue.
For this, the eos token is re-defined to mark a speaker change (represented by the [SEP] token in the input).

## Citation
```
@inproceedings{zhang-etal-2020-dialogpt,
    title = "{DIALOGPT} : Large-Scale Generative Pre-training for Conversational Response Generation",
    author = "Zhang, Yizhe  and
      Sun, Siqi  and
      Galley, Michel  and
      Chen, Yen-Chun  and
      Brockett, Chris  and
      Gao, Xiang  and
      Gao, Jianfeng  and
      Liu, Jingjing  and
      Dolan, Bill",
    booktitle = "Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics: System Demonstrations",
    year = "2020",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://www.aclweb.org/anthology/2020.acl-demos.30",
    doi = "10.18653/v1/2020.acl-demos.30",
    pages = "270--278",
    abstract = "We present a large, tunable neural conversational response generation model, DIALOGPT (dialogue generative pre-trained transformer). Trained on 147M conversation-like exchanges extracted from Reddit comment chains over a period spanning from 2005 through 2017, DialoGPT extends the Hugging Face PyTorch transformer to attain a performance close to human both in terms of automatic and human evaluation in single-turn dialogue settings. We show that conversational systems that leverage DialoGPT generate more relevant, contentful and context-consistent responses than strong baseline systems. The pre-trained model and training pipeline are publicly released to facilitate research into neural response generation and the development of more intelligent open-domain dialogue systems.",
}
```
## Implementation

* Are you the creator/co-creator of this language model? 
  
No.
* Are you the creator/co-creator of this implementation of this language model? 
  
No. 
* Is this implementation the official implementation of the language model? 
  
Yes. (see https://huggingface.co/microsoft/DialoGPT-medium)
* What licensing restrictions (if any) apply to this implementation of this language model? 
  
MIT License, Copyright (c) Microsoft Corporation.

## Training

* What corpus was this model trained on? 
  * 147M conversation-like exchanges extracted from Reddit comment chains over a period spanning from 2005 through 2017, in total 1.8 billion words, vocabulary of 50,257 (see Zhang et al. for details) 
  * extends the Hugging Face PyTorch transformer with additional Mutual Information Maximization objective
 * If possible, provide some standard performance measures (e.g. test perplexity) and complexity measures (e.g. parameter count, number of layers, etc.):
   * Parameters: 345M, Layers: 24, Embedding size: 1024, Batch size: 64
   * See Tables 2 & 3 in Zhang et al. (2020) for performance measures

## Licensing

MIT License

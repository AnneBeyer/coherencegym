# Adding models to [LM Zoo](https://cpllab.github.io/lm-zoo) to be used with [SyntaxGym](https://github.com/cpllab/syntaxgym-core)

## General setup
To include docker images into LM Zoo locally (for development), copy the file `models.py` into the directory where `lm-zoo` is installed; replacing the original `models.py` there.

How to deploy a model to LM Zoo after development is explained [here](https://cpllab.github.io/lm-zoo/contributing.html)

## Using models in SyntaxGym using docker locally
Availabe models are:

Model | Tag
----- | ---
DialoGPT-small | dialogpt-small:dialogpt-small
DialoGPT-medium | dialogpt-medium:dialogpt-medium
DialoGPT-large | dialogpt-large:dialogpt-large
T5-small | t5-small:t5-small
T5-base | t5-base:t5-base
T5-large | t5-large:t5-large
T5-3b | t5-3b:t5-3b
T5-11b | t5-11b:t5-11b
RobertaUSR | robertausr:robertausr

## DialoGPT-medium as example
1. Navigate to the `DialoGPT/DialoGPT-medium` directory.
1. Build the docker image: `docker build . --tag=dialogpt-medium:dialogpt-medium`. Make sure to give the image this exact tag, as `lm-zoo` finds the image through the tag.
1. Now you can run syntaxgym like so: `syntaxgym run DialoGPT-medium /path/to/test_suite`.
The setup for the other models works analogously.

# Including another model as docker image
1. Copy the directory `DialoGPT-medium` to `NewModel`.
1. Change the `Dockerfile`:
   1. Change the filepath `/opt/dialogpt-medium` (e.g. in line 20) to `/opt/newmodel` (just for clarity).
   1. Replace the download urls in the `curl` commands with the urls for the new model.
      * As of transformers 4.0.0, you can find the urls by going into the `file_utils.py` of your `transformers` installation and inserting a print statement in the function `url_to_filename` to print the url.
      * Open a python shell, import transformers, and create the model and tokenizer, that you want. E.g. `tr.AutoTokenizer.from_pretrained("microsoft/DialoGPT-medium")` and `tr.AutoModel.from_pretrained("microsoft/DialoGPT-medium")`.
      * This will give you a number of possible urls. However, not all of these files exist for each model. Therefore, you should try and download each file manually (`curl url`). If it returns an error like `The specified key does not exist.`, you can ignore that url. It is also possible that curl gives an output like `Found. Redirecting to https://...`. In that case, use the different url given there.
      * Include the found urls in the dockerfile.
   1. Possibly update the versions for PyTorch and the various pip installs to versions working with the new model.
1. Change `bin/spec`:
   1. Change the path for the AutoTokenizer to `/opt/newmodel` (or whatever you set in the dockerfile).
   1. Update the other information about the model as needed. Pay special attention to the `"sentinel_pattern"` of the tokenizer as setting this incorrectly can lead to errors in splitting the results into `syntaxgym` regions.
1. Change `get_surprisals.py`:
   1. In the `main` function, change the paths for the `AutoTokenizer` and the `AutoModel` to `/opt/newmodel`.
   1. Update the `get_suprisals` function, to insert the correct tokens for `[SEP]`. This depends on what separators for utterances your model was trained with.
      1. In line 66 to calculate the length of the resulting string.
      1. And in line 79 to actually insert them.
   1. The function returns a list with suprisal information about each token in the text. This list however, needs to include `[SEP]` again, rather than the separator tokens inserted earlier, otherwise it will lead to errors in `syntaxgym`. (And make comparing models harder.) This is relatively simple when (like for DialoGPT), `[SEP]` was replaced with exactly one token, in which case the replacement can just be reversed. If that is not the case, you need to average the surprisals over the two (or more) inserted tokens and then return that average as surprisal for `[SEP]`. If the model does not insert any tokens in place of `[SEP]`, calculate the surprisals for `[SEP]` as the mean surprisal of the preceding utterance.
1. Change the file paths in `bin/get_surprisals`, `bin/tokenize`, and `bin/unkify` to `/opt/newmodel/{get_surprisals.py, tokenize.py}`.
1. Change `tokenizer.py`:
  1. In `main`, change the path for the `AutoTokenizer`.
  1. Update the `tokenize` and `unkify` functions, to deal with `[SEP]` tokens in the same way as in `get_surprisals.py`. This is neccessary, because the tokenizer will return different results, depending on how the input is split. So if these functions don't match with `get_surprisals.py`, it will cause an error in `syntaxgym`.
1. If you only keep your docker image locally: Change `models.py` for `lm_zoo`.
   1. Add an entry for you new model to the `add_registry` dict. You can mostly copy the one for DialoGPT, but change name, tag, and shortname, so that `lm_zoo` can recognize the new docker image.

# Before uploading an image to `lm-zoo`
Make sure to update the file `spec.template.json` in the directory of the model with the relevant information about the model. I haven't done this so far, since the models still keep changing.

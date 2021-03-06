[中文版本的 README](README.md)
------------------------------

# Use RASA NLU to build a Chinese Natural Language Understanding System (NLU)

This repository provides cutting-edge, detailed and complete guidance for the construction of a Chinese natural language understanding system.

## Online Demo

TODO

## Features
- Provide Chinese Corpus
- Provide corpus conversion tool to help users transfer corpus data
- Provide multiple Chinese language processing workflow based on RASA NLU
- Provide model performance evaluation tools to help automatically select and optimize models

## System Requirements

Python 3 (perhaps supporting python2 but not tested well)

## Process Flow

For more information, visit [workflow.md](workflow.md)

## available pipeline list
### MITIE+jieba
#### description
* jieba provide tokenizer for Chinese
* MITIE is used for `intent classification` and `slot filling`
#### Install Dependent Packages
```bash
pip install git+https://github.com/mit-nlp/MITIE.git
Pip install jieba
```
#### Download the required model data
MITIE needs a model file, in my another project: [MITIE_Chinese_Wikipedia_corpus] (https://github.com/howl-anderson/MITIE_Chinese_Wikipedia_corpus) [release] (https://github.com/howl-anderson/MITIE_Chinese_Wikipedia_corpus/ Releases) Download `total_word_feature_extractor.dat.tar.gz`. Extract `total_word_feature_extractor.dat` to `data`
#### pipeline
```yaml
language: "zh"

pipeline:
- name: "nlp_mitie"
  Model: "data/total_word_feature_extractor.dat"
- name: "tokenizer_jieba"
- name: "ner_mitie"
- name: "ner_synonyms"
- name: "intent_featurizer_mitie"
- name: "intent_classifier_sklearn"
```

#### Training Script
```bash
trainer/MITIE+jieba.bash
```

#### Evaluation Script
```bash
cross_validation/MITIE+jieba.bash
```

### tensorflow_embedding
#### description
* jieba provide tokenizer for Chinese
* tensorflow_embedding used for `intent classification`
* MITIE is used for `slot filling`

#### Install Dependent Packages
```bash
pip install git+https://github.com/mit-nlp/MITIE.git
Pip install jieba
pip install tensorflow
```
#### Download the required model data
MITIE needs a model file, in my another project: [MITIE_Chinese_Wikipedia_corpus] (https://github.com/howl-anderson/MITIE_Chinese_Wikipedia_corpus) [release] (https://github.com/howl-anderson/MITIE_Chinese_Wikipedia_corpus/ Releases) Download `total_word_feature_extractor.dat.tar.gz`. Extract `total_word_feature_extractor.dat` to `data`
#### pipeline
```yaml
language: "zh"

pipeline:
- name: "nlp_mitie"
  model: "data/total_word_feature_extractor.dat"
- name: "tokenizer_jieba"
- name: "intent_featurizer_count_vectors"
- name: "intent_classifier_tensorflow_embedding"
- name: "ner_mitie"
- name: "ner_synonyms"
```

#### Training Script
```bash
trainer/tensorflow_embedding.bash
```

#### Evaluation Script
```bash
cross_validation/tensorflow_embedding.bash
```

### spacy
#### description
* [Chinese_models_for_SpaCy](https://github.com/howl-anderson/Chinese_models_for_SpaCy) used for `intent classification` and `slot filling`
#### Install Dependent Packages
```bash
pip install https://github.com/howl-anderson/Chinese_models_for_SpaCy/releases/download/v2.0.3/zh_core_web_sm-2.0.3.tar.gz
./spacy_model_link.bash
```
#### pipeline
```yaml
language: "zh"

pipeline:
- name: "nlp_spacy"
  model: "zh"
- name: "tokenizer_spacy"
- name: "intent_entity_featurizer_regex"
- name: "intent_featurizer_spacy"
- name: "ner_crf"
- name: "ner_synonyms"
- name: "intent_classifier_sklearn"
```

#### Training Script
```bash
trainer/spacy.bash
```

#### Evaluation Script
```bash
cross_validation/spacy.bash
```


## Performance Testing
### DialogFlow > weather
<table>
    <thead>
    <tr>
        <th></th>
        <th colspan="6">Intent</th>
        <th colspan="6">Entity</th>
    </tr>
    <tr>
        <th></th>
        <th colspan="3">train</th>
        <th colspan="3">test</th>
        <th colspan="3">train</th>
        <th colspan="3">test</th>
    </tr>
    <tr>
        <th>No</th>
        <th>ACC</th>
        <th>F1</th>
        <th>PRC</th>
        <th>ACC</th>
        <th>F1</th>
        <th>PRC</th>
        <th>ACC</th>
        <th>F1</th>
        <th>PRC</th>
        <th>ACC</th>
        <th>F1</th>
        <th>PRC</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>1</td>
        <td>0.986</td>
        <td>0.986</td>
        <td>0.986</td>
        <td>0.665</td>
        <td>0.631</td>
        <td>0.648</td>
        <td>0.987</td>
        <td>0.987</td>
        <td>0.988</td>
        <td>0.967</td>
        <td>0.968</td>
        <td>0.973</td>
    </tr>
    <tr>
        <td>2</td>
        <td>0.990</td>
        <td>0.990</td>
        <td>0.990</td>
        <td>0.434</td>
        <td>0.406</td>
        <td>0.432</td>
        <td>0.987</td>
        <td>0.987</td>
        <td>0.988</td>
        <td>0.968</td>
        <td>0.970</td>
        <td>0.975</td>
    </tr>
    <tr>
        <td>3</td>
        <td>0.992</td>
        <td>0.992</td>
        <td>0.992</td>
        <td>0.657</td>
        <td>0.598</td>
        <td>0.587</td>
        <td>0.987</td>
        <td>0.987</td>
        <td>0.988</td>
        <td>0.939</td>
        <td>0.934</td>
        <td>0.947</td>
    </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="13">
                ACC: Accuracy; F1: F1-score; PRC: Precision;
            </td>
        </tr>
    </tfoot>
</table>

### Model List

| No  | Pipeline             | Configure                                                                                   |
|-----|----------------------|---------------------------------------------------------------------------------------------|
| 1   | MITIE+jieba          | `total_word_feature_extractor.dat` provided by the `MITIE_Chinese_Wikipedia_corpus` project |
| 2   | tensorflow_embedding | `total_word_feature_extractor.dat` provided by the `MITIE_Chinese_Wikipedia_corpus` project |
| 3   | spacy                | Chinese SpaCy model provided by the `Chinese_models_for_SpaCy` project                      |

## How to contribute

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) and submit pull requests to us.

## Version Control

We use [SemVer](http://semver.org/) as a versioned standard. See `tags` for all versions.

## Authors

* **Xiaoquan Kong** - *Initial work* - [howl-anderson](https://github.com/howl-anderson)

For more contributor information, please refer to `contributors`.

## Copyright

MIT License - See [LICENSE.md](LICENSE.md) for details

## Acknowledges

* TODO
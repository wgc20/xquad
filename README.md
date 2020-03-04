# XQuAD

XQuAD (Cross-lingual Question Answering Dataset) is a benchmark dataset for evaluating cross-lingual question answering performance.
The dataset consists of a subset of 240 paragraphs and 1190 question-answer pairs from
the development set of SQuAD v1.1 [(Rajpurkar et al., 2016)](https://www.aclweb.org/anthology/D16-1264/) together with their professional
translations into ten languages: Spanish, German, Greek, Russian, Turkish, Arabic, Vietnamese, Thai, Chinese, and Hindi.
Consequently, the dataset is _entirely parallel_.

For more information on how the dataset was created, refer to our paper,
[On the Cross-lingual Transferability of Monolingual Representations](https://arxiv.org/abs/1910.11856).

All files are in json format following the SQuAD dataset format. 

![An example][xquad_example.png]

## Data

This directory contains files in the following languages:
- Arabic: `xquad.ar.json`
- German: `xquad.de.json`
- Greek: `xquad.el.json`
- English: `xquad.en.json`
- Spanish: `xquad.es.json`
- Hindi: `xquad.hi.json`
- Russian: `xquad.ru.json`
- Thai: `xquad.th.json`
- Turkish: `xquad.tr.json`
- Vietnamese: `xquad.vi.json`
- Chinese: `xquad.zh.json`

As the dataset is based on SQuAD v1.1, there are no unanswerable questions in the data. We chose this
setting so that models can focus on cross-lingual transfer.

We show the average number of tokens per paragraph, question, and answer for each language in the
table below. The statistics were obtained using [Jieba](https://github.com/fxsjy/jieba) for Chinese
and the [Moses tokenizer](https://github.com/moses-smt/mosesdecoder/blob/master/scripts/tokenizer/tokenizer.perl)
for the other languages. 

|           |   en  |   es  |   de  |   el  |   ru  |   tr  |   ar  |   vi  |   th  |   zh  |   hi  |
|-----------|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| Paragraph | 142.4 | 160.7 | 139.5 | 149.6 | 133.9 | 126.5 | 128.2 | 191.2 | 158.7 | 147.6 | 232.4 |
| Question  |  11.5 |  13.4 |  11.0 |  11.7 |  10.0 |  9.8  |  10.7 |  14.8 |  11.5 |  10.5 |  18.7 |
| Answer    |  3.1  |  3.6  |  3.0  |  3.3  |  3.1  |  3.1  |  3.1  |  4.5  |  4.1  |  3.5  |  5.6  |

## Training and evaluation

In order to evaluate on XQuAD, models should be trained on the SQuAD v1.1 training file. which can be
downloaded from [here](https://github.com/rajpurkar/SQuAD-explorer/blob/master/dataset/train-v1.1.json). 
Model validation similarly should be conducted on the SQuAD v1.1 validation file.

For evaluation, we use the official SQuAD `evaluate-v1.1.py` script, which can be obtained from
[here](https://raw.githubusercontent.com/allenai/bi-att-flow/master/squad/evaluate-v1.1.py). Note that 
the SQuAD evaluation script normalises the answer based on heuristics that are specific to English.
We have observed language-specific normalisation heuristics only to have a marginal impact on performance,
which is why we use the English SQuAD v1.1 evaluation script for convenience.

## Baselines

We show results with baseline methods in the table below. For translate-train, 
we fine-tune mBERT on the SQuAD v1.1 training data, which has been automatically translated
to the target language. For translate-test, we fine-tune BERT-Large on the SQuAD v1.1 training set
and evaluate it on the XQuAD test set of the target language, which has been automatically translated to English.

| Model                 |      en     |      ar     |      de     |      el     |      es     |      hi     |      ru     |      th     |      tr     |      vi     |      zh     |     avg     |
|-----------------------|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|
| mBERT                 | 83.5 / 72.2 | 61.5 / 45.1 | 70.6 / 54.0 | 62.6 / 44.9 | 75.5 / 56.9 | 59.2 / 46.0 | 71.3 / 53.3 | 42.7 / 33.5 | 55.4 / 40.1 | 69.5 / 49.6 | 58.0 / 48.3 | 64.5 / 49.4 |
| XLM-R                 | 86.5 / 75.7 | 68.6 / 49.0 | 80.4 / 63.4 | 79.8 / 61.7 | 82.0 / 63.9 | 76.7 / 59.7 | 80.1 / 64.3 | 74.2 / 62.8 | 75.9 / 59.3 | 79.1 / 59.0 | 59.3 / 50.0 | 76.6 / 60.8 |
| Translate-train mBERT | 83.5 / 72.2 | 68.0 / 51.1 | 75.6 / 60.7 | 70.0 / 53.0 | 80.2 / 63.1 | 69.6 / 55.4 | 75.0 / 59.7 | 36.9 / 33.5 | 68.9 / 54.8 | 75.6 / 56.2 | 66.2 / 56.6 | 70.0 / 56.0 |
| Translate-test BERT-L | 87.9 / 77.1 | 73.7 / 58.8 | 79.8 / 66.7 | 79.4 / 65.5 | 82.0 / 68.4 | 74.9 / 60.1 | 79.9 / 66.7 | 64.6 / 50.0 | 67.4 / 49.6 | 76.3 / 61.5 | 73.7 / 59.1 | 76.3 / 62.1 |

## Best practices

XQuAD is intended as an evaluation corpus for _zero-shot_ cross-lingual transfer.
Evaluation on the test data should ideally only be conducted at the very end of
the experimentation in order to avoid overfitting to the data.

If you are evaluating on XQuAD in the zero-shot setting, please state explicitly 
your experimental settings, particularly what monolingual and cross-lingual data 
you used for pre-training and fine-tuning your model.

## Reference

If you use this dataset, please cite [[1]](https://arxiv.org/abs/1910.11856):

[1] Artetxe, M., Ruder, S., & Yogatama, D. (2019). [On the cross-lingual transferability of monolingual representations](https://arxiv.org/abs/1910.11856). arXiv preprint arXiv:1910.11856.

```
@article{Artetxe:etal:2019,
      author    = {Mikel Artetxe and Sebastian Ruder and Dani Yogatama},
      title     = {On the cross-lingual transferability of monolingual representations},
      journal   = {CoRR},
      volume    = {abs/1910.11856},
      year      = {2019},
      archivePrefix = {arXiv},
      eprint    = {1910.11856}
}
```

## License

This dataset is distributed under the [CC BY-SA 4.0 license](https://creativecommons.org/licenses/by-sa/4.0/legalcode).

This is not an officially supported Google product.

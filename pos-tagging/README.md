# Part-of-Speech (POS) Tagging

This is a simple tutorial for POS tagging using the scripts here.

1. Extract `brown.zip` to get `brown.tagged.merged.uniq.ptb`, a slightly refined version of [the Brown Corpus](https://en.wikipedia.org/wiki/Brown_Corpus).

```
unzip brown.zip
```

2. You can train models with your own tagset by running a script like `brown2ptb.py`, which converts the Brown Corpus tagset to a Penn Treebank (PTB) like tagset.

```
python3 brown2ptb.py
```

3. Modify the following settings in `parameters.py` for a sequence labelling task.

```
UNIT = "word"
TASK = None
```

4. Split the data into training, test and validation sets and run `prepare.py` to make CSV and index files.

```
shuf brown.tagged.merged.uniq.ptb > data
head -1000 data > test
sed -n '1001,2000p' data > valid
tail +2001 data > train
python3 prepare.py train
```

5. Train your model. You can also modify the hyperparameters in `parameters.py`.

```
python3 train.py model train.char_to_idx train.word_to_idx train.tag_to_idx train.csv valid 100
```

6. Predict and evaluate your model.

```
python3 predict.py model.epoch100 train.char_to_idx train.word_to_idx train.tag_to_idx test
python3 evaluate.py model.epoch100 train.char_to_idx train.word_to_idx train.tag_to_idx test
```

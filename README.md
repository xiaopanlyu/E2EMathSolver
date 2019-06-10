# End-to-End Math Solver

The implementation of the NAACL 2019 paper [Semantically-Aligned Equation Generation for Solving and Reasoning Math Word Problems](https://arxiv.org/abs/1811.00720).

## Usage

###0. Download required files:
   - Math23K dataset. If it is not in the valid JSON format, you need to fix it.
   revise JSON format like:
   [
   {
   "id":"20268",
    "original_text":"一个数减去它的15%后再乘(5/34)，积是(1/8)，求这个数．",
    "segmented_text":"一 个数 减去 它 的 15% 后 再 乘 (5/34) ， 积 是 (1/8) ， 求 这个 数 ．",
    "equation":"x=(1/8)/(5/34)/(1-15%)",
    "ans":"1"
    },
   { 
   "id":"20271",
    "original_text":"张家村今年春季植树1480棵，比李家村植树的棵数少245棵，李家村今年植树多少棵？",
    "segmented_text":"张家村 今年 春季 植树 1480 棵 ， 比 李家 村 植树 的 棵 数 少 245 棵 ， 李家 村 今年 植树 多少 棵 ？",
    "equation":"x=1480+245",
    "ans":"1725"
   }]
   - [Chinese FastText word vectors](https://dl.fbaipublicfiles.com/fasttext/vectors-crawl/cc.zh.300.vec.gz)
###1. Preprocess dataset:

```
cd src/
mkdir ../data/
python make_train_valid.py ~/storage/MWP/data/math23k_fix.json ~/storage/cc.zh.300.vec ../data/train_valid.pkl --valid_ratio 0 --char_based
```
Note that the warning is not unexpected, because some of the problems use operator out of `+, -, *, /`. The purpose of the flags are as follow:
   - `--valid-ratio 0`: Set the ratio the validation dataset should take. It should be set to 0 when running 5-fold cross validation.
   - `--char_based`: Specify if you want to tokenize the problem text into characters rather into words.
2. Start 5-fold cross-validation by
```
mkdir ../models
python train.py ../data/train_valid.pkl ../models/model.pkl --five_fold
```

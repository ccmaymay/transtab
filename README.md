# Fork: TransTab: A flexible transferable tabular learning framework

[![License](https://img.shields.io/badge/License-BSD_2--Clause-orange.svg)](https://opensource.org/licenses/BSD-2-Clause)


## Links from Original Repository

* [Repository](https://github.com/RyanWangZf/transtab)
* [Package Documentation](https://transtab.readthedocs.io/en/latest/index.html)
* [Paper](https://arxiv.org/abs/2205.09328)
* [5 min blog to understand TransTab](https://realsunlab.medium.com/transtab-learning-transferable-tabular-transformers-across-tables-1e34eec161b8)!


## Getting Started

Install this fork of the transtab package directly from GitHub:

```bash
pip install git+https://github.com/ccmaymay/transtab.git
```

This repository provides the python package `transtab`, which can be used as follows:

```python
import transtab

# load dataset by specifying dataset name
allset, trainset, valset, testset, cat_cols, num_cols, bin_cols \
     = transtab.load_data('credit-g')

# build classifier
model = transtab.build_classifier(cat_cols, num_cols, bin_cols)

# build regressor
# model = transtab.build_regressor(cat_cols, num_cols, bin_cols)

# start training
transtab.train(model, trainset, valset, **training_arguments)

# make predictions, df_x is a pd.DataFrame with shape (n, d)
# return the predictions ypred with shape (n, 1) if binary classification;
# (n, n_class) if multiclass classification.
ypred = transtab.predict(model, df_x)
```


## Installation

This fork of transtab is tested on Python 3.10.

Install the forked package directly from GitHub:

```bash
pip install git+https://github.com/ccmaymay/transtab.git
```


## Transfer learning across tables

A novel feature of `transtab` is its ability to learn from multiple distinct tables. It is easy to trigger the training like

```python
# load the pretrained transtab model
model = transtab.build_classifier(checkpoint='./ckpt')

# load a new tabular dataset
allset, trainset, valset, testset, cat_cols, num_cols, bin_cols \
     = transtab.load_data('credit-approval')

# update categorical/numerical/binary column map of the loaded model
model.update({'cat':cat_cols,'num':num_cols,'bin':bin_cols})

# then we just trigger the training on the new data
transtab.train(model, trainset, valset, **training_arguments)
```


## Contrastive pretraining on multiple tables

We can also conduct contrastive pretraining on multiple distinct tables like

```python
# load from multiple tabular datasets
dataname_list = ['credit-g', 'credit-approval']
allset, trainset, valset, testset, cat_cols, num_cols, bin_cols \
     = transtab.load_data(dataname_list)

# build contrastive learner, set supervised=True for supervised VPCL
model, collate_fn = transtab.build_contrastive_learner(
    cat_cols, num_cols, bin_cols, supervised=True)

# start contrastive pretraining training
transtab.train(model, trainset, valset, collate_fn=collate_fn, **training_arguments)
```


## Citation

If you find transtab package useful, please cite the original paper:

```latex
@inproceedings{wang2022transtab,
  title={TransTab: Learning Transferable Tabular Transformers Across Tables},
  author={Wang, Zifeng and Sun, Jimeng},
  booktitle={Advances in Neural Information Processing Systems},
  year={2022}
}
```

Please also give this fork a shoutout. :)

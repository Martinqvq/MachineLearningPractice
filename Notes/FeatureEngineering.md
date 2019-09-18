# Feature Engineering

[Data Preprocessing](MachineLearningConcepts.md#Data-Preprocessing)

## Concept of Data

* **Structured data**
  * numeric data
  * catagorical data
* **Non-structured data**
  * text
  * image
  * sound
  * video

## Tips for Structured Data

### Tips for Categorical Feature

* [Sample categorical feature encoding methods](https://www.kaggle.com/c/avito-demand-prediction/discussion/55521)
  * label-encode
  * mean-encoding (be careful, easy over fitting if you have not a right validation strategy)
  * factorize-encoding
  * frequency-encoding

* [Cat2Vec (paper pdf)](https://openreview.net/pdf?id=HyNxRZ9xg) - LEARNING DISTRIBUTED REPRESENTATION OF MULTI-FIELD CATEGORICAL DATA

### Encoding

* Ordinal Encoding (序號編碼)
* One-Hot (benefit for linear models or NN)
  * use sparse vector to reduce space
  * co-operate with feature selection to reduce dimension
    * if dimension is too high => hard to calculate distance, and easy to overfit due to too many parameters
  * Python
    * [pandas.get_dummies](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.get_dummies.html)
* Binary Encoding

### Tips for Continuous Feature

* mean
* sum
* max
* min
* std
* var
* np.ptp
* ... TBD

## Tips for Non-structured Data

### Tips for Text Data

[Information Retrieval - Text Representation](Information_Retrieval.md#Text-Representation)

* Bag-of-Words + TF-IDF & N-gram

* Topic Model
  * A model based on probability graph => generative model
  * Infer the latent variable (i.e. the topic)
  * Example
    * LDA
* Word Embedding
  * Using neural network model
  * Example
    * Word2Vec

#### Word Stemming

Changing a different type words into same word

e.g. speek, spoke, spoken => speek

#### Word2Vec vs. LDA

* Word2Vec: Use neural network to generate a dense representation of a word
  * can be considered as learning the "context-word" matrix
  * model
    * CBOW (Continues Bag of Words)
    * Skip-gram
* LDA: Use the "showing-at-the-same-time" relationship to cluster word with "topic"
  * can be considered as factorize the "document-word" matrix

### Tips for Image Data

Prior hypothesis/information

* on model
  * Transfer Learning
* on condition
* on constrain
* on dataset
  * Data Augmentation

Model

* Transfer Learning + Fine-tune
* GAN

#### Data Augmentation

1. rotate, shift, scale, cut, fill, flip, ...
2. add noise
3. change color
4. change brightness, clarity, contrast, sharpness

## Tips for Special Types of Data

### Tips for Count Feature

* Unique length
* Data length
* ...

### Tips for Time Series Feature

#### First Derivative

#### Descrete Fourier Transform

* [What FFT descriptors should be used as feature to implement classification or clustering algorithm?](https://stackoverflow.com/questions/27546476/what-fft-descriptors-should-be-used-as-feature-to-implement-classification-or-cl)
* [numpy.fft](https://docs.scipy.org/doc/numpy/reference/generated/numpy.fft.fft.html)

tsfresh

* [tsfresh document](https://tsfresh.readthedocs.io/en/latest/index.html)
* [tsfresh github](https://github.com/blue-yonder/tsfresh)

## Tips for Getting New Feature

Model may not extract some message like human does. We can build new feature by combining other features to gain accuracy.

* Think about business situation
* Learn from the open source kernel and the discussion session

### What to define a good feature

* This feature gain a lot of improvement
* **Feature importance**
  * LightGBM
    * lgb.plot_importance(lgb_model, max_num_features=10)
    * feature_name = lgb_model.feature_name()
    * importance = lgb_model.feature_importance(importance_type='split')
  * XGBoost
    * xgb.plot_importance(xgb_model, max_num_features=25)
    * xgb_model.get_fscore()

### Feature Combination

* Naive combine
* Combine and represent with lower dimension

## Tips for Memory Usage

Feature engineering will need lots of RAM (usually > 16GB).

### Pandas

#### astype

Most of the time, you won't need very precise data. Use not-so-precise data type will save memory and time.

* float: float32 (default float64)
* string: int32

#### [pandas.get_dummies](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.get_dummies.html)

#### [dataframe.groupby.agg](https://pandas.pydata.org/pandas-docs/version/0.22/generated/pandas.core.groupby.DataFrameGroupBy.agg.html)

* [Summarising, Aggregating, and Grouping data in Python Pandas](https://www.shanelynn.ie/summarising-aggregation-and-grouping-data-in-python-pandas/)

- np.ptp (peak to peak)
  - Range of values (maximum - minimum) along an axis.

#### [pandas.Series.dt.month](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dt.month.html)

### Pickle

#### MacOS file large than 4GB

* [stackoverflow](https://stackoverflow.com/questions/31468117/python-3-can-pickle-handle-byte-objects-larger-than-4gb)

```py
def dump_bigger(data, output_file):
    """
    pickle.dump big file which size more than 4GB
    """
    max_bytes = 2**31 - 1
    bytes_out = pickle.dumps(data, protocol=4)
    with open(output_file, 'wb') as f_out:
        for idx in range(0, len(bytes_out), max_bytes):
            f_out.write(bytes_out[idx:idx + max_bytes])


def load_bigger(input_file):
    """
    pickle.load big file which size more than 4GB
    """
    max_bytes = 2**31 - 1
    bytes_in = bytearray(0)
    input_size = os.path.getsize(input_file)
    with open(input_file, 'rb') as f_in:
        for _ in range(0, input_size, max_bytes):
            bytes_in += f_in.read(max_bytes)
    return pickle.loads(bytes_in)
```

## Tips for Fast Evaluation

### [Compiling Python code with @jit](http://numba.pydata.org/numba-doc/0.17.0/user/jit.html)

## Tips for parameter

### Gain parameter

#### Bayesian Optimization

#### Optuna

#### Hyperopt

### Test parameter

#### K-Fold Cross-Validation

## Links

* [Wiki - Feature engineering](https://en.wikipedia.org/wiki/Feature_engineering)
* [機器學習筆記 - 特徵工程](https://feisky.xyz/machine-learning/basic/feature-engineering.html)
* [**Understanding Feature Engineering (Part 1) — Continuous Numeric Data**](https://towardsdatascience.com/understanding-feature-engineering-part-1-continuous-numeric-data-da4e47099a7b)

### Good Example

* [Data Fountain光伏發電量預測 Top1 開源分享](https://zhuanlan.zhihu.com/p/44755488?utm_source=qq&utm_medium=social&utm_oi=623925402599559168)

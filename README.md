# Selection pipeline for _hctsa_ features
This is a pipeline for selecting small time-series feature sets from the comprehensive feature collection contained in the [_hctsa_ toolbox](https://github.com/benfulcher/hctsa).
Features are selected by their classification performance across a collection of time-series classification problems.
The pipeline was used to generate the small feature set [_catch22_](https://github.com/chlubba/catch22) - CAnonical Time-series CHaracteristics based on the problems contained in the [UEA/UCR time-series classification repository](http://timeseriesclassification.com).

For information the pipeline and the _catch22_ feature set see our preprint:

* C.H. Lubba, S.S. Sethi, P. Knaute, S.R. Schultz, B.D. Fulcher, N.S. Jones. [_catch22_: CAnonical Time-series CHaracteristics](https://doi.org/10.1007/s10618-019-00647-x). *Data Mining and Knowledge Discovery* (2019).

For information on the full _hctsa_ library of over 7000 features, see the following (open-access) publications:

* B.D. Fulcher and N.S. Jones. [_hctsa_: A computational framework for automated time-series phenotyping using massive feature extraction](http://www.cell.com/cell-systems/fulltext/S2405-4712\(17\)30438-6). *Cell Systems* **5**, 527 (2017).
* B.D. Fulcher, M.A. Little, N.S. Jones [Highly comparative time-series analysis: the empirical structure of time series and their methods](http://rsif.royalsocietypublishing.org/content/10/83/20130048.full). *J. Roy. Soc. Interface* **10**, 83 (2013).

# Running the pipeline

## Computing the _hctsa_-matrices

The selection process relies on computed and normalized feature-matrices from the _hctsa_ toolbox.

:wave::wave::wave: ***Computed data (using v0.97 of _hctsa_) that we used for our analysis can be downloaded from [this figshare repository](https://figshare.com/articles/Computed_HCTSA_matrices_for_the_UEA_UCR_2018_time-series_classification_tasks/6865163).*** :wave::wave::wave:

See [_hctsa_](https://github.com/benfulcher/hctsa) for instructions on how to compute the construct _hctsa_ files from your data, run the features and normalize the matrices (_hctsa_ relies on Matlab).
The computed, normalized HCTSA mat files should be placed into a folder called `input_data` inside the `op_importance` folder with file names `HCSTA_<dataset name>_N.mat`.


## Running the `op_importance` pipeline

The pipeline can be launched from the `op_importance` directory as

```
python Workflow.py <runtype>
```

Where `<runtype>` is a string composed of 2-3 parts delimited by an underscore: `<classifier>_<normalisation>(_null)`. Where `<classifier>` selects the classifier type used among `svm`, `dectree`, `linear` and `normalisation` is either `scaledrobustsigmoid` or `maxmin`. An appended `_null` in the `<runtype>`-string means that distributions of classification accuracies for each feature are generated in a permutation-based procedure that shuffles the labels of the classification problems.

### First step: compute null accuracy distributions

First, null distributions need to by generated by e.g.,

```
python Workflow.py dectree_maxmin_null
```

This can take long as 1000 classification runs are done on each dataset. It it preferable to do this computation on a cluster.

Make sure that `compute_features = True` is set in the main function of `Workflow.py`.

### Second step: compute valid accuracy distributions

```
python Workflow.py dectree_maxmin_null
```

### Third step: further analyses

Once the valid and null accuracies have been computed, all following analyses can be run without re-classification by setting `compute_features = False`.

See below for an example output of the pipeline that plots correlations in performance across datasets of the 500 best features as well as the clusters they end up in.

![Example output](example_pipeline_output.png?raw=true "Example output")

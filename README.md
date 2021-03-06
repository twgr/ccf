
<!-- README.md is generated from README.Rmd. Please edit that file -->
[![Build Status](https://travis-ci.org/jandob/ccf.svg?branch=master)](https://travis-ci.org/jandob/ccf) [![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/ccf)](http://cran.r-project.org/package=ccf) [![Coverage Status](https://img.shields.io/codecov/c/github/jandob/ccf/master.svg)](https://codecov.io/github/jandob/ccf?branch=master)

The package `ccf` implements canonical correlations forests (CCFs) for use inside R. These present a novel classification algorithm for machine learning tasks, that are often able outperform common methods for predictive classifiers. The CCF algorithm is based on an ensemble of decision trees together with a canonical correlation analysis. The purpose is to de-correlated individual trees and thus improve the predictive performance.

Brief summary of CCF algorithm
------------------------------

A decision tree is a predictive model that sequentially divides the input space, each for which a local classification and regression model is calculated (e.g. with a simple majority vote). Thereby, it generates a tree-like structure, whose leaves usually group data points belonging ideally to the same class. One can often achieve a better performance by combining individual trees and average over them. This is known as a decision forest or random forest.

A canonical correlation forest is a now tree ensemble method. While the concept is similar to a forest, its specific characteristics often achieve a favorable predictive performance. It trains the trees by using a canonical correlation analysis (CCA) in order to find a feature projection that gives a maximal correlation between features. It then chooses the best split in this projected space.

For a thorough explanation and derivation refer to:

-   Rainforth, T., and Wood, F. (2015): [Canonical correlation forest](https://arxiv.org/pdf/1507.05444.pdf), arXiv preprint, arXiv:1507.05444.

Overview
--------

The most important functions in `ccf` are:

-   `canonical_correlation_forest()` compute classifier based on canonical correlation forests. It supports both a matrix-like input, as well as the common convention using a `formula`.

-   `predict()` applies classifier to unseen data and predicts the class outcome.

-   `plot` visualizates the underlying decision surface.

To see examples of these functions in use, check out the help pages, the demos and this README (which is identical to the vignette).

Installation
------------

Using the **devtools** package, you can easily install the latest development version of `ccf` with

``` r
install.packages("devtools")

# Option 1: download and install latest version from ‘GitHub’
devtools::install_github("jandob/ccf")

# Option 2: install directly from bundled archive
# devtoos::install_local("ccf_0.1.0.tar.gz")
```

Notes:

-   In the case of option 2, you have to specify the path either to the directory of `ccf` or to the bundled archive **ccf\_1.0.0.tar.gz**

-   A CRAN version has not yet been released, but we are working on it. This also applies to the integration into predictive frameworks such as `caret` or `mlr`.

Usage
-----

This section shows the basic functionality of how to train a canonical correlation forests and make predictions based on it. First, load the corresponding package `ccf`.

``` r
library(ccf)
```

The interface follows common R conventions as used by other machine learning routines. Therefore, the usage is fairly straightforward.

``` r
# load sample dataset
data(spirals)

d_train <- spirals[1:1000, ]
d_test <- tail(spirals, 1000)

# compute classifier on training data
## variant 1: matrix input
m1 <- canonical_correlation_forest(d_train[, c("x", "y")], d_train$class, ntree = 10)
## variant 2: formula notation
#m2 <- canonical_correlation_forest(class ~ ., d_train)

# compute predictive accuracy
#get_missclassification_rate(m1, d_test)
#get_missclassification_rate(m2, d_test)

# plot the decision surface of the classifier
ccf_plot <- plot_decision_surface(
  m1, d_test[, c("x", "y")], d_test$class, title = "CCF with 20 trees")
```

License
-------

`ccf` is released under the [MIT License](https://opensource.org/licenses/MIT)

Copyright (c) 2016 Janosch Dobler & Stefan Feuerriegel

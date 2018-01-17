
<!-- README.md is generated from README.Rmd. Please edit that file -->
R/`tstmle`
==========

[![Travis-CI Build Status](https://travis-ci.org/podTockom/tstmle.svg?branch=master)](https://travis-ci.org/podTockom/tstmle) [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/podTockom/tstmle?branch=master&svg=true)](https://ci.appveyor.com/project/podTockom/tstmle) [![Coverage Status](https://img.shields.io/codecov/c/github/podTockom/tstmle/master.svg)](https://codecov.io/github/podTockom/tstmle?branch=master) [![Project Status: WIP - Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](http://www.repostatus.org/badges/latest/wip.svg)](http://www.repostatus.org/#wip) [![MIT license](http://img.shields.io/badge/license-MIT-brightgreen.svg)](http://opensource.org/licenses/MIT)

> Data-adaptive Estimation and Inference for Causal Effects with a Single Time Series

**Authors:** [Ivana Malenica](https://github.com/podTockom)

What's `tstmle`?
----------------

The `tstmle` package implements targeted maximum likelihood estimation (TMLE) of different causal effects based on the observation of a single time series. We consider the case where we observe a single sequence of dependent random variables *O*(1),…*O*(*n*), where each *O*(*t*) with *t* ∈ {1, …*n*} takes values in **R**<sup>*p*</sup>. Further, we assume that at each time *t*, we have a chronological order of the exposure *A*(*t*), outcome *Y*(*t*), and the covariate vector *W*(*t*).

The `tstmle` package focuses on estimation of target parameters of the conditional distribution of *O*(*t*) given *C*<sub>*o*(*t*)</sub>, where *C*<sub>*o*(*t*)</sub> is some fixed-dimensional summary function of the past *O*(*t* − 1),…*O*(1) (van der Laan and Malenica 2018) In particular, `tstmle` provides estimation and inference for the following:

-   data-dependent, *C*<sub>*o*(*t*)</sub>− specific, causal effect within a single time series.

-   adaptive design for learning the optimal treatment rule within a single time series.

Here, initial estimation is based on the [sl3](https://github.com/jeremyrcoyle/sl3) package, which constructs ensemble models with proven optimality properties for time-series data (Malenica and van der Laan 2018).

------------------------------------------------------------------------

Installation
------------

You can install a stable release of `tstmle` from GitHub via [`devtools`](https://www.rstudio.com/products/rpackages/devtools/) with:

``` r
devtools::install_github("podTockom/tstmle")
```

Note that in order to run `tstmle` you will also need `sl3`:

``` r
devtools::install_github("jeremyrcoyle/sl3")
```

<!--

In the future, the package will be available from
[CRAN](https://cran.r-project.org/) and can be installed via


```r
install.packages("tstmle")
```

-->

------------------------------------------------------------------------

Issues
------

If you encounter any bugs or have any specific feature requests, please [file an issue](https://github.com/podTockom/tstmle/issues).

------------------------------------------------------------------------

Examples
--------

To illustrate how `tstmle` may be used to ascertain the effect of an intervention on a single time series, consider the following examples.

### Context-specific causal effect of single-time point intervention

In this section we utilize a simple, short data-set in order to estimate the causal effect of a single time-point intervention on the next outcome.

``` r
#Load relevant packages:
library(sl3)
library(origami)

#Load the data:
data("sim_ts_s1_n50")

#Set library:
Q_library=list("Lrnr_mean", "Lrnr_glm_fast", "Lrnr_glmnet","Lrnr_randomForest","Lrnr_xgboost")
g_library=list("Lrnr_mean", "Lrnr_glm_fast", "Lrnr_glmnet","Lrnr_randomForest","Lrnr_xgboost")

#Obtain estimates:
res<-tstmleSI(sim_ts_s1_n50, Cy=6, Ca=5, V=3)

#TMLE:
res$tmlePsi
#IPTW:
res$iptwPsi
```

### Adaptive design learning the optimal individualized treatment rule

Similarly to the last example, we again use the same short time-series data-set. However, in this example we are interested in adaptive design learning the optimal individualized treatment rule within a single time-series.

``` r
#Load relevant packages:
library(sl3)
library(origami)

#Load the data:
data("sim_ts_s1_n50")

#Set libraries:
Q_library=list("Lrnr_mean", "Lrnr_glm_fast", "Lrnr_glmnet","Lrnr_randomForest","Lrnr_xgboost")
g_library=list("Lrnr_mean", "Lrnr_glm_fast", "Lrnr_glmnet","Lrnr_randomForest","Lrnr_xgboost")
blip_library=list("Lrnr_glm_fast", "Lrnr_glmnet","Lrnr_randomForest","Lrnr_xgboost", "Lrnr_nnls")

#Obtain estimates:
res<-tstmleOPT(sim_ts_s1_n50, Cy=6, Ca=5, V=3)

#TMLE:
res$tmlePsi
```

------------------------------------------------------------------------

License
-------

© 2018 [Ivana Malenica](https://github.com/podTockom)

The contents of this repository are distributed under the MIT license. See below for details:

    The MIT License (MIT)

    Copyright (c) 2017-2018

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
    SOFTWARE.

------------------------------------------------------------------------

References
----------

Malenica, Ivana, and Mark J van der Laan. 2018. “Oracle Inequality for Cross-Validation Estimator Selector for Dependent Time-Ordered Experiments.”

van der Laan, Mark J, and Ivana Malenica. 2018. “Robust Estimation of Data-Dependent Causal Effects Based on Observing a Single Time-Series.”

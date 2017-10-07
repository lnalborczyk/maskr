maskr
====================

[![CRAN status](http://www.r-pkg.org/badges/version/maskr)](https://cran.r-project.org/package=maskr) [![Build Status](https://travis-ci.org/cedricbatailler/maskr.svg?branch=master)](https://travis-ci.org/cedricbatailler/maskr)

## Installation

Development version from Github:

``` r
if(!require(devtools)){install.packages("devtools")}
devtools::install_github("cedricbatailler/maskr")
```

## What does it do ?

According to Wicherts et al. (2016), insufficient blinding during the data anaysis phase is considered as one of the numerous *degrees of freedom* that researchers have during the design, collection, analysis and report of psychological studies. They say:

> "It is surprising that blinding to conditions and hypotheses of experimenters, coders, and observers is considered to be crucial during data collection, while in practice, the analyses are typically conducted by a person who is not only aware of the hypotheses, but also benefits directly from corroborating them (commonly by means of a significance test). Together with the many researcher DFs during the analyses, these factors do not entail the most optimal mix for objective and unbiased results."

One suggestion to prevent such biases would be then to divide data collection and data analysis between two persons: an experimenter, and a data analyst. However, this solution is costly and is not always practicable. Another solution would be to use a tool that allows the experimenter to blind himself during the analysis stage.

That's what `maskr` is for.

More precisely, `maskr` allow to perform the sensitive processing of outliers data in a blinded way, in a way that is proof to the above mentioned biases.

## How to use it ?

We use below the `mtcars` dataset. Consider that we are interested in the effects of the number of cylinders `cyl` on fuel consumption. We start by encrypting this particular column.

``` r
library(maskr)
data(mtcars)
mtcars_encrypted <- encrypt(mtcars, cyl)
```

We can then process outliers on the encrypted dataframe...

``` r
devtools::install_github("cedricbatailler/LIPmisc")
mtcars_decrypted <- decrypt(mtcars_encrypted, cyl)
mtcars_decrypted <- mtcars_decrypted[!sdr>2,]
```

And finally conduct our analyses, as planned, with the back-decrypted dataframe.

``` r
mtcars_decrypted <- decrypt(mtcars_encrypted, cyl)
lm(mpg ~ cyl, mtcars_decrypted)
```

## References

Wicherts, J. M., Veldkamp, C. L. S., Augusteijn, H. E. M., Bakker, M., van Aert, R. C. M., & van Assen, M. A. L. M. (2016). Degrees of Freedom in Planning, Running, Analyzing, and Reporting Psychological Studies: A Checklist to Avoid p-Hacking. Frontiers in Psychology, 7(November), 1–12. http://doi.org/10.3389/fpsyg.2016.01832

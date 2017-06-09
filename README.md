## About lme4qtl

lme4qtl extends the [lme4](https://github.com/lme4/lme4) R package for quantitative trait locus (qtl) mapping. It is all about the covariance structure of random effects. `lme4qtl` supports user-defined matrices for that,
e.g. kinship or IBDs.

See slides [bit.ly/1UiTZvQ](http://bit.ly/1UiTZvQ) introducing the `lme4qtl` R package.

The release of `lme4qtl` is scheduled according to manuscript submission. Meantime, the preprint is available in biorxiv: [lme4qtl: linear mixed models with flexible covariance structure for genetic studies of related individuals](http://biorxiv.org/content/early/2017/05/18/139816.1).

|  Package | Continuous response |
|----------|---------------------|
| stats   | `lm(myTrait ~ myCovariate, myData)` |
| lme4    | `lmer(myTrait ~ myCovariate + (1\|myID), myData)` |
| lme4qtl | `relmatLmer(myTrait ~ myCovariate + (1\|myID), myData, relmat = list(myID = myMatrix))` |

|  Package | Binary response |
|----------|---------------------|
| stats    | `glm(myStatus ~ 1, myData, family = binomial)` |
| lme4    | `glmer(myStatus ~ (1\|myID), myData, family = binomial)` |
| lme4qtl | `relmatGlmer(myStatus ~ (1\|myID), myData, relmat = list(myID = myMatrix), family = binomial)` |


## Quick start

```
library(lme4) # needed for `VarCorr` function
library(lme4qtl)

# load synthetic data set `dat40` distributed within `lme4qtl`
# - table of phenotypes `dat40`
# - the double kinship matrix `kin2`
data(dat40)

# (1) model continiuous trait `trait1`
mod <- relmatLmer(trait1 ~ AGE + SEX + (1|FAMID) + (1|ID), dat40, relmat = list(ID = kin2))

# get the estimation of h2
(vf <- as.data.frame(VarCorr(mod))[, c("grp", "vcov")])
#       grp      vcov
#1       ID 5.2845001
#2    FAMID 0.0000000
#3 Residual 0.6172059

prop <- with(vf, vcov / sum(vcov))

(h2 <- prop[1]) 
#[1] `0.895419`

# (2) model binary trait `trait1bin`
gmod <- relmatGlmer(trait1bin ~ (1|ID), dat40, relmat = list(ID = kin2), family = binomial)
```

## Install

`lme4qtl` is ditributed via binary files:

- [lme4qtl_0.1.7.tgz](https://github.com/variani/lme4qtl/releases/download/v0.1.7/lme4qtl_0.1.7.tgz) for Linux, Mac;
- [lme4qtl_0.1.7.zip](https://github.com/variani/lme4qtl/releases/download/v0.1.7/lme4qtl_0.1.7.zip) for Windows.

### Install from a local file

A source file is something like `lme4qtl_0.1.7.zip` (Windows) or `lme4qtl_0.1.7.tgz` (Linux, Mac).

Type in R:

```
# install dependencies, if necessary
p <- c("Matrix", "lme4", "kinship2", "plyr", "tibble", "ggplot2")
install.packages(p)

# install lme4qtl from a local file
# - the first argument is path to the installation file
# - the second argument (`repos = NULL`) says to install locally rather than from a repository
install.packages("lme4qtl_0.1.7.zip", repos = NULL)
```

Type in terminal:

```
R CMD INSTALL lme4qtl_0.1.7.tgz
```


# Getting started

MendelIHT is designed to be user-friendly. In this section, we outline the basic procedure to analyze your GWAS data with MendelIHT. 

## Installation

Press `]` to enter package manager mode and type the following (after `pkg>`):
```
(v1.0) pkg> add https://github.com/OpenMendel/SnpArrays.jl
(v1.0) pkg> add https://github.com/OpenMendel/MendelSearch.jl
(v1.0) pkg> add https://github.com/OpenMendel/MendelBase.jl
(v1.0) pkg> add https://github.com/biona001/MendelIHT.jl
```
The order of installation is important!

## 3 Step Workflow

Most analysis consists of 3 simple steps:

1. Import data.
2. Run `cv_iht` to determine best model size.
3. Run `L0_reg` to obtain final model.

We believe the best way to learn is through examples. Head over to the example section to see these steps in action. Nevertheless, below contains function signatures and use cautions that any users should be aware. 

!!! note

    (1) MendelIHT.jl assumes there are **NO missing genotypes**, and (2) the trios (`.bim`, `.bed`, `.fam`) must all be present in the same directory. 

## Core Functions

A standard user should only ever run 2 functions, other than importing data.

```@docs
  cv_iht
```   

!!! note 

    **Do not** delete intermediate files (e.g. `train.bed`) which will be created in the current directory when you run `cv_iht`. These are memory-mapped training/testing files that are necessary to run cross validation. This means that **you must have `x` GB of free space on your hard disk** where `x` is how much memory it takes to store your `.bed` file.


```@docs
  L0_reg
```

## Supported GLM models and Link functions

MendelIHT borrows the distribution and link function implementations in [GLM.jl](http://juliastats.github.io/GLM.jl/stable/).

Distributions (listed with their canonical link) that work with `L0_reg` and `cv_iht` are:

              Normal (IdentityLink)
           Bernoulli (LogitLink)
             Poisson (LogLink)
    NegativeBinomial (LogLink)
               Gamma (InverseLink) **(not tested)**
     InverseGaussian (InverseSquareLink) **(not tested)**

Available link functions are:

    CauchitLink
    CloglogLink
    IdentityLink
    InverseLink
    InverseSquareLink
    LogitLink
    LogLink
    ProbitLink
    SqrtLink
    
!!! tip
    
    For d = NegativeBinomial or d=Gamma, the link function must be `LogLink`. For Bernoulli, the `ProbitLink` seems to work better than `LogitLink`.

## Specifying Groups and Weights

When you have group and weight information, you input them as optional arguments in `L0_reg` and `cv_iht`. The weight vector is a vector of Float64, while the group vector is a vector of integers. 

## Simulation utilities

MendelIHT provides some simulation utilities that help users explore the function and capabilities of iterative hard thresholding. 

```@docs
  simulate_random_snparray
```

!!! note
    Simulating a SnpArray with $n$ subjects and $p$ SNPs requires roughly $n \times p \times 4$ bits. Make sure you have enough RAM before simulating very large SnpArrays.

```@docs
  simulate_random_response
```

!!! note
    For negative binomial and gamma, the link function must be LogLink. For Bernoulli, the probit link seems to work better than logitlink when used in `cv_iht` or `L0_reg`. 

```@docs
  adhoc_add_correlation
```

```@docs
  simulate_rare_variants
```

```@docs
  make_bim_fam_files
```

## Other Useful Functions

MendelIHT additionally provides useful utilities that may be of interest to a few advanced users. 

```@docs
  iht_run_many_models
```

```@docs
  loglikelihood
```

```@docs
  project_k!
```

```@docs
  project_group_sparse!
```

```@docs
  maf_weights
```

```@docs
  initialize_beta!
```
01\_explore-libraries\_jenny.R
================
karley1hj
Wed Jan 31 14:05:30 2018

``` r
## how jenny might do this in a first exploration
## purposely leaving a few things to change later!
```

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/Library/Frameworks/R.framework/Versions/3.3/Resources/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/Library/Frameworks/R.framework/Resources/library"

``` r
library(fs)
path_real(.Library)
```

    ## /Library/Frameworks/R.framework/Versions/3.3/Resources/library

Installed packages

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 3.3.2

    ## Loading tidyverse: ggplot2
    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: readr
    ## Loading tidyverse: purrr
    ## Loading tidyverse: dplyr

    ## Warning: package 'ggplot2' was built under R version 3.3.2

    ## Warning: package 'readr' was built under R version 3.3.2

    ## Warning: package 'purrr' was built under R version 3.3.2

    ## Warning: package 'dplyr' was built under R version 3.3.2

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 229

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##   LibPath                                                 Priority       n
    ##   <chr>                                                   <chr>      <int>
    ## 1 /Library/Frameworks/R.framework/Versions/3.3/Resources… base          14
    ## 2 /Library/Frameworks/R.framework/Versions/3.3/Resources… recommend…    15
    ## 3 /Library/Frameworks/R.framework/Versions/3.3/Resources… <NA>         200

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## Warning: package 'bindrcpp' was built under R version 3.3.2

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                 109 0.476 
    ## 2 yes                102 0.445 
    ## 3 <NA>                18 0.0786

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   Built     n  prop
    ##   <chr> <int> <dbl>
    ## 1 3.3.0   123 0.537
    ## 2 3.3.1    44 0.192
    ## 3 3.3.2    62 0.271

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ##   [1] "abind"                "acepack"              "AlgDesign"           
    ##   [4] "arm"                  "assertthat"           "backports"           
    ##   [7] "base64enc"            "BH"                   "bindr"               
    ##  [10] "bindrcpp"             "bit"                  "bit64"               
    ##  [13] "bitops"               "broom"                "callr"               
    ##  [16] "car"                  "caTools"              "cellranger"          
    ##  [19] "checkmate"            "cli"                  "clipr"               
    ##  [22] "clisymbols"           "coda"                 "colorspace"          
    ##  [25] "corpcor"              "corrplot"             "crayon"              
    ##  [28] "curl"                 "d3Network"            "data.table"          
    ##  [31] "data.tree"            "DBI"                  "dbplyr"              
    ##  [34] "debugme"              "DEoptimR"             "desc"                
    ##  [37] "DiagrammeR"           "dichromat"            "digest"              
    ##  [40] "doParallel"           "dplyr"                "DT"                  
    ##  [43] "ellipse"              "enc"                  "evaluate"            
    ##  [46] "fdrtool"              "feather"              "forcats"             
    ##  [49] "foreach"              "formatR"              "Formula"             
    ##  [52] "fs"                   "ggm"                  "ggplot2"             
    ##  [55] "gh"                   "git2r"                "glasso"              
    ##  [58] "glue"                 "Gmedian"              "gridBase"            
    ##  [61] "gridExtra"            "gtable"               "gtools"              
    ##  [64] "gutenbergr"           "haven"                "highr"               
    ##  [67] "Hmisc"                "hms"                  "htmlTable"           
    ##  [70] "htmltools"            "htmlwidgets"          "httpuv"              
    ##  [73] "httr"                 "huge"                 "hunspell"            
    ##  [76] "igraph"               "import"               "influenceR"          
    ##  [79] "ini"                  "irlba"                "iterators"           
    ##  [82] "janeaustenr"          "jpeg"                 "jsonlite"            
    ##  [85] "knitr"                "labeling"             "latticeExtra"        
    ##  [88] "lavaan"               "lazyeval"             "lisrelToR"           
    ##  [91] "lme4"                 "lubridate"            "magrittr"            
    ##  [94] "markdown"             "matrixcalc"           "MatrixModels"        
    ##  [97] "mclust"               "measurements"         "mi"                  
    ## [100] "mime"                 "minqa"                "mnormt"              
    ## [103] "modelr"               "modeltools"           "multcomp"            
    ## [106] "munsell"              "mvtnorm"              "network"             
    ## [109] "NeuralNetTools"       "nloptr"               "NLP"                 
    ## [112] "NMF"                  "openssl"              "pbivnorm"            
    ## [115] "pbkrtest"             "pillar"               "pkgconfig"           
    ## [118] "pkgmaker"             "plogr"                "plyr"                
    ## [121] "png"                  "polycor"              "praise"              
    ## [124] "pryr"                 "psych"                "purrr"               
    ## [127] "qgraph"               "quadprog"             "quantreg"            
    ## [130] "R6"                   "radiant"              "radiant.basics"      
    ## [133] "radiant.data"         "radiant.design"       "radiant.model"       
    ## [136] "radiant.multivariate" "RColorBrewer"         "Rcpp"                
    ## [139] "RcppEigen"            "RCurl"                "readr"               
    ## [142] "readxl"               "registry"             "rematch"             
    ## [145] "rematch2"             "reprex"               "reshape2"            
    ## [148] "rjson"                "rlang"                "rmarkdown"           
    ## [151] "rngtools"             "ROAuth"               "robustbase"          
    ## [154] "rockchalk"            "rprojroot"            "RSpectra"            
    ## [157] "rstudioapi"           "rvest"                "sandwich"            
    ## [160] "scales"               "selectr"              "sem"                 
    ## [163] "semPlot"              "sfsmisc"              "shiny"               
    ## [166] "shinyAce"             "slam"                 "sna"                 
    ## [169] "SnowballC"            "sourcetools"          "SparseM"             
    ## [172] "statnet.common"       "stringi"              "stringr"             
    ## [175] "styler"               "testthat"             "TH.data"             
    ## [178] "tibble"               "tidyr"                "tidyselect"          
    ## [181] "tidytext"             "tidyverse"            "tm"                  
    ## [184] "tokenizers"           "topicmodels"          "translations"        
    ## [187] "triebeard"            "twitteR"              "urltools"            
    ## [190] "usethis"              "utf8"                 "viridis"             
    ## [193] "visNetwork"           "whisker"              "withr"               
    ## [196] "wordcloud"            "XML"                  "xml2"                
    ## [199] "xtable"               "yaml"                 "zoo"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n  prop
    ##   <lgl>  <int> <dbl>
    ## 1 F        127 0.555
    ## 2 T        102 0.445

-   [Find onboarded packages](#find-onboarded-packages)

Find onboarded packages
=======================

``` r
reviews <- readr::read_csv("data/Reviews.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   id_no = col_integer(),
    ##   reviewer = col_character(),
    ##   review_url = col_character(),
    ##   package = col_character(),
    ##   onboarding_url = col_character(),
    ##   review_hours = col_double()
    ## )

``` r
reviews <- dplyr::select(reviews, package, onboarding_url) 
reviews <- unique(reviews)
library("gh")
creation <- NULL
repo <- NULL
is_ok <- TRUE
page <- 1
while(is_ok){
  print(page)
  issues <- try(gh("/repos/:owner/:repo/issues", owner = "ropensci",
             repo = "onboarding", labels = "package",
             state = "all",
             page = page), silent = TRUE)
  is_ok <- is(issues, "list") & issues[[1]] != ""
  if(is_ok){
  page <- page + 1
  creation <- c(creation, vapply(issues, "[[", "", "created_at"))
  repo <- c(repo, vapply(issues, "[[", "", "body"))
  }
}
```

    ## [1] 1

    ## Warning in if (is_ok) {: la condición tiene longitud > 1 y sólo el primer
    ## elemento será usado

    ## Warning in while (is_ok) {: la condición tiene longitud > 1 y sólo el
    ## primer elemento será usado

    ## [1] 2

    ## Warning in if (is_ok) {: la condición tiene longitud > 1 y sólo el primer
    ## elemento será usado

    ## Warning in if (is_ok) {: la condición tiene longitud > 1 y sólo el primer
    ## elemento será usado

    ## [1] 3

    ## Warning in if (is_ok) {: la condición tiene longitud > 1 y sólo el primer
    ## elemento será usado

    ## Warning in if (is_ok) {: la condición tiene longitud > 1 y sólo el primer
    ## elemento será usado

    ## [1] 4

``` r
repo <- stringr::str_extract(repo, "Package:.*")
repo <- stringr::str_replace(repo, "Package: ", "")
repo <- unique(repo)
repo <- na.omit(repo)
```

``` r
repo
```

    ##  [1] "data.world"         "codemetar"          "rtimicropem"       
    ##  [4] "bib2df"             "bomrang"            "bittrex"           
    ##  [7] "RefManageR"         "rrricanes"          "bikedata"          
    ## [10] "roadoi"             "cyphr"              "gitlabr"           
    ## [13] "patentsview"        "virustotal"         "ccex"              
    ## [16] "iheatmapr"          "osmdata"            "cleanEHR"          
    ## [19] "cmipr"              "rBBS"               "getCRUCLdata"      
    ## [22] "lingtypology"       "charlatan"          "biomartr"          
    ## [25] "wordVectors"        "sperrorest"         "visdat"            
    ## [28] "JSTORr"             "ccafs"              "EML"               
    ## [31] "GSODR"              "refimpact"          "hddtools"          
    ## [34] "camsRad"            "rdpla"              "fuse"              
    ## [37] "rdefra"             "dbhydroR"           "plateR"            
    ## [40] "getlandsat"         "ezknitr"            "gtfsr"             
    ## [43] "mregions"           "genbankr"           "monkeylearn"       
    ## [46] "geoparser"          "tabulizer"          "gutenbergr"        
    ## [49] "convertr"           "riem"               "opencage"          
    ## [52] "DoOR.data"          "DoOR.functions"     "tokenizers"        
    ## [55] "SwissHistMunData"   "rebi"               "rgeospatialquality"
    ## [58] "osmplotr"           "robotstxt"          "Ropenaq"           
    ## [61] "assertr"            "rnaturalearth"      "textreuse"         
    ## [64] "oai"                "rusda"              "rotl"              
    ## [67] "rsdmx"              "lab.note"           "FedData"           
    ## [70] "stplanr"            "rrlite"            
    ## attr(,"na.action")
    ## [1] 3
    ## attr(,"class")
    ## [1] "omit"

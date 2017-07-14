-   [Find submitted packages](#find-submitted-packages)
-   [Find the ones that are on CRAN](#find-the-ones-that-are-on-cran)

Find submitted packages
=======================

``` r
library("gh")
creation <- NULL
repo <- NULL
is_ok <- TRUE
page <- 1
while(is_ok){
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

repo <- stringr::str_extract(repo, "Package:.*")
repo <- stringr::str_replace(repo, "Package: ", "")
```

``` r
packages <- tibble::tibble(package = repo, 
                           ropensci_submission = lubridate::ymd_hms(creation))

packages <- unique(packages)
packages <- dplyr::filter(packages, !is.na(package))
packages
```

    ## # A tibble: 72 x 2
    ##        package ropensci_submission
    ##          <chr>              <dttm>
    ##  1  data.world 2017-07-10 22:38:52
    ##  2   codemetar 2017-07-05 23:38:45
    ##  3 rtimicropem 2017-06-14 11:42:28
    ##  4      bib2df 2017-06-06 18:59:01
    ##  5     bomrang 2017-06-03 09:01:11
    ##  6     bittrex 2017-06-02 20:02:27
    ##  7  RefManageR 2017-06-02 14:59:34
    ##  8   rrricanes 2017-06-01 19:17:01
    ##  9    bikedata 2017-05-31 11:35:20
    ## 10      roadoi 2017-05-17 16:23:09
    ## # ... with 62 more rows

Find the ones that are on CRAN
==============================

``` r
library("crandb")
#package("ropenaq", version = "all")
```

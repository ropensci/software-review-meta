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
get_cran_oldest_date <- function(package){
  results <- try(package(package, version = "all"), silent = TRUE)
  if(is(results, "try-error")){
    date <- "9999-01-01 00 00 00"
  }else{
    date <- results$versions[[1]]$date
  }
  
  return(date)
}
get_cran_oldest_date("ropenaq")
```

    ## [1] "2016-07-05T11:01:42+00:00"

``` r
dates <- purrr::map_chr(packages$package, get_cran_oldest_date)
dates <- lubridate::ymd_hms(dates)
packages$cran_submission <- dates
packages <- dplyr::mutate(packages, 
                          ropensci_submission = lubridate::as_date(ropensci_submission), 
                          cran_submission = lubridate::as_date(cran_submission))
```

``` r
knitr::kable(packages)
```

| package            | ropensci\_submission | cran\_submission |
|:-------------------|:---------------------|:-----------------|
| data.world         | 2017-07-10           | 2017-06-23       |
| codemetar          | 2017-07-05           | 9999-01-01       |
| rtimicropem        | 2017-06-14           | 9999-01-01       |
| bib2df             | 2017-06-06           | 2016-08-09       |
| bomrang            | 2017-06-03           | 9999-01-01       |
| bittrex            | 2017-06-02           | 9999-01-01       |
| RefManageR         | 2017-06-02           | 2014-02-27       |
| rrricanes          | 2017-06-01           | 9999-01-01       |
| bikedata           | 2017-05-31           | 2017-05-31       |
| roadoi             | 2017-05-17           | 2017-04-06       |
| cyphr              | 2017-05-08           | 9999-01-01       |
| gitlabr            | 2017-04-26           | 2015-10-28       |
| patentsview        | 2017-04-25           | 2017-07-12       |
| virustotal         | 2017-04-14           | 2016-06-02       |
| ccex               | 2017-04-06           | 9999-01-01       |
| iheatmapr          | 2017-03-30           | 9999-01-01       |
| osmdata            | 2017-03-08           | 2017-05-18       |
| cleanEHR           | 2017-02-21           | 2017-02-02       |
| cmipr              | 2017-02-16           | 9999-01-01       |
| rBBS               | 2017-02-16           | 9999-01-01       |
| getCRUCLdata       | 2017-01-30           | 2017-01-05       |
| lingtypology       | 2017-01-29           | 2016-12-29       |
| charlatan          | 2017-01-28           | 2017-06-16       |
| biomartr           | 2017-01-25           | 2015-07-22       |
| wordVectors        | 2017-01-24           | 9999-01-01       |
| sperrorest         | 2017-01-09           | 2012-06-20       |
| visdat             | 2017-01-04           | 2017-07-11       |
| JSTORr             | 2016-12-20           | 9999-01-01       |
| ccafs              | 2016-10-27           | 2017-02-24       |
| EML                | 2016-10-14           | 2016-12-20       |
| GSODR              | 2016-10-14           | 2016-04-20       |
| refimpact          | 2016-10-03           | 2016-09-26       |
| hddtools           | 2016-09-05           | 2014-08-13       |
| camsRad            | 2016-08-22           | 2016-11-30       |
| rdpla              | 2016-08-16           | 2016-10-14       |
| fuse               | 2016-08-08           | 9999-01-01       |
| rdefra             | 2016-08-08           | 2016-06-10       |
| dbhydroR           | 2016-08-01           | 2016-08-03       |
| plateR             | 2016-07-26           | 9999-01-01       |
| getlandsat         | 2016-07-13           | 2016-08-16       |
| ezknitr            | 2016-06-25           | 2015-12-08       |
| gtfsr              | 2016-06-24           | 9999-01-01       |
| mregions           | 2016-06-16           | 2016-08-03       |
| genbankr           | 2016-05-20           | 9999-01-01       |
| monkeylearn        | 2016-05-19           | 2016-09-11       |
| geoparser          | 2016-05-08           | 2016-07-05       |
| tabulizer          | 2016-05-07           | 9999-01-01       |
| gutenbergr         | 2016-05-02           | 2016-05-03       |
| convertr           | 2016-04-26           | 2016-10-13       |
| riem               | 2016-04-25           | 2016-05-28       |
| opencage           | 2016-04-12           | 2016-05-12       |
| DoOR.data          | 2016-04-11           | 9999-01-01       |
| DoOR.functions     | 2016-04-11           | 9999-01-01       |
| tokenizers         | 2016-04-04           | 2016-04-02       |
| SwissHistMunData   | 2016-03-31           | 9999-01-01       |
| rebi               | 2016-03-11           | 9999-01-01       |
| rgeospatialquality | 2016-03-11           | 2016-06-28       |
| osmplotr           | 2016-03-04           | 2016-03-02       |
| robotstxt          | 2016-01-09           | 2016-02-08       |
| Ropenaq            | 2015-12-31           | 9999-01-01       |
| assertr            | 2015-12-24           | 2015-03-22       |
| rnaturalearth      | 2015-11-26           | 2017-03-21       |
| textreuse          | 2015-09-18           | 2015-10-31       |
| oai                | 2015-08-21           | 2015-09-11       |
| rusda              | 2015-06-29           | 2015-11-13       |
| rotl               | 2015-06-16           | 2015-07-24       |
| rsdmx              | 2015-06-02           | 2014-08-27       |
| lab.note           | 2015-05-21           | 9999-01-01       |
| FedData            | 2015-05-06           | 2015-02-13       |
| stplanr            | 2015-04-08           | 2015-11-02       |
| rrlite             | 2015-03-31           | 9999-01-01       |
| rrlite             | 2015-03-10           | 9999-01-01       |

We're looking at 72 packages. For 21 there was a CRAN version before the rOpenSci submission. For 50 there wasn't a CRAN version before the rOpenSci submission.

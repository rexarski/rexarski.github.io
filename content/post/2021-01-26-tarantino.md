---
title: "The tarantino package"
date: 2021-01-26T20:17:11+11:00
slug: "tarantino"
description: "My first attempt to R package development."
draft: false
tags: ["R", "Visualization", "R package"]
math: false
showToc: true
---

<img src="/image/tarantino-logo.png" width="160px" align="right" />

I want to practice the contents covered in [R packages](https://r-pkgs.org/), and learn by example. So I am trying to make a copy of of `wesanderson`, an R package listing representative color palettes of Wes Anderson's movies. My [`tarantino`](https://github.com/rexarski/tarantino) might not be that practical in real life, since it's not that vivid in color selections. It just strengthens my understanding of the R package dev workflow, as well as the package structures, some components not covered in that previous book. (It is still a must-read.)

1. Get palettes of classic scenes from Quentin Tarantino's movies. [Movies in Color](https://moviesincolor.com). It's also the source of package [`wesanderson`](https://github.com/karthik/wesanderson) and [`ghibli`](https://github.com/ewenme/ghibli).
2. Write in `/R` directory and use `devtools::load_all()` in terminal to do some manual tests.
3. `styler::style_pkg()` to style my coding.
4. `devtools::test()`, interactively select 1 to create a directory `tests/` and `testthat.R`, `testthat` sub-directory.
5. `usethis::use_mit_license()`, change the `COPYRIGHT HOLDER` in newly generated `LICENSE` file
6. Now use `roxygen2` style to write the documentation above every functions in `R/`. Execute `devtools::document()` to generate corresponding `.Rd` files in `/man`.
7. No need to write vignettes for this package though, it's pretty simple.
8. Modify details in `DESCRIPTION` file.
9. Back to write some tests. Write in the `tests/testthat` folder. `devtools::test()` to execute and read the results.
10. `usethis::use_readme_rmd()`, update `README.Rmd` then `usethis::use_readme_md()`.
    - Use `devtools::build_readme()` to update `README.md` based on changes made in `.Rmd`.
11. `install.packages("pkgdown)"`, then `usethis::use_pkgdown()` and `pkgdown::build_site()`. This will build a site based on my `README.md`
    - Figma to create the package badge.
12. `usethis::use_news_md()` adds a `NEWS.md` to keep track of changes in the package.

<img class="special-img-class" style="auto" src="/image/palettes.png" />

## Issue

I actually did not pay much attention to the hue, saturation, brightness of the selection of colors. So this leads to a pretty funny dilemma that:

- If I order the colors in a spectrum order, the colors might not be distinguishable enough.
- If I just leave the colors in an random order as they are, the generated "continuous" colors will show no logic in a "heatmap". For example,

```r
pal <- tarantino_palette("KillBillVol1", 100, "continuous")
heatmap(as.matrix(mtcars), scale="column", col = pal)
```

<img class="special-img-class" style="width:100%" src="/image/heatmap_mtcars.png" />

<img class="special-img-class" style="width:100%" src="/image/kil_bill_vol_1_spectrum.png" />

## Todos

- [x] `README.md` and `README.Rmd`
- [x] [`pkgdown`](https://pkgdown.r-lib.org) set up.
- [x] [`codemeta.json`](https://docs.ropensci.org/codemetar/index.html)
- [x] Turn `NEWS.md` to a separate page in `pkgdown` website with `pkgdown::build_news()`
- [x] *Updated on 2021-02-13.* Add color palettes of *The Hateful Eight* and *Once Upon a Time in Hollywood*.
- [ ] Publish the `pkgdown` site with GitHub Actions.

***

You can download and use the development version of this package from [GitHub](https://github.com/rexarski/tarantino).
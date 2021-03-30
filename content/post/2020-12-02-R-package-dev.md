---
title: "A quickstart to R package development"
date: 2020-12-02T22:10:34+10:00
slug: "r-package-dev"
description: ""
draft: false
tags: ["R"]
math: false
showToc: true
---

I followed the [tutorial](https://www.pipinghotdata.com/posts/2020-10-25-your-first-r-package-in-1-hour/) to get myself familiar with some basics of R package development. Really like the table summary with commands in the end.

<!--mo -->

> Better check Ch18 in [R Packages](https://r-pkgs.org/) by Hadley Wickham and Jenny Bryan for `use this::use_git()` and `usethis::use_github()`.

To create an R project, open the project in a new session, create the minimal essential files and structure for R packages.

```r
usethis::create_package(“~/Developer/radiorktiv”)
devtools::check() # updates package docs, builds the package, and submits checks.
```

Now I got a warning saying that i need a `DESCRIPTION` file.

```r
usethis::use_mit_license("Rui Qiu") # check again, now it’s warning-free.
```

Say we’ve defined a standalone dummy function `demo()`  which does nothing but print a silly line. We add it to `radiorktiv` package:

```r
usethis::use_r(“demo”) # it creates a blank script named `R/demo.R`.
```

We then paste the function in it. And test it with:

```r
devtools::load_all()
```

Next thing we need to document the function `radiorktiv::demo()` using the _Roxygen skeleton_. In `demo.R`, we can either:

1. `Code -> Insert Roxygen Skeleton`, or
2. `Control + Alt + Shift + R`

Modify so that it looks like this:

```r
#' Demo function
#'
#' @return A text sentence
#' @export
#'
#' @examples
#' It's trivial.
demo <- function() {
  print("IT'S RADIOACTIVE.")
}
```

Now we test the document:

```r
devtools::document()
```

This one does:

1. Create a file called `demo.Rd` in `man` folder, this is the manual. Do not modify it manually.
2. The `NAMESPACE` now specifies the package will export the `demo` function.

You can use `?demo` to read the document as usual.

Don’t forget to `devtools::check()` .

And use `usethis::use_package(“package”)` to specify package dependencies, use `package::function` within (my developing) packages.

Last but not the least, use `devtools::install(“path/package”)` or `devtools::install_github(“user/repo”)` to install the developing package.

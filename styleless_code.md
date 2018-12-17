---
title: "Styleless guide to organize code"
date: "December, 2018"
---

# Introduction

This document attempts to present some standard ways to organize, document and test code in R and python.

As always, it's better to stand on the shoulders of giants. Or, in the absence of giants, in whoever we can find. A big part of what's written here will be a compilation of guides and web pages that are well known or that at least make sense to the kind of work I do.

The main guides we cite are the following:

* python: [hitchhiker's guide to python](https://docs.python-guide.org/). 
* R: [building R packages](http://r-pkgs.had.co.nz/).

Other resources will be cited at each section. I'm going to paste examples of each resource so as to avoid having to jump everytime to the source. Having said that, it's *highly recommended* to go to the link to read in detail about each functionality at least once.

# Structure

## python

The guidelines [mentioned here](https://docs.python-guide.org/writing/structure/) have an example in a [github repository](https://github.com/kennethreitz/samplemod). The python generally agreed guidelines are found [PEP8](https://www.python.org/dev/peps/pep-0008/). Finally, [google's python style guide](http://google.github.io/styleguide/pyguide.html) and [this](https://docs.python-guide.org/writing/style/) show the style.

Our projects usually demand some additional directories (that are sometimes shown as optional in the examples given above). For example: data, reports, etc.
As always the directories in each project will vary depending on the specific requirements of the project.

Based on these guides and our additional particular demands, I show an example of python project named `sample`:

    README.rst
    LICENSE
    .gitignore
    setup.py
    requirements.txt  # see pip
    sample/__init__.py
    sample/core.py # => this is the package main directory. Almost all code goes here.
    sample/helpers.py  # => this is the package main directory. Almost all code goes here.
    docs/conf.py
    docs/index.rst  # => raw processed input data. See "documentation".
    tests/test_basic.py
    tests/test_advanced.py
    scripts/  # => python executable scripts that make use of the sample package.
    data/  # => raw processed input data

## R

In R, [there's Hadley Wickham's bible for packages](http://r-pkgs.had.co.nz/) that shows how to structure R packages (not necessarily R projects).
Other resources I found interesting are [this blog post](https://chrisvoncsefalvay.com/structuring-r-projects/).

Both resources show several alternatives to naming and structure so feel free to check them in case an alternative convention suits your project better. Having said that, if an r package wants to be built, it's better to follow Hadley Wickham's guide.

Also, see [Google's R guidelines](https://google.github.io/styleguide/Rguide.xml) for more details on code style.

An example proposal would be the following:

    README.rst
    LICENSE
    DESCRIPTION
    NAMESPACE
    .gitignore
    man/  #  documentation
    vignetes/  # code examples
    tests/  # testing scripts. See testing
    R/  # main directory with actual code in .R files
    data/  # raw, processed
    inst/
    exec/

## Mixed

If a project involves more than one language, the structure should be as similar as possible to the language's individual convention. Of course, some documentation and some general directories (`data` for example) may be better suited in the root directory.

For example, the following could be a way to structure a multi-language project while keeping a common doc and data. An option could be to rename the R directory inside the R folder.

    LICENSE
    .gitignore
    data/  # => raw processed input data
    README.rst
    docs/conf.py
    docs/index.rst  # => raw processed input data. See "documentation".
    python/README.rst
    python/setup.py
    python/requirements.txt  # see pip
    python/sample/__init__.py
    python/sample/core.py # => this is the package main directory. Almost all code goes here.
    python/sample/helpers.py  # => this is the package main directory. Almost all code goes here.
    python/tests/test_basic.py
    python/tests/test_advanced.py
    python/scripts/  # => python executable scripts that make use of the sample package.
    R/README.rst
    R/DESCRIPTION
    R/NAMESPACE
    R/vignetes/  # code examples
    R/tests/  # testing scripts. See testing
    R/R/  # main directory with actual code in .R files
    R/inst/
    R/exec/    


# Documentation

There are many levels of documentation. In this document, only code comments will be mentioned. A separate section dedicated to technical documentation and reports should be available.
There is the technical documentation, code comments and reports.

In **R**, [Roxygen](http://r-pkgs.had.co.nz/man.html) is shown to produce standardized comments that can later be compiled into package documentation.

A brief example taken from the document:

    #' Add together two numbers.
    #' 
    #' @param x A number.
    #' @param y A number.
    #' @return The sum of \code{x} and \code{y}.
    #' @examples
    #' add(1, 1)
    #' add(10, 1)
    add <- function(x, y) {
      x + y
    }


**Python** comments are done via [docstrings](https://www.python.org/dev/peps/pep-0287/). Pycharm (see *IDE*) uses reST formatting and it autocompletes when writing the three commas inside a function.
The following example is taken from [this SO](https://stackoverflow.com/questions/3898572/what-is-the-standard-python-docstring-format) post:

    """
    This is a reST style.

    :param param1: this is a first param
    :param param2: this is a second param
    :returns: this is a description of what is returned
    :raises keyError: raises an exception
    """

# Testing

python has the [unittest](https://docs.python.org/2/library/unittest.html) library, explained also [here](https://docs.python-guide.org/writing/tests/).

A minimal example mentioned in the first link:

    import unittest

    class TestStringMethods(unittest.TestCase):

        def test_upper(self):
            self.assertEqual('foo'.upper(), 'FOO')

    if __name__ == '__main__':
        unittest.main()

R has the [testthat](https://cran.r-project.org/web/packages/testthat/index.html) library, explained [here](http://r-pkgs.had.co.nz/tests.html).

An minimal example mentioned in the second link:

    context("String length")
    library(stringr)

    test_that("str_length is number of characters", {
      expect_equal(str_length("a"), 1)
      expect_equal(str_length("ab"), 2)
      expect_equal(str_length("abc"), 3)
    })

# IDE

[pycharm](https://www.jetbrains.com/pycharm/download/) for python.
[RStudio](https://www.rstudio.com/products/rstudio/download/) for R.

Both are cross-platform, powerful, specialized, easy to use and quite similar one to the other.
They have also a free open-source version.

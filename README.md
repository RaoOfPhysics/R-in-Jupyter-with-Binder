# R in Jupyter with Binder

An example of how to use R in Jupyter notebooks and then make a Binder environment to run them interactively on the web.
This repo was inspired from [a Tweet](https://twitter.com/Alex_Danvers/status/1016395766250627072) in a discussion about [Episode 7 of The Bayes Factor podcast](https://sites.tufts.edu/hilab/podcast/liz-page-gould-and-alex-danvers/).

> Disclaimer: I am a physicist and primarily a Python and C++ programmer and I don't use/know R. This repo is just what I know from being able to read code and understanding how Jupyter works.

[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)
[![Build Status](https://travis-ci.com/matthewfeickert/R-in-Jupyter-with-Binder.svg?branch=master)](https://travis-ci.com/matthewfeickert/R-in-Jupyter-with-Binder)
[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/matthewfeickert/R-in-Jupyter-with-Binder/master)

## Check it out first

Before learning how to setup R in Jupyter, first go check out how cool it is in Binder! Just click the "launch Binder" badge above (once the server loads click on the `R-in-Jupyter-Example.ipynb` file).

## Table of contents

- [Requirements](#requirements)
- [Setup and Installation](#setup-and-installation)
- [Using papermill with Jupyter](#using-papermill-with-jupyter)
- [Testing Jupyter notebooks with pytest](#testing-jupyter-notebooks-with-pytest)
- [Automating testing alongside development with CI](#automating-testing-alongside-development-with-ci)
- [Setting up a Binder environment](#setting-up-a-binder-environment)
- [Further Reading and Resources](#further-reading-and-resources)

## Requirements

Before you can begin you need to make sure that you have the following in your working environment
- [Jupyter](http://jupyter.org/)
- A modern version of [R](https://www.r-project.org/)

## Setup and Installation

- [Install Jupyter](http://jupyter.org/install)
   - If you aren't familiar with Python and `pip` then just follow the instructions for installing with Anaconda
- [Install R](https://cran.r-project.org/doc/manuals/r-release/R-admin.html)
- Install the [R kernel for Jupyter](https://github.com/IRkernel/IRkernel) ([IRkernel](https://irkernel.github.io/))

## Using [papermill](https://github.com/nteract/papermill) with Jupyter

> Papermill is a tool for parameterizing, executing, and analyzing Jupyter Notebooks.

This means that you can use papermill to _externally_ run, manipulate, and test Jupyter notebooks. This allows you to use Jupyter notebooks as components of an automated data analysis pipeline or for procedurally testing variations.

- To use with Jupyter notebooks running in the IRkernel [install the R bindings for papermill (`papermillR`)](https://github.com/nteract/papermillr)

A toy example of how to use papermill is demonstrated in the [example Jupyter notebook](https://github.com/matthewfeickert/R-in-Jupyter-with-Binder/blob/master/R-in-Jupyter-Example.ipynb).

## Testing Jupyter notebooks with pytest

To provide testing for Jupyter notebooks we can use [pytest](https://docs.pytest.org/en/latest/) in combination with papermill.

- [Install pytest](https://docs.pytest.org/en/latest/getting-started.html)
   - If you installed Jupyter with Conda then you can also [install pytest with Conda](https://anaconda.org/anaconda/pytest)

One you have installed pytest and done some [minimal reading of the docs](https://docs.pytest.org/en/latest/getting-started.html#create-your-first-test) then create a `tests` directory and write your test files in Python inside of it.

An example of some very simple tests using papermill is provided in [`tests/test_notebooks.py`](https://github.com/matthewfeickert/R-in-Jupyter-with-Binder/blob/master/tests/test_notebooks.py). Once you read though and understand what the testing file is doing execute the tests with `pytest` in the top level directory of the repo by running

```
pytest
```

To see the output that the individual testing functions would normally [print to `stdout`](https://docs.pytest.org/en/latest/capture.html) run with the `-s` flag

```
pytest -s
```

### Why write tests?

There are numerous reasons to test your code, but as a scientist an obvious one is ensured reproducibility of experimental results. If your analysis code has unit tests and the analysis itself exists in an automatically testable pipeline then you and your colleagues should have more confidence in your analysis. Additionally, your analysis becomes (by necessity) a well documented and transparent process.

Want to learn more? Check out the [Test and Code podcast](http://testandcode.com/) hosted by [Brian Okken](https://github.com/okken).

## Automating testing alongside development with CI

Assuming that you're using Git to develop your analysis code then you can have a continuous integration service (such as [Travis CI](https://travis-ci.org/) or [CircleCI](https://circleci.com/)) automatically test your code in a fresh environment every time you push to GitHub. Testing with CI is a great way to know that your analysis code is working exactly as expected in a reproducible environment from installation all the way through execution as you develop, revise, and improve it. To start off with I would recommend using Travis CI (it is the easiest to get up and running).

- [Getting started with Travis CI](https://docs.travis-ci.com/user/getting-started/)
- Example [`.travis.yml`](https://github.com/matthewfeickert/R-in-Jupyter-with-Binder/blob/master/.travis.yml) in this repo
- [Travis CI docs on writing YAML CI files for R](https://docs.travis-ci.com/user/languages/r/)

## Setting up a [Binder](https://mybinder.org/) environment

[Binder](https://mybinder.org/) turns your GitHub repository into a fully interactive computational environment (as you hopefully have already seen from the demo notebook). It then allows people to run any code that exists in the repository from their web browser without having to install any code and is a great tool for collaboration and sharing results.

The Binder team has done amazing work to make "Binderizing" a GitHub repository as simple as possible. In the case of getting an R computing environment many times all that you need (in addition to a `DESCRIPTION` file and maybe an `install.R`) is a `runtime.txt` file that dictates which daily snapshot of [MRAN](https://mran.microsoft.com/documents/rro/reproducibility) to use. See the [`binder`](https://github.com/matthewfeickert/R-in-Jupyter-with-Binder/tree/master/binder) directory for example of what is needed to get this repository to run in Binder.

- [Specifying an R environment with a runtime.txt file](https://mybinder.readthedocs.io/en/latest/sample_repos.html#specifying-an-r-environment-with-a-runtime-txt-file)
   - Example Jupyter+R environment on Binder: [![Binder](http://mybinder.org/badge.svg)](http://mybinder.org/v2/gh/binder-examples/r/master?filepath=index.ipynb)
- [Binder FAQs](https://mybinder.readthedocs.io/en/latest/faq.html)

## Further Reading and Resources

- [Jupyter And R Markdown: Notebooks With R](https://www.datacamp.com/community/blog/jupyter-notebook-r), by [Karlijn Willems](https://github.com/Kacawi)
- [Rocker](https://www.rocker-project.org/)'s [R configurations for Docker repo](https://github.com/rocker-org/rocker)
- [rOpenSci](https://ropensci.org/)

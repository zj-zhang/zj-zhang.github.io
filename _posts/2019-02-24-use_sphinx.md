---
layout: post
title:  "Host a ReadTheDoc site for your package in 7 Steps by Sphinx"
categories: Programming
tags: python programming
author: ZijunZhang_
academia: true
mathjax: false
---

* content
{:toc}

So today I decided to give it another try for documentation genration for my new package [Darts](https://github.com/Xinglab/DARTS).
I previously tried [Sphinx](http://www.sphinx-doc.org/en/master/) for [CLAM](https://github.com/Xinglab/CLAM), but that didn't work out well. Since I am now a bit smarter to
have successfully compiled a pypi and Anaconda packages, I think maybe 
it's time to write down my experience for Sphinx as well, 
so that next time I could simply follow the dummie's guide for Python documentation site generation, and 
hopefully help others save some time.




## 1. Preparation works

First install `Sphinx`. There are different ways to this, but let's go with the mighty `conda`:
```bash
$ conda install sphinx
```

Now in the github repo, checkout a new branch "doc" so that we don't mess up with other devs.

Make a new folder named "docs" - this is where the documents will reside, and is consistent with Github page's
requirements.

```bash
git checkout -B doc
mkdir docs
```

## 2. Initialize a skeleton for Sphinx doc. 

This involves a few different options - I would like to separate the source
and build directories, enable autodoc, and have view source code linked.
```bash
cd docs/
sphinx-quickstart
```
I am building the doc for [Darts_BHT](#) for this example.

So now we can look at the initial product by

```bash
make html
firefox build/html/index.html
```

Firefox should pop-up and show the almost blank index page:
![1.png](https://raw.githubusercontent.com/zj-zhang/picture_store/master/2019-02-24-use_sphinx/1.png)


## 3. Adding an API/module page

Currently the doc site does not contain anything. Let's add the docstring from Python scripts to the doc, by using
the `sphinx-apidoc` utility. In fact, it has become a fairly common practice to write comprehensive docstrings within the
python code, to increase readability. For me, it helps a lot to review code that I wrote a few years ago, so is a good
habit to keep.

```bash
sphinx-apidoc -o source/ ../Darts_BHT
```

If you have trouble in this step, make sure your package is *importable* by first running

```bash
python setup.py develop
python -c "import Darts_BHT"
```
This will install the package in a virtual env. To test the installation is ok, use the "python -c" line to import it.


Change "Darts_BHT" to your package name. You should have the `modules.rst` file generated. Include this .rst file in the index by changing `index.rst` to

```
.. toctree::
   :maxdepth: 2
   :caption: Contents:

   modules
```

Now generate the html again by
```bash
make html
firefox build/html/index.html
```
You can see the API doc has now been added to the "Table of Contents":

![2.png](https://raw.githubusercontent.com/zj-zhang/picture_store/master/2019-02-24-use_sphinx/2.png)

You can change the level of depth to display by changing the ":maxdepth: N" line in the above `index.rst` file.


## 4. Changing HTML theme and Google-style docstring.

Next we can change a few things in the `source/conf.py` to customize the doc site. For example, we shall first change the style from 
the default "alabaster" to "sphinx_rtd_theme" (i.e. ReadTheDocs theme). 

![3.png](https://raw.githubusercontent.com/zj-zhang/picture_store/master/2019-02-24-use_sphinx/3.png)

You might have to install the theme manually:
```bash
pip install sphinx_rtd_theme
```

Before you enjoy the new good-looking rtd theme, add another few lines to `conf.py` so that the google-style docstring is properly 
recognized:
```python
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.viewcode',
    'sphinx.ext.napoleon',
]
 
napoleon_google_docstring = True
napoleon_use_param = True
napoleon_use_ivar = True
```

Now the new doc site should look like this - time to actually write your docstrings! OK I admit I left most docstrings as "TODO"
in the initial development..

![4.png](https://raw.githubusercontent.com/zj-zhang/picture_store/master/2019-02-24-use_sphinx/4.png)

## 5. Writting the docstring.

I guess there are a lot to write about the docstrings, but one good example is worth one thousand words!
I've been using this source as a reference to write docstrings:

[https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)


It takes time to write good docstrings, but think about the process from one year later, and
you would thank yourself for writing the details in the implementation at this time!


## 6. Add "README.md", "Get Started" to the doc site

A lot of times we also wrote an informative README file on the Github repo, so that people get to know what
this project does at the first glance. We can incorporate that information within our doc site - and to reduce
the amount of work and keep things in consistency, we can just use `include` to do the work.

Specifically, write the following content to `source/read.rst`:
```markdown
.. include:: ../../README.rst
```

If your `README` is in [Markdown](#) format, like I do, you will need another package [m2r](#) by `pip install m2r`. First, change the above `.. include::` to `.. mdinclude:: ../../README.md`


And also add the `m2r` extension within the `source/conf.py`, just like how we added the Google-style docstring `napoleon` extension in step #7:
```python
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.viewcode',
    'sphinx.ext.napoleon',
    'm2r'
]
```

If you still encounters issues, see the `m2r` github page for help: [https://github.com/miyakogi/m2r#sphinx-integration](https://github.com/miyakogi/m2r#sphinx-integration).

If your package is mostly about API, running the `sphinx-apidoc` will mostly do the trick.

On the other hand, it's always good to have a minimal set of examples to get people started quickly.
You can do this the same way as writing a "README.md", then either include that in the `index.rst` file
by adding that to the `toc`, or use `.. mdinclude::` to directly render it within the index page.

Till now, the doc site should look pretty close to the standards:

![5.png](https://raw.githubusercontent.com/zj-zhang/picture_store/master/2019-02-24-use_sphinx/5.png)

## 7. Connect to ReadTheDoc for hosting the doc site


---
layout: post
title:  "Steps and Tips for Python Packaging"
categories: Programming
tags: programming python
author: ZijunZhang_
academia: true
mathjax: false
---

Packaging a set of python scripts as a package is easy, and it makes your work more 
accessible to users. Moreover, you can better track the usage of your package 
over time! 


* content
{:toc}


##  1. Going from Python Scripts to Package

### 1.0 handling imports within
This should have been done during the development. But if not, make sure the 
`import` you used in your scripts are properly using [relative imports](https://docs.python.org/3/reference/import.html),
and you have a file called `__init__.py` for each submodule (i.e., under each folder).

### 1.1 write a command-line interface (CLI)
Often, you would want your users to be able to access programs and 
the set of tools without opening Python every time. Thus,
a command-line user interface is useful to provide this utility by 
calling your scripts directly from the command line.

The script should be named such that it's easy to remember & called 
from the terminal. Look at this example called `Darts_DNN` [here](https://github.com/Xinglab/DARTS/blob/master/Darts_DNN/bin/Darts_DNN),
or another example `CLAM` [here](https://github.com/Xinglab/CLAM/blob/master/bin/CLAM).

The essence of the CLI is a caller script that accepts arguments. 
If you have not used [argparse](https://docs.python.org/3/library/argparse.html) before,
be sure to check out its quick start usage!


A few things to keep in mind:
- it needs the [shebang line](https://stackoverflow.com/questions/2429511/why-do-people-write-usr-bin-env-python-on-the-first-line-of-a-python-script) so when directly called, the system knows to use Python interpreter
- it's usually put in a folder called `bin`, but feel free to use other names; just remember to 
change the folder name in the `setup.py` script, as discussed in the next section.
- use [argparse](https://docs.python.org/3/library/argparse.html) to manipulate 
input and check its sanity for multiple usages, check out how `sub-commands` can be 
implemented like [here](https://docs.python.org/3/library/argparse.html#sub-commands).
 


### 1.2 write a setup script
Write a script named `setup.py` following the instructions [here](https://docs.python.org/3/distutils/setupscript.html).

A few notes: 
- a useful way to test and debug your scripts is through `python setup.py develop`, once
you have written up a setup script. This will allow you to edit the code, and the changes
will be reflected without re-installing evey time. Read more about the development mode
[here](https://stackoverflow.com/questions/19048732/python-setup-py-develop-vs-install).



## 2. Publishing through PYPI
### 2.1 Register an account
Register here for official PyPI: https://pypi.org/
For the test-drive repository, regiter here: https://test.pypi.org/

### 2.2 Upload
CAUTION: once you upload to the official PYPI website, the script will be frozen
and not further changes are allowed. You can only update with a version bump, so
be sure to test everything before releasing!

Now you can upload following this tutorial, with your own account name and password!
https://packaging.python.org/tutorials/packaging-projects/

If you have followed the setup and README properly, just jump to this 
[section](https://packaging.python.org/tutorials/packaging-projects/#generating-distribution-archives)
in the above tutorial to build the distribution archive and publish.

The tutorial also only uploads to a test-drive repository that is independent from
the official repository.

For uploading to the official PyPI repository, just use this
```shell script
python3 -m twine upload 
```


## 3. Track usage reports
This website https://pepy.tech/ provides the easist way to track the download stats
from PyPI as of September 2021.

Alternatively, Google BigQuery is hosting more detailed information for download and
usage stats. A brief tutorial using SQL and Google Cloud
is [here](https://cloud.google.com/blog/topics/developers-practitioners/analyzing-python-package-downloads-bigquery).


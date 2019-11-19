---
layout: post
title:  "Using Souceforge.net to Host large files"
categories: Programming
tags: github sourceforge
author: ZijunZhang_
academia: true
mathjax: false
---

Using Souceforge.net to Host large files


A lot of times my GitHub repositories will depend on some external large
binary files. These files can range anywhere from trained deep learning model
parameters, or resources for a demo/test. Because GitHub will by default track
 files, and not easily accessible by a URL, making it not really ideal for 
 hosting large files.




* content
{:toc}

Hence what I do is to use Sourceforge.net to host external files, and keep 
GitHub for just development purposes. Sourceforge also provides a download statistics
counter and analyzer, which is a lot more convenient than the GitHub API for releases since the
last time I checked.

There are a few simple steps to use ```scp``` to upload files to Sourceforge.net.

### 1. Create a new project in Sourceforge.net

Remember to check "Unlimited File Downloads and Stats", which should be checked by default.
> **Note**: One of the very few things I don't like about Sourceforge is that all
>project names are unique IDs. Hence if there exists a registered project that matches a
>your project name, you would need to re-name your project:(

Suppose everything is set up; let's say, your user name is "user", and your project's name is "foo_proj";
the target folder we want to upload to is "foobar"

### 2. Use ``scp`` to upload to your project

```shell script
scp data.h5 user@frs.sourceforge.net:/home/frs/p/foo_proj/foobar/
```

Upon finished, the ```data.h5``` file should be accessible online.

> **Note**: Remember to check the ```md5sum``` of the online copy to see if that matches
>your local file!

### 3. Download using static url
There will a few mirrors for hosting your file. The general file download link pattern is:

https://master.dl.sourceforge.net/project/${foo_proj}/${foobar}/data.h5

Now you can use this static link in your GitHub code with ease. Bravo!
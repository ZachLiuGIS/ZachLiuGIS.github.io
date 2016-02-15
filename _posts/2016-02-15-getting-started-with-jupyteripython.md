---
layout: post
title: "Getting Started with Jupyter(IPython)"
description: ""
category: Python
tags: [python, ipython, jupyter, data analysis]
---
{% include JB/setup %}

I have been working with IPython for a while but haven't dig into what is happening with its seperation with Jupyter.

As is described on [ipython.org](http://ipython.org/)

>IPython is a growing project, with increasingly language-agnostic components. 
IPython 3.x was the last monolithic release of IPython, containing the notebook server, qtconsole, etc. 
As of IPython 4.0, the language-agnostic parts of the project: 
the notebook format, message protocol, qtconsole, notebook web application, etc. 
have moved to new projects under the name Jupyter. 
IPython itself is focused on interactive Python, part of which is providing a Python kernel for Jupyter.

With Jupyter, IPython itself has become a pure interactive kernel, which can be connected through Jupyter user interface
as sush Notebook, console, and qtconsole.

## Installation

To install Jupyter with IPython, run `pip install jupyter` for python2 or `pip3 install jupyter`. For installing
jupyter in a virtualenv, simply create the virtualenv and make sure the terminal is working on that virtualenv.

Installation through anaconda can be done by run `conda install jupyter`

## Using Jupyter in terminal console

Jupyter console uses terminal console as UI to interact with IPython kernel, similar to original ipython.

Run the following to open a Jupyter console

```
jupyter console
```

The complete document can be found at 
[http://jupyter-console.readthedocs.org/en/latest/](http://jupyter-console.readthedocs.org/en/latest/).

## Using Jupyter in Notebook

Jupyter Notebook is a super rich-featured web-based IPython user interface that can run interactive python code, preserve 
input/output, work with documents, etc. 
 
Try opening a Jupyter Notebook in your web browser by running

```
jupyter notebook
```

Read the whole Jupyter Notebook documentation here at 
[https://jupyter-notebook.readthedocs.org/en/latest/notebook.html](https://jupyter-notebook.readthedocs.org/en/latest/notebook.html).


## Using Jupyter in Qt Console

Jupyter qtconsole uses PyQt as its user interface with IPython. 

*Because Qt is not installed with Jupyter, separate installation is needed.*

Here is the detailed instruction of installing PyQt5 on Mac 
I found from [here](https://gist.github.com/guillaumevincent/10983814) 

### Requirements

  * xcode 
  * python 
  * Qt libraries 
  * SIP 
  * PyQt

### Download

  * [xcode](https://developer.apple.com/xcode/downloads/)
  * [python](https://www.python.org/download/)
  * [Qt libraries](http://qt-project.org/downloads)
  * [SIP](http://www.riverbankcomputing.co.uk/software/sip/download)
  * [PyQt](http://www.riverbankcomputing.co.uk/software/pyqt/download5)

### installation

  * install xcode
  * install the Command Line Tools (open Xcode > Preferences > Downloads)
  * install Qt libraries (qt-opensource-mac-x64-clang-5.*.dmg)
  * install python
  * create a virtual env
  * unzip and compile SIP and PyQt

Here are the whole commands for installation after download:

```bash
cd /var/tmp
cp /Users/gvincent/Downloads/PyQt-gpl-5.*.tar.gz .
cp /Users/gvincent/Downloads/sip-4.*.tar.gz .
tar xvf PyQt-gpl-5.*.tar.gz
tar xvf sip-4.*.tar.gz
cd sip-*/
python3 configure.py -d /path/to/virtualenv/site-packages --arch x86_64
make
sudo make install
sudo make clean
cd ../PyQt-gpl-5.2.1/
python3 configure.py --destdir /path/to/virtualenv/site-packages --qmake ~/Qt/5.*/clang_64/bin/qmake
make
sudo make install
sudo make clean
~/.env/ariane_mail/bin/python -c "import PyQt5"
```

I got a `fatal error: 'qgeolocation.h' file not found` error during installing PyQT5, I changed

```
python3 configure.py --destdir /path/to/virtualenv/site-packages --qmake ~/Qt/5.*/clang_64/bin/qmake
```

to

```
python3 configure.py --destdir /path/to/virtualenv/site-packages --qmake ~/Qt/5.*/clang_64/bin/qmake --disable=QtPositioning
```

and the problem is solved.

After these are successfully installed, you can open a Jupyter Qt console by running

```
jupyter qtconsole
```

Read the whole Jupyter QtConsole documentation here at [https://jupyter.org/qtconsole/stable/](https://jupyter.org/qtconsole/stable/)

## Summary

This post summarizes how to install Jupyter and IPython, and how to start using Jupyter in three different consoles:

- Web-based Notebook
- Terminal Console
- Qt Console

It also highlights how to install PyQt5 so Qt Console can be started.

After these initial setup and exploration, we can further explore the rich world of Jupyter that can help our python
workflow greatly.
---
layout: post
title:  "Setup Mac for Deep Learning"
date:   2017-12-15 11:44:00
---

If you are serious about building experience on Deep Learning, the first step 
is making your PC or MAC ready with all the necessary tools. It is a good idea to start
with Python since it is widely used.  

Once you follow the steps in this tutorial, you will have an environment to experiment 
and build deep learning solutions.

### Step 1 - Install Anaconda

There are a number of ways to install Python on MAC, easiest route being installing
 "Anaconda", which is freely available.

Go to the [this page](https://conda.io/miniconda.html) and download 64 bit installer 
for Python 3.6. On Mac, you can just double click on the .pkg file to install Anaconda.

Follow the prompts on the installer screens by accepting the default values. Typically,
installation will create a folder "anaconda" in your home directory.
After successfully completing the installation, you can test it by running these
commnds

```bash
> conda list
# packages in environment at /Users/kamal/anaconda:
#
_license                  1.1                      py36_1
alabaster                 0.7.10                   py36_0
anaconda                  4.4.0               np112py36_0
..
```

You may have a previous installation of Python already in the system. It is recommended
to remove the previous install to avoid any conflict. Otherwise you can change the
PATH variable to give preference to the Anaconda version of Python. Run the following
command to check the path of Python.

```bash
> which python
/Users/kamal/anaconda/bin/python
```

### Step 2 - Verify SciPy libraries
SciPy is a set of Python libraries used for scientific computing. These are the core
pacakges:

* NumPy
* SciPy
* Matplotlib
* IPython
* Sympy
* Pandas

By default, Anaconda will install these packages. You can verify it by running following
command:

```bash
conda list | egrep "(scikit|scipy|numpy|matplotlib|pandas)"
matplotlib                2.0.2               np112py36_0
numpy                     1.12.1                   py36_0
numpydoc                  0.6.0                    py36_0
pandas                    0.20.1              np112py36_0
scikit-image              0.13.0              np112py36_0
scikit-learn              0.18.1              np112py36_1
scipy                     0.19.0              np112py36_0
```

You can update invdividual packages such as scikit-learn using "conda update command"

```bash
> conda update scikit-learn
```

### Step 3 - Instal Deep Learning libraries

We need to install Keras with Tensorflow or Theano for Deep Learning. 

#### Install tensorflow

Tensorflow can be installed using conda or pip. Follow instructions from [here](https://www.tensorflow.org/install/install_mac) 
if you want to use pip.

```bash
> conda install -c conda-forge tensorflow

conda install -c conda-forge tensorflow
Fetching package metadata .............
Solving package specifications: .

Package plan for installation in environment /Users/kamal/anaconda:

The following NEW packages will be INSTALLED:

    markdown:     2.6.9-py36_0          conda-forge
    protobuf:     3.4.0-py36_0          conda-forge
    tensorboard:  0.4.0rc3-py36_2       conda-forge
    tensorflow:   1.4.0-py36_0          conda-forge
    webencodings: 0.5-py36_0            conda-forge

The following packages will be UPDATED:

    anaconda:     4.4.0-np112py36_0                 --> custom-py36ha4fed55_0
    html5lib:     0.999-py36_0                      --> 0.9999999-py36_0      conda-forge

The following packages will be SUPERSEDED by a higher-priority channel:

    conda:        4.3.30-py36h173c244_0             --> 4.3.29-py36_0         conda-forge
    conda-env:    2.6.0-0                           --> 2.6.0-0               conda-forge

Proceed ([y]/n)? y

conda-env-2.6. 100% |#####################################################################| Time: 0:00:00 726.31 kB/s
anaconda-custo 100% |#####################################################################| Time: 0:00:00   4.09 MB/s
markdown-2.6.9 100% |#####################################################################| Time: 0:00:00 601.81 kB/s
webencodings-0 100% |#####################################################################| Time: 0:00:00   3.41 MB/s
html5lib-0.999 100% |#####################################################################| Time: 0:00:00 711.88 kB/s
protobuf-3.4.0 100% |#####################################################################| Time: 0:00:01   2.53 MB/s
tensorboard-0. 100% |#####################################################################| Time: 0:00:00   2.32 MB/s
conda-4.3.29-p 100% |#####################################################################| Time: 0:00:00   1.88 MB/s
tensorflow-1.4 100% |#####################################################################| Time: 0:00:14   2.30 MB/s
```

### Install Keras
Keras is a library on top of Tensorflow, which alows fast experimentation with deep
neural networks.

Install Keras by running:

```bash
conda install -c conda-forge keras
Fetching package metadata .............
Solving package specifications: .

Package plan for installation in environment /Users/kamal/anaconda:

The following NEW packages will be INSTALLED:

    keras:      2.0.9-py36_0 conda-forge
    mock:       2.0.0-py36_0 conda-forge
    pbr:        3.1.1-py36_0 conda-forge

The following packages will be DOWNGRADED:

    tensorflow: 1.4.0-py36_0 conda-forge --> 1.0.0-py36_0 conda-forge

Proceed ([y]/n)? y

pbr-3.1.1-py36 100% |#####################################################################| Time: 0:00:00 665.21 kB/s
mock-2.0.0-py3 100% |#####################################################################| Time: 0:00:00 918.28 kB/s
tensorflow-1.0 100% |#####################################################################| Time: 0:00:16   2.06 MB/s
keras-2.0.9-py 100% |#####################################################################| Time: 0:00:00   1.70 MB/s
```

### Verify

Verify that you have the correct versions by running 

```bash
> conda list | egrep "(tensor|keras)"
```

Congratulations! You have installed the basic packages that are required for jump-starting
your journey. 
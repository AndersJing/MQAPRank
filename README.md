# MQAPRank
Protein Model Quality Assessment Program by Learning-to-Rank.

## Overview

MQAPRank is a global protein model quality assessment program based on learning-to-rank. It firstly sorts the decoy models by using single method based on learning-to-rank algorithm to indicate their relative qualities for the target protein firstly. And then it takes the first five models as references to predict the qualities of other models by using average GDT_TS scores between reference models and other models. The MQAPRank has participated in the CASP12 under the group name FDUBio and achieved the state-of-the-art performances.

## Requirements

The implementation was developed on CentOS 6.5(64bit), and therefore would execute properly on other Linux systems.

Platform:

* Linux 64bit (CentOS 6.5 (64bit) is recommended)

Software (the following softwares (and their updated versions) are recommanded):

* Linux Package
	- gcc-4.4.7-16.el6.x86_64
	- gcc-c++-4.4.7-16.el6.x86_64
	- kernel-devel-2.6.32-573.12.1.el6.x86_64
	- blas-devel-3.2.1-4.el6.x86_64
	- lapack-devel-3.2.1-4.el6.x86_64
	- glibc-2.12-1.166.el6_7.3.i686
* Perl 5
* Python 2.7.6
	- numpy 1.8.2
	- scipy 0.14.1
* Modeller 9.14

Disk Space:

> 6.2G

## Download and Install

The program is available at the following location:

https://gitlab.com/AndersJing/mqaprank.git

If the specific softwares in the 'Requirements' section have been installed (there is a common way of installation in the next section.), unpack the archive using the shell command:

	$ tar -zxvf MQAPRank.tar.gz

Change directory to the resulting folder:

	$ cd MQAPRank/

Run the provided installation script (requiring root privileges):

	$ sudo ./install.py

This will install the required files to the specified location and set the related parameters.

## A common way of installation

If you want to install the MQAPRank on a CentOS 6.5(64bit) system, follow the steps below:

* Install gcc, gcc-c++, kernel-devel, blas-devel, lapack-devel and glibc.i686:

	$ yum install gcc gcc-c++ kernel-devel blas-devel lapack-devel glibc.i686

* Install Python 2.7.6 (Download) :

	$ cd Python-2.7.6/

	$ ./configure

	$ make all

	$ make install

	$ make clean

	$ make distclean

	$ mv /usr/bin/python /usr/bin/python2.6.6

	$ ln -s /usr/local/bin/python2.7 /usr/bin/python

The yum is not compatible with Python 2.7, so we need to specify the Python version of the yum to Python 2.6 by changing the header of file '/usr/bin/yum' from '!/usr/bin/python' to '!/usr/bin/python2.6.6'.

* Install numpy 1.8.2 (Download) :

	$ cd numpy-1.8.2/

	$ python setup.py install

* Install scipy 0.14.1 (Download) :

	$ cd scipy-0.14.1/

	$ python setup.py install

* Install modeller 9.14 (XXXXXXXXXXX is the Modeller license key) (Download) :

	$ env KEY_MODELLER=XXXXXXXXXXX rpm -Uvh modeller-9.14-1.x86_64.rpm

* Copy related files to the Python 2.7.6 directory:

	$ cp -r /usr/lib64/python2.7/site-packages/* /usr/local/lib/python2.7/

* Install MQAPRank:

	$cd MQAPRank/
	$./install.py

## How to Use

MQAPRank consists two protein model quality assessment strategies, which are the single method and the quasi-single method, and MQAPRank will output the predicted values of both methods. Also, it could assess models using two structure similarity metrics: 'GDTTS' or 'TMscore'.

The typical command to call MQAPRank is:

	./MQAPRank.py modelList_input result_output GDTTS

* MQAPRank.py: the call script for the MQAPRank.
* modelList_input: the input file for the MQAPRank, containing the full path of the protein models to be evaluated (one line contains one model).
* result_output: the output file for the MQAPRank, containing three items in every line: full path of the model, the predicted value of the corresponding model by the single method (MQAPRank) and the predicted value of the corresponding model by the quasi single method (Quasi-MQAPRank).
* GDTTS: the structure similarity metric for the model, which could be 'GDTTS' or 'TMscore'.

## Getting started: a test Example

You will find an example problem at the test/ folder.

The test/ folder contains a subdirectory models/ and a textfile modelList. There are 10 protein models to be evaluated in the subdirectory models/ and the corresponding full path of these protein models are included in the textfile modelList ($$PATH is the installation path of the MQAPRank):
```
1 $$PATH/MQAPRank/test/models/model_1.pdb
2 $$PATH/MQAPRank/test/models/model_2.pdb
3 $$PATH/MQAPRank/test/models/model_3.pdb
4 $$PATH/MQAPRank/test/models/model_4.pdb
5 $$PATH/MQAPRank/test/models/model_5.pdb
6 $$PATH/MQAPRank/test/models/model_6.pdb
7 $$PATH/MQAPRank/test/models/model_7.pdb
8 $$PATH/MQAPRank/test/models/model_8.pdb
9 $$PATH/MQAPRank/test/models/model_9.pdb
10 $$PATH/MQAPRank/test/models/model_10.pdb
```

To run the example, execute the command:

	./MQAPRank.py test/modelList test/result GDTTS

The output in the result file would be as follows:
```
  1 Model Name  MQAPRank score  quasi-MQAPRank score
  2 $$PATH/MQAPRank/test/models/model_4.pdb    0.66    87.79
  3 $$PATH/MQAPRank/test/models/model_1.pdb    0.65    85.87
  4 $$PATH/MQAPRank/test/models/model_3.pdb    0.65    85.57
  5 $$PATH/MQAPRank/test/models/model_5.pdb    0.64    84.35
  6 $$PATH/MQAPRank/test/models/model_6.pdb    0.56    81.93
  7 $$PATH/MQAPRank/test/models/model_10.pdb   0.55    66.72
  8 $$PATH/MQAPRank/test/models/model_2.pdb    0.51    56.57
  9 $$PATH/MQAPRank/test/models/model_9.pdb    0.49    56.21
 10 $$PATH/MQAPRank/test/models/model_7.pdb    0.38    38.69
 11 $$PATH/MQAPRank/test/models/model_8.pdb    0.25    28.23
```

The first column contains the full paths of protein models, the second column contains the predicted values of the corresponding models by the single method (MQAPRank) and the third column contains the predicted values of the corresponding models by the quasi single method (Quasi-MQAPRank).

## Disclaimer

This software is free only for non-commercial use. It must not be distributed without prior permission of the author. The author is not responsible for implications from the use of this software.

## Known Problems

none

## Reference

If you use MQAPRank in your scientific work, please cite as:

* Xiaoyang Jing, Kai Wang, Ruqian Lu and Qiwen Dong. Sorting protein decoys by machine-learning-to-rank[J]. Scientific Reports, 2016, 6.
* Xiaoyang Jing, Qiwen Dong. MQAPRank: improved global protein model quality assessment by learning-to-rank[J]. BMC bioinformatics, 2017, 18(1): 275.


## Contact

xyjing14(at)fudan.edu.cn

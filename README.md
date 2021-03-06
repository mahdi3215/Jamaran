# Kaggle-aquire-valued-shoppers-challenge

An Implementation of Kaggle Valued Shoppers Challenge - 

## Kaggle Contest Page

See http://www.kaggle.com/c/acquire-valued-shoppers-challenge/ for the competition

## DataSet

The contest dataset will be used except that its transaction.csv file size(22GB) will be reduced
to around 1.6GB. If you check out the offers.csv file you’ll see all the categories and companies 
a coupon offer can have. We can discard the rows from the transactions data which don’t have a 
category id or a company id which is on offer. It is also possible to use the complete dataset with
this code but it will certainly take more time. so make sure to pick up the option which best suits 
you. 

See https://www.kaggle.com/c/acquire-valued-shoppers-challenge/data for rules and data.

## Project Files and Directories

* *workspace*: as the name suggests, every file , whenever needed to be saved, will be saved in this directory <br />
* *utils*: some utility code reside in this directory<br />
* *varinfo*: the generated vowpal wabbit varinfo must be saved in this 
category, following the option 3 in linux_wx_bashscript.txt<br />
* *histogram.py*:each varinfo output file, once generated , will be
plotted using this file <br />
* *RFM_based_feature_extraction.py*: extracting features based on RFM model <br />
* *Improved_RFM_feature_extraction.py*: not only extracting RFM features but also
considering some hueristical features to obtain better performance.<br />
* *submission.py*:transforms the final generated file predictions.txt into a kaggle submission file.<br />
* *Evaluation*: contains files needed for evaluating results <br />

## Installing prerequisites

1) Make Sure you have Python 2.7 Installed <br />
2) You'll then need to install numpy, scipy and matplotlib modules of python. <br />
In ubuntu you can install them with this command:

    sudo apt-get install python-numpy python-scipy python-matplotlib
    
If you are on windows, then you might use pip in this order:<br />
```pip install numpy ```<br />
```pip install scipy  ```<br />
```pip install matplotlib ```<br />

3)(optional) install scikit-learn for AUC evaluation.
                
see https://pypi.python.org/pypi/scikit-learn/0.15.2 for tutorial on how to install.<br />


## How to run?

A step by step process to run the code is listed below: <br />

2) Put the Kaggle dataset files in current( root ) directory

1) run Improved_RFM_feature_extraction.py file: <br />

    python Improved_RFM_feature_extraction.py

Data is reduced and features are generated by the end of this step. <br />

3) follow the command line for train in linux_vw_bashscript.txt: <br />

    vw workspace/train.vw -c -k --passes 40 -l 0.85 -f workspace/model.vw --loss_function quantile --quantile_tau 0.6

This is the first option in the command file.
In this Step, vowpal wabbit(version 7.1) is used so as to train the model<br />

4) (Optional) follow the command line for generating vw-varinfo feature relevance in linux_vw_bashscript.txt <br />

    vw-varinfo workspace/train.vw > varinfo/result.varinfo.vw

This is option 3 in the command file. we can use vm-varinfo to expose all variables of 
our model in a human readable form.<br />

5) (if step 3 is done) run the historgam.py<br />

    python histogram.py
    
by the end of this step, vm-varinfo outputs
will be converted to visual images by ploting.<br /><br />

see https://github.com/JohnLangford/vowpal_wabbit/wiki/using-vw-varinfo for more.<br />

6) follow the command line for test in linux_vw_bashscript.txt<br />

This is the second option in the command file.<br />

    vw workspace/test.vw -t -i workspace/model.vw -p workspace/predictions.txt 

In this Step, vowpal wabbit(version 7.1) is used so as 
to test the model. the output file will be saved to 
workspace as predictions.txt<br />


7) finally, run the file submission.py<br />

    python submission.py

all we need to do after step 5 is to convert our predictions.txt <br />
into a kaggle submission .csv format.This is achieved in this step.<br />

See https://kaggle.com for submission sample.

8) (optional) In case you need to evaluate results yourself, you then need to run the files <br />
in the evaluation directory.To this, we first divide the train file, half for training and<br />
half for testing, using the results for evaluation. after following previous steps for this<br />
newly generated files, we then add expected labels for the newly submission.csv file created <br />
using combineExpected.py. then eval.py is all we need to do our evaluations<br />

see http://scikit-learn.org/stable/modules/model_evaluation.html for more on evaluation



## Vowpal Wabbit:

The Vowpal Wabbit (VW) project is a fast out-of-core learning system sponsored by Microsoft Research and (previously) 
Yahoo! Research. Support is available through the mailing list.</br>

if you are on linux, you can download Vowpal Wabbit precompiled statically-linked binaries in this link: http://finance.yendor.com/ML/VW/Binaries/ 


see https://github.com/JohnLangford/vowpal_wabbit/wiki for more

## Data Reduction

The written script for data reduction is at ./utils directory in reduceData.py file. however you can also use other reduction
strategies discussed here: https://www.kaggle.com/c/acquire-valued-shoppers-challenge/forums/t/7666/getting-started-data-reduction/




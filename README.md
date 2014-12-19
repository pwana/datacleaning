# Gettting and Cleaning Data HAR dataset project 1.
# Pat Warner

## Executive Summary: We are given a raw zipped file that contains various measurements
selected from various HumanActivityRecognition monitors. 

* We create a tidy dataset, creating and using the R script, run_analysis.R.
* The tidy data file is written to a txt file, HARtidy.txt.
* A codebook, describing the HARtidy.txt file is cretated, codebook.txt.

Comments are shown below for the run_analysis.R script and methodologies.

### Methods:

Our objective is to satisfactorily
Load and Clean the raw file into a tidy data set. We begin by creating a directory to 
load the zipped data. We point to the directory and unzip the associated subdirectories and
files.

### We start the cleaning of data by loading the set of feature and activity labels.
We point to paths by pasting directory and files together. We typically can
set header = FALSE, as none of the files contain heaers. We also want strings 
for the main HAR.features. However, our activity labels are treated as factors.
Next we load train and test datasets and merge subject.id, activity, and
HAR.data into dataframes, then merge train and test into one set by use of the rbind command.
We make sure to label the activity and features with the associated y_train/y_test and
activity.labels files. Our features and activities are now labeled.


Using grepl commands, we can get those files with -mean() and -std()
in the labels, and select them exclusively.

### Lastly, we use aggregate to subset mean and std variables by the activity and subject IDs.












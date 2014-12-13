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


setwd("E://Coursera/cleandata/project")
unzip("getdata_projectfiles_UCI HAR Dataset(1).zip")

### We then point to sub-directories to fetch various relevant files.

dir.names <- list.dirs()
[1] "."                                       
[2] "./UCI HAR Dataset"                       
[3] "./UCI HAR Dataset/test"                  
[4] "./UCI HAR Dataset/test/Inertial Signals" 
[5] "./UCI HAR Dataset/train"                 
[6] "./UCI HAR Dataset/train/Inertial Signals"

### We start the cleaning of data by loading the set of feature and activity labels.
We point to paths by pasting directory and files together. We typically can
set header = FALSE, as none of the files contain heaers. We also want strings 
for the main HAR.features. However, our acrivity labels are treated as factors.

### feature labeling
DF is total dataset (subject.id,activity,HAR features)
HAR.features <- read.table(paste(dir.names[2],"/features.txt",
sep=""),header=FALSE,stringsAsFactors=FALSE,sep="")$V2
HAR.labels <- c("subject.id","activity",HAR.features)
activity.labels <- read.table(paste(dir.names[2],"/activity_labels.txt",sep=""),
header=FALSE,stringsAsFactors=TRUE)[,2]

Next we load train and test datasets and merge subject.id, activity, and
HAR.data into dataframes, then merge train and test into one set by use of the rbind command.
We make sure to label the activity and features with the associated y_train/y_test and
activity.labels files. Our features and activities are now labeled.


### get training data
HAR.train.subject <- read.table(paste(dir.names[5],
"/subject_train.txt",sep=""),sep="")
HAR.train.activity <- read.table(paste(dir.names[5],"/y_train.txt",
sep=""),colClasses=c("factor"), sep="")$V1 
levels(HAR.train.activity) <- as.character(activity.labels)
HAR.train <- read.table(paste(dir.names[5],"/X_train.txt",
sep=""),sep="")

train.df <- cbind(HAR.train.subject,HAR.train.activity,HAR.train)
colnames(train.df) <- HAR.labels

### get testing data
HAR.test.subject <- read.table(paste(dir.names[3],
"/subject_test.txt",sep=""),sep="")
HAR.test.activity <- read.table(paste(dir.names[3],"/y_test.txt",
sep=""),colClasses=c("factor"),sep="")$V1
levels(HAR.test.activity) <- levels(activity.labels)
HAR.test <- read.table(paste(dir.names[3],"/X_test.txt",
sep=""),sep="")
test.df <- cbind(HAR.test.subject,HAR.test.activity,HAR.test)
colnames(test.df) <- HAR.labels

### train/test dataframe
HAR.dataframe <- rbind(train.df,test.df)

Using grepl commands, we can get those files with -mean() and -std()
in the labels, and select them exclusively.


### subset by mean and std measures only
labels.meanstd <- HAR.labels[grepl("-mean()",HAR.labels,
fixed=TRUE) | grepl("-std()",HAR.labels,fixed=TRUE)]
meanstd.df <- HAR.dataframe[c("subject.id","activity",labels.meanstd)] # 10297X67

### Lastly, we use aggregate to subset mean and std variables by the activity and subject IDs.

final dataframe will aggregate all mean and std variables 
by activty and subject, using mean function.
avg.meanstd.ID <- aggregate(meanstd.df[,3:dim(meanstd.df)[2]],by=list(meanstd.df$activity,meanstd.df$subject.id),mean)
names(avg.meanstd.ID)[1] <- "activity"
names(avg.meanstd.ID)[2] <- "subject.id"

### clean up names of dataset
names(avg.meanstd.ID) <- gsub("-mean()",".mean",names(avg.meanstd.ID),fixed=TRUE)
names(avg.meanstd.ID) <- gsub("-std()",".std",names(avg.meanstd.ID),fixed=TRUE)
names(avg.meanstd.ID) <- gsub("-",".",names(avg.meanstd.ID),fixed=TRUE)


write.table(avg.meanstd.ID,file="HARtidy.txt",row.names=FALSE)












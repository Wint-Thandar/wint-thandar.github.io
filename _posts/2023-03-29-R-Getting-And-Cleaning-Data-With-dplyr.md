---
layout: post
title: Getting and Cleaning Data with dplyr
image: "/posts/dplyr.png"
tags: [R, dplyr]
---

## Project Purpose
This project was done as part of the [JHU Getting and Cleaning Data Course](https://www.coursera.org/learn/data-cleaning). The purpose of this project is to showcase the ability to collect, work with, and clean a data set. The goal is to prepare the tidy data that can be used for later analysis. The `dplyr` package, one of the core packages of the `Tidyverse`, is used to manipulate data in this project.<br><br>

## Git Hub Repository Link
The source code and documentations for this project can be found at my [GitHub repository](https://github.com/Wint-Thandar/jhu-getting-and-cleaning-data-week4-project).<br><br>

## Data Source
The data used in this project is the data collected from the accelerometers from the Samsung Galaxy S smartphones. The data is downloaded from the [Human Activity Recognition Using Smartphones Data Set](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones) webpage of the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php) website.

**Dataset Download Link**: <https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip><br>
**Dataset Description**: <http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones><br><br>

## Dataset Information
The dataset information below is directly quoted from the UCI webpage.
<br>
> "The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data.<br>
> <br>
> The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain.<br>
> <br>
> For each record in the dataset it is provided:<br>
> - Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.<br>
> - Triaxial Angular velocity from the gyroscope.<br>
> - A 561-feature vector with time and frequency domain variables.<br>
> - Its activity label.<br>
> - An identifier of the subject who carried out the experiment."

<br>
## Data Manipulation
The data transformation for this project is done in five steps.<br>
**Step 1:** Merge the downloaded training and test data set files to create one data set.<br>
**Step 2:** Extract only the measurements on the mean and the standard deviation for each measurement.<br>
**Step 3:** Use descriptive activity names to name the activities in the data set.<br>
**Step 4:** Appropriately label the data set with descriptive variable names.<br>
**Step 5:** From the data set in step 4, create a second, independent tidy data set with the average of each variable, for each activity and each subject.<br>
<br>
### Step 1: Merge the downloaded training and test data set files to create one data set.
First, import `dplyr` package.
```r
library(dplyr)
```
Then, set the file URL to be downloaded from and the file name to be saved as. The file path is not required, because the zip file is to be downloaded to the working directory. 
```r
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
fileName <- "project_data.zip"
```
Download the zip file containing the data sets by using `download.file` function. Use `curl` as the download method.
```r
download.file(fileUrl, destfile = fileName, method = "curl")
```
Extract the downloaded zip file using `unzip` function. 
```r
unzip(zipfile = fileName)
```
After extracting the files, use `read.table` data import to read each of the necessary data files and create a data frame for each.
```r
# Read training data files.
training_subject <- read.table('./UCI HAR Dataset/train/subject_train.txt', header=FALSE)
x_train <- read.table('./UCI HAR Dataset/train/X_train.txt', header=FALSE)
y_train <- read.table('./UCI HAR Dataset/train/y_train.txt', header=FALSE)
```

```r
# Read test data files.
test_subject <- read.table('./UCI HAR Dataset/test/subject_test.txt', header=FALSE)
x_test <- read.table('./UCI HAR Dataset/test/X_test.txt', header=FALSE)
y_test <- read.table('./UCI HAR Dataset/test/y_test.txt', header=FALSE)
```

```r
# Read features data files.
features <- read.table('./UCI HAR Dataset/features.txt', header=FALSE)
```

```r
# Read activity labels data files.
activity_labels <- read.table('./UCI HAR Dataset/activity_labels.txt', header=FALSE)
```
After all the files are read into data frames, set the column names to the data frame columns of training and test data frames. Doing so will help the data to merge correctly, when merging the training and test data in the next step. For both training and test data frames, set the column names for training/test subject, features, and activity by using `colnames` function. Use the data frame `features` to get all the feature names.
```r
# Add column names to training data frames.
colnames(training_subject) <- "subjectID"
colnames(x_train) <- features[,2]
colnames(y_train) <- "activityID"

# Add column names to test data frames.
colnames(test_subject) <- "subjectID"
colnames(x_test) <- features[,2]
colnames(y_test) <- "activityID"
```
Merge above data frames into a single data set, using `cbind` data frame method to bind the columns, and `rbind` data frame method to bind the rows together.
```r
all_data <- rbind(
    cbind(y_train, x_train, training_subject),
    cbind(y_test, x_test, test_subject)
)
```
<br>
### Step 2: Extract only the measurements on the mean and the standard deviation for each measurement.<br>
To extract the mean and the standard deviation for each measurement, `subjectID` column, `activityID` column and all the columns with `mean` and `std` as part of the column name need to be extracted. Use the `grepl` function to search the column names that matches the pattern. 
```r
required_col <- grepl("subjectID|activityID|mean|std", colnames(all_data))
```
As the logical vector is returned from the previous step, the data frame can now be filtered by using that logical vector. 
```r
all_data <- all_data[, required_col]
```
<br>
### Step 3: Use descriptive activity names to name the activities in the data set.<br>
For step 3, use `activity_labels` data frame to replace the `activityID` with the activity type labels using `factor` function.
```r
all_data$activityID <- factor(all_data$activityID, levels = activity_labels[,1], labels = activity_labels[, 2])
```
<br>
### Step 4: Appropriately label the data set with descriptive variable names.<br>
To change the column labels of the data set, get all column names of `all_data` data frame first by using `colnames` function.  
```r
all_data_col <- colnames(all_data)
```
Then, remove the parentheses by using `gsub` function to replace them with `""` (no character).
```r
all_data_col <- gsub("[\\(\\)]", "", all_data_col)
```
Replace abbreviations with descriptive column names by using `gsub` function.
```r
all_data_col <- gsub("activityID", "activityType", all_data_col)
all_data_col <- gsub("^t", "time", all_data_col)
all_data_col <- gsub("^f", "frequency", all_data_col)
all_data_col <- gsub("Acc", "Accelerometer", all_data_col)
all_data_col <- gsub("Gyro", "Gyroscope", all_data_col)
all_data_col <- gsub("Mag", "Magnitude", all_data_col)
all_data_col <- gsub("Freq", "Frequency", all_data_col)
all_data_col <- gsub("BodyBody", "Body", all_data_col)
```
Apply the cleaned column names to `all_data` data frame.
```r
colnames(all_data) <- all_data_col
```
<br>
### Step 5: From the data set in step 4, create a second, independent tidy data set with the average of each variable, for each activity and each subject.<br>
Aggregate data by using `aggregate` function to get the average of each variable for each activity and each subject.
```r
tidy_data <- aggregate(. ~subjectID + activityType, all_data, mean)
```
Sort the data by subject ID and activity type.
```r
tidy_data <- tidy_data[order(tidy_data$subjectID, tidy_data$activityType),]
```
To create a text file with the tidy data, use `write.table` function to write the cleaned data frame to `tidy_data_set.txt` file. The file will be created in the working directory.
```r
write.table(tidy_data, file = "tidy_data_set.txt", row.names = FALSE)
```
<br>

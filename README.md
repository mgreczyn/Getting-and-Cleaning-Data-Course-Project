# Getting-and-Cleaning-Data-Course-Project
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

 You should create one R script called run_analysis.R that does the following. 
- Merges the training and the test sets to create one data set.
- Extracts only the measurements on the mean and standard deviation for each measurement. 
- Uses descriptive activity names to name the activities in the data set
- Appropriately labels the data set with descriptive variable names. 
- From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity 
and each subject.

## How to use this script
This script will work as long as the data for the project are in the working directory and unzipped. The script will return a 
data frame meeting the requirements above. In addition, it will write the data frame's contents to a text file called 
GettingCleaningProjectOutput.txt, creating it if it doesn't already exist. The text file may not be easily readable, but 
running the following code in R will read the table back into R: 

```Output<-read.table("GettingCleaningDataProjectOutput.txt", header=TRUE,stringsAsFactors = FALSE); View(Output)```

## Process details
The script is pulling from two datasets located in the UCI HAR Dataset. One we'll call "test" and the other we'll call "train".
Within the "UCI HAR Dataset" folder, there should be two folders and several text files. The folders are labelled "test" and 
"train" and the text files are labelled as follows:
- activity_labels.txt
- features_info.txt
- features.txt
- README.txt
The actual datasets are inside additional text files located within the two folders
- a "subject" file, containing subject identifiers 
- an "X" file containing the data measurements
- and a "y" file  containing activity identifiers
There are 30 subjects between the "test" and "train" datasets, with 30% in "test" and 70% in "train. 

Overall, the script follows the following pseudocode:
- read in subject_test.txt into subjectTest and subject_train.txt into subjectTrain using read/table(), labeling the variable 
"SubjectID"
- read in activity_labels.txt to ActivityLabels, (this will be used to change the numeric activity identifiers to something more intuitive)
- read in y_test.txt to yTest, labelling the variable "ActivityDescription" 
(this contains the activity identifiers for the two datasets)
- create a character vector called "Activity"
- loop through y_test, appending the string activitity description corresponding to each ActivityDescription value
to the Activity vector
- Replace the numeric values in y_test with the string values in Activity
- Repeat the above 4 steps for y_train, reading into yTrain.
- read in features.txt, assigning the values in the second column to a character vector called variableNames, which we will 
use to name the variables in the measurement files
- Read in X_test.txt and X_train.txt (the measurement files) to XTest and XTrain, using variableNames to label the columns
- Identify the column indeces in the two datasets that contain mean and standard deviation estimates by using grep()
- Extract only the columns containing mean and standard deviation measurements from XTest and XTrain
- Use cbind() to clip subjectTest, yTest and XTest into one data frame called Test
- Repeat for subjectTrain, yTrain and XTrain, creating Train
- Merge Test and Train into mergedData
- melt mergedData into newSet, using all of the columns except SubjectID and Activity as the measure variables
- use dcast() to recast newSet so it shows average values for all measurements by activity by subject.
- rename newSet's variables to indicate that it shows average values
- Finally, write newSet to a file and return newSet from the function

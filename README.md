# Getting and cleaning data: peer-assesment

This file provides information about the `run_analysis.R` script, which is a part of the Course Project submission on Getting and Cleaning Data from the DataScience Specialization on Coursera.


## The assignment

>You should create one R script called run_analysis.R that does the following. 
  1.  Merges the training and the test sets to create one data set.
  2. Extracts only the measurements on the mean and standard deviation for each measurement. 
  3. Uses descriptive activity names to name the activities in the data set
  4. Appropriately labels the data set with descriptive variable names. 
  5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.


## Project

The project consists of the following items:

1. [run_analysis.R](./run_analysis.R) Which is used to perform all merging, tidying, transformation and extraction.
2. [README.md](./README.md) (this file)
3. [The Code Book](./CodeBook.md) describing data collection, variables and methods used
4. [independent_tidy_data.csv]( ./independent_tidy_data.csv ) <sub>The script outputs a text file via `write.table()` which in this repo has been converted to .csv for better readability</sub>
5. [features_info.txt](./features_info.txt) and [features.txt](./features_info.txt) as they appear in the source package.
6. [The raw data](./getdata_projectfiles_UCI HAR Dataset.zip)


## Coding

Development was performed using RStudio 0.99.486, R version 3.2.2. "Fire Safety" on Windows 8.1

Adhering to the Coding Standard lecture from Week 2 of the R Programing course, single-lined instructions are wrapped as to not exceed the 80 characters screen margin.

Payload containing the raw data was obtained on 2015-10-11 11:05 AM from:   
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 


## Processing

The assignment calls for the script to be runnable as long as the working directory contains "Samsung Data" but is unclear about what that means; a directory "Samsung Data" or the directory containing data from Samsung.

To make it easier for others to execute and examine the script a block of code has been added which shall upon finding both `"test"` and `"train"` directories in the currently active working directory, assume the script has been dropped into the root directory of the extracted source package. 

Although the script was developed on Windows, `file.path()`s were used to construct the paths to the data-sets, so it should work on *nix systems without modification.

##
## As long as the train and test directories are found, the script will assume
## it's been place into the root directory of the source package data.
##

1. Merge training and test set
   * `x_train` data-set is initialiy loaded using `read.csv()` with a row limit set to `10`, providing adequate number of sample rows for R to determine column classes.
   * `x_train` id loaded specifying the column classes determined above, in it's full length. This approach significantly reduces the load-time. <sub>See R Programming course, Week 1: Loading Large Tables</sub>
   * `x_train` and `x_test` data-sets are merged into data.frame `x` using `rbind()`
   * `y_train` and `y_test` data-sets are loaded, in full, without determining column classes due to their small size. `rbind()` merges the two into data.frame `y`
   * the same process is performed on `subject_train` and `subject_test` data-sets producing data.frame `subjects`

2. Extract measurements on mean and standard deviation
   * `features.txt` dataset is loaded into a data.frame from which only variable names containing `std()` and `mean()` are extracted 
   and applied to a subseting function on `x` to obtain the desired data.frame `data`
   * a character vector `sanitized_names` of names to tidy-up is obtained from vector `features` using `grep()`
   * list `transform_map` lists in a *search-and-replace* notation what is to be transformed
   * a loop performs the tidying. <sub>See Week 4 of Getting and Cleaning data : Regular expressions</sub>
   * Dashes `-` and underscores `_` are handled recursively using `gsub()`
   * `names( data )` is assigned the value of this new `sanitized_names` vector
   
  List of transformations is contained within [The Code Book](./CodeBook.md)

3. Uses descriptive activity names   
   * `activity_labes` dataset is loaded and transformed to lowercase with all underscores `_` dropped.
   * vector `activity_labes` is factorized using `as.factor()` and assigned to column 1 of `y` data.frame (the merged activities data-set) <sub>See Week 4 of Getting and Cleaning data: Editing text variables.</sub>

4. Appropriately label the data set with descriptive variable names
   * `names( subject )` is assigned the value `subject`
   * `tidy_data` data.frame is obtained via merging of `subjects`, `y` and `data` data.frames using cbind()

5. Creates independent tidy data set with averages of each variable for each activity and each subject
   * vector containing group ids `grouped` is obtained by `split()`-ing the `tidy_data` data.frame to which a factrorized column had been added by `paste()`-ing the `subcject` and `activity` values.
   A value from this concatinated field might look something like `1.walking`
   * `lapply()` is used with an annonimous `function()` containing `colMeans()` for only numerical fields. `NA` values are removed. <sub>See R Programming Course, Week 3: loop functions - lapply()</sub>
   * the resulting data.frame must be passed through `t()` in order to switch rows/columns, thus producing data.frame `report`
   * as `report` now lacks non-numeric fields these are to be reintroduced by means of parsing `rownames()`
   * `activity` is factorized as per guide-lines introduced in Week 4 of the course
   * `subject` is passed through `as.numeric()` as to permit proper ordering
   * the `report` object is ordered by `subject` and `activity` than written to disk using `write.table()` with `row.names` set to `FALSE`
    
   The resulting file (in .csv format) is available [here](./independent_tidy_data.csv).



# Getting and cleaning data: peer assesment

This file provides information primarily on the `run_analysis.R` script, which is a part of the Project Course submission on Getting and Cleaning Data from the DataScience Specialization on Coursera.


## Project

The project consists of the following items:

1. [run_analysis.R](./run_analysis.R) Which is used to perform all merging, tidying, transformation and extraction.
2. [README.md](./README.md) (this file)
3. [The Code Book](./CodeBook.md) describing data collection, variables and methods used
4. [independent_tidy_data.csv]( ./independent_tidy_data.csv ) (the final report file)
5. [features_info.txt](./features_info.txt) and [features.txt](./features_info.txt) as they appear in the source package.
6. [The raw data](./getdata_projectfiles_UCI HAR Dataset.zip)



## Processing

Function `path()` is created as to handle the possibility of the data directory being moved or renamed. `path( <filename> )` will always return a valid *windows* path to the `filename` passed.

1. Merge training and test set
   * `x_train` data-set is initialiy loaded using `read.csv()` with a row limit set to `10`, providing adequate number of sample rows for R to determine column class.
   * `x_train` id loaded specifying the column classes determined above, in it's full length. This approach significantly reduces the load-time.
   * `x_train` and `x_test` data-sets are merged into data.frame `x` using `rbind()`
   * `y_train` and `y_test` data-sets are loaded, in full, without determining column classes due to their small size. `rbind()` merges the two into data.frame `y`
   * the same process is performed on `subject_train` and `subject_test` data-sets producing data.frame `subjects`

2. Extract measurements on mean and standard deviation
   * `features.txt` dataset is loaded into a data.frame from which only variable names containing `std()` and `mean()` are extracted 
   and applied to a subseting function to `x` to obtain the desired data.frame `data`
   * a characted vector `sanitized_names` of names to tidy-up is obtained from `features` using `grep()`
   * list `transform_map` lists in a *search-and-replace* notation what is to be transformed
   * a loop performs all the tidying
   * Dashes `-` and underscores `_` are handled recursivly using `gsub()`
   * `names( data )` is assigned the value of this new `sanitized_names` vector

3. Uses descriptive activity names   
   * `activity_labes` dataset is loaded and transformed to lowercase with all underscores `_` dropped.
   * vector `activity_labes` is factorized using `as.factor()` and assigned to column 1 of `y` data.frame (the merged activities data-set)

4. Appropriately label the data set with descriptive variable names
   * `names( subject )` is assigned the value `subject`
   * `tidy_data` data.frame is obtained via merging of `subjects`, `y` and `data` data.frames using cbind()

5. Creates independent tidy data set with averages of each variable for each activity and each subject
   * vector containing group ids `grouped` is obtained by `split()`-ing the `tidy_data` data.frame to which a factrorized column was added by `paste0()`-ing the `subcject` and `activity` values.
   A value from this concatinated field might look something like `1.walking`
   * `lapply()` is used with an annonimous `function()` containing `colMeans()` for only numerical fields. `NA` values are removed.
   * the resulting data.frame must be passed through `t()` in order to switch rows/columns, thus producing data.frame `report`
   * as `report` nnow lacks non-numeric fields these have to be reintroduced by parsing it's `rownames()`
   * `activity` is factorized as per guide-lines introduced in Week 4 of the course
   * `subject` is passed through `as.numeric()` as to permit proper ordering (Excel)
   * the `report` is written to disk using `write.csv()` with `row.names` set to `FALSE`


## In closing

It is this future data scientist's view (with 20 years of programming experience) that the tidy data rules are in some cases taken into extreme.
Uncompromizingly adhering to the rules, in a large data-set with long variable names, produces column names utterly unreadable to a human. And while they might be harded to debug
as suggested by our kind TAs... well, that's why we have debuggers :)

The answer to the question of sanitizing variable names like `fBodyGyro-bandsEnergy()-25,48` without using at least underscore `_` eludes me.

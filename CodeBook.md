# Code book

This Code Book is part of the Course project solution on *Getting and Cleaning data* for the DataScience Specialization offered by Johns Hophins Bloomberg school of public medicine via Coursera.
It provides information on the data collection process, variables and transformations performed.

This codebook is, in part, based on an example provided here:  
http://www.icpsr.umich.edu/icpsrweb/ICPSR/help/cb9721.jsp

## Data collection

The original, for the purposes of this analysis considered raw, data-set can be obtained from   
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Raw data collection process is quoted here from the source README.txt file for conveniance
> The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 
  
> The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details.


## Processing

The raw data was merged using `cbind()` and sanitized to obtain a tidy date-set, from which only the mean() and std() values were extracted. Grouping of the resulting data-set
was performed using `split()` based on `subject` and `activity` and the resulting independant tidy dataset was obtained by calling `colMeans()` for each group using `lapply()`. See [README.md](./README.md) for details on script execution.



## Transformations

Original variables from the raw data-set found in [features.txt](./features.txt) went through a tidying process as per Week 4 lecture of Getting and Cleaning data, summarized here:

1. Names of variables should be:
   * All lower case when possible
   * Descriptive
   * Not duplicated
   * Not have underscores, dots or whitespaces (or punctuation marks)

2. Variables with character values:
   * Should usually be made info factors
   * Should be descriptive


Specifically, this means:  

1. column names have been made lower-cased
2. commas and underscores have been dropped (this includes the text values in column `activity`)
3. function names as [described here](./features_info.txt) have been replaced with their *descriptive* names (std() to 'StandardDeviation')
4. domain measurement prefixes 'f' and 't' have been replaced with 'frequency' and 'time' respectively
5. shorthands as Acc have been replaced with more descriptive names ie. 'acceleration'


For the purposed of this project *only mean and standard deviation* values have been extracted from the merged and tidyed dataset obtained by a process describer in the [README.md](./README.md)

## Variables
Provided is the list of sanitized variable names as they appear in independent_tidy_data.csv

| Variable name | Variable position | Variable                     | Values and explanations |
| ------------- | -----------------:| ---------------------------- | ----------------------- |
| subject | 1 | numeric | The id of the test subject. The independant tidy dataset contains subjects `1` to `30` |
| activity| 2 | factor  | The actual activity being performed during the experiament. Possible values are: `sitting`, `walking`, `walkingupstairs`, `walkingdownstairs`, `laying`, `standing` |
|timebodyaccelerationmeanx | 3 |numeric| See original [features_info.txt](./features_info.txt) for details.|
|timebodyaccelerationmeany | 4 |numeric|units: G|
|timebodyaccelerationmeanz | 5 |numeric|units: G|
|timebodyaccelerationstandarddeviationx | 6 |numeric|units: G|
|timebodyaccelerationstandarddeviationy | 7 |numeric|units: G|
|timebodyaccelerationstandarddeviationz | 8 |numeric|units: G|
|timegravityaccelerationmeanx | 9  |numeric|units: G|
|timegravityaccelerationmeany | 10  |numeric|units: G|
|timegravityaccelerationmeanz | 11  |numeric|units: G|
|timegravityaccelerationstandarddeviationx | 12  |numeric|units: G|
|timegravityaccelerationstandarddeviationy | 13  |numeric|units: G|
|timegravityaccelerationstandarddeviationz | 14  |numeric|units: G|
|timebodyaccelerationjerkmeanx | 15  |numeric|units: m/s<sup>3</sup>|
|timebodyaccelerationjerkmeany | 16  |numeric|units: m/s<sup>3</sup>|
|timebodyaccelerationjerkmeanz | 17  |numeric|units: m/s<sup>3</sup>|
|timebodyaccelerationjerkstandarddeviationx | 18  |numeric|units: m/s<sup>3</sup>|
|timebodyaccelerationjerkstandarddeviationy | 19  |numeric|units: m/s<sup>3</sup>|
|timebodyaccelerationjerkstandarddeviationz | 20  |numeric|units: m/s<sup>3</sup>|
|timebodygyromeanx | 21  |numeric||
|timebodygyromeany | 22  |numeric||
|timebodygyromeanz | 23  |numeric||
|timebodygyrostandarddeviationx | 24  |numeric||
|timebodygyrostandarddeviationy | 25  |numeric||
|timebodygyrostandarddeviationz | 26  |numeric||
|timebodygyrojerkmeanx | 27  |numeric||
|timebodygyrojerkmeany | 28  |numeric||
|timebodygyrojerkmeanz | 29  |numeric||
|timebodygyrojerkstandarddeviationx | 30  |numeric||
|timebodygyrojerkstandarddeviationy | 31  |numeric||
|timebodygyrojerkstandarddeviationz | 32  |numeric||
|timebodyaccelerationmagnitudemean | 33  |numeric||
|timebodyaccelerationmagnitudestandarddeviation | 34  |numeric||
|timegravityaccelerationmagnitudemean | 35  |numeric||
|timegravityaccelerationmagnitudestandarddeviation | 36  |numeric||
|timebodyaccelerationjerkmagnitudemean | 37  |numeric||
|timebodyaccelerationjerkmagnitudestandarddeviation | 38  |numeric||
|timebodygyromagnitudemean | 39  |numeric||
|timebodygyromagnitudestandarddeviation | 40  |numeric||
|timebodygyrojerkmagnitudemean | 41  |numeric||
|timebodygyrojerkmagnitudestandarddeviation | 42  |numeric||
|frequencybodyaccelerationmeanx | 43  |numeric||
|frequencybodyaccelerationmeany | 44  |numeric||
|frequencybodyaccelerationmeanz | 45  |numeric||
|frequencybodyaccelerationstandarddeviationx | 46  |numeric||
|frequencybodyaccelerationstandarddeviationy | 47  |numeric||
|frequencybodyaccelerationstandarddeviationz | 48  |numeric||
|frequencybodyaccelerationjerkmeanx | 49  |numeric||
|frequencybodyaccelerationjerkmeany | 50  |numeric||
|frequencybodyaccelerationjerkmeanz | 51  |numeric||
|frequencybodyaccelerationjerkstandarddeviationx | 52  |numeric||
|frequencybodyaccelerationjerkstandarddeviationy | 53  |numeric||
|frequencybodyaccelerationjerkstandarddeviationz | 54  |numeric||
|frequencybodygyromeanx | 55  |numeric||
|frequencybodygyromeany | 56  |numeric||
|frequencybodygyromeanz | 57  |numeric||
|frequencybodygyrostandarddeviationx | 58  |numeric||
|frequencybodygyrostandarddeviationy | 59  |numeric||
|frequencybodygyrostandarddeviationz | 60  |numeric||
|frequencybodyaccelerationmagnitudemean | 61  |numeric||
|frequencybodyaccelerationmagnitudestandarddeviation | 62  |numeric||
|frequencybodybodyaccelerationjerkmagnitudemean | 63  |numeric||
|frequencybodybodyaccelerationjerkmagnitudestandarddeviation | 64  |numeric||
|frequencybodybodygyromagnitudemean | 65  |numeric||
|frequencybodybodygyromagnitudestandarddeviation | 66  |numeric||
|frequencybodybodygyrojerkmagnitudemean | 67  |numeric||
|frequencybodybodygyrojerkmagnitudestandarddeviation | 68  |numeric||

The raw data excerpt is quoted here for convenience.
> The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

>Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

>Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

>These signals were used to estimate variables of the feature vector for each pattern:  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

For aditional information see features_info.txt


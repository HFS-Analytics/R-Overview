In this lesson, we will discuss how to read data into your R session as a data frame and some basic cleaning and manipulation tasks (commonly known as "munging").

# Read Data

The easiest data file to work with is a comma-separated values (CSV) file. CSVs are really just text, but organized in a row-column format that most PCs can regognize and open with standard spreadsheet software (e.g., Excel). Other R packages such as `foreign`, `haven`, and `readxl` offer support for reading in other types of data files, such as files for SPSS, SAS, Stata, and Excel.

This repository has two sample data files representing the results of a baseline survey and history of reassessments conducted over time.

### Baseline Data 
```
people_id,gender,dob,assessment_date,referral_source,aces_score,psych_well_being,phys_well_being,social_well_being
1,Female,04/09/1955,06/02/2020,Self,3,5.610661369,5.021519171,5.44290545
2,Female,09/14/1993,11/03/2019,Self,7,5.238287589,5.509972079,5.827185969
3,Male,10/31/1999,04/17/2019,Police,5,5.37622648,5.661410867,5.213884869
4,Male,07/12/2003,11/21/2019,Hospital,6,5.928802686,4.546293504,4.93215248
5,Female,08/11/2000,08/21/2019,Self,7,3.896451849,4.237038441,3.681670597
6,Male,06/27/1968,05/14/2020,Self,1,6.933623004,5.433397385,5.771167985
```

### Reassessment Data
```
ID,assessment_date,domain,score
1,7/5/2020,Psychological Well-Being,3.290981079
1,7/5/2020,Psychological Well-Being,4.479059715
1,7/5/2020,Psychological Well-Being,5.372696839
2,1/16/2021,Psychological Well-Being,4.95704547
2,1/16/2021,Psychological Well-Being,5.306973742
2,1/16/2021,Psychological Well-Being,6.445742201
2,2/19/2021,Physical Wel-Being,7.146366568
2,2/19/2021,Physical Wel-Being,3.955101134
2,2/19/2021,Physical Wel-Being,6.363945271
2,4/6/2021,Social Well-Being,4.983148396
2,4/6/2021,Social Well-Being,5.601610588
2,4/6/2021,Social Well-Being,5.611968202
2,5/29/2021,Psychological Well-Being,5.07142245
2,5/29/2021,Psychological Well-Being,4.998119318
2,5/29/2021,Psychological Well-Being,4.990570174
3,6/29/2021,Physical Wel-Being,4.278290418
3,6/29/2021,Physical Wel-Being,3.901646118
3,6/29/2021,Physical Wel-Being,5.243152297
3,8/25/2021,Social Well-Being,4.579883264
3,8/25/2021,Social Well-Being,5.681931404
3,8/25/2021,Social Well-Being,5.026721126
3,8/30/2021,Psychological Well-Being,5.742101384
3,8/30/2021,Psychological Well-Being,4.744083197
3,8/30/2021,Psychological Well-Being,4.408579002
3,10/8/2021,Physical Wel-Being,5.105879039
3,10/8/2021,Physical Wel-Being,7.556197528
3,10/8/2021,Physical Wel-Being,4.917236289
3,11/30/2021,Social Well-Being,5.570600453
3,11/30/2021,Social Well-Being,6.227973087
3,11/30/2021,Social Well-Being,4.904987402
4,7/27/2020,Physical Wel-Being,4.992904019
4,7/27/2020,Physical Wel-Being,4.443517995
4,7/27/2020,Physical Wel-Being,5.870073072
4,8/24/2020,Social Well-Being,4.193653068
4,8/24/2020,Social Well-Being,5.295195987
4,8/24/2020,Social Well-Being,6.405222303
6,10/12/2020,Psychological Well-Being,5.180991487
6,10/12/2020,Psychological Well-Being,3.526554477
6,10/12/2020,Psychological Well-Being,5.349547333
6,11/2/2020,Physical Wel-Being,6.678588671
6,11/2/2020,Physical Wel-Being,4.100665671
6,11/2/2020,Physical Wel-Being,4.514397697
6,12/5/2020,Social Well-Being,4.78086556
6,12/5/2020,Social Well-Being,7.382569935
6,12/5/2020,Social Well-Being,5.902486624
```

## Accessing your file system from R

R can find and read files directly from your computer's file system using the directory path of each file. You can provide an absolute path to a file (e.g., providing a full address starting from C:\ through the folder structure to the file) or a relative path from the current working directory. Your relative file path is in reference to the current working directory for your session, which you can confirm using `getwd()`. If you specify a relative file path, R will start in the working directory and assume the address is a file or path linked from there.

For example, my current working directory is: `\\fs02\users\me\My Documents`. If I tell R to find a file at `sample data/sample data - baseline.csv`, it will start in My Documents (per the working directory), look for a folder called 'sample data', and look for the appropriate file in there.

You can reset the working directory using `setwd`, e.g.,

```r
setwd(r"(C:\Data Files\my project)")
```

## Reading a CSV file
Save the sample data files to your computer if you want for this next section.

Use `read.csv` to read in a data file and return a data frame. This function has fairly sensible defaults, so you usually will not need to specify more than the file path. However, if the data file does not include column names, you will want to include the argument `header = FALSE`. `read.csv` also accepts web addresses as a file path.

`read.csv` will only *read* the data file. To save the data for later use, assign the data to a variable name. 

```r
baseline <- read.csv("https://raw.githubusercontent.com/HFS-Analytics/R-Overview/main/sample%20data/sample%20data%20-%20baseline.csv")
reassessment <- read.csv("https://raw.githubusercontent.com/HFS-Analytics/R-Overview/main/sample%20data/sample%20data%20-%20reassessment.csv")
```

## Filtering a data frame

Use the `subset` function to filter a data frame down to a smaller set of rows and columns. The three most common arguments are: x (the data), subset (a filter condition applied to rows), and select (which columns to include.)

```r
subset(
  x = baseline, 
  subset = !is.na(people_id) & referral_source == 'Self', 
  select = c(people_id, dob, aces_score)
) 
```

The result:
```
  people_id        dob aces_score
1         1 04/09/1955          3
2         2 09/14/1993          7
5         5 08/11/2000          7
6         6 06/27/1968          1
```

Note that the subset and select arguments do not require quotation marks around the column names referenced. The function assumes these are columns in the dataset. The subset argument also should be a calculation that returns either TRUE or FALSE, values that are TRUE are kept in the result set.


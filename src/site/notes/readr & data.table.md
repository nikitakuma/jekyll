---
{"dg-publish":true,"permalink":"/readr-and-data-table/","noteIcon":""}
---

Course:: [[Resources/Classes/Lectures/Introduction to Importing Data in R\|Introduction to Importing Data in R]]
# readr & data.table
From the previous chapter, we know how to import flat files with the utils package loaded by default, but R has more options with specialized packages:[^1]
Some of these are faster than just utils
+ readr
+ data.table

read_csv(“yourfile.csv”)
read_tsv(“yourfile.txt”)
these are all wrappers around the main function read_delim()
![](https://i.imgur.com/Dkt885G.png)
## read_csv
CSV files can be imported. This function will assume the first row are column names[^2]
### Example:
```R
# Load the readr package
library(readr)

# Import potatoes.csv with read_csv(): potatoes
potatoes <- read_csv("potatoes.csv")
```
## read_tsv
You can also use `read_tsv()` to read TSV, or tab-separated values. You can also specify the column names manually[^3]
### Example:
```R
# readr is already loaded

# Column names
properties <- c("area", "temp", "size", "storage", "method", "texture", "flavor", "moistness")

# Import potatoes.txt: potatoes
potatoes <- read_tsv("potatoes.txt", col_names = properties)

# Call head() on potatoes
head(potatoes)
```
### Output:
```R
# A tibble: 6 × 8
   area  temp  size storage method texture flavor moistness
  <dbl> <dbl> <dbl>   <dbl>  <dbl>   <dbl>  <dbl>     <dbl>
1     1     1     1       1      1     2.9    3.2       3  
2     1     1     1       1      2     2.3    2.5       2.6
3     1     1     1       1      3     2.5    2.8       2.8
4     1     1     1       1      4     2.1    2.9       2.4
5     1     1     1       1      5     1.9    2.8       2.2
6     1     1     1       2      1     1.8    3         1.7
```
## read_delim
You can use `read_delim()` just like `read.table` but with less words. You still specify the symbol separating the data[^4]
![](https://i.imgur.com/RZ4iXoV.png)
By default, it assumes the first row are column names, but if they are not column names, you can specify `col_names = FALSE`
You can also specify column names with `col_names = c("example")`[^5]
![](https://i.imgur.com/FqjCcd3.png)
### Example:
```R
# readr is already loaded

# Column names
properties <- c("area", "temp", "size", "storage", "method", "texture", "flavor", "moistness")

# Import potatoes.txt using read_delim(): potatoes
potatoes <- read_delim("potatoes.txt", delim = "\t", col_names = properties)

# Print out potatoes
potatoes
```
### Output:
```R
# A tibble: 160 × 8
    area  temp  size storage method texture flavor moistness
   <dbl> <dbl> <dbl>   <dbl>  <dbl>   <dbl>  <dbl>     <dbl>
 1     1     1     1       1      1     2.9    3.2       3  
 2     1     1     1       1      2     2.3    2.5       2.6
 3     1     1     1       1      3     2.5    2.8       2.8
 4     1     1     1       1      4     2.1    2.9       2.4
 5     1     1     1       1      5     1.9    2.8       2.2
 6     1     1     1       2      1     1.8    3         1.7
 7     1     1     1       2      2     2.6    3.1       2.4
 8     1     1     1       2      3     3      3         2.9
 9     1     1     1       2      4     2.2    3.2       2.5
10     1     1     1       2      5     2      2.8       1.9
# ℹ 150 more rows
# ℹ Use `print(n = ...)` to see more rows
```
## skip and n_max
With `skip` and `max` you can specify which part of the flat file to import[^6]
+ `skip` = the lines you’re ignoring before starting the import
+ `n_max` = the number of lines actually importing
eg: in a CSV file of 20 lines, `skip = 2` and `n_max = 3` will only read lines 3, 4, 5
*Remember that this may skip the first line containing column names*
![](https://i.imgur.com/HxVodB2.png)
### Example:
```R
# readr is already loaded

# Column names
properties <- c("area", "temp", "size", "storage", "method","texture", "flavor", "moistness")

# Import 5 observations from potatoes.txt: potatoes_fragment
potatoes_fragment <- read_tsv("potatoes.txt", skip = 6, n_max = 5, col_names = properties)
```
## col_types
You can specify which types of data the columns should be:[^7]
+ `c`haracter
+ `d`ouble
+ `i`nteger
+ `l`ogical
+ `_` skips the column as a whole
![](https://i.imgur.com/pVtnHzy.png)
### Example:
```R
# readr is already loaded

# Column names
properties <- c("area", "temp", "size", "storage", "method", "texture", "flavor", "moistness")

# Import all data, but force all columns to be character: potatoes_char
potatoes_char <- read_tsv("potatoes.txt", col_types = "cccccccc", col_names = properties)

# Print out structure of potatoes_char
str(potatoes_char)
```
### Output:
```R
spc_tbl_ [160 × 8] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ area     : chr [1:160] "1" "1" "1" "1" ...
 $ temp     : chr [1:160] "1" "1" "1" "1" ...
 $ size     : chr [1:160] "1" "1" "1" "1" ...
 $ storage  : chr [1:160] "1" "1" "1" "1" ...
 $ method   : chr [1:160] "1" "2" "3" "4" ...
 $ texture  : chr [1:160] "2.9" "2.3" "2.5" "2.1" ...
 $ flavor   : chr [1:160] "3.2" "2.5" "2.8" "2.9" ...
 $ moistness: chr [1:160] "3.0" "2.6" "2.8" "2.4" ...
 - attr(*, "spec")=
  .. cols(
  ..   area = col_character(),
  ..   temp = col_character(),
  ..   size = col_character(),
  ..   storage = col_character(),
  ..   method = col_character(),
  ..   texture = col_character(),
  ..   flavor = col_character(),
  ..   moistness = col_character()
  .. )
 - attr(*, "problems")=<externalptr> 
```
## col_types with collectors
You can use collector functions to set types of imported columns[^8]

# References

[^1]: [readr: read\_csv & read\_tsv | R](https://campus.datacamp.com/courses/introduction-to-importing-data-in-r/readr-datatable?ex=1)
[^2]: [read\_csv | R](https://campus.datacamp.com/courses/introduction-to-importing-data-in-r/readr-datatable?ex=2)
[^3]: [read\_tsv | R](https://campus.datacamp.com/courses/introduction-to-importing-data-in-r/readr-datatable?ex=3)
[^4]: [readr: read\_delim | R](https://campus.datacamp.com/courses/introduction-to-importing-data-in-r/readr-datatable?ex=4)
[^5]: [read\_delim | R](https://campus.datacamp.com/courses/introduction-to-importing-data-in-r/readr-datatable?ex=5)
[^6]: [skip and n\_max | R](https://campus.datacamp.com/courses/introduction-to-importing-data-in-r/readr-datatable?ex=6)
[^7]: [col\_types | R](https://campus.datacamp.com/courses/introduction-to-importing-data-in-r/readr-datatable?ex=7)
[^8]: [col\_types with collectors | R](https://campus.datacamp.com/courses/introduction-to-importing-data-in-r/readr-datatable?ex=8)
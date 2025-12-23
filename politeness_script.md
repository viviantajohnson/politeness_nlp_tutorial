# Use ```politeness``` package to extract politeness-related linguistic features in open-ended text

## Install packages
```
install.packages(c("politeness", "tidyverse", "readxl", "openxlsx", "janitor"))
```
You only have to install these once.

## Load packages
```
library(politeness)
library(tidyverse)
library(readxl)
library(openxlsx)
library(janitor)
```

## Many ```politeness``` features depend on dependency parsing (grammar parsing), meaning you need to run **SpaCy** through ```spacyr```.
Run the following in R:
```
install.packages("spacyr")
spacyr::spacy_install()          # installs spaCy + a default English model (in most setups)
spacyr::spacy_initialize()       # start spaCy for this R session
```
If you have issues installing SpaCy on your computer, you can still run an incomplete but usable version by setting ```parser = "none"``` in ```politeness()``` (see below). Let me know and I can help you troubleshoot it. 


## Load data
Run the following in R to upload the _cmv_sample_data.xlsx_ dataset and name it _df_.  Replace the pathway of the file to the pathway on your computer.
```
df <- as.data.frame(read_excel("path/to/the/dataset/data/cmv_sample_data.xlsx"))
```
The dataframe contains N = 3668 and 6 variables:
* **challenger_author**: The user challening the OP (original poster). Also the author of a 'response'.
* **comment_id**: The unique ID assigned to every comment. Each comment belongs to a parent post (see 'post_parent_id' below).
* **post_parent_id**: The unique ID of the parent post that each comment directly replies to. Parent posts contain comments. 
* **response**: The content of each comment. 
* **delta_awarded**: 0 = the OP did not award a delta to the challenger_author. 1 = the OP did award a delta to the challenger_author.
* **op_author**: The user who authored a parent post. Aka the OP.

## Ensure variables are in the correct class
```
df$challenger_author <- as.factor(df$challenger_author) # should be a factor (i.e., categorical variable)  
df$comment_id <- as.factor(df$comment_id) # should be a factor (i.e., categorical variable)
df$post_parent_id <- as.factor(df$post_parent_id) # should be a factor (i.e., categorical variable)
df$response <- as.character(df$response) # should be character  
df$delta_awarded <- as.integer(df$delta_awarded) # should be integer (or factor depending on what kind of analysis you're doing, but for now, let's go with integer)    
df$op_author <- as.factor(df$op_author) # should be a factor (i.e., categorical variable)  
```
You can check if those variables are in the correct class by running the following in R:
```
str(df)
```
## Extract politeness measures from df$response
Run the following in R:
```
polite_features <- politeness(
text = df$response_clean, # Indicate the column to analyze
parser = "spacy", # If you successfully installed ```spacy```, enter ```spacy```. If not, enter ```none```
metric = "average", # Choose between ```count``` (raw frequency), ```binary``` (yes, no), or ```average``` (proportion; most often used)  
drop_blank = TRUE, # Should features that weren't found in any text be removed from the dataframe? (TRUE, FALSE)  
uk_english = FALSE, # Does the text contain any British English spelling?
num_mc_cores = 1 # Number of cores for parallelization. Default is 1
)
```
This will create a dataframe named ```polite_features``` with averages of lingusitic features in each response.

## Merge ```polite_features``` with your original dataframe (```df```) and name this dataframe ```new_df```
Run the following:
```
new_df <- cbind(df, polite_features)
```
Now you should have a dataframe named ```new_df``` in your environment.

## Optional: Save ```new_df``` as excel file on your computer
```
write.xlsx(new_df, file = "pathname/where/you/want/to/save/file/new_df.xlsx", rowNames = FALSE)
```




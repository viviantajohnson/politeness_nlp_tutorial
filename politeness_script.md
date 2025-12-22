# Use ```politeness``` package to examine open-ended text

## Install packages
```
install.packages(c("politeness", "tidyverse", "readxl", "janitor"))
```
You only have to install these once.

## Load packages
```
library(politeness)
library(tidyverse)
library(readxl)
library(janitor)
```

## Many ```politeness``` features depend on dependency parsing (grammar parsing), meaning you need to run **SpaCy** through ```spacyr```.
Run the following:
```
install.packages("spacyr")
spacyr::spacy_install()          # installs spaCy + a default English model (in most setups)
spacyr::spacy_initialize()       # start spaCy for this R session
```
If SpaCy is painful on your machine, you can still run an incomplete but usable version by setting ```parser = "none"``` in ```politeness()```.
You’ll lose/approximate some features.


## Load data
Run this script to upload the _cmv_sample_data.xlsx_ dataset and name it _df_.  Replace the pathname of the file to the pathname on your computer.
```
df <- as.data.frame("path/to/the/dataset/data/cmv_sample_data.xlsx")
```
The dataframe contains 6 variables:
* challenger_author: The user challening the OP (original poster). Also the author of a 'response'.
* comment_id: The unique ID assigned to every comment. Each comment belongs to a parent post (see 'post_parent_id' below).
* post_parent_id: The unique ID of the parent post that each comment directly replies to. Parent posts contain comments. 
* response: The content of each comment. 
* delta_awarded: 0 = the OP did not award a delta to the challenger_author. 1 = the OP did award a delta to the challenger_author.
* op_author: The user who authored a parent post. Aka the OP.

## Make sure variables are in the correct class
```
df$challenger_author <- as.factor(df$challenger_author)
df$comment_id <- as.factor(df$comment_id)

polite_features <- politeness(
  text       = df$response_clean,
  parser     = parser_choice,
  metric     = "count",
  drop_blank = TRUE,
  num_mc_cores = 1
)



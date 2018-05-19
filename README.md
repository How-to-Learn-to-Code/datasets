# Example Datasets

A collection of example datasets for teaching purposes. 

# Using this resource:
Add tags to dataset for easy indexing later. Include links to data &
descriptions of data if available. 

**optional:** include code chunk for cleaning messy data

## Tags:
* biological - biological examples
* non-biological 
* messy - data that requires cleaning
* clean - data ready to use


## FBI College Crime Statistics (2015)
### Tags: non-biological, messy
Large excel table with messy formatting--could be good for tidying data examples.

* [description](https://ucr.fbi.gov/crime-in-the-u.s/2015/crime-in-the-u.s.-2015/tables/table-9/table_9_offenses_known_to_law_enforcement_by_state_by_university_and_college_2015.xls/vie://ucr.fbi.gov/crime-in-the-u.s/2015/crime-in-the-u.s.-2015/tables/table-9/table-9-data-declaration_final)

* [excel data](https://ucr.fbi.gov/crime-in-the-u.s/2015/crime-in-the-u.s.-2015/tables/table-9/table_9_offenses_known_to_law_enforcement_by_state_by_university_and_college_2015.xls)

```{r}
library(readxl)
library(magrittr)

link <- "https://ucr.fbi.gov/crime-in-the-u.s/2015/crime-in-the-u.s.-2015/tables/table-9/table_9_offenses_known_to_law_enforcement_by_state_by_university_and_college_2015.xls"
file <- "crime.xls"
download.file(link, file)
df <- read_xls(file, skip = 3)  # skip header

# drop annotations at bottom of data table
drop <- nrow(df) - 8 
df <- df[1:drop,]

# drop extra columns read in because of annotations at bottom
df %<>% 
  dplyr::select(-grep("X_", names(.)))

# clean up col names
names(df) %<>% gsub("\n", "_", .) %>% 
  gsub(" ", "_", .) %>% 
  gsub("-", "", .) %>% 
  gsub("/", ".", .) %>% 
  gsub("\\d", "", .) %>% 
  gsub("[()]", "", .) %>% 
  tolower()

# fill in missing values caused by using merged cells in excel
df %<>% 
  tidyr::fill(state) %>% 
  tidyr::fill(university.college)
```

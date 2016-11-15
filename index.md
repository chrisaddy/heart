    library(data.table)

    #heart <- fread("curl ftp://ftp.cdc.gov/pub/Health_Statistics/NCHS/Datasets/NHIS/2015/personsx.zip | funzip")
    dim(heart)

    ## [1] 103789    606
---
title: "Heart Data"
author: "Chris Addy"
output: md_document
---
    
    ### 606 variables, drop many

    heart_cleaned <- as_data_frame(heart) %>%
        select(SEX, AGE_P, R_MARITL, MRACRPI2, EDUC1, DOINGLWP)

    ### create tables for codes to join

    sex_table <- data_frame(SEX = c(1, 2), sex = c("male", "female"))

    marital_table <- data_frame(R_MARITL = c(0, 1, 2, 3, 4, 5, 6, 7, 8, 9), marital = c("Under 14 years", "Married - spouse in household", "Married - spouse not in household", "Married - spouse in household unknown", "Widowed", "Divorced", "Separated", "Never married", "Living with partner", "Unknown marital status"))

    race_table <- data_frame(MRACRPI2 = c(1, 2, 3, 6, 7, 12, 16, 17), race = c("White", "Black/African American", "Indian (American) (includes Eskimo, Aleut)", "Chinese", "Filipino", "Asian Indian", "Other race*", "Multiple race, no primary race selected"))

    education_table <- data_frame(c(0:21, 96:99), c("Never attended/kindergarten only", "1st grade", "2nd grade", "3rd grade", "4th grade", "5th grade", "6th grade", "7th grade", "8th grade", "9th grade", "10th grade", "11th grade", "12th grade, no diploma", "GED or equivalent", "High School Graduate", "Some college, no degree", "Associate degree: occupational, technical, or vocational program", "Associate degree: academic program", "Bachelor's degree (Example: BA, AB, BS, BBA)", "Master's degree (Example: MA, MS, MEng, MEd, MBA)", "Professional School degree (Example: MD, DDS, DVM, JD)", "Doctoral degree (Example: PhD, EdD)", "Child under 5 years old", "Refused", "Not ascertained", "Don't know"))

    heart_cleaned <- heart_cleaned %>%
        inner_join(sex_table, by = "SEX") %>%
        inner_join(marital_table, by = "R_MARITL") %>%
        inner_join(race_table, by = "MRACRPI2")

    heart_cleaned

    ## # A tibble: 96,580 × 9
    ##      SEX AGE_P R_MARITL MRACRPI2 EDUC1 DOINGLWP    sex
    ##    <dbl> <int>    <dbl>    <dbl> <int>    <int>  <chr>
    ## 1      1    25        7        1    16        1   male
    ## 2      2    31        1        1    14        1 female
    ## 3      1    32        1        1    18        1   male
    ## 4      1     8        0        1     1       NA   male
    ## 5      2     2        0        1    96       NA female
    ## 6      2    49        7        1    19        1 female
    ## 7      2    48        1        1    14        1 female
    ## 8      1    49        1        1    14        1   male
    ## 9      1    53        5        2    17        1   male
    ## 10     2    66        4        2    12        1 female
    ## # ... with 96,570 more rows, and 2 more variables: marital <chr>,
    ## #   race <chr>

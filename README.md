
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ forcats   1.0.0     ✔ readr     2.1.5
    ## ✔ ggplot2   3.4.4     ✔ stringr   1.5.1
    ## ✔ lubridate 1.9.3     ✔ tibble    3.2.1
    ## ✔ purrr     1.0.2     ✔ tidyr     1.3.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
deaths <- av %>% select(
  URL, 
  Name.Alias, 
  Appearances,
  Current.,
  Gender,
  Probationary.Introl,
  Full.Reserve.Avengers.Intro,
  Year,
  Years.since.joining,
  Honorary,
  starts_with("Death"),
  Notes
) %>% pivot_longer(
  cols = Death1:Death5,
  names_to = "Time",
  values_to = "Died"
)

head(deaths)
```

    ## # A tibble: 6 × 13
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 3 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 7 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Notes <chr>, Time <chr>,
    ## #   Died <chr>

Similarly, deal with the returns of characters.

``` r
returns <- av %>% select(
  URL, 
  Name.Alias, 
  Appearances,
  Current.,
  Gender,
  Probationary.Introl,
  Full.Reserve.Avengers.Intro,
  Year,
  Years.since.joining,
  Honorary,
  starts_with("Return"),
  Notes
) %>% pivot_longer(
  cols = Return1:Return5,
  names_to = "Time",
  values_to = "Returned"
)

head(returns)
```

    ## # A tibble: 6 × 13
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 3 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 7 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Notes <chr>, Time <chr>,
    ## #   Returned <chr>

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
freq <- deaths %>% count(Died=="YES")

freq
```

    ## # A tibble: 2 × 2
    ##   `Died == "YES"`     n
    ##   <lgl>           <int>
    ## 1 FALSE             776
    ## 2 TRUE               89

``` r
#As seen here, there have been 89 deaths

avg <- 89/length(unique(deaths$Name.Alias))

avg
```

    ## [1] 0.5460123

``` r
#So we can see that each Avenger dies an average of .546 times
```

## Zane Eason

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

## Michaela Beck

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> But you can only tempt death so many times. There’s a 2-in-3 chance
> that a member of the Avengers returned from their first stint in the
> afterlife.

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

``` r
#count the number of return 1
# Count the number of "yes" values in Return1 column
count_yes <- returns %>%
  filter(Time == "Return1", Returned == "YES") %>%
  count()

count_death1 <- deaths %>%
  filter(Time == "Death1", Died == "YES") %>%
  count()

count_yes
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1    46

``` r
count_death1
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1    69

``` r
count_yes/count_death1
```

    ##           n
    ## 1 0.6666667

In the returns table I counted the number of times “YES” was in the
Deaths1 columns and also counted the number of times Return1 appeared
after Death1. After reviewing the data set I found 69 first deaths and
46 returns from the first deaths resulting in an average of .667. This
confirms there’s a 2-in-3 chance that a member of the Avengers returned
from their first stint in the afterlife.

**<span style="font-size: 200%; font-weight: bold;">\#Varun</span>**

``` r
library(dplyr)
library(tidyr)
```

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

``` r
class(df)
```

    ## [1] "function"

``` r
df <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv")  # Adjust this line as per your actual code
class(df)  
```

    ## [1] "data.frame"

``` r
deaths <- av %>%
  pivot_longer(
    cols = starts_with("Death"), 
    names_to = "Time", 
    values_to = "Death",
    names_prefix = "Death"
  ) %>%
  mutate(
    Time = as.integer(parse_number(Time)),
    Death = case_when(
      Death == "YES" ~ "yes",
      Death == "NO" ~ "no",
      TRUE ~ ""
    )
  )
head(deaths)
```

    ## # A tibble: 6 × 18
    ##   URL                 Name.Alias Appearances Current. Gender Probationary.Introl
    ##   <chr>               <chr>            <int> <chr>    <chr>  <chr>              
    ## 1 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 2 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 3 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 4 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 5 http://marvel.wiki… "Henry Jo…        1269 YES      MALE   ""                 
    ## 6 http://marvel.wiki… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Return1 <chr>, Return2 <chr>,
    ## #   Return3 <chr>, Return4 <chr>, Return5 <chr>, Notes <chr>, Time <int>,
    ## #   Death <chr>

``` r
library(dplyr)
library(tidyr)

returns <- av %>%
  pivot_longer(
    cols = starts_with("Return"), 
    names_to = "Time", 
    values_to = "Return",
    names_prefix = "Return"
  ) %>%
  mutate(
    Time = as.integer(parse_number(Time)),
    Return = case_when(
      Return == "YES" ~ "yes",
      Return == "NO" ~ "no",
      TRUE ~ ""
    )
  ) %>%
  head(n = 10)  # This line was missing the pipe operator (%>%)

returns  # Print or further process the result
```

    ## # A tibble: 10 × 18
    ##    URL                Name.Alias Appearances Current. Gender Probationary.Introl
    ##    <chr>              <chr>            <int> <chr>    <chr>  <chr>              
    ##  1 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  2 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  3 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  4 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  5 http://marvel.wik… "Henry Jo…        1269 YES      MALE   ""                 
    ##  6 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  7 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  8 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ##  9 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## 10 http://marvel.wik… "Janet va…        1165 YES      FEMALE ""                 
    ## # ℹ 12 more variables: Full.Reserve.Avengers.Intro <chr>, Year <int>,
    ## #   Years.since.joining <int>, Honorary <chr>, Death1 <chr>, Death2 <chr>,
    ## #   Death3 <chr>, Death4 <chr>, Death5 <chr>, Notes <chr>, Time <int>,
    ## #   Return <chr>

how many deaths on average, does an Avenger suffer?

``` r
average_deaths_per_avenger <- deaths %>%
  filter(Death == "yes") %>% # Filter for rows where Death is "yes"
  group_by(Name.Alias) %>% # Group by Avenger's name or alias
  summarise(Deaths = n()) %>% # Count the number of deaths for each Avenger
  summarise(Average_Deaths = mean(Deaths)) # Calculate the average number of deaths

print(average_deaths_per_avenger)
```

    ## # A tibble: 1 × 1
    ##   Average_Deaths
    ##            <dbl>
    ## 1           1.39

On average, an Avenger suffers approximately 1.39 deaths.

Each team member picks one of the statements in the FiveThirtyEight
analysis and fact checks it based on the data. Use dplyr functionality
whenever possible.

**Most Avengers were introduced before the year 2000**

``` r
avengers_pre_2000 <- df %>%
  summarise(Total = n(),
            Pre_2000 = sum(Year < 2000),
            Percent_Pre_2000 = Pre_2000 / Total * 100)

print(avengers_pre_2000)
```

    ##   Total Pre_2000 Percent_Pre_2000
    ## 1   173       90         52.02312

Out of 173 Avengers in the dataset, 90 were introduced before the year
2000. This means that approximately 52.02% of the Avengers made their
first appearance before the year 2000.

**There are Avengers who have died more than once**

``` r
died_more_than_once <- deaths %>%
  filter(Death == "yes") %>%
  group_by(Name.Alias) %>%
  summarise(Times_Died = n()) %>%
  filter(Times_Died > 1)

print(died_more_than_once)
```

    ## # A tibble: 16 × 2
    ##    Name.Alias                       Times_Died
    ##    <chr>                                 <int>
    ##  1 ""                                        7
    ##  2 "Anthony Ludgate Druid"                   2
    ##  3 "Ares"                                    2
    ##  4 "Clinton Francis Barton"                  2
    ##  5 "Dennis Dunphy"                           2
    ##  6 "Eric Kevin Masterson"                    2
    ##  7 "Heather Douglas"                         2
    ##  8 "Jocasta"                                 5
    ##  9 "Jonathan Hart"                           2
    ## 10 "Mar-Vell"                                3
    ## 11 "Marrina Smallwood"                       2
    ## 12 "Matthew Liebowitz (birth name)"          2
    ## 13 "Maya Lopez"                              2
    ## 14 "Peter Benjamin Parker"                   2
    ## 15 "Ravonna Lexus Renslayer"                 2
    ## 16 "Thor Odinson"                            2

The output lists Avengers who have died and returned more than once.
Notably, Jocasta has died 5 times, Mar-Vell 3 times, and several others
like Anthony Ludgate Druid, Ares, Clinton Francis Barton (Hawkeye),
Dennis Dunphy, Eric Kevin Masterson, Heather Douglas, and Jonathan Hart
have died 2 times each.

**The majority of Avengers are male**

``` r
gender_distribution <- df %>%
  group_by(Gender) %>%
  summarise(Count = n()) %>%
  mutate(Percent = Count / sum(Count) * 100)
print(gender_distribution)
```

    ## # A tibble: 2 × 3
    ##   Gender Count Percent
    ##   <chr>  <int>   <dbl>
    ## 1 FEMALE    58    33.5
    ## 2 MALE     115    66.5

Among the Avengers, there are 58 females and 115 males. This indicates
that 33.53% of the Avengers are female, while 66.47% are male.

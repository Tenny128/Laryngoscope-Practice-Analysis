Laryngoscope: Practice analysis
================
Akpan Taiwo
2025-09-24

# Laryngoscope

A laryngoscope is a medical instrument which is used to examine the
larynx(voice box) during a laryngoscopy. It has lights and a lens, and
can be used during intubation to insert a breathing tube in a patient
who is having difficulty breathing.

The Laryngoscope dataset in R is part of the medical data package which
comes from a study by Abdullah et al., published in Anasthesia Analgesia
in 2011. It compares the Pentax AWS Video Laryngoscope and the Macintosh
Laryngoscope.
<https://search.r-project.org/CRAN/refmans/medicaldata/html/laryngoscope.html>.

## Setting up my environment

Note: To set up the environment by installing and loading tidyverse, and
the medicaldata in R datasets

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.2
    ## ✔ ggplot2   4.0.0     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
install.packages("medicaldata")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.5'
    ## (as 'lib' is unspecified)

``` r
library(medicaldata)
```

Selecting the laryngoscope data and saving it as a tibble called
‘t_laryngoscope’

``` r
data("laryngoscope")
t_laryngoscope <- as_tibble(laryngoscope)
head(t_laryngoscope)
```

    ## # A tibble: 6 × 22
    ##     age gender   asa   BMI Mallampati Randomization attempt1_time attempt1_S_F
    ##   <dbl>  <dbl> <dbl> <dbl>      <dbl>         <dbl>         <dbl>        <dbl>
    ## 1    51      0     3  56.2          1             0            29            1
    ## 2    52      0     3  44.6          2             0            29            1
    ## 3    37      0     3  41.6          1             0            31            0
    ## 4    20      0     3  46.3          2             0            31            0
    ## 5    35      0     3  61            2             0            21            1
    ## 6    39      0     3  44            2             0            10            1
    ## # ℹ 14 more variables: attempt2_time <dbl>, attempt2_assigned_method <dbl>,
    ## #   attempt2_S_F <dbl>, attempt3_time <dbl>, attempt3_assigned_method <dbl>,
    ## #   attempt3_S_F <dbl>, attempts <dbl>, failures <dbl>,
    ## #   total_intubation_time <dbl>, intubation_overall_S_F <dbl>, bleeding <dbl>,
    ## #   ease <dbl>, sore_throat <dbl>, view <dbl>

## Data Cleaning

The data columns are all presented as numerical data. In order to be
able to explore the data using charts, I would need to convert some of
the data to categorical data.

``` r
t_laryngoscope$gender <- factor(t_laryngoscope$gender, levels = c(1, 0), labels = c("Male", "Female"))
t_laryngoscope$asa <- as.factor(t_laryngoscope$asa)
t_laryngoscope$Mallampati <- as.factor(t_laryngoscope$Mallampati)
t_laryngoscope$Randomization <- factor(t_laryngoscope$Randomization, levels = c(1, 0), labels = c("Video", "Standard"))
t_laryngoscope$attempts <- as.factor(t_laryngoscope$attempts)
t_laryngoscope$failures <- as.factor(t_laryngoscope$failures)
t_laryngoscope$intubation_overall_S_F <- factor(t_laryngoscope$intubation_overall_S_F, levels = c(1, 0), labels = c("yes", "no"))
t_laryngoscope$bleeding <- factor(t_laryngoscope$bleeding, levels = c(1, 0), labels = c("yes", "no"))
t_laryngoscope$sore_throat <- factor(t_laryngoscope$sore_throat, levels = c(3, 2, 1, 0), labels = c("severe", "moderate", "mild", "none"))

head(t_laryngoscope)
```

    ## # A tibble: 6 × 22
    ##     age gender asa     BMI Mallampati Randomization attempt1_time attempt1_S_F
    ##   <dbl> <fct>  <fct> <dbl> <fct>      <fct>                 <dbl>        <dbl>
    ## 1    51 Female 3      56.2 1          Standard                 29            1
    ## 2    52 Female 3      44.6 2          Standard                 29            1
    ## 3    37 Female 3      41.6 1          Standard                 31            0
    ## 4    20 Female 3      46.3 2          Standard                 31            0
    ## 5    35 Female 3      61   2          Standard                 21            1
    ## 6    39 Female 3      44   2          Standard                 10            1
    ## # ℹ 14 more variables: attempt2_time <dbl>, attempt2_assigned_method <dbl>,
    ## #   attempt2_S_F <dbl>, attempt3_time <dbl>, attempt3_assigned_method <dbl>,
    ## #   attempt3_S_F <dbl>, attempts <fct>, failures <fct>,
    ## #   total_intubation_time <dbl>, intubation_overall_S_F <fct>, bleeding <fct>,
    ## #   ease <dbl>, sore_throat <fct>, view <dbl>

Outlining the column names to make them easily accessible.

``` r
colnames(t_laryngoscope)
```

    ##  [1] "age"                      "gender"                  
    ##  [3] "asa"                      "BMI"                     
    ##  [5] "Mallampati"               "Randomization"           
    ##  [7] "attempt1_time"            "attempt1_S_F"            
    ##  [9] "attempt2_time"            "attempt2_assigned_method"
    ## [11] "attempt2_S_F"             "attempt3_time"           
    ## [13] "attempt3_assigned_method" "attempt3_S_F"            
    ## [15] "attempts"                 "failures"                
    ## [17] "total_intubation_time"    "intubation_overall_S_F"  
    ## [19] "bleeding"                 "ease"                    
    ## [21] "sore_throat"              "view"

## Data Exploration Using Plots

Using scatter plots and bar charts to explore the data for significant
insights.

``` r
ggplot(data = t_laryngoscope) +
  geom_bar(mapping = aes(x = Randomization, fill = intubation_overall_S_F)) +
  facet_wrap(~ attempts) +
  theme(axis.text.x = element_text(angle = 45)) +
  labs(title="Successful Itubation, Method and Attempts")
```

![](Laryngoscope--Practice-analysis_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

This plot indicates that both methods are quite effective. However, In a
few cases, There is need for a secod or even a third attempt at
intubation. Is this in anyway related to the level of obesity as
measured by the BMI?

``` r
ggplot(data = t_laryngoscope) +
  geom_point(mapping = aes(x = Randomization, y = BMI, color =intubation_overall_S_F)) +
  facet_wrap(~ attempts) +
  theme(axis.text.x = element_text(angle = 45)) +
  labs(title="Successful Itubation, Method and BMI")
```

    ## Warning: Removed 2 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](Laryngoscope--Practice-analysis_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

This doesnt indicate that at all. Infact, majority of the patients have
BMI between 30 and 50. Those with higher, had a successful intubation on
the first attempt, and only a few who are slightly abve 40 needed a
third attempt.

``` r
ggplot(data = t_laryngoscope) +
  geom_point(mapping = aes(x = ease, y = total_intubation_time, colour = attempts)) +
  labs(title="Ease, Time of Intubation")
```

![](Laryngoscope--Practice-analysis_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

This plot indicates that easier intubations are likely to require only
one attempt and take less time.

``` r
ggplot(data = t_laryngoscope) +
  geom_bar(mapping = aes(x = attempts, fill = bleeding )) +
  facet_wrap(~ view) +
  labs(title="Occurence of Bleeding")
```

![](Laryngoscope--Practice-analysis_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

Finally, this plot indicates that bleeding can occur even when the
glottic view is good. However occurence of bleeding seems to be very
few.

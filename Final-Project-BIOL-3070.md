Final Project BIOL 3070_Crook
================
Whitney Crook
2025-10-30

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
- [STUDY QUESTION and HYPOTHESIS](#study-question-and-hypothesis)
  - [Question](#question)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [METHODS](#methods)
  - [Procedure](#procedure)
- [DISCUSSION](#discussion)
  - [Interpretation 1](#interpretation-1)
  - [Interpretation 2](#interpretation-2)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

# BACKGROUND

``` r
# Manually transcribe duration (mean, lo, hi) from the last table column
duration <- data.frame(
  Bird = c("Canada Goose","Mallard", 
           "American Kestrel","Northern Bobwhite",
           "Japanese Quail","Ring-necked Pheasant",
           "American Coot","Killdeer",
           "Ring-billed Gull","Mourning Dove",
           "Rock Dove","Monk Parakeet",
           "Budgerigar","Great Horned Owl",
           "Northern Flicker","Blue Jay",
           "Black-billed Magpie","American Crow",
           "Fish Crow","American Robin",
           "European Starling","Red-winged Blackbird",
           "Common Grackle","House Finch","House Sparrow"),
  mean = c(4.0,4.0,4.5,4.0,1.3,3.7,4.0,4.5,5.5,3.7,3.2,2.7,1.7,6.0,4.0,
           4.0,5.0,3.8,5.0,4.5,3.2,3.0,3.3,6.0,4.5),
  lo   = c(3,4,4,3,0,3,4,4,4,3,3,1,0,6,3,
           3,5,3,4,4,3,3,3,5,2),
  hi   = c(5,4,5,5,4,4,4,5,7,4,4,4,4,6,5,
           5,5,5,7,5,4,3,4,7,6)
)

# Choose some colors
cols <- c(rainbow(30)[c(10:29,1:5)])  # rainbow colors

# horizontal barplot
par(mar=c(5,12,2,2))  # wider left margin for names
bp <- barplot(duration$mean, horiz=TRUE, names.arg=duration$Bird,
              las=1, col=cols, xlab="Days of detectable viremia", xlim=c(0,7))

# add error bars
arrows(duration$lo, bp, duration$hi, bp,
       angle=90, code=3, length=0.05, col="black", xpd=TRUE)
```

<img src="Final-Project-BIOL-3070_files/figure-gfm/viremia-1.png" style="display: block; margin: auto auto auto 0;" />

# STUDY QUESTION and HYPOTHESIS

## Question

Do different regions of the U. S vary in asthma prevalence and mortality
rates?

## Hypothesis

We predict that regions that are more densely populated will experience
higher rates of asthma due to pollution from large cities.

## Prediction

# METHODS

## Procedure

``` r
library(ggplot2)

age <- data.frame(category = c(" 0-4", " 5-11", "12-17", "18-24", "25-34", "35-64", "65+"), value = c(1.4, 2.4, 2.0, 3.8, 6.4, 11.5, 27.1))

ggplot(age, aes(x = category, y = value)) +

  geom_bar(stat = "identity", fill = "darkred") + # stat="identity" uses y-values directly

  labs(title = "Asthma Deaths By Age",x = "Age (Years)",y = "Asthma-Related Deaths Per Million")
```

![](Final-Project-BIOL-3070_files/figure-gfm/age%20and%20deaths-1.png)<!-- -->

# DISCUSSION

## Interpretation 1

## Interpretation 2

# CONCLUSION

# REFERENCES

1.  ChatGPT. OpenAI, version Jan 2025. Used as a reference for functions
    such as ggplot() and troubleshooting error messages. Accessed
    2025-10-30.

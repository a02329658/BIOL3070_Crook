Final Project Asthma
================
Whitney Crook
2025-12-06

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
- [STUDY QUESTION and HYPOTHESIS](#study-question-and-hypothesis)
  - [Question](#question)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [METHODS](#methods)
  - [Procedure](#procedure)
  - [Boxplots by region showing population density and asthma (figures 1
    and
    2)](#boxplots-by-region-showing-population-density-and-asthma-figures-1-and-2)
  - [ANOVA Test](#anova-test)
- [DISCUSSION](#discussion)
  - [Interpretation - Box plots (figures 1 and
    2)](#interpretation---box-plots-figures-1-and-2)
  - [Interpretation - ANOVA results](#interpretation---anova-results)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

Write at the end. Summarize the study.

# BACKGROUND

Asthma is a chronic lung condition that can cause the airways to become
inflamed at times making it harder to breathe. It is a very common
condition in the United States. According to the CDC, about 1 in 13
people have asthma in the United States (CDC, 2025). Asthma symptoms can
be triggered by many things, including pollen, exercise, cold air, or
air pollution.

Air pollution can increase the risk of developing asthma and as well as
making symptoms worse if a person already has asthma. The airborne
particles that causes air pollution can irritate airways which triggers
asthma symptoms (Asthma and Allergy Foundation of America).

There is typically more air pollution in more highly populated regions.
For this reason, we hypothesize that more urbanized regions with higher
population density will be associated with higher asthma prevalence. The
Northeastern United States are more highly populated than the West, so
we predict that the Northeast will have a higher asthma prevalence.

``` r
# Map data
states_map <- map_data("state")

# Merge map data with asthma data
map_data <- states_map %>%
  left_join(asthma_data, by = c("region" = "state_lower"))

ggplot(map_data, aes(long, lat, group = group, fill = prevalence)) +
  geom_polygon(color = "gray90", size = 0.3) +  # state borders
  geom_polygon(aes(fill = prevalence), color = "white") +
  coord_fixed(1.3) +
 scale_fill_gradientn(
  name = "Asthma Prevalence (%)",
  colors = c("yellow2", "orange", "red", "red4")
) +
  labs(
    title = "Asthma Prevalence by U.S. State"
  ) +
  theme_void() +
  theme(
    legend.position = "right",
    plot.title = element_text(size = 16, face = "bold"),
    plot.subtitle = element_text(size = 12)
  )
```

<img src="FinalProjectAsthmaCrook_files/figure-gfm/prevalence by state-1.png" style="display: block; margin: auto;" />

``` r
# U.S. states map data
states_map <- map_data("state")

# Put the states into regions in a dataframe
state_regions <- data.frame(
  state = tolower(state.name),
  region = state.region
)

# Merge map data with region info
map_data <- states_map %>%
  left_join(state_regions, by = c("region" = "state"))

# Plot
ggplot(map_data, aes(x = long, y = lat, group = group, fill = region.y)) +
  geom_polygon(color = "white") +
  coord_fixed(1.3) +
  labs(title = "U.S. Regions",
       fill = "Region") +
  theme_void()
```

<img src="FinalProjectAsthmaCrook_files/figure-gfm/region map-1.png" style="display: block; margin: auto;" />
These are the regional divisions used for this study.

# STUDY QUESTION and HYPOTHESIS

## Question

Does asthma prevalence differ across U.S. regions?

## Hypothesis

Regions with higher population density and greater urbanization will be
associated with higher asthma prevalence and mortality rates.

## Prediction

We predict that the Northeastern United States will have a higher
prevalence of asthma and asthma-related deaths because of the higher
population density.

# METHODS

### Procedure

## Boxplots by region showing population density and asthma (figures 1 and 2)

To find the difference in asthma prevalence across regions of the United
States, we will create two box plots comparing the population density
per region and the asthma prevalence per region.

``` r
library(ggplot2)

#Asthma prevalence v region
ggplot(asthma_data, aes(x = region, y = prevalence, fill= region)) +
  geom_boxplot() +
  labs(title = "Asthma Prevalence by U.S. Region",
       x = "Region",
       y = "Asthma Prevalence (%)",
       fill = "Region" ) +
  theme_minimal()
```

<img src="FinalProjectAsthmaCrook_files/figure-gfm/box plot-1.png" style="display: block; margin: auto;" />

``` r
#Population density v region
ggplot(asthma_data, aes(x = region, y = pop_density_sqmile, fill= region)) +
  geom_boxplot() +
  labs(title = "Population Density by U.S. Region",
       x = "Region",
       y = "Population Density (square mile)",
       fill = "Region") +
  theme_minimal()
```

<img src="FinalProjectAsthmaCrook_files/figure-gfm/box plot-2.png" style="display: block; margin: auto;" />

## ANOVA Test

We will run two ANOVA tests. The first test is for state population
density against asthma prevalence. The second test is for region
population density against asthma prevalence.

``` r
# load necessary libraries

library("car")
library("lme4")

# create a linear model for population density per square mile and asthma prevalence by state

m1 <- lm(prevalence ~ pop_density_sqmile, data=asthma_data)

# perform statistical test

Anova(m1)
```

    ## Anova Table (Type II tests)
    ## 
    ## Response: prevalence
    ##                    Sum Sq Df F value Pr(>F)
    ## pop_density_sqmile  0.343  1  0.1877 0.6668
    ## Residuals          87.694 48

``` r
summary(m1)
```

    ## 
    ## Call:
    ## lm(formula = prevalence ~ pop_density_sqmile, data = asthma_data)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.5804 -0.8478 -0.0491  0.6314  2.6415 
    ## 
    ## Coefficients:
    ##                     Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)        1.044e+01  2.404e-01  43.446   <2e-16 ***
    ## pop_density_sqmile 3.042e-04  7.022e-04   0.433    0.667    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.352 on 48 degrees of freedom
    ## Multiple R-squared:  0.003894,   Adjusted R-squared:  -0.01686 
    ## F-statistic: 0.1877 on 1 and 48 DF,  p-value: 0.6668

``` r
# create a linear model for average population density per region and asthma prevalence

r1 <- lm(prevalence ~ region, data=asthma_data)

# perform statistical test

Anova(r1)
```

    ## Anova Table (Type II tests)
    ## 
    ## Response: prevalence
    ##           Sum Sq Df F value Pr(>F)  
    ## region    16.928  3  3.6503 0.0192 *
    ## Residuals 71.109 46                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
summary(r1)
```

    ## 
    ## Call:
    ## lm(formula = prevalence ~ region, data = asthma_data)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.8111 -0.7702  0.1231  0.7597  2.7500 
    ## 
    ## Coefficients:
    ##                     Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          11.7111     0.4144  28.258  < 2e-16 ***
    ## regionSouth          -1.5611     0.5180  -3.013  0.00419 ** 
    ## regionNorth Central  -1.5944     0.5483  -2.908  0.00558 ** 
    ## regionWest           -1.2342     0.5391  -2.289  0.02671 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.243 on 46 degrees of freedom
    ## Multiple R-squared:  0.1923, Adjusted R-squared:  0.1396 
    ## F-statistic:  3.65 on 3 and 46 DF,  p-value: 0.0192

# DISCUSSION

## Interpretation - Box plots (figures 1 and 2)

These bar plots provides some support for our hypothesis that asthma
prevalence will vary by region of the United States. We can see that the
region with the highest population density–the Northeast– also has the
highest prevalence of asthma. There is an obvious variation in asthma
prevalence based on region but it is important to note that the
percentage of people with asthma are not extremely different. The range
of percentages for asthma prevalence is just below 10% for the lowest
region and just above 12% for the highest region. The correlation
between a higher population density and a higher asthma prevalence only
seems to be true for the Northeast. The region with the second highest
asthma percentage is the West but the West has the lowest population
density. For our hypothesis to be fully supported by these plots, we
would see both of them follow the same trend of the highest/lowest
regions for population density would also be the highest/lowest region
for asthma prevalence.

## Interpretation - ANOVA results

We ran two ANOVA tests. The first is to see if there is a significant
difference in asthma prevalence by state when compared with population
density. The results for this first ANOVA of 0.667 which is not
significant meaning that there is not a significant correlation between
asthma prevalence and population density when comparing by state.

The second ANOVA test we ran was comparing the population density by
region to asthma prevalence. This test did have a significant p-value of
0.0192. This results supports our hypothesis that asthma prevalence
varies by region because there is a significant different between the
regions.

# CONCLUSION

In conclusion, there is a slight correlation between population density
and asthma prevalence. It is not a strong correlation as shown by the
ANOVA test comparing between states but there is a significant enough
difference by region to say that there is a correlation when comparing
by region. Our hypothesis that asthma prevalence varies by region is
supported by our results. There is room for further study to see what
factors, other than population density, affect asthma prevalence.

# REFERENCES

1.  ChatGPT. OpenAI, version Jan 2025. Used as a reference for functions
    such as ggplot(), creating the figures and troubleshooting error
    messages. Accessed 2025-11-10.
2.  Centers for Disease Control and Prevention. (2024, November 21).
    Most recent asthma data. U.S. Department of Health and Human
    Services.
    <https://www.cdc.gov/asthma-data/about/most-recent-asthma-data.html>
3.  National Heart, Lung, and Blood Institute. (2025). Asthma. U.S.
    Department of Health and Human Services.
    <https://www.nhlbi.nih.gov/health/asthma>
4.  Asthma and Allergy Foundation of America. (n.d.). Air pollution and
    asthma. Asthma & Allergy Foundation of America.
    <https://aafa.org/asthma/asthma-triggers-causes/air-pollution-smog-asthma/>
5.  Kudo, M., Ishigatsubo, Y., & Aoki, I. (2013, September 9). Pathology
    of asthma. Frontiers.
    <https://www.frontiersin.org/journals/microbiology/articles/10.3389/fmicb.2013.00263/full>
6.  National Heart, Lung, and Blood Institute. (2025). Asthma. U.S.
    Department of Health and Human Services.
    <https://www.nhlbi.nih.gov/health/asthma>
7.  Paciência, I., & Cavaleiro Rufo, J. (2020). Urban-level
    environmental factors related to pediatric asthm… : Porto Biomedical
    Journal.
    <https://journals.lww.com/pbj/FullText/2020/02000/Urban_level_environmental_factors_related_to.2.aspx>
8.  Sockrider, M., & Fussner, L. (2020). What is asthma? \| American
    Journal of Respiratory and Critical Care Medicine.
    <https://www.atsjournals.org/doi/10.1164/rccm.2029P25>
9.  Win, P. H., & Hussain, I. (2009, May 22). Asthma triggers: What
    really matters?. Clinical Asthma.
    <https://pmc.ncbi.nlm.nih.gov/articles/PMC7152189/>

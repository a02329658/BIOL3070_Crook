Final Project: Asthma and Mortality
================
Whitney Crook
2025-11-12

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
- [STUDY QUESTION and HYPOTHESIS](#study-question-and-hypothesis)
  - [Question](#question)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [METHODS](#methods)
  - [Procedure](#procedure)
  - [Asthma Prevalence by State](#asthma-prevalence-by-state)
  - [Scatter Plot by State](#scatter-plot-by-state)
  - [Box Plot for Asthma Prevalence by
    Region](#box-plot-for-asthma-prevalence-by-region)
- [DISCUSSION](#discussion)
  - [Interpretation - Figure 1 ()](#interpretation---figure-1-)
  - [Interpretation - Figures 2 (Bar
    Plot)](#interpretation---figures-2-bar-plot)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

Write at the end. Summarize the study.

# BACKGROUND

Write like a funnel from general to specific

``` r
library(ggplot2)
library(maps)
library(dplyr)

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
    title = "Asthma Prevalence by U.S. State",
    subtitle = "Higher prevalence shown in darker colors"
  ) +
  theme_void() +
  theme(
    legend.position = "right",
    plot.title = element_text(size = 16, face = "bold"),
    plot.subtitle = element_text(size = 12)
  )
```

<img src="Final-Project-BIOL-3070-Crook_files/figure-gfm/prevalence by state-1.png" style="display: block; margin: auto;" />

# STUDY QUESTION and HYPOTHESIS

## Question

Do asthma prevalence and asthma mortality rates differ across U.S.
regions?

## Hypothesis

Regions with higher population density and greater urbanization will be
associated with higher asthma prevalence and mortality rates.

## Prediction

We predict that the Northeastern United States will have a higher
prevalence of asthma and asthma-related deaths because of the higher
population density.

# METHODS

### Procedure

## Asthma Prevalence by State

``` r
library(ggplot2)

ggplot(asthma_data, aes(x = reorder(state, prevalence), y = prevalence)) +
  geom_col(fill = "steelblue") +
  coord_flip() +
  labs(title = "Asthma Prevalence by U.S. State",
       x = "State",
       y = "Asthma Prevalence (%)") +
  theme_minimal()
```

<img src="Final-Project-BIOL-3070-Crook_files/figure-gfm/bar plot per state-1.png" style="display: block; margin: auto;" />

## Scatter Plot by State

``` r
ggplot(asthma_data, aes(x = population, y = prevalence)) +
  geom_point(aes(color=region)) +
  geom_text(aes(label = state_abrv), vjust = -0.5, size = 3)+
  labs(title = "Number of Individuals With Asthma vs. Asthma Prevalence by State",
       x = "Population",
       y = "Asthma Prevalence (%)") +
  theme_minimal()
```

<img src="Final-Project-BIOL-3070-Crook_files/figure-gfm/scatter plot-1.png" style="display: block; margin: auto;" />

## Box Plot for Asthma Prevalence by Region

To create this bar plot, we split the United States into four
regions-Northeast, South, North Central, and West. The plot shows the
average asthma prevalence (percentage) for each of the four regions
showing the amount of error. The standard error comes from finding the
average asthma percentage among all of the states included in each
region.

``` r
library(ggplot2)

ggplot(asthma_data, aes(x = region, y = prevalence)) +
  geom_boxplot() +
  labs(title = "Asthma Prevalence by U.S. Region",
       x = "Region",
       y = "Asthma Prevalence (%)") +
  theme_minimal()
```

<img src="Final-Project-BIOL-3070-Crook_files/figure-gfm/box plot-1.png" style="display: block; margin: auto;" />

# DISCUSSION

## Interpretation - Figure 1 ()

## Interpretation - Figures 2 (Bar Plot)

This bar plot supports our prediction that the Northeast would have the
highest asthma prevalence

# CONCLUSION

# REFERENCES

1.  ChatGPT. OpenAI, version Jan 2025. Used as a reference for functions
    such as ggplot(), creating the map and troubleshooting error
    messages. Accessed 2025-11-10.
2.  Centers for Disease Control and Prevention. (2024, November 21).
    Most recent asthma data. U.S. Department of Health and Human
    Services.
    <https://www.cdc.gov/asthma-data/about/most-recent-asthma-data.html>

Look into these as possible sources
<https://pmc.ncbi.nlm.nih.gov/articles/PMC3620776/?utm_source=chatgpt.com>
<https://pmc.ncbi.nlm.nih.gov/articles/PMC3620776/?utm_source=chatgpt.com>
<https://pubmed.ncbi.nlm.nih.gov/34606402/>
<https://ij-healthgeographics.biomedcentral.com/articles/10.1186/s12942-023-00343-6?utm_source=chatgpt.com>

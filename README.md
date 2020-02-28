# Google-Search-Query-Gtrends
This project uses Gtrends to perform queries on Google searches for desired keywords in desired locations over a desired time period.
I do not have pictures for you but can confirm it works and is quite useful in data collection.

# install.packages("tidyverse")
# install.packages("gtrendsR")
# install.packages("openxlsx")
# install.packages("writexl")
# link below to find details about gtrends including time
# https://cran.r-project.org/web/packages/gtrendsR/gtrendsR.pdf

library(tidyverse)
library(gtrendsR)
library(writexl)

# Get Google Web query activity for keyword = "XXXXXX" in US for "XXX" time, several different times and locations can be swapped out, check the Cran.
res <- gtrends(c("XXXXXX"),geo=c("US"), gprop = "web", time = "all", low_search_volume = TRUE)

# Plot most popular related queries in US but could search by different states to narrow a desired field.
res$related_queries %>%
  filter(related_queries=="top", geo=="US") %>%
  mutate(value=factor(value,levels=rev(as.character(value))),
         subject=as.numeric(subject)) %>%
  ggplot(aes(x=value,y=subject)) +
  geom_bar(stat='identity') +
  coord_flip()

# Get web query activity for keyword = "XXXXXX"
test <- gtrends(c("XXXXXX"), geo=c("US"))

# Creates a csv file at "XXX" destination for three interest queries. Could be expanded.
write.csv(res$interest_by_dma, file = "C:/XXXXXXX.csv")
write.csv(res$interest_by_city, file = "C:/XXXXXXX.csv")
write.csv(res$related_queries, file = "C:/XXXXXXX.csv")

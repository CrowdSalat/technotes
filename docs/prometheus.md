# my cheat sheet

```promql
# filter
metricname{fieldName="filter1", fieldName2="filter2", fieldName3=~"regexFilter.+"}

# range 
metricname{fieldName="filter1"}[5m]


```
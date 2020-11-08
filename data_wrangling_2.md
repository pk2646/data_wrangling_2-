data wrangling 2
================
Pallavi Krishnamurthy

## Scrape a table

I want the first table from this page -
“<http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm>”

Read in the html

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"
```

``` r
drug_use_html = read_html(url)
```

extract the table(s) focus on the first one

``` r
tab_marij = 
  drug_use_html %>% 
  html_nodes(css = "table") %>% 
  first() %>%
  html_table() %>% 
  slice(-1) %>% 
  as_tibble()
```

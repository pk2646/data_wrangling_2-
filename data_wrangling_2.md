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

## Non- table collection of data from a website

Star wars movie info

``` r
url = "https://www.imdb.com/list/ls070150896/"

star_wars_html = read_html(url)
```

Grab elements that I want

``` r
title_vector =
  star_wars_html %>% 
  html_nodes(css = ".lister-item-header a") %>% 
  html_text()

grossrev_vector =
  star_wars_html %>% 
  html_nodes(css = ".text-small:nth-child(7) span:nth-child(5)") %>% 
  html_text()

runtime_vector =
  star_wars_html %>% 
  html_nodes(css = ".runtime") %>% 
  html_text()

swm_df = 
  tibble(
    title = title_vector,
    gross_rev = grossrev_vector,
    runtime = runtime_vector)
```

## API

This is coming from an API

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>% 
  content("parsed")
```

    ## Parsed with column specification:
    ## cols(
    ##   year = col_double(),
    ##   new_york_city_population = col_double(),
    ##   nyc_consumption_million_gallons_per_day = col_double(),
    ##   per_capita_gallons_per_person_per_day = col_double()
    ## )

Extract from a JSON file

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>% 
  content("text") %>%
  jsonlite::fromJSON() %>%
  as_tibble()
```

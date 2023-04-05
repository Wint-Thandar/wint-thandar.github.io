---
layout: post
title: Earthquake Data Preparation & Analysis
image: "/posts/earthquake-analysis.png"
tags: [R, Tableau, Data Preparation, Data Manipulation, Data Analysis]
---

## Project Purpose
The purpose of this project is to create a function in Python that can quickly find all the prime numbers below a given value.<br><br>

## Git Hub Repository Link
<https://github.com/Wint-Thandar/r-projects/tree/main/earthquake_data_download><br><br>

## Data Source 
**Search Earthquake Catalog: ** <https://earthquake.usgs.gov/earthquakes/search/>
**ANSS Comprehensive Earthquake Catalog (ComCat) Documentation: ** <https://earthquake.usgs.gov/data/comcat/><br>
**Web Service API Documentation - Earthquake Catalog: ** <https://earthquake.usgs.gov/fdsnws/event/1/><br>
**ComCat Wrapper Libraries and Demonstration Code: ** <https://github.com/usgs/devcorner><br>
**rcomcat: ** <https://github.com/usgs/rcomcat><br>

`stringr` package is used to manipulate strings easily. 

```r
primes_finder(100)
>>> [1] "Successfully downloaded 1054028 records in 123 files."
>>> [1] "File created with name earthquakes-data.csv"
>>>    user  system elapsed 
>>>  161.11   13.41  925.02 
```
<br>
<br>
```r

```
---
layout: post
title: Earthquake Data Analysis Dashboards using Tableau
toc: true
image: "/posts/earthquake-dashboard.png"
tags: [Tableau Public, Reporting Dashboard, Data Analysis]
---

## Project Purpose
The purpose of this project is to create earthquake data reporting dashboards using the earthquake data CSV file created in [Earthquake Data Preparation & Analysis](https://wint-thandar.github.io/Earthquake-Data-Preparation-And-Analysis/) project.<br><br>

## Data Source 
The earthquake data for this project is collected from the USGS Earthquake Hazards Program website using the web service API. The detailed process of data collection and transformation is written in the [Earthquake Data Preparation & Analysis](https://wint-thandar.github.io/Earthquake-Data-Preparation-And-Analysis/) post. The tidy data file has `**1,056,144** records` and `**9** columns` of earthquakes data `from January 2013 to April 2023` with `magnitude 1 and above`.<br><br>

## Dataset Information 
The USGS monitors and reports on earthquakes, assesses earthquake impacts and hazards, and conducts targeted research on the causes and effects of earthquakes.

The variables and their data types, typical values and descriptions are documented at 
[ANSS Comprehensive Earthquake Catalog (ComCat) Documentation.](https://earthquake.usgs.gov/data/comcat/#event-terms) The full comprehensive documentation can be found [here.](https://earthquake.usgs.gov/data/comcat/)<br><br>

## Final Output
Earthquake data analysis dashboards created in Tableau Public server.
<iframe seamless frameborder="0" src="https://public.tableau.com/views/Earthquakes_2013-2023_Dashboard/EarthquakesTrackerPage1?:embed=yes&:display_count=yes&:showVizHome=no" width = '1090' height = '900'></iframe>
<br>

To view the data better, you can hide the filters by clicking the cross button. You can view the filters again by clicking on the same button. There are three dashboards created for data analysis, you can view each one by clicking on the respective tab. The filters are shared between all dashboards. So, the filtered values will be active for all the dashboards. The filter icon on the right side of the filter name can be used to clear the filter and show all the values. The down arrow icon on the right side of the filter icon can be used to select whether to include or exclude the selected data of that filter from the dashboard. 
![Earthquake Data Dashboard](/img/posts/dashboard-live.png)
<br>

At the bottom right of the dashboard, you can see a share icon and a download icon. Share button can be used to get a sharable link of the dashboard. Download button can be used to download the file as an image, PDF, or PowerPoint file. If viewing the embed dashboard is too small, you can click on the Tableau logo at the bottom left of the dashboard. That will open the dashboard in a new tab. The advantage of that is that you can use the full screen feature by clicking on the full screen icon at the bottom right of the dashboard.
![Earthquake Data Dashboard](/img/posts/dashboard-live-2.png)
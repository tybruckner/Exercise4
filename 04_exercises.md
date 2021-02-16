---
title: 'Weekly Exercises #4'
author: "Ty Bruckner"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.0.5     v dplyr   1.0.3
## v tidyr   1.1.2     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.0
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(lubridate)     # for date manipulation
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(openintro)     # for the abbr2state() function
```

```
## Loading required package: airports
```

```
## Loading required package: cherryblossom
```

```
## Loading required package: usdata
```

```r
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
```

```
## 
## Attaching package: 'maps'
```

```
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(ggmap)         # for mapping points on maps
```

```
## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.
```

```
## Please cite ggmap if you use it! See citation("ggmap") for details.
```

```r
library(gplots)        # for col2hex() function
```

```
## 
## Attaching package: 'gplots'
```

```
## The following object is masked from 'package:stats':
## 
##     lowess
```

```r
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
```

```
## Linking to GEOS 3.8.0, GDAL 3.0.4, PROJ 6.3.1
```

```r
library(leaflet)       # for highly customizable mapping
library(carData)       # for Minneapolis police stops data
library(ggthemes)      # for more themes (including theme_map())
theme_set(theme_minimal())
```


```r
# Starbucks locations
Starbucks <- read_csv("https://www.macalester.edu/~ajohns24/Data/Starbucks.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Brand = col_character(),
##   `Store Number` = col_character(),
##   `Store Name` = col_character(),
##   `Ownership Type` = col_character(),
##   `Street Address` = col_character(),
##   City = col_character(),
##   `State/Province` = col_character(),
##   Country = col_character(),
##   Postcode = col_character(),
##   `Phone Number` = col_character(),
##   Timezone = col_character(),
##   Longitude = col_double(),
##   Latitude = col_double()
## )
```

```r
starbucks_us_by_state <- Starbucks %>% 
  filter(Country == "US") %>% 
  count(`State/Province`) %>% 
  mutate(state_name = str_to_lower(abbr2state(`State/Province`))) 

# Lisa's favorite St. Paul places - example for you to create your own data
favorite_stp_by_lisa <- tibble(
  place = c("Home", "Macalester College", "Adams Spanish Immersion", 
            "Spirit Gymnastics", "Bama & Bapa", "Now Bikes",
            "Dance Spectrum", "Pizza Luce", "Brunson's"),
  long = c(-93.1405743, -93.1712321, -93.1451796, 
           -93.1650563, -93.1542883, -93.1696608, 
           -93.1393172, -93.1524256, -93.0753863),
  lat = c(44.950576, 44.9378965, 44.9237914,
          44.9654609, 44.9295072, 44.9436813, 
          44.9399922, 44.9468848, 44.9700727)
  )

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   date = col_date(format = ""),
##   state = col_character(),
##   fips = col_character(),
##   cases = col_double(),
##   deaths = col_double()
## )
```

## Put your homework on GitHub!

If you were not able to get set up on GitHub last week, go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) and get set up first. Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 4th weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab under Stage and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 


## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises from tutorial

These exercises will reiterate what you learned in the "Mapping data with R" tutorial. If you haven't gone through the tutorial yet, you should do that first.

### Starbucks locations (`ggmap`)

  1. Add the `Starbucks` locations to a world map. Add an aesthetic to the world map that sets the color of the points according to the ownership type. What, if anything, can you deduce from this visualization?  
  

```r
world <- get_stamenmap(
    bbox = c(left = -180, bottom = -57, right = 179, top = 82.1), 
    maptype = "terrain",
    zoom = 2)
```

```
## Source : http://tile.stamen.com/terrain/2/0/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/0.png
```

```
## Source : http://tile.stamen.com/terrain/2/0/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/1.png
```

```
## Source : http://tile.stamen.com/terrain/2/0/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/1/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/2/2.png
```

```
## Source : http://tile.stamen.com/terrain/2/3/2.png
```

```r
ggmap(world)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude),
             alpha = .3,
             size = .1)+
  theme_map()+
  labs(title = "Worldwide Starbucks Locations")
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-1-1.png)<!-- -->
  

  2. Construct a new map of Starbucks locations in the Twin Cities metro area (approximately the 5 county metro area).  

```r
tcs <- get_stamenmap(
    bbox = c(left = -94.2, bottom = 44, right = -92, top = 46), 
    maptype = "terrain",
    zoom = 8)
```

```
## Source : http://tile.stamen.com/terrain/8/61/91.png
```

```
## Source : http://tile.stamen.com/terrain/8/62/91.png
```

```
## Source : http://tile.stamen.com/terrain/8/61/92.png
```

```
## Source : http://tile.stamen.com/terrain/8/62/92.png
```

```
## Source : http://tile.stamen.com/terrain/8/61/93.png
```

```
## Source : http://tile.stamen.com/terrain/8/62/93.png
```

```r
ggmap(tcs)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude),
             alpha = .3,
             size = 1)+
  theme_map()+
  labs(title = "Twin Cities Starbucks")
```

```
## Warning: Removed 25432 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-2-1.png)<!-- -->
  

  3. In the Twin Cities plot, play with the zoom number. What does it do?  (just describe what it does - don't actually include more than one map).  

A higher zoom shows more detail in the map. It also takes a much longer time to run because it brings in more detail.
  4. Try a couple different map types (see `get_stamenmap()` in help and look at `maptype`). Include a map with one of the other map types.  
  

```r
tcs_tb <- get_stamenmap(
    bbox = c(left = -94.2, bottom = 44, right = -92, top = 46), 
    maptype = "terrain-background",
    zoom = 8)
```

```
## Source : http://tile.stamen.com/terrain-background/8/61/91.png
```

```
## Source : http://tile.stamen.com/terrain-background/8/62/91.png
```

```
## Source : http://tile.stamen.com/terrain-background/8/61/92.png
```

```
## Source : http://tile.stamen.com/terrain-background/8/62/92.png
```

```
## Source : http://tile.stamen.com/terrain-background/8/61/93.png
```

```
## Source : http://tile.stamen.com/terrain-background/8/62/93.png
```

```r
ggmap(tcs_tb)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude),
             alpha = .3,
             size = 1)+
  theme_map()
```

```
## Warning: Removed 25432 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
  

```r
tcs_toner <- get_stamenmap(
    bbox = c(left = -94.2, bottom = 44, right = -92, top = 46), 
    maptype = "toner",
    zoom = 8)
```

```
## Source : http://tile.stamen.com/toner/8/61/91.png
```

```
## Source : http://tile.stamen.com/toner/8/62/91.png
```

```
## Source : http://tile.stamen.com/toner/8/61/92.png
```

```
## Source : http://tile.stamen.com/toner/8/62/92.png
```

```
## Source : http://tile.stamen.com/toner/8/61/93.png
```

```
## Source : http://tile.stamen.com/toner/8/62/93.png
```

```r
ggmap(tcs_toner)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude),
             alpha = .3,
             size = 1)+
  theme_map()
```

```
## Warning: Removed 25432 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
tcs_watercolor <- get_stamenmap(
    bbox = c(left = -94.2, bottom = 44, right = -92, top = 46), 
    maptype = "watercolor",
    zoom = 8)
```

```
## Source : http://tile.stamen.com/watercolor/8/61/91.jpg
```

```
## Source : http://tile.stamen.com/watercolor/8/62/91.jpg
```

```
## Source : http://tile.stamen.com/watercolor/8/61/92.jpg
```

```
## Source : http://tile.stamen.com/watercolor/8/62/92.jpg
```

```
## Source : http://tile.stamen.com/watercolor/8/61/93.jpg
```

```
## Source : http://tile.stamen.com/watercolor/8/62/93.jpg
```

```r
ggmap(tcs_watercolor)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude),
             alpha = .3,
             size = 1)+
  theme_map()
```

```
## Warning: Removed 25432 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

  5. Add a point to the map that indicates Macalester College and label it appropriately. There are many ways you can do think, but I think it's easiest with the `annotate()` function (see `ggplot2` cheatsheet).
  

```r
tcs <- get_stamenmap(
    bbox = c(left = -94.2, bottom = 44, right = -92, top = 46), 
    maptype = "terrain",
    zoom = 8)

ggmap(tcs)+
  geom_point(data = Starbucks,
             aes(x = Longitude,
                 y = Latitude),
             alpha = .3,
             size = 1)+
  theme_map()+
 annotate('text', x= -93.17123, y= 44.93790, label = 'Macalester College', colour = "red")+
   annotate('point', x= -93.17123, y= 44.93790, colour = "red")
```

```
## Warning: Removed 25432 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
  Add Macalester College

### Choropleth maps with Starbucks data (`geom_map()`)

The example I showed in the tutorial did not account for population of each state in the map. In the code below, a new variable is created, `starbucks_per_10000`, that gives the number of Starbucks per 10,000 people. It is in the `starbucks_with_2018_pop_est` dataset.


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>%  #reads in the data 
  separate(state, into = c("dot","state"), extra = "merge") %>% #turns a single column into multiple
  select(-dot) %>% # removes the dot column
  mutate(state = str_to_lower(state)) # makes all the state names lowercase
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   state = col_character(),
##   est_pop_2018 = col_double()
## )
```

```r
starbucks_with_2018_pop_est <-
  starbucks_us_by_state %>% #pipes in starbucks data
  left_join(census_pop_est_2018,
            by = c("state_name" = "state")) %>% # joins starbucks data to population by state row
  mutate(starbucks_per_10000 = (n/est_pop_2018)*10000) # creates proportion of starbucks 
```

  6. **`dplyr` review**: Look through the code above and describe what each line of code does.
  
  I annotated the code above with comments

  7. Create a choropleth map that shows the number of Starbucks per 10,000 people on a map of the US. Use a new fill color, add points for all Starbucks in the US (except Hawaii and Alaska), add an informative title for the plot, and include a caption that says who created the plot (you!). Make a conclusion about what you observe.



```r
states_map <- map_data("state")

starbucks_with_2018_pop_est %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state_name,
               fill = starbucks_per_10000)) +
  #This assures the map looks decently nice:
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map()+
  labs(title = "Starbucks Locations per State, Accounting for Populaiton Density")
```

![](04_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
<br />
The West Coast has the most Starbucks per 10000 people.
### A few of your favorite things (`leaflet`)

  8. In this exercise, you are going to create a single map of some of your favorite places! The end result will be one map that satisfies the criteria below. 

  * Create a data set using the `tibble()` function that has 10-15 rows of your favorite places. The columns will be the name of the location, the latitude, the longitude, and a column that indicates if it is in your top 3 favorite locations or not. For an example of how to use `tibble()`, look at the `favorite_stp_by_lisa` I created in the data R code chunk at the beginning.  

```r
favorite_stp_by_ty <- tibble(
  place = c("Home", "Macalester College", "Raisin Canes", 
            "US Bank Stadium", "Brasa", "Mall of America",
            "YMCA", "Target", "MSP Airport","Target Center"),
  long = c(-93.16444, -93.1712321, -93.22568, 
           -93.25572, -93.13303, -93.24558, 
           -93.17449, -93.15693, -93.22408,
           -93.27696),
  lat = c(44.93864, 44.9378965, 44.97380,
          44.97269, 44.94048, 44.85679, 
          44.95752, 44.95210, 44.88284,
          44.98064),
  top_three = c("yes","yes","no","yes","no","no","no","no","no","no")
  )

favorite_stp_by_ty <- favorite_stp_by_ty %>%
arrange(lat)
```
  

  * Create a `leaflet` map that uses circles to indicate your favorite places. Label them with the name of the place. Choose the base map you like best. Color your 3 favorite places differently than the ones that are not in your top 3 (HINT: `colorFactor()`). Add a legend that explains what the colors mean.  

```r
pal <- colorFactor("viridis",
                   domain = favorite_stp_by_ty$top_three)

leaflet(data = favorite_stp_by_ty) %>%
  addTiles()%>%
   addCircles(lng = ~long, 
             lat = ~lat, 
             label = ~place, 
             weight = 10, 
             opacity = 1, 
             color = ~pal(top_three)) %>% 
  addPolylines(lng = ~long, 
               lat = ~lat, 
               color = col2hex("darkred")) %>%
    addLegend(pal = pal, values = ~top_three)
```

```{=html}
<div id="htmlwidget-4495ffefffc224c7ee26" style="width:672px;height:480px;" class="leaflet html-widget"></div>
<script type="application/json" data-for="htmlwidget-4495ffefffc224c7ee26">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["//{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"http://openstreetmap.org\">OpenStreetMap<\/a> contributors, <a href=\"http://creativecommons.org/licenses/by-sa/2.0/\">CC-BY-SA<\/a>"}]},{"method":"addCircles","args":[[44.85679,44.88284,44.9378965,44.93864,44.94048,44.9521,44.95752,44.97269,44.9738,44.98064],[-93.24558,-93.22408,-93.1712321,-93.16444,-93.13303,-93.15693,-93.17449,-93.25572,-93.22568,-93.27696],10,null,null,{"interactive":true,"className":"","stroke":true,"color":["#440154","#440154","#FDE725","#FDE725","#440154","#440154","#440154","#FDE725","#440154","#440154"],"weight":10,"opacity":1,"fill":true,"fillColor":["#440154","#440154","#FDE725","#FDE725","#440154","#440154","#440154","#FDE725","#440154","#440154"],"fillOpacity":0.2},null,null,["Mall of America","MSP Airport","Macalester College","Home","Brasa","Target","YMCA","US Bank Stadium","Raisin Canes","Target Center"],{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null,null]},{"method":"addPolylines","args":[[[[{"lng":[-93.24558,-93.22408,-93.1712321,-93.16444,-93.13303,-93.15693,-93.17449,-93.25572,-93.22568,-93.27696],"lat":[44.85679,44.88284,44.9378965,44.93864,44.94048,44.9521,44.95752,44.97269,44.9738,44.98064]}]]],null,null,{"interactive":true,"className":"","stroke":true,"color":"#8B0000","weight":5,"opacity":0.5,"fill":false,"fillColor":"#8B0000","fillOpacity":0.2,"smoothFactor":1,"noClip":false},null,null,null,{"interactive":false,"permanent":false,"direction":"auto","opacity":1,"offset":[0,0],"textsize":"10px","textOnly":false,"className":"","sticky":true},null]},{"method":"addLegend","args":[{"colors":["#440154","#FDE725"],"labels":["no","yes"],"na_color":null,"na_label":"NA","opacity":0.5,"position":"topright","type":"factor","title":"top_three","extra":null,"layerId":null,"className":"info legend","group":null}]}],"limits":{"lat":[44.85679,44.98064],"lng":[-93.27696,-93.13303]}},"evals":[],"jsHooks":[]}</script>
```

  
  * Connect all your locations together with a line in a meaningful way (you may need to order them differently in the original data).  
  
  * If there are other variables you want to add that could enhance your plot, do that now.  
  
## Revisiting old datasets

This section will revisit some datasets we have used previously and bring in a mapping component. 

### Bicycle-Use Patterns

The data come from Washington, DC and cover the last quarter of 2014.

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usualy, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`. This code reads in the large dataset right away.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   name = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   nbBikes = col_double(),
##   nbEmptyDocks = col_double()
## )
```

  9. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. This time, plot the points on top of a map. Use any of the mapping tools you'd like.
  Stations %>%
  left_join(Trips,
            by= c("name" = "sstation")) %>%
  group_by(long,lat)%>%
  mutate(numtimes = n(),
         prop = mean(client == "Casual"))%>%
  ggplot(aes(x = long, y = lat, color = prop))+
  geom_point()+
  labs(title = "Proportion of Casual Users by Station")
  
  

```r
wash <- get_stamenmap(
    bbox = c(left = -77.2483, bottom = 38.7353, right = -76.7814, top = 39.0523), 
    maptype = "terrain",
    zoom = 11)
```

```
## Source : http://tile.stamen.com/terrain/11/584/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/585/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/586/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/587/782.png
```

```
## Source : http://tile.stamen.com/terrain/11/584/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/585/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/586/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/587/783.png
```

```
## Source : http://tile.stamen.com/terrain/11/584/784.png
```

```
## Source : http://tile.stamen.com/terrain/11/585/784.png
```

```
## Source : http://tile.stamen.com/terrain/11/586/784.png
```

```
## Source : http://tile.stamen.com/terrain/11/587/784.png
```
  

```r
new_data <- Stations %>%
  left_join(Trips,
             by = c("name" = "sstation")) %>%
    group_by(long,lat)%>%
  mutate(numtimes = n(),
         prop = mean(client == "Casual"))
```


```r
 ggmap(wash) +
    geom_point(data = new_data,
               aes(x = long, y = lat,
                   color = numtimes),
               alpha = .3, size = 2)
```

```
## Warning: Removed 1246 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
  10. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? Also plot this on top of a map. I think it will be more clear what the patterns are.
  

```r
 ggmap(wash) +
    geom_point(data = new_data,
               aes(x = long, y = lat,
                   color = prop),
               alpha = .3, size = 2)
```

```
## Warning: Removed 1246 rows containing missing values (geom_point).
```

![](04_exercises_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
### COVID-19 data

The following exercises will use the COVID-19 data from the NYT.

  11. Create a map that colors the states by the most recent cumulative number of COVID-19 cases (remember, these data report cumulative numbers so you don't need to compute that). Describe what you see. What is the problem with this map?

```r
states_map <- map_data("state")

covid_table <- covid19%>%
  filter(date == max(ymd(date))) %>%
  mutate(state_name = str_to_lower(state)) 
```


```r
covid_table %>%
  ggplot()+
  geom_map(map = states_map,
           aes(map_id = state_name,
               fill = cases))+
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map() +
  labs(title = "Cumulative Covid Cases")
```

![](04_exercises_files/figure-html/unnamed-chunk-17-1.png)<!-- -->
  <br />
  This map does not account for population density or date of the first case like we talked about on a previous assignment.
  12. Now add the population of each state to the dataset and color the states by most recent cumulative cases/10,000 people. See the code for doing this with the Starbucks data. You will need to make some modifications. 

```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   state = col_character(),
##   est_pop_2018 = col_double()
## )
```

```r
covid_new_table <-
  covid_table %>% 
  left_join(census_pop_est_2018,
            by = c("state_name" = "state")) %>% 
  mutate(covid_per_10000 = (cases/est_pop_2018)*10000)
```

```r
covid_new_table %>%
   ggplot()+
  geom_map(map = states_map,
           aes(map_id = state_name,
               fill = covid_per_10000))+
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map()+
  labs(title = "Cases per 10000 People")
```

![](04_exercises_files/figure-html/unnamed-chunk-19-1.png)<!-- -->
  
  13. **CHALLENGE** Choose 4 dates spread over the time period of the data and create the same map as in exercise 12 for each of the dates. Display the four graphs together using faceting. What do you notice?
  
## Minneapolis police stops



These exercises use the datasets `MplsStops` and `MplsDemo` from the `carData` library. Search for them in Help to find out more information.

  14. Use the `MplsStops` dataset to find out how many stops there were for each neighborhood and the proportion of stops that were for a suspicious vehicle or person. Sort the results from most to least number of stops. Save this as a dataset called `mpls_suspicious` and display the table.  
  

  
  15. Use a `leaflet` map and the `MplsStops` dataset to display each of the stops on a map as a small point. Color the points differently depending on whether they were for suspicious vehicle/person or a traffic stop (the `problem` variable). HINTS: use `addCircleMarkers`, set `stroke = FAlSE`, use `colorFactor()` to create a palette.  
  
  
  16. Save the folder from moodle called Minneapolis_Neighborhoods into your project/repository folder for this assignment. Make sure the folder is called Minneapolis_Neighborhoods. Use the code below to read in the data and make sure to **delete the `eval=FALSE`**. Although it looks like it only links to the .sph file, you need the entire folder of files to create the `mpls_nbhd` data set. These data contain information about the geometries of the Minneapolis neighborhoods. Using the `mpls_nbhd` dataset as the base file, join the `mpls_suspicious` and `MplsDemo` datasets to it by neighborhood (careful, they are named different things in the different files). Call this new dataset `mpls_all`.


```r
mpls_nbhd <- st_read("Minneapolis_Neighborhoods/Minneapolis_Neighborhoods.shp", quiet = TRUE)
```

  17. Use `leaflet` to create a map from the `mpls_all` data  that colors the neighborhoods by `prop_suspicious`. Display the neighborhood name as you scroll over it. Describe what you observe in the map.
  
  18. Use `leaflet` to create a map of your own choosing. Come up with a question you want to try to answer and use the map to help answer that question. Describe what your map shows. 
  
  
## GitHub link

  19. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 04_exercises.Rmd, provide a link to the 04_exercises.md file, which is the one that will be most readable on GitHub.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**

![](SUHI_9session_overview.png)

# Table of Contents

```Session 1```
[Map Chicago's Community Areas](#session-1-map-chicagos-community-areas)

```Session 2```
[Map life expectancy by Chicago Community Areas](#session-2-map-life-expectancy-by-chicago-community-areas)

```Session 3```
[Make a Shiny App](#session-3-make-a-shiny-app)

```Session 4```
[Plan future sessions)](#session-4-plan-future-sessions)

# Session 1 (map Chicago's Community Areas)

#### Step 1
Go to the Chicago data portal at [https://data.cityofchicago.org/](https://data.cityofchicago.org/). Search for ```community areas``` and click on ```Boundaries - Community Areas (current)```. Then export the geospatial data file as a ```shapefile```.
**Note**: we want to keep this downloaded file zipped, some mac computers may automaticall unzip this file, if this happens to you open the folder you just downloaded (it will contain serveral files which together act as one shapefile), highlight all of these files and click to compress/zip them.

![](SUHI_session1_data_portal.png)

#### Step 2
Sign-up for a free RStudio Cloud account at [https://rstudio.cloud/](https://rstudio.cloud/)

![](SUHI_session1_Rstudio_cloud.png)

#### Step 3
In Rstudio Cloud create a ```New Project```. Click on the project title to rename it as ```GIS_Session_1``` and start a new ```R Script``` by clicking the ```Plus``` arrow on the top left of the screen. At this point let's save our new script. Click ```File - Save As```. Save it as ```my_first_script.R```. As a note, an ```.R``` file is a R script file.

![](SUHI_session1_Rstudio_new_project.png)
![](SUHI_session1_new_script.png)

#### Step 4
R is an open source coding software with a lot of pre-built libraries of code. We can freely use these libraries but we have to install and load them first. Type the code below in your R-script, the first three lines are used to install the libraries, the next four lines of code are used to load libraries. After you copied and pasted this code, highlight the first three lines of code and click ```run``` or ```ctrl enter``` to execute the code.
```
install.packages("sf")
install.packages("raster")
install.packages("ggplot2")

library(sf)
library(sp)
library(raster)
library(ggplot2)
```

#### Step 5
Next, add the zipped Chicago Community Area shapefile we downloaded earlier into your Rstudio folder. Click upload, and select the downloaded shapefie. The contents of the zipped folder will now apear in your Rstudio folder location and will show up as four seperate files.

![](SUHI_session1_upload.png)

#### Step 6
Use the code below to read-in the shapefile. Note: if the code below does not work, it is probably becuase the name of your shapefile you uploaded is different than mine. Make sure to type the name of your shapefile. After you load in the data you can review the shapefile's meta data to see what ```projection``` our shapefile is in using the second line of code below. We will talk more about ```projections``` in a later session.

```
Chicago_CCA <- st_read("/cloud/project/geo_export_8234332b-d045-40c0-9022-795cc89c5f1e.shp")

st_crs(Chicago_CCA)
```

#### Step 7
Finally, let's create a map of the Chicago Community Areas using the code below. The last line of code saves our map as a ```.png``` in our Rstudio folder. You can play around with the ggplot code and change the ```size``` of the borders from 0.2 to say 2, and the colors. As you play around with this code and try to make sense of it you will begin to appreciate how you can customize your map using R code. The result of this code is the map image below.
```
plot(Chicago_CCA)

ggplot() +
  geom_sf(data = Chicago_CCA, size = 0.2, color = "grey", fill = "cornsilk") +
  coord_sf() + theme(panel.background = element_blank(),
                     axis.text.x=element_blank(),
                     axis.ticks.x=element_blank(),
                     axis.text.y=element_blank(),
                     axis.ticks.y=element_blank())
                     
 ggsave("CCA.png")
 ```
![](SUHI_session1_final_map.png)

### Click to return to [Table of Contents](#table-of-contents)

# Session 2 (map life expectancy by Chicago Community Areas)
Session 2 builds off session 1 in that it assumes we have already added our Chicago Community Area (CCA) shapefile and installed/loaded all needed libraries (see session 1). In session 2, we will download and clean life expectancy data from the Chicago Health Atals and merge this data with our shapefile so it can be spatially visualized.

#### Step 1
Go to the Chicago Health Atlas at [https://www.chicagohealthatlas.org/](https://www.chicagohealthatlas.org/). Click ```view all indicators``` 

![](SUHI_session2_1_health_atlas.png)

#### Step 2
Click the ```life expectancy``` indicator and then click to download the data.

![](SUHI_session2_2_download_life_exp.png)

#### Step 3
Open up the data in Excel; click ```enable editing``` if this pop-up appears. Now clean the data so we only have rows for the year 2017, and for Community areas (there should be 77 rows of data with 1 row of headers). Also, delete columns so all that remain are: ```Year```, ```Geography```, ```Geo_group```, and ```Number```. Save your cleaned data as a ```.csv``` file.

I achieved this by clicking ```Data``` and turning on ```Filters```. I then selected to only show the year 2017 and the ```Geo_Group``` type Community Area. I then selected the visable data, copied it, and pasted it into a new sheet. I highlighted columns I did not need, right clicked, and deleted the non-needed data (see image below).

![](SUHI_session2_3_clean_data.png)

#### Step 4
Log-in to your free RStudio Cloud account at [https://rstudio.cloud/](https://rstudio.cloud/) and open up our project from session 1; if you named your project the same as I did in session 1 your project will be called ```GIS_Session1```.

#### Step 5
Next, upload the clean life expectancy data you made into your Rstudio folder.

![](SUHI_session1_upload.png)

#### Step 6
Use the code below to read in the life expectancy data frame and view its contents.
```
Chicago_Life <- read.csv("CCA_life_data.csv", header = TRUE)
View(Chicago_Life)
```
In order to join our life expectancy data with our Chicago Community Areas shapefile we need to identify a column in each data frame that is common. Use the code below to open up the CCA data and look for a common column between it and the life expectancy data frame.
```
View(Chicago_CCA)
```

#### Step 7
It looks like the ```area_numbe``` column/variable from the shapefile and the column/variable ```Geo_ID``` from our life expectancy data are a match! Use the code below to make the join and check your work (by viewing the new data table to see if things look right).

```
Merged_Life <- merge(Chicago_CCA, Chicago_Life, by.x="area_numbe", by.y="Geo_ID")
View(Merged_Life)
```

#### Step 8
To create our first cholopleth map we need to assign color values that match up with an attribute/variable/column value. The code below allows you to do this by using the ```aes(fill=__)``` function. The rest of the code is borrowed from the code we used in session 1 to make a map of CCA's. The last line of code saves the image as a png. We just created our first choropleth map showing 2017 life expectancy by Chicago Community Area!

```
ggplot(data = Merged_Life) +
  geom_sf(aes(fill = Number), size = 0.2, color = "white") +
  coord_sf() + theme(panel.background = element_blank(),
                     axis.text.x=element_blank(),
                     axis.ticks.x=element_blank(),
                     axis.text.y=element_blank(),
                     axis.ticks.y=element_blank())
 ggsave("CCA_life.png")
 ```
 ![](SUHI_session2_4_map.png)
 
### Click to return to [Table of Contents](#table-of-contents)
 
# Session 3 (make a Shiny App)
Session 3 uses what we learned in sessions 1 and 2 to create a ```Shiny App```. A ```Shiny App``` is a web application (aka a website) that is built using R code. In this session we will learn the basics for creating an interactive website to visually display Chicago Health Atlas data.

#### Step 1
Log-in to Rstudio Cloud [https://rstudio.cloud/](https://rstudio.cloud/) and create a ```New Project```. Click on the project title; rename it ```my_shiny_app```. Like shown in the image below, use the plus-sign to create a new ```Shiny Web App```. When prompted, click yes to install an updated version of the ```shiny``` package. Name your new shiny app ```health_atlas``` and select ```Multiple File``` from the pop-up menu (see image below). This process will take a few minutes but when done, two ```r-scripts``` will have been created, one will be named ```UI``` - this is where you will write your user interface code - and the other ```server``` - this is where you will write the back end code; together these two scripts make up your interactive website.

![](SUHI_session3_1_r_script.png)

#### Step 2
Let's run our app to see what it currently looks like by pressing ```run app```. As you can seen, we have a simple website with a visualziation section and a user input section. We are going to replace both of these so the visualziation section shows a map of Chicago, and the user input section allows the user to pick from different health atlas variables.

![](SUHI_session3_2_first_run_ap.png)

#### Step 3
Before we edit the code, let's understand what code exists in the ```UI``` and ```server``` scripts.

```
UI script

hinyUI(fluidPage(
```

![](SUHI_session3_3_explore_first_app.png)


the ```console``` type the code below to install the shiny package and load the shiny library. Remeber to click run or press ```Ctrl + Enter``` to run your code.
```
install.packages("shiny")
library(shiny)
```

#### Step 2
To make your app (aka website) we need to set-up the ```front-end``` which is the user interface and the ```back-end``` which is a server that works behind the scenes. Both will be done in ```R```! Start by creating two new ```R Scripts```. As a reminder, you can do this by clicking the ```Plus``` arrow on the top left of the screen and selecting "new R script" (do this twice). Click the first script (it should be listed as "Untitled1"); click ```Save-As``` and name the script ```ui``` for user interface. Click on the second script (named "Untitled2") and save this one as ```server```.

![](SUHI_session3_1_r_script.png)



### Click to return to [Table of Contents](#table-of-contents)

# Session 4 (plan future sessions)

### Click to return to [Table of Contents](#table-of-contents)

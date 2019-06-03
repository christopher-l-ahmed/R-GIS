![](SUHI_9session_overview.png)

# GIS in R: Session 1

#### Step 1
Go to the Chicago data portal at [https://data.cityofchicago.org/](https://data.cityofchicago.org/). Search for ```community areas``` and click on ```Boundaries - Community Areas (current)```. Then export the geospatial data file as a ```shapefile```.

![](SUHI_session1_data_portal.png)

#### Step 2
Sign-up for a free RStudio Cloud account at [https://rstudio.cloud/](https://rstudio.cloud/)

![](SUHI_session1_Rstudio_cloud.png)

#### Step 3
In Rstudio Cloud create a ```New Project```. Click on the project title to rename it as ```GIS_Session_1``` and start a new ```R Script``` by clicking the ```Plus``` arrow on the top left of the screen. At this point let's save our new script. Click ```File - Save As```. Save it as ```my_first_script.R```. As a note, an ```.R``` file is a R script file.

![](SUHI_session1_Rstudio_new_project.png)
![](SUHI_session1_new_script.png)

#### Step 4
R is an open source coding software with a lot of pre-built librarys of code. We can freely use these librarys but we have to install them. We will be using several librayrs today. Type the code below in your R-script, after each line of code click ```ctrl enter``` to run the code.
```
install.packages("sf")
install.packages("raster")
install.packages("ggplot2")

library(sf)
library(sp)
library(raster)
library(gglplot2)
```

#### Step 5
Next, add the Chicago Community Area shapefile we downloaded earlier into your Rstudio folder. Click upload, and select the downloaded shapefie.

![](SUHI_session1_upload.png)

#### Step 6
Use the code below to read-in the shapefile and to review the shapefile's meta data.

```
Chicago_CCA <- st_read("/cloud/project/geo_export_8234332b-d045-40c0-9022-795cc89c5f1e.shp")
st_crs(Chicago_CCA)
```

#### Step 7
Finally, visualize the map using the following code and export it as a ```.png``` file. You should have create a map that looks like the image below.
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

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

![]()

#### Step 6
Use the code below to read-in the shapefile and to review the shapefile's meta data.

#### Step 7
Finally, visualize the map using the following code and export it as a ```.png``` file.

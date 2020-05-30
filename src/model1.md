# Modeling & Predicting Spatial Values

In this lab, you will use the model parameters your previously estimated in order to predict spatial values across the landscape of your selected LMIC.  To do this create a new script, install and load needed packages and libraries, and set your working directory.  Once you have your script set up with `sf::` , `raster::`, `tidyverse::`, `doParallel::`, and `snow::` all in place, again load your `RasterStack` into your RStudio workspace.  I named my 12 layer `RasterStack` that describes land use and land cover throughout Liberia, `lulc`, and just as before it displays the following summary characteristics.

![](../.gitbook/assets/screen-shot-2019-10-06-at-6.53.01-pm.png)

Also, load the `adm0` \(or international boundary\), `adm1` and `adm2` for your LMIC.  If you don't have your international boundary, go back to GADM, HDX or also look on Geoboundaries for the shapefile in order to import to your RStudio work session.

{% embed url="https://www.gadm.org" caption="Link to GADM" %}

{% embed url="https://data.humdata.org" caption="Link to HDX" %}

{% embed url="http://www.geoboundaries.org/data/1\_3\_3/zip/shapefile/" caption="Geoboundaries subdirectory with shapefiles by ISO code" %}

For this exercise, I will retrieve my international border shapefile from the Geoboundaries subdirectory and then use the `read_sf()` command to import the data as an object I named `lbr_int`.

You will also need the WorldPop raster later in the exercise in order to calculate a basic estimate of spatial error.  To improve upon the predictive capability of the model, I have gone back and retrieved the 2015 WorldPop raster file of persons per pixel \(ppp\) for Liberia, rather than the 2019 population counts we previously used.  It is probably not going to have a huge impact, but if you want to try for the best results, the date value of your response variable should be the same as the date value of your geospatial covariates.

```r
lbr_pop15 <- raster("lbr_ppp_2015.tif")
```

You may have noticed when reviewing the characteristics of your `RasterStack` that many of the layers had `?` as the value for both the `min values` and `max values`.  The reason this occurred is because each of the layers within the `raster` is presenting values outside of the boundaries of our LMIC.  For example, with Liberia, you will notice that there is a large portion of the area within the plot but outside of the national border that has some assigned value \(likely `NA`\).  These values are not part of our analytical area and should be omitted.  In order to fix this problem, we need to use the `mask()` command to remove all of the gridcells that are not explicitly within the international border of our LMIC.

```r
yourRasterStack <- add_command_here(yourRasterStack, yourLMIC_intlborder)
```

It might take a few minutes to run the `mask()` command.  On a MBAir using the `mask()` command on a 12 layer `RasterStack` \(each layer having about 25 million gridcells\) it takes about 10 minutes.  Once the command is completed, you should now notice `min values` and `max values` when retrieving a summary of what has now been transformed from a `RasterStack` to a `RasterBrick`.

![](../.gitbook/assets/screen-shot-2019-10-06-at-7.50.14-pm.png)

As in the previous exercise, estimate your linear model using the `pop15` variable as your response \(dependent variable\) and all of the covariates from your `adm2` sf object as the predictors \(independent variables\).

```r
model <- add_command_here(depvar ~  ind1 + ... + indN, data=your_adm2_sf)
```

Once you have estimated your model, use the `summary()` command to review a summary of its characteristics.

![](../.gitbook/assets/screen-shot-2019-10-06-at-8.23.25-pm.png)

Confirm that each variable in your `lulc` object has a corresponding variable in the linear model you just estimated.  You will use these estimates with the 12 different geospatial coverariate layers within your `RasterBrick` to predict the population at each gridcell across your LMIC.  Use the `predict()` function from the `raster::` package with your `lulc` object as well as your `model` to predict the population value of every gridcell within the borders of your LMIC.

```r
predicted_values <- library::function(RasterBrick, your_model, progress="window")
```

Adding the `progress="window"` argument at the end of the command should force a progress window to appear on main desktop that informs you of how many steps are needed to completely execute the command as well as how many have been completed.  The `progress="window"` argument is purely optional.

The resulting object `predicted_values` should be a single `RasterLayer` with the same number of gridcells as each layer within your `RasterBrick`.   Type the name of the `RasterLayer` into the console to review its summary output, while also noting the minimum and maximum values across all gridcells.

Use the `cellStats(predicted_values, sum)` command to calculate the sum of all the values in every gridcell throughout your newly created `RasterLayer`.  With the model estimated for Liberia, the sum total of all predicted values is `113413402375`\(113 billion\).  Compared with the output from `sum(your_adm2$pop15)` \(which is `4039128`for Liberia\) your will very likely that your model has massively overestimated population values \(in this case by an order of about 28,000 times\).

While these predicted values are no where near the real population count at each gridcell, they do nonetheless provide a spatial description of the proportion of persons as distributed across the landscape of your LMIC.  In fact, if we execute some basic raster algebra and subtract the minimium value from my `predicted_values` `RasterLayer` and then sum the values of all gridcells, we will find that while the total population predicted is still very likely a gross overestimation, it is getting closer to our best estimate of the real value \(in the case of Liberia, now an order of 15 times `pop15`\).

```r
base <- predicted_values - minValue(predicted_values)
cellStats(base, sum) 
```

We will proceed with the `RasterLayer` by using the `extract()`command to assign the ID from each `adm2` object to each gridcell containing a predicted value from our linear model using the `lulc` geospatial covariates as indenpendent variables.  To effectively use the `extract()` command, set `ncores` object to one less than your total and then also execute the `beginCluster()` command as you have done in previous exercises.  In this case, since it is only one layer, the extract command should take less time to execute \(about 10 to 15 minutes on an MBAir\).

```r
ncores <- detectCores() - 1
beginCluster(ncores)
pred_vals_adm2 <- raster::extract(your_pred_vals_raster, your_adm2, df=TRUE)
endCluster()
```

Once you have assigned the `ID` to each gridcell according to its `adm2` location, aggregate the values.  This time we will use a slightly different command to sum the values by `adm2` unit, although either way could work.  Also, bind this new column to your `adm2` `sf` object.

```r
pred_ttls_adm2 <- aggregate(. ~ ID, pred_vals_adm2, sum)
lbr_adm2 <- bind_cols(lbr_adm2, pred_ttls_adm2)
```

Your `sf` object now has a new column named `layer` that has the sum of all predicted values for each `adm2` subdivision of the LMIC.  Assign the value for `layer` to each gridcell according to its `adm2` subdivision.  Use the `rasterize()` command with your `adm2` object and your `predicted_values` raster in order to create a new `raster` that has the predicted totals of every `adm2` assigned to each gridcell according to its location.  The `raster` object used in the argument \(in this case `predicted_values`\) is the base template for the new raster produced as a result of the command. 

```r
new_raster <- command(adm2, template_raster, field = "layer")
```

Create a new raster that represents each gridcells proportion of the total within its `adm2` subdivision.

```r
another_new_raster  <- raster_1 / raster_2
```

Again use the `rasterize()` command to assign a value to every gridcell according to its `adm2` location, but this time use the `pop15` variable.

```r
yet_another_new_raster <- command(adm2, template_raster, field = "pop15")
```

Distribute the `adm2` populatoin totals across each gridcell according to its fractional proportion of summed predicted population \(also at `adm2`\), multiply the proportion by the totals.

```text
population <- gridcell_proportions * population_adm2
```

Confirm your results by evaluating the totals.

```text
cellStats(population, sum)
[1] 4039128
```

R should return the same total previously used when you calcualte `sum(your_adm2$pop15)`.

![](../.gitbook/assets/rplot01%20%289%29.png)

## Investigate Margins of Error

Our model is serving to allocate population totals across all gridcells, but how accurate is it?  To start we can calculate the different of our predicted values - the worldpop values and sum the totals.

```text
diff <- population - lbr_pop15
```

In order to establish a basis for comparing total error across your LMIC take the absolute value of the differences and sum them to arrive a term that represnts total error.

```text
cellStats(abs(diff), sum)
```

Taking the `hist(diff)` will also inform you of the magnitude and direction of error in your predicted values.  Use the `plot(diff)` command to have a look at the resulting raster.

![](../.gitbook/assets/rplot02%20%285%29.png)

By looking at the histogram and the above difference of predicted value from worldpop raster it appears that most of the error is slightly above or below 0, and is also distributed fairly evenly across the entire space.  Looking closely though, the area close to the southwest coast appears to exhibit a different phenomenon.  This is the capital of Liberia, Monrovia.  For your investigation, select the primary urban area and conduct the same analysis as follows.

First subset your `adm2` by using the name of the administrative subdivision.

```text
new_name <- your_adm2 %>%
  filter(name_var == "Add Name Here")
```

Use the `mask()` command with this newly created `sf` object to focus your analysis on the primate city within your LMIC.

```text
urban_diff <- mask(diff, urban_adm2)
urban_pop <- mask(population, urban_adm2)
```

Create an object the defines the boundaries of your identified urban area.  The bounding box used in your `crop()` command should be defined according the following order.

```text
c(western_most_longitude, eastern_most_longitude, southern_most_latitude, northern_most_latitude)
```

Following is the bounding box and `crop()` command used for Monrovia, Liberia.

```text
extGMN <- c(-10.83, -10.64, 6.20, 6.42)
gmonrovia_diff <- crop(gmonrovia_diff, extGMN)
gmonrovia_pop <- crop(gmonrovia_pop, extGMN)
```

Plot your Monrovia rasters.

![Error in terms of Predicted Values - WorldPop estimates](../.gitbook/assets/rplot03%20%285%29.png)

![](../.gitbook/assets/rplot04%20%283%29.png)

Finally, plot a three dimension map of the values, to gauge exactly how much variation was exhibited in the predicted values.  Install and load the `rgl::` and `rasterVis::` libraries in order to execute the following command.

```text
rasterVis::plot3D(gmonrovia_pop)
```

![](../.gitbook/assets/screen-shot-2019-10-07-at-12.26.00-am.png)

Finally, add the `tmap::` library and overlay your differences plot.

```r
mapview::mapview(gmonrovia_diff, alpha = .5)
```

Do you identify a geospatial trend associated with the error resulting from your predicted values?

![](../.gitbook/assets/rplot07%20%282%29.png)

## Team Challenge Question

Follow the steps from above used to produce the plots describing Liberia, but instead each team member should use their own selected LMIC country.  Investigate the results from your model at different scales and locations across your selected LMIC.  How effective was your model?  Do you identify any trends?  Produce a variety of plots that investigate, describe and analyze your dasymmetric population allocation using a linear model with land use and land cover geospatial covariates.  Investigate at least two different locations and two different scales.  Use adm1, adm2 or adm3 units of analysis, either in combination or alone to define the boundaries of your analysis.

Meet with your group and prepare to present three different plots from at least three different countries \(or team members\) for the Friday informal group presentation.  You are welcome to combine output from the previous Project 2 exercise \(part 1\) as you wish.  Then as a group, upload all 5 team members plots to \#data100\_igps \(informal group presentations\) by Sunday night.  Each member should upload at least **four plots** that describe **at least two different locations of differing scales** within your LMIC.




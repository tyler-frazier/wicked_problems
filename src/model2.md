# Investigating and Comparing Results

Load all of your covariates, import your adm0 \(international boundary\) to `crop()` and `mask()` your `RasterBrick` and then use `writeRaster()` to save it.  Later, use the `brick()` function to import your `RasterBrick` to your RStudio work session as needed.  Make sure the layers in your `RasterBrick` are named after using the `brick()` command.  Use the `names()` command to name your layers.

```r
f <- list.files(pattern="lbr_esaccilc_dst", recursive=TRUE)
lulc <- stack(lapply(f, function(i) raster(i, band=1)))
nms <- sub("_100m_2015.tif", "", sub("lbr_esaccilc_", "", f))
names(lulc) <- nms
topo <- raster("lbr_srtm_topo_100m.tif")
slope <- raster("lbr_srtm_slope_100m.tif")
ntl <- raster("lbr_viirs_100m_2015.tif")
lulc <- addLayer(lulc, topo, slope, ntl)
names(lulc)[c(1,10:12)] <- c("water","topo","slope", "ntl")

lbr_adm0  <- read_sf("gadm36_LBR_0.shp")

lulc <- crop(lulc, lbr_adm0)
lulc <- mask(lulc, lbr_adm0)

writeRaster(lulc, filename = "lulc.tif", overwrite = TRUE)
# lulc <- stack("lulc.tif")
lulc <- brick("lulc.tif")

names(lulc) <- c("water", "dst011" , "dst040", "dst130", "dst140", "dst150", 
                 "dst160", "dst190", "dst200", "topo", "slope", "ntl")
```

Obtain the 2015 WorldPop persons per pixel file for your LMIC.  Make sure your adm `sf` object has the total population per subdivision as well as its area and density.  Summarize all twelve geospatial covariates in adm groups by `sum` and by `mean`.  Modify the following code to create your adm2 or adm3 object \(you only have to select one, in the following example I have selected adm3 for Liberia\).

```r
lbr_pop15 <- raster("lbr_ppp_2015.tif")
# lbr_adm1  <- read_sf("gadm36_LBR_1.shp")
# lbr_adm2  <- read_sf("gadm36_LBR_2.shp")
lbr_adm3  <- read_sf("gadm36_LBR_3.shp")

# ncores <- detectCores() - 1
# beginCluster(ncores)
# pop_vals_adm1 <- raster::extract(lbr_pop15, lbr_adm1, df = TRUE)
# pop_vals_adm2 <- raster::extract(lbr_pop15, lbr_adm2, df = TRUE)
# pop_vals_adm3 <- raster::extract(lbr_pop15, lbr_adm3, df = TRUE)
# endCluster()
# save(pop_vals_adm1, pop_vals_adm2, pop_vals_adm3, file = "lbr_pop_vals.RData")

load("lbr_pop_vals.RData")

# totals_adm1 <- pop_vals_adm1 %>%
#   group_by(ID) %>%
#   summarize(pop15 = sum(lbr_ppp_2015, na.rm = TRUE))
# 
# lbr_adm1 <- lbr_adm1 %>%
#   add_column(pop15 = totals_adm1$pop15)
# 
# lbr_adm1 <- lbr_adm1 %>%
#   mutate(area = st_area(lbr_adm1) %>%
#            set_units(km^2)) %>%
#   mutate(density = pop15 / area)

# totals_adm2 <- pop_vals_adm2 %>%
#   group_by(ID) %>%
#   summarize(pop15 = sum(lbr_ppp_2015, na.rm = TRUE))
# 
# lbr_adm2 <- lbr_adm2 %>%
#   add_column(pop15 = totals_adm2$pop15)
# 
# lbr_adm2 <- lbr_adm2 %>%
#   mutate(area = st_area(lbr_adm2) %>%
#            set_units(km^2)) %>%
#   mutate(density = pop15 / area)

totals_adm3 <- pop_vals_adm3 %>%
  group_by(ID) %>%
  summarize(pop15 = sum(lbr_ppp_2015, na.rm = TRUE))

lbr_adm3 <- lbr_adm3 %>%
  add_column(pop15 = totals_adm3$pop15)

lbr_adm3 <- lbr_adm3 %>%
  mutate(area = st_area(lbr_adm3) %>% 
           set_units(km^2)) %>%
  mutate(density = pop15 / area)

#save(lbr_adm1, lbr_adm2, lbr_adm3, file = "lbr_adms.RData")

# lulc <- brick("lulc.tif")

# names(lulc) <- c("water", "dst011" , "dst040", "dst130", "dst140", "dst150", 
#                 "dst160", "dst190", "dst200", "topo", "slope", "ntl")

# ncores <- detectCores() - 1
# beginCluster(ncores)
# lulc_vals_adm1 <- raster::extract(lulc, lbr_adm1, df = TRUE)
# lulc_vals_adm2 <- raster::extract(lulc, lbr_adm2, df = TRUE)
# lulc_vals_adm3 <- raster::extract(lulc, lbr_adm3, df = TRUE)
# endCluster()
# save(lulc_vals_adm1, lulc_vals_adm2, lulc_vals_adm3, file = "lulc_vals_adms.RData")

load("lulc_vals_adms.RData")

# lulc_ttls_adm1 <- lulc_vals_adm1 %>%
#   group_by(ID) %>%
#   summarize_all(sum, na.rm = TRUE)

# lulc_ttls_adm2 <- lulc_vals_adm2 %>%
#   group_by(ID) %>%
#   summarize_all(sum, na.rm = TRUE)

lulc_ttls_adm3 <- lulc_vals_adm3 %>%
  group_by(ID) %>%
  summarize_all(sum, na.rm = TRUE)

lulc_means_adm3 <- lulc_vals_adm3 %>%
  group_by(ID) %>%
  summarize_all(mean, na.rm = TRUE)

# lbr_adm1 <- bind_cols(lbr_adm1, lulc_ttls_adm1)
# lbr_adm2 <- bind_cols(lbr_adm2, lulc_ttls_adm2)

lbr_adm3 <- bind_cols(lbr_adm3, lulc_ttls_adm3, lulc_means_adm3)

save(lbr_adm3, file = "lbr_adm3.RData")
```

Load your geospatial covariate `RasterBrick` of land use and land cover `lulc`, your 2015 WorldPop `raster` and  your adm `sf` object.  View your adm `sf` object and notice that you now have two summary sets of columns that describe each of your twelve geospatial covariates.  The first set of columns describes the sum of all values per adm while the second set describes the mean of all values per adm.  Also notice that in the second set, each variable  has had the number 1 added to its name to differentiate it from the first series.

Use the `lm()` function to estimate three models.  First use `pop15` as the response variable and the `sum` of each geospatial covariate per adm as the predictors.  Second, again use `pop15` as the response variable but this time instead use the `mean` of each geospatial covariate per adm as the predictors.  Third, use the logarithm of 2015 population `log(pop15)` as the response and the `mean` of each geospatial covariate per adm as the predictors.  Notice I created a new variable named `logpop15` but you could just as easily have specified `log(pop15)` within the call of your model.

```r
model.sums <- lm(pop15 ~ water + dst011 + dst040 + dst130 + dst140 + dst150 + dst160 + dst190 + dst200 + topo + slope + ntl, data=lbr_adm3)
model.means <- lm(pop15 ~ water1 + dst0111 + dst0401 + dst1301 + dst1401 + dst1501 + dst1601 + dst1901 + dst2001 + topo1 + slope1 + ntl1, data=lbr_adm3)

lbr_adm3$logpop15 <- log(lbr_adm3$pop15)
model.logpop15 <- lm(logpop15 ~ water1 + dst0111 + dst0401 + dst1301 + dst1401 + dst1501 + dst1601 + dst1901 + dst2001 + topo1 + slope1 + ntl1, data=lbr_adm3)
```

Check the summary output from each model.

```r
summary(model.sums)
summary(model.means)
summary(model.logpop15)
```

Make sure the names of each layer in your `RasterBrick` matches the names of the independent variables in each of your models.  Notice how the second two models use the `mean` of each geospatial covarariate and the layer names are modified accordingly to match.

```r
names(lulc) <- c("water", "dst011" , "dst040", "dst130", "dst140", "dst150", "dst160", "dst190", "dst200", "topo", "slope", "ntl")
lulc1 <- lulc
names(lulc1) <- c("water1", "dst0111" , "dst0401", "dst1301", "dst1401", "dst1501", "dst1601", "dst1901", "dst2001", "topo1", "slope1", "ntl1")
```

Use the `predict()` function from the `raster::` package with the appropriate `RasterBrick` and `model` to predict each gridcells value.  Use the `save()` and then the `load()` command in order to reduce computation time after restarting your work session. 

```r
predicted_values_sums <- raster::predict(lulc, model.sums)
predicted_values_means <- raster::predict(lulc1, model.means)
predicted_values_logpop15 <- raster::predict(lulc1, model.logpop15)

#save(predicted_values_sums, predicted_values_means, predicted_values_logpop15, file = "predicted_values.RData")
```

Use the `extract()` function from the `raster::` package to assign the adm ID to each gridcell.  Again use the `save()` and `load()` commands to reduce computation time.

```r
ncores <- detectCores() - 1
beginCluster(ncores)
pred_vals_adm3_sums <- raster::extract(predicted_values_sums, lbr_adm3, df=TRUE)
pred_vals_adm3_means <- raster::extract(predicted_values_means, lbr_adm3, df=TRUE)
pred_vals_adm3_logpop15 <- raster::extract(predicted_values_logpop15, lbr_adm3, df=TRUE)
endCluster()

#save(pred_vals_adm3_sums, pred_vals_adm3_means, pred_vals_adm3_logpop15, file = "predicted_values_adm3s.RData")
```

Aggregate all values.

```r
pred_ttls_adm3_sums <- aggregate(. ~ ID, pred_vals_adm3_sums, sum)
pred_ttls_adm3_means <- aggregate(. ~ ID, pred_vals_adm3_means, sum)
pred_ttls_adm3_logpop15 <- aggregate(. ~ ID, pred_vals_adm3_logpop15, sum)
```

Create a new data frame that contains the aggregate sums from each model's predictions.

```r
ttls <- cbind.data.frame(preds_sums = pred_ttls_adm3_sums$layer, 
                         preds_means = pred_ttls_adm3_means$layer, 
                         resp_logpop = pred_ttls_adm3_logpop15$layer)
```

Bind the new values as columns within your adm.  Notice I assigned a name to each column.

```r
lbr_adm3 <- bind_cols(lbr_adm3, ttls)
```

Use the `rasterize()` command to create a new `RasterLayer` containing the predicted values from each of your models.  The second object in your `rasterize()` command is the raster that is used as the template to produce the new raster.  The values of the template raster are not used in the calculation, instead the values in the column of your adm `sf` object that identify the sum of predicted values from your model is used. 

```r
predicted_totals_sums <- rasterize(lbr_adm3, predicted_values_sums, field = "preds_sums")
predicted_totals_means <- rasterize(lbr_adm3, predicted_values_sums, field = "preds_means")
predicted_totals_logpop <- rasterize(lbr_adm3, predicted_values_sums, field = "resp_logpop")
```

Calculate the gridcell proportions for each result.

```r
gridcell_proportions_sums  <- predicted_values_sums / predicted_totals_sums
gridcell_proportions_means  <- predicted_values_means / predicted_totals_means
gridcell_proportions_logpop  <- predicted_values_logpop15 / predicted_totals_logpop
```

Check the `cellStats()` to confirm that the sum of each objects proportions is equal to the number of adms in your `sf` object.

```r
cellStats(gridcell_proportions_sums, sum)
cellStats(gridcell_proportions_means, sum)
cellStats(gridcell_proportions_logpop, sum)
```

Produce a raster object that contains the WorldPop values we will use as our comparison spatial data set for validation.

```r
population_adm3 <- rasterize(lbr_adm3, predicted_values_sums, field = "pop15")
```

Calculate the final predicted value for each gridcell according to the output from each of the three models.

```r
population_sums <- gridcell_proportions_sums * population_adm3
population_means <- gridcell_proportions_means * population_adm3
population_logpop <- gridcell_proportions_logpop * population_adm3
```

Check `cellStats()` to verify that total population matches the initial value used in this dasymmetric allocation.

```text
cellStats(population_sums, sum)
cellStats(population_means, sum)
cellStats(population_logpop, sum)

sum(lbr_adm3$pop15)
```

Calculate the difference between each `RasterLayer` and the WorldPop `RasterLayer`.

```r
diff_sums <- population_sums - lbr_pop15
diff_means <- population_means - lbr_pop15
diff_logpop <- population_logpop - lbr_pop15
```

Finally, produce a raster plot of each model's predicted output as well as the differences and a 3D plot to visualize the results.

```r
plot(population_sums)
plot(diff_sums)
rasterVis::plot3D(diff_sums)
cellStats(abs(diff_sums), sum)

plot(population_means)
plot(diff_means)
rasterVis::plot3D(diff_means)
cellStats(abs(diff_means), sum)

plot(population_logpop)
plot(diff_logpop)
rasterVis::plot3D(diff_logpop)
cellStats(abs(diff_logpop), sum)

plot(lbr_pop15)

rgl.snapshot("diff", fmt = "png", top = TRUE )
```

![Population: Predictors - Sums](../.gitbook/assets/rplot%20%286%29.png)

![Difference: Predictors - Sums](../.gitbook/assets/rplot10.png)

![3D Difference: Predictors - Sums](../.gitbook/assets/diff1.png)

![Population: Predictors - Means](../.gitbook/assets/rplot11.png)

![Difference: Predictors - Means](../.gitbook/assets/rplot12.png)

![3D Difference: Predictors - Means](../.gitbook/assets/diff2.png)



![Population: Response - Log of Population](../.gitbook/assets/rplot13.png)

![Difference: Response - Log of Population ](../.gitbook/assets/rplot14.png)

![3D Difference: Response - Log of Population ](../.gitbook/assets/diff.png)

## Project 2. Individual Deliverable

Upload three sets of spatial plots that describe the predicted population of your selected LMIC using each of the three models.

1. Response variable is population and the predictors are sum of covariates
2. Response variable is population and the predictors are mean of covariates
3. Reponse variable is log of population and the predictors are mean of covariates

Each of the three sets of plots should also have three plots.

1. A plot that describes the predicted population of your LMIC using the model
2. A plot that describes the difference between your predicted results and the WorldPop estimates for 2015
3. A three dimension plot that visualizes the population or difference

Accompany your series of plots with a written statement that identifies which of the three models produced the best results.  Justify your assessment. 

Upload your deliverable to the slack channel \#data100\_project2 no later than 11:59PM on Sunday, October 20th.

## Individual Stretch Goal 1

Conduct the same analysis as above at an increased scale, analyzing one of your LMIC's largest or most significant urban areas or cities.  Subset your adm using the `%>%` operator and the `|` as needed.

```r
mts_bomi <- lbr_adm3 %>%
 filter(NAME_1 == "Montserrado" | NAME_1 == "Bomi")
```

Use the `mapview()` command to identify trends and provide further support for your assessment of each model as you did in the previous execise.

```r
mapview::mapview(gmonrovia_diff, alpha = .5)
```

Again compare the results.  Are you able to identify any trends?

## Individual Stretch Goal 2

Estimate a random forest model using the same data you previously used.  Use the mean values of all grid cells within each adm as the predictors \(independent variable\) and the log of population as the response \(dependent variable\).  Start by loading the World Pop raster you will use to validate your resuts against first.  Then load your adm0 to use in your `crop()` and `mask()` commands.  Load your adm3 that has all of your variables needed to estimate your random forest model.  Also be sure to load the land use and land cover variables you will use to predict the population of each individual grid cell.

```text
rm(list=ls(all=TRUE))

# install.packages("raster", dependencies = TRUE)
# install.packages("sf", dependencies = TRUE)
# install.packages("tidyverse", dependencies = TRUE)
# install.packages("doParallel", dependencies = TRUE)
# install.packages("snow", dependencies = TRUE)
# install.packages("randomForest", dependencies = TRUE)

#library(sp)
library(sf)
library(raster)
library(tidyverse)
library(doParallel)
library(snow)
library(randomForest)

### Import Administrative Boundaries ###

setwd("~/Tresors/teaching/project_folder/data/")

lbr_pop15 <- raster("lbr_ppp_2015.tif")

lbr_adm0  <- read_sf("gadm36_LBR_0.shp")
load("lbr_adm3.RData")

lulc <- brick("lulc.tif")

lulc <- crop(lulc, lbr_adm0)
lulc <- mask(lulc, lbr_adm0)
```

Simplify your adm3 by extracting only the needed columns.  Remove the geometry from the object by using the `st_geometry()` command and assigning it as `NULL`.  Add the log of population as a variable to your newly created data set.  Simply the class of your data set by rewriting it as a `data.frame`.  

```text
model_data <- lbr_adm3[ ,c(18:20, 35:46)]
st_geometry(model_data) <- NULL
model_data$logpop15 <- as.numeric(log(model_data$pop15))
model_data <- as.data.frame(model_data)
```

Your object `model_data` should have the following structure.

![](../.gitbook/assets/screen-shot-2019-10-21-at-11.23.53-pm.png)

Simplify the objects you will use as the predictors and response by creating two new objects.  The `x_data` object are your predictors and coorespond to the mean values of each `lulc` variable at each adm.  The `y_data` is your response variable, in this case log of population.

```text
x_data <- model_data[ ,4:15]
y_data <- model_data[ ,16]
```

First, tune your random forest model in order to determine which of the variables are important.  The `ntreeTry =`  argument specifies how many trees the model will estimate at the tuning step.  The `mtryStart =`  argument specifies how many variables will be tried at each split.  Other arguments are also important, but you can simply follow the following chunk of code to start.

```text
init_fit <- tuneRF(x = x_data, 
                   y = y_data,
                   plot = TRUE,
                   mtryStart = length(x_data)/3,
                   ntreeTry = length(y_data)/20,
                   imrpove = 0.0001,
                   stepFactor = 1.20,
                   trace = TRUE,
                   doBest = TRUE,
                   nodesize = length(y_data)/1000,
                   na.action = na.omit,
                   importance = TRUE,
                   proximity = TRUE,
                   sampsize = min(c(length(y_data), 1000)),
                   replace = TRUE)
```

After you get a result from your tuning step, check the importance scores from your model.  Use `importance(init_fit)` to have RStudio return measures for each variable.  Assign those scores to a vector and then subset using subscripting operators all variables that have a positive value.  Retain only those variables that have a positive importance score. 

```text
importance_scores <- importance(init_fit)
pos_importance <- rownames(importance_scores)[importance_scores[ ,1] > 0]
pos_importance

x_data <- x_data[pos_importance]
```

After respecifying your random forest model, estimate it again.

```text
pop_fit <- tuneRF(x = x_data,
                  y = y_data,
                  plot = TRUE,
                  mtryStart = length(x_data)/3,
                  ntreeTry = length(y_data)/20,
                  imrpove = 0.0001,
                  stepFactor = 1.20,
                  trace = TRUE,
                  doBest = TRUE,
                  nodesize = length(y_data)/1000,
                  na.action = na.omit,
                  importance = TRUE,
                  proximity = TRUE,
                  sampsize = min(c(length(y_data), 1000)),
                  replace = TRUE)
```

Finally, use several of the parameters from `pop_fit` in the arguments of your final model.

```text
model <- randomForest(x = x_data,
                      y = y_data,
                      mtry=pop_fit$mtry,
                      ntree = pop_fit$ntree,
                      nodesize = length(y_data)/1000,
                      importance = TRUE,
                      proximity = TRUE,
                      do.trace = FALSE)
```

Check the output from your model.

```text
print(model)
plot(model)
varImpPlot(model)
```

![Capacity of RF model to explain variance with its 500 trees](../.gitbook/assets/screen-shot-2019-10-21-at-11.40.59-pm.png)

![Number of trees needed before Out of Bag Error stabilized](../.gitbook/assets/rplot.png)

![Two measures of importance for each of the predictor variables](../.gitbook/assets/rplot01.png)

Confirm that the names in your random forest model match those found in your `rasterBrick`.

```text
names(lulc) <- c("water1", "dst0111" , "dst0401", "dst1301", "dst1401", "dst1501", "dst1601", "dst1901", "dst2001", "topo1", "slope1", "ntl1")
```

Now predict your population values using the model with the 12 different geospatial covariate layers.

```text
preds_rf <- raster::predict(lulc, model)
```

After you have predicted your population values for each gridcell, back transform the log of population to its original estimate.

```text
preds_rf_exp <- exp(preds_rf)
```

Next, extract all of the predicted values by assigning the ID for each adm unit where it is located.

```text
ncores <- detectCores() - 1
beginCluster(ncores)
preds_ttls_rf <- raster::extract(preds_rf_exp, lbr_adm3, df=TRUE)
endCluster()
```

Aggregate all of the values by adm ID and sum.

```text
preds_area_totals_rf <- aggregate(. ~ ID, preds_ttls_rf, sum)
```

Bind the columns.

```text
lbr_adm3 <- bind_cols(lbr_adm3, preds_area_totals_rf)
```

Finally, `rasterize()` the value of the total estimates per adm and then calculate the gridcell proportionate share across the entire LMIC.  Confirm that `cellStats()` returns a value equal to the number of adms in your LMIC.

```text
preds_ttls <- rasterize(lbr_adm3, preds_rf, field = "layer")
props  <- preds_rf_exp / preds_ttls
cellStats(props, sum)
```

Again, `rasterize()` population values and then multiply the gridcell proportions by the population values to estimate each gridcells proportion of the total population per gridcell.

```text
pops <- rasterize(lbr_adm3, preds_rf, field = "pop15")
gridcell_pops <- props * pops
cellStats(gridcell_pops, sum)
```

Check `cellStats()` to confirm your totals match population values calculated from the WorldPop persons per pixel raster layer.

Finally, subtract the raster layer with predicted values from your random forest model from the WorldPop ppp raster layer.  Calculate the sum of absolute value of differences between the two rasters.

```text
diff <- gridcell_pops - lbr_pop15
cellStats(abs(diff), sum)

rasterVis::plot3D(gridcell_pops)
rasterVis::plot3D(diff)
```

What can you surmise?  Have you improved your predictive power by applying a maching learning approach?




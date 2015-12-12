---
layout: post
title: "Lesson 01: Plot Raster Data in R"
date:   2015-10-28
authors: [Kristina Riemer, Jason Williams, Jeff Hollister, Mike Smorul, 
Zack Brym, Leah Wasser]
contributors: [Megan A. Jones]
packagesLibraries: [raster, rgdal]
dateCreated:  2015-10-23
lastModified: 2015-12-11
category: spatio-temporal-workshop
tags: [raster-ts-wrksp, raster]
mainTag: raster-ts-wrksp
description: "This lesson reviews how to plot a raster in R using the plot() 
command. It also covers how to layer a raster on top of a hillshade for a 
eloquent map.
output."
code1: Plot-Rasters-In-R.R
image:
  feature: NEONCarpentryHeader_2.png
  credit: A collaboration between the National Ecological Observatory Network (NEON) and Data Carpentry
  creditlink: http://www.neoninc.org
permalink: /R/Plot-Rasters-In-R
comments: false
---

{% include _toc.html %}

##About
This lesson reviews how to use the `plot` function in `R` to plot raster data.

**R Skill Level:** Intermediate - you've got the basics of `R` down.

<div id="objectives" markdown="1">

###Goals / Objectives

After completing this activity, you will:

* Be able how to plot a single band raster in R.
* Be able to layer a raster dataset on top of a hillshade to create an elegant 
basemap.

###Challenge Code
Throughout the lesson we have Challenges that reinforce learned skills. Possible
solutions to the challenges are not posted on this page, however, the code for 
each challenge is in the `R` code that can be downloaded for this lesson (see 
footer on this page).

###Things You'll Need To Complete This Lesson
You will need the most current version of R, and preferably RStudio, loaded on
your computer to complete this lesson.

####R Libraries to Install:

* **raster:** `install.packages("raster")`
* **rgdal:** `install.packages("rgdal")`

####Data to Download

Download the raster files for the Harvard Forest dataset:

<a href="http://files.figshare.com/2434040/NEON_RemoteSensing.zip" class="btn btn-success"> DOWNLOAD Sample NEON Airborne Observation Platform Raster Data</a> 

The LiDAR and imagery data used to create the rasters in this dataset were 
collected over the <a href="http://www.neoninc.org/science-design/field-sites/harvard-forest" target="_blank" >Harvard</a> and 
<a href="http://www.neoninc.org/science-design/field-sites/san-joaquin-experimental-range" target="_blank" >San Joaquin</a> field sites 
and processed at <a href="http://www.neoninc.org" target="_blank" >NEON </a> 
headquarters. The entire dataset can be accessed by request from the 
<a href="http://www.neoninc.org/data-resources/get-data/airborne-data" target="_blank"> NEON 
website.</a>

####Setting the Working Directory
The code in this lesson assumes that you have set your working directory to the
location of the unzipped file of data downloaded above.  If you would like a
refresher on setting the working directory, please view the [Setting A Working Directory In R]({{site.baseurl}}/R/Set-Working-Directory "R Working Directory Lesson") 
lesson prior to beginning this lesson.

####Raster Lesson Series 
This lesson is a part of a series of raster data in R lessons:

* [Lesson 00 - Intro to Raster Data in R]({{ site.baseurl}}/R/Introduction-to-Raster-Data-In-R/)
* [Lesson 01 - Plot Raster Data in R]({{ site.baseurl}}/R/Plot-Rasters-In-R/)
* [Lesson 02 - Reproject Raster Data in R]({{ site.baseurl}}/R/Reproject-Raster-In-R/)
* [Lesson 03 - Raster Calculations in R]({{ site.baseurl}}/R/Raster-Calculations-In-R/)
* [Lesson 04 - Work With Multi-Band Rasters - Images in R]({{ site.baseurl}}/R/Multi-Band-Rasters-In-R/)
* [Lesson 05 - Raster Time Series Data in R]({{ site.baseurl}}/R/Raster-Times-Series-Data-In-R/)
* [Lesson 06 - Plot Raster Time Series Data in R Using RasterVis and LevelPlot]({{ site.baseurl}}/R/Plot-Raster-Times-Series-Data-In-R/)
* [Lesson 07- Extract NDVI Summary Values from a Raster Time Series]({{ site.baseurl}}/R/Extract-NDVI-From-Rasters-In-R/)

###Sources of Additional Information

* <a href="http://cran.r-project.org/web/packages/raster/raster.pdf" target="_blank"> Read more about the `raster` package in R.</a>

</div>

##Plot Raster Data in R
In this lesson, we will plot the raster for the Digital Surface Model for
Harvard Forest. We will use the `hist()` function as a tool to explore raster 
values. And render categorical plots, using `breaks` or bins that are meaningful
in our data. 

We will continue to use the `raster` and `rgdal` libraries in this
lesson.  If you do not have the `DSM_HARV` object from [the Intro To Raster In R lesson]({{ site.baseurl}}/R/Introduction-to-Raster-Data-In-R "First Lesson in Series"), please create it now.  


    #if they are not already loaded
    library(rgdal)
    library(raster)
    
    #set working directory to ensure R can find the file we wish to import
    #setwd("working-dir-path-here")
    
    #import raster
    DSM_HARV <- raster("NEON_RemoteSensing/HARV/DSM/HARV_dsmCrop.tif")

First, let's plot our Digital Surface Model object (`DSM_HARV`) using the `plot`
function. We add a title using `main=""`.


    #Plot raster object
    plot(DSM_HARV,
         main="NEON Digital Surface Model\nHarvard Forest Field Site")

![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/hist-raster-1.png) 

# Plotting Data Using Breaks
Often we may want to view our data subsetted by a range of values rather than
using a continuous color ramp. This is comparable to a "classified" map in a
tool like ArcGIS or QGIS.

Unless there is a reason for having specific breaks, we want to use a
histogram of our data to determine where the breaks in our data should go. We 
can use `breaks=` to tell `R` to use fewer or more breaks or bins in our 
histogram. 
If we name the histogram we can then enter the name an find out much of the
underlying information in the histogram.  


    #Plot distribution of raster values 
    DSMhist<-hist(DSM_HARV,
         breaks=3,
         main="Histogram Digital Surface Model\nHarvard Forest Field Site",
         col="wheat3",  #changes bin color
         xlab= "Elevation (m)")  #label the x-axis

    ## Warning in .hist1(x, maxpixels = maxpixels, main = main, plot = plot, ...):
    ## 4% of the raster cells were used. 100000 values used.

![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/create-histogram-breaks-1.png) 

    #Where are breaks and how many pixels in each category?
    DSMhist$breaks

    ## [1] 300 350 400 450

    DSMhist$counts

    ## [1] 31941 67618   441

Warning message!?  Remember, the default for the histogram is to include only 
100,000 values from
the data set.  We could force it to show all the pixel values or we can use the
histogram as is and figure that the sample of 100,000 values represents our data
well. 

Looking at our histogram, `R` has binned out the data as follows:

* 300-350m, 350-400m, 400-450m

We have most of our pixel values in the middle range with a few lower values,
and very few higher values.  If we wanted a more even number of pixels in each
bin we could specify different breaks. 

We can use those bins to plot our raster data. We will use the 
`terrain.colors(3)` function to create a palette of 3 colors to use in our plot.

We can use the `breaks` method to add breaks.  To specify where the breaks
occur, we need to use the following syntax: `breaks=c(value1,value2,value3)`.
We can include as few or many breaks as we'd like.


    #plot using breaks.
    plot(DSM_HARV, 
         breaks = c(350, 300, 400, 450), 
         col = terrain.colors(3),
         main="NEON Digital Surface Model\nHarvard Forest Field Site")

![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/plot-with-breaks-1.png) 

###Formatting Color & Plot Axes
If we're going to be creating multiple plots and we want to easily be able to
change the color across the plots, we can create an `R` object (`myCol`) for the
color we want and use that object in the plot code. Then if we want to change
the color theme (or number of bins) we can just change the code once, not in all
plots.  

We can label the x- and y-axes of our plot too. 


    #Assign color to a object for repeat use/ ease of changing
    myCol = terrain.colors(3)
    
    #Add axis labels
    plot(DSM_HARV, 
         breaks = c(300, 350, 400, 450), 
         col = myCol,
         main="NEON Digital Surface Model\nHarvard Forest Field Site", 
         xlab = "UTM Westing Coordinate", 
         ylab = "UTM Northing Coodinate")

![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/add-plot-title-1.png) 

Or we can also turn off the axes altogether. 


    #or we can turn off the axis altogether
    plot(DSM_HARV, 
         breaks = c(300, 350, 400, 450), 
         col = myCol,
         main="NEON Digital Surface Model\nHarvard Forest Field Site", 
         axes=FALSE)

![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/turn-off-axes-1.png) 

##Challenge
Create a plot of the Harvard DSM that has

* has six categories that are evenly divided among the range of pixel values. 
* axis labels
* a plot title

![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/challenge-code-plotting-1.png) 


#Layering One Raster on Another 
Sometimes we want to layer a raster on top of another raster and use a
transparency to give it texture. The most common approach to this is to use a
*hillshade* layer as the base raster layer. A hillshade is simply a raster that
maps the shadows and texture that you would see from above when viewing terrain.
It gives the data a nice 3D appearance. 


    #import DSM hillshade
    DSM_hill_HARV <- raster("NEON_RemoteSensing/HARV/DSM/HARV_DSMhill.tif")
    
    #plot hillshade using a grayscale color ramp that looks like shadows.
    plot(DSM_hill_HARV,
        col=grey(1:100/100),  #create a color ramp of grey colors
        legend=F,
        main="NEON Hillshade - DSM\n Harvard Forest",
        axes=FALSE)

![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/hillshade-1.png) 

Now that we have the hillshade we can then layer another raster on top by
using `add=TRUE`. Let's overlay `DSM_HARV` on top of the `hill_HARV`.


    #plot hillshade using a grayscale color ramp that looks like shadows.
    plot(DSM_hill_HARV,
        col=grey(1:100/100),  #create a color ramp of grey colors
        legend=F,
        main="NEON DSM with Hillshade \n Harvard Forest",
        axes=FALSE)
    
    #add the DSM on top of the hillshade
    plot(DSM_HARV,
         col=rainbow(100),
         alpha=0.4,
         add=T,
         legend=F)

![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/overlay-hillshade-1.png) 

The alpha value determines how transparent the colors will be (0 being
transparent, 1 being opaque).  Note that here we used the color palette
`rainbow()` instead of `terrain.color()`.

For information on other <a href="https://stat.ethz.ch/R-manual/R-devel/library/grDevices/html/palettes.html" target="_blank">R color palettes</a>. 

##Challenge: Create DTM & DSM for SJER
Use the files in the `NEON_RemoteSensing/SJER/` to create a Digital Terrian
Model map and Digital Surface Model map of the San Joaquin Experimental Range
study site.
Make sure to
 
 * include hillshade in the maps,
 * label axes on the DSM map and exclude them from the DTM map, 
 * a title for the maps,
 * experiment with various alpha values and color palettes to represent the
 data.

![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/challenge-hillshade-layering-1.png) ![ ]({{ site.baseurl }}/images/rfigs/01-Plot-Raster/challenge-hillshade-layering-2.png) 

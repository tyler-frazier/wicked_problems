images/# Installing R and R Studio on your Computer

## What is R?

**R** is a free, open source, programming environment for statistical computing and graphics that compiles and runs on Linux, Windows and Mac OSX.  It is a very capable programming environment for accomplishing everything from an introduction to data science to some of the most poweful, advanced, and state of the art computational and statistical methods.  R is capable of working with big data, high dimension data, spatial data, temporal data, as well as data at pretty much any scale imaginable, from the cosmos to the quark and everywhere in between.

The statistical programming language R is often called an interpreted programming language, which is different from machine or native programming languages such a **C** or **Java**.  An interpreted programming language is distinguished from machine languages because commands and arguments are interpreted prior to being executed by the programming engine.  **Python** is another, closely related interpreted programming language that is also popular amongst data scientists.  Although the use of an interpreter compromises speed, interpreted languages have a distinct advantage in their capacity to be more readily accessible and understandable.  For example, commands such as `plot`, `read.csv()`, or `cbind()` can be fairly easily understood as the commands for plotting an object, importing a `.csv` file or binding together columns of data.  This accessibility has led to the strength of an open source community that is constantly developing new functions for use within the R programming framework and well as supporting their use by the larger community.  One of the major advantages of an open source approach to programming is the community that supports and contributes to **R** continued development.

![An introduction to open source solutions by Hengl, Wheeler &amp; MacMillan](https://peerj.com/preprints/27127.pdf)

In addition to being an open source programming framework, learning to use **R** also fullfills one of the fundamental principals of the scientific method, reproducibility.  A reproducible programming environment functions by always keeping source data in its original state external from the R framework.  Data is then imported to the work session and all changes occur within the framework through the code as it is sequentially executed.  In this manner, any code one writes is perfectly reproducible not only an unlimited number of times you choose to run it as well as by any other person who has access to your code \(unless you are incorporating probabilities type methods in your code\).  Compare this reproducible workflow concept to software that employs a graphic user interface \(GUI\), where commands are executed by selecting a pull down menu and following a series of preset options associated with each command.  Excel, Pages, Arc or QGIS are examples of software that use a GUI as their primary means of user interaction. Most programming environments keep the code separate from the interpreter or compiler and is much more easily reproducible.    

While **R** is easier to learn than more difficult programming languages such as C or Java, increasing its ease of use can be greatly advanced by using an integrated developer environment \(IDE\).  One of the most popular IDEs for **R** is called RStudio.  RStudio is dependent upon R in order to function, and literally sends commands and receives results to/from the interpreter.  RStudio has a number of different features that facilitate programming, project management, graphics production, reviewing data and a whole slew of other useful functions.  First you will want to install R and the associated tools, then follow by installing RStudio on your computer.

## Installing R

Before installing R on your operating system, it is a good idea to briefly assess the state of your computer and its constituent hardware as well as the state of your operating system.  Prior to installing a new software environment, such as R, I always recommend the following.

1. Do your best to equip your personal computer with the latest release of your operating system
2. Make sure you have installed all essential updates for your operating system
3. Restart your computer
4. Make sure that all non essential processes have not automatically opened at login, such as e-mail, messaging systems, internet browsers or any other software

After you have updated your computer and done your best to preserve all computational power for the installation process, go the **R Project for Statistical Computing** website.

{% embed url="https://www.r-project.org" %}

Find the **download** link and click on it.  If this is the first time you have downloaded **R**, then it is likely that you will also need to select a CRAN mirror, from which you will download your file.  Choose one of the mirrors from within the USA, preferable a server that is relatively close to your current location.  I typically select, Duke, Carnegie Mellon or Oak Ridge National Laboratory.   A more comprehensive install of R on a Mac OS X will include the following steps.

1. Click on the `R.pkg`  file to download the latest release.  Following the steps and install **R** on your computer.
2. Click on the **XQuartz** link and download the latest release of `XQuartz.dmg` .  It is recommended to update your XQuartz system each time you install or update **R**.
3. Click on the **tools** link and download the latest `clang.pkg` and `gfortran.pkg`. Install both.

Following are two video tutorials that will also assist you to install **R** on your personal computer.  The first one is for installing **R** on a Mac, while the second video will guide you through the process on Windows.

{% embed url="https://www.youtube.com/watch?v=V2x\_SWJCd1A&t=11s" caption="Video tutorial of how to install R on a Mac" %}

{% embed url="https://www.youtube.com/watch?v=1A-xvxNhd2w" caption="Video tutorial of how to install R on Windows" %}

## Installing RStudio

RStudio is an integrated developer environment that provides an optional front end, graphic user interface \(GUI\) that "sits on top" of the R statistical framework. In simple terms, RStudio will make your programming experience much easier, and is typically a good way for beginners to start off with a programmer langauge such as R.  RStudio assists with coding, executing commands, saving plots and a number of other different functions.  While the two are closely aligned in design and function, it is important to recognizing that RStudio is a separate program, which depends on **R** having first been installed.

To install RStudio go to the following webpage and download the appropriate installer for your operating system.  

{% embed url="https://www.rstudio.com/products/rstudio/download/\#download" %}

# Getting Started with RStudio & R

## Starting RStudio

Once you have finished the installation process, run the RStudio IDE, which will automatically find R on your computer.  Find the application RStudio on your computer.  The RStudio executable should be located in the applications folder on a Mac.  Once running the RStudio application on a Mac it is often helpful to keep the application icon in the dock, which is the bar of applications that exists along the bottom of your computer desktop screen.  On Windows, if you select the Window icon in the bottom left corner and begin typing RStudio, you should see the application icon appear. On a Linux system, the application should appear in one of the OS drop down windows.  Choose the application icon and open it.

If both R and RStudio were properly installed, then the start up for RStudio should appear something like the following image.

![A new RStudio work session at initial startup](images/rstudio.png)

One of the first things to note at start up is the bottom left hand pane, which is essentially a window to the R interpreter.  RStudio reports the version of R that has been installed on your computer.  Next go to the pull down menu for **File &gt; New Script &gt; R Script** and select that option.  This will create a new R Script that will appear in the top left pane of your R Studio IDE.  One can think of the script as the location where all computer code will be written and saved, in a manner somewhat analogous to writing a letter or essay with a work processor.  Below the script in RStudio is the console or the location where your commands are sent and responses from R will be returned.  You can think of the **&gt;** symbol in the console as R somewhat figuratively waiting for your command and subsequently also the location where R will respond.

Since you now have a script file where you will save your first R commands, you should also have a working director where you will save your `my_first_script.R` file.  At this point you should minimize RStudio, and return to your file explorer or finder and create a folder that you will use to save your script, output as well as any files you may import to R.  Generally, I create a project specific folder and then within that folder I begin with two subdirectories, one that is dedicated for storing data and a second one that is dedicated to saving my scripts.  After you have created your project folder, return to RStudio and then select **Session &gt;** **Set Working Directory &gt; Choose Directory** from the drop down menu.  Choosing this command will result in a file explorer window appearing in order for RStudio to select the working directory for your work session.  The working directory is the default location where R will automatically look in order to import or export and data.  Go ahead and select the **data** subdirectory you just created within your project folder.  Upon selecting your data folder, you should notice a command appear within the console pane in the bottom left hand corner of RStudio.

`setwd("~/my_folder/my_project_folder/my_data_folder")`

By using the RStudio IDE GUI you have just executed your first command.  You can confirm that the command was properly executed from within the console by typing the following command directly in the console.

`getwd()`

You should notice that R returns the path from the working directory you just designated.  Instead of setting your working directory using the drop down menu, the preferred method is to designate that path by using code in R.  Fortunately, RStudio has already specified the command for you, so just go ahead and copy the `setwd()` command from above and paste it into your script file in the top left hand corner of your RStudio work session.  You could also retype it, but in general, copying and pasting is going to be much more efficient.  On a Mac copying and pasting is accomplished by using the **⌘C** & **⌘V** keys or on Windows **control-C** and **control-V**. ****  After copying and pasting that line of code within your script, go ahead and execute the function again, except this time, send the command directly from your script to the console.  To do this use, move the cursor to the line where you pasted the code and then select **⌘return** on a Mac or **control-return** on Windows.  Congratulations! You have just written and executed your first line of code.

Now that your script has content, you should save the file.  Select the **&gt;File&gt;Save** command from the drop down window and choose the data subdirectory you created within your project folder.  Name your script file and then select save.  Your RStudio work space should appear similar to the following image.

![State of your RStudio workspace after setting the working directory and saving your script](images/rstudio1.png)

One noteworthy observation regarding the command `setwd()`, notice how the path to the working directory is specified within quotation marks.  In general, whenever RStudio is communicating with your operating system \(OS\) or any entity outside of its workspace, what ever is being sent to that computer will be included within quotation marks.  For example `setwd("the/path/to_my/working/directory")` is contained within quotation marks in order for RStudio to traverse the path  as defined by your OS to that location on your computer.

---
description: >-
  In this exercise you will learn how to create vector and data frame objects,
  use the sample function and generate a plot that includes different types of
  squares, circles and lines.
---

# Creating and Plotting Objects

## Creating an object & creating a plot

Since you have already set your working directory in the previous step, now you can create your first object.  Do so by writing the following command in your script.

`x <- 1:10`

There are essentially three parts to this command.  First take note of the `<-` symbol, which is often called the assignment operator.  The `<-` operator will function by assigning everything on its right hand side to the newly named object on the left.  For example, if you entered the command `t <- 1` and then typed `t` directly in the console and pressed return, R would inform you of the value of `t`, which would be 1.  After creating and defining `x` do the same thing for `y` but this time start with the highest value and decrease sequentially to 1.

`y <- 10:1`

Now lets move to the console and ask R a few things directly. Sometimes we want to save our script, while at other times we just want to ask R a quick question. First lets ask R to list all the objects that exist in our workspace at this point in time.  We use the `ls()` command to list all objects that exist in our workspace.

`[1] "x" "y"`

Let's also ask R to tell us more about the two objects we have created and placed in our workspace. Go ahead and type `x` and then `y` directly into your console and consider the output.

`x`

`[1] 1 2 3 4 5 6 7 8 9 10` 

`y`

`[1] 10 9 8 7 6 5 4 3 2 1`

We can see that our earlier use of the colon in `x <- 1:10` created an object named `x` that contains each whole number in sequence from 1 to 10, while y likewise did the same except in reverse. Also by simply typing the name of the object, R reveals to us everything it knows.  Since we have two objects of equal length, lets plot x & y together.

`plot(x,y)`

![A plot of x increasing while y is decreasing](images/rplot01%20%283%29.png)

We can continue to describe our plot by adding an argument to our command by specifying the plot type as a line and not simply points

`plot(x, y, type = "l")`

or alternatively a plot with both a line and points over that line.

`plot(x, y, type = "o")`

![A plot produced using the &quot;over&quot; specification in the argument ](images/rplot02%20%282%29.png)

We can also add some description to our plot in order to better communicate our results.  We can begin by adding a title, indicating the units of measurement while also adding labels for both the x and y axes.

```r
plot(x, y, type = "o", 
main = "The Path of a Running Boy",
sub = "units of distance = meters",
xlab = "longitude", 
ylab = "latitude")
```

![Plot with a Title, Sub-Title and Axes Labels](images/rplot03.png)

We can also change the linetype by specifying the `lty =` argument or set the lineweight by using the `lwd =` argument.  The color of our line can be changed using the `col = "some_color"` argument, while the point symbol itself can be modified by using the `pch =` argument.  Scale of the symbol is increased or descreased using `cex =`.  Have a look at the [Quick-R](https://www.statmethods.net/advgraphs/parameters.html) website for a comprehensive list of some available graphical parameters.

```r
plot(x, y, type = "b", main = "The Path of a Running Boy", 
     sub = "units of distance = meters", 
     xlab = "longitude", 
     ylab = "latitude",
     lty = 2,
     lwd = .75,
     col = "blue",
     pch = 0,
     cex = 1.5)
```

![A Plot with Some Point and Line Type Modifications](images/rplot04%20%281%29.png)

## Creating a More Complicated Plot while also creating and then using a Data Frame

Now lets make your plot a bit more complicated than simply a line with points.  First increase the scale of our plot area by increasing the range of values for both the x & y axes.

```r
x <- 1:100
y <- 1:100
```

Now instead of using those values, let's randomly select from both `x` & `y` in order to produce a random series of x & y coordinates.

```r
east <- sample(x, size = 10, replace = TRUE)
north <- sample(y, size = 10, replace = TRUE)
```

The above command `sample()` will randomly select in a uniform manner, one number from `x` and then also `y`, 10 times, creating the vector objects `east` & `north`.  I have also included the `replace = TRUE` argument, such that each time a number is selected, it is returned and potentially can be selected again in the next draw.  Now, lets take each value and use it as the coordinates for the center point of a number of squares.  We will use the `symbols()` command in order to add additional specifications to our command.

```r
symbols(east, north, squares = rep(.75,10), inches = FALSE)
```

Following is one possible outcome produced by the randomly produced coordinates.  While the squares produced in your plot will be in different locations, the number of squares as well as the size of each, should be very similar.  Lets also consider the additional arguments in the `symbols()` command.  In the `squares =` argument within the command, I have also used the `rep()` function, which will repeat the length of each square, `.75` in this case, 10 times, or 1 time for each square.  I have also added the `inches = FALSE` argument so the units are considered to be similar to the axes.

![Squares within a Defined Area](images/rplot01%20%286%29.png)

Now lets add some circles to our plot.  This time, instead of assigning an object a permanent value by randomly selecting from a series of numbers, lets randomly select values as part of creating the plot with the `symbol()` function.

```text
symbols(sample(x, 10, replace = TRUE), 
        sample(y, 10, replace = TRUE), 
        circles = rep(.75,10), 
        inches = FALSE,
        fg = "green",
        add = TRUE)
```

Where as before I created two objects and plotted their values as x & y coordinates, this time I have nested the `sample()` command within the `symbols()` function, in the place where R is looking for the x & y value coordinates.  In this manner, each time I execute the command, 10 circles will be randomly placed throughout the defined area, each with a radius of `.75`.  I have also included the `add = TRUE` argument within the command, in order to add the circles to our previous plot of square.  The `fg =` argument permits us to select a color for each circle.

![Squares with Randomly Placed Circles within a Defined Area](images/rplot02%20%283%29.png)

Let's also add some larger trees and specify their color as well.  Again we will randomly place them while using the `add = TRUE` argument so they are added to our previous plot.  Also, consider a wider range of colors to use as the outline for each circle, while also filling each circle with a color.  In order to determine how to fill the circle with a color, use the `?` followed by the command you are interested in learning more about in order to view all of the available options.  In this case you can type `?symbols` directly in the console in order to see all of the arguments possible.  If you scroll down in the help window, you will see that `fg =` is used to specify the color or your symbol border, while `bg =`  is used to indicate the color for your symbol's fill.  You may also be interested to know which colors are available to select.  In order to review a list of all available colors, simply type `colors()` directly into your console.  Running the following chunk of commands will then produce a plot similar to the following image.

```text
symbols(east, north, squares = rep(.75,10), inches = FALSE)

symbols(sample(x, 10, replace = TRUE), 
        sample(y, 10, replace = TRUE), 
        circles = rep(.75,10), 
        inches = FALSE,
        fg = "green1",
        bg = "beige",
        add = TRUE)

symbols(sample(x, 10, replace = TRUE), 
        sample(y, 10, replace = TRUE), 
        circles = rep(1.5,10), 
        inches = FALSE,
        fg = "green4",
        bg = "beige",
        add = TRUE)

```

![Squares with Two Types of Circles within a Defined Area](images/rplot03%20%284%29.png)

Thus far we have only created R objects that are of the vector class.  We can review the class of one of the objects we have created by typing `class(east)` directly into the console and observe that R informs us that the object is a vector of integers.  Now let's create a new class of an object called a data frame that contains a series of rows and columns where each row represents an observation while each column represents a different variable.  We can start with the coordinates that represent the center point of each square.

```r
dwellings <- cbind.data.frame(id = 1:10, east, north)
```

In this case, we are using the `cbind.data.frame()` command to column bind together the newly created variable named `id` with our two integer vectors `east` & `north` into a newly formed data frame named `dwellings`.  After executing the above command, you can type the name of your data frame directly into the console to review its content.  Within the environment pane in the top right hand window, under the data tab, you can also use your mouse to click on the data frame symbol that is off to the right of the `dwellings` data object.

|  | id | east | north |
| :--- | :--- | :--- | :--- |
| 1 | 1 | 48 | 64 |
| 2 | 2 | 25 | 74 |
| 3 | 3 | 59 | 10 |
| 4 | 4 | 37 | 83 |
| 5 | 5 | 97 | 29 |
| 6 | 6 | 74 | 92 |
| 7 | 7 | 84 | 16 |
| 8 | 8 | 17 | 98 |
| 9 | 9 | 70 | 21 |
| 10 | 10 | 33 | 69 |

You'll notice that R also provides row numbers that in this case are identical to the identification number we have assigned to each square.  Instead of assigning our `id` variable manually, we could have just as easily used `id = row.numbers(dwellings)` in order to achieve the same result, if the object `dwellings` already exists.

Now let's add a line that represents some type of transportation activity between each of the different dwelling units we have represented within our plot as squares.  We can add lines to the plot with the `lines()` command.  In order to identify the beginning and ending point of each line we set the `x =` argument to the `east` variable within the `dwellings` data frame.  Likewise we set the the `y =` argument to the `north` variable also witin the `dwellings` data frame.  One manner of informing R which variable is needed within a data frame is to use the `$` operator.  In general terms, the form to call a variable is `my_data_frame$my_variable`.  Following is an example, which should produce something similar to the subsequent plot.

```r
lines(x = dwellings$east,
      y = dwellings$north,
      lty = 2,
      lwd = .75,
      col = "blue")
```

![Paths travelled from House to House](images/rplot04.png)

You'll notice that unlike the previous commands, it wasn't necessary to include `add = TRUE` in order to add the lines to the plot.  Perhaps it would also be helpful to add some text to annotate each household.  We can accomplish this using the `text()` command.  As with `lines()`, we also do not need to use the `add = TRUE` argument.  The `text()` function also requires identifying the location of the text you will use to annotate each dwelling unit.  That is accomplished through using the `labels =` argument.  Again, identify the variable where the id is located within the data frame using the `my_data_frame$my_variable` format.

```r
text(x = dwellings$east,
     y = dwellings$north,
     labels = dwellings$id)
```

 Since label coordinates are the same as the center point for each square, reading the labels is confounded.  Instead of placing the label directly on top of the dwelling unit, add a few units north to the `y =` argument in order to displace each label a bit in the northerly direction.

![Paths &amp; House Numbers](images/rplot05%20%283%29.png)

Now perhaps instead of traversing a path between each house sequentially, our traveling person selected on 3 of the dwellings and moved between each of those buildings.  First we will randomly select 3 numbers that will be used to identify the chosen homes.  This time, set the `replace =` argument to `FALSE` since our traveling person will only visit each dwelling unit one time.

```r
locs <- sample(1:10, 3, replace = FALSE)
```

Now instead of using the `lines()` command to identify the `x =` and `y =` coordinates of each dwelling unit's center point, we will select only 3 building locations.  To do this, we wil lintroduce another method of traversing and selecting rows and/or columns from a data frame for use in an command and its arguments.  The `[` and `]` symbols are extremely powerful operators and can be used to subscript from within a function or argument.  Subscripting operators follow the format of first selecting the rows followed by a comma and then columns in this `[row_numbers, column_numbers]` format.  If either the rows space or columns space is left blank, then R assumes ALL rows and/or columns should be selected.  In the following command, I am using these subscripting operators to first select the 3 rows from the data frame that were randomly identified and then also include either column 2 for the easterly coordinate, or column 3 for the northerly coordinate.

```r
lines(x = dwellings[locs, 2],
      y = dwellings[locs, 3],
      lty = 2,
      lwd = .75,
      col = "blue")
```

Alternatively I could have also specified `x = dwellings[locs, ]$east` and `y = dwellings[locs, ]$north` in order to achieve the same result.  The following snippet demonstrates how that is accomplished while adding text in order to annotate each house with its id.

```r
# text(x = dwellings$east,
#      y = dwellings$north + 3,
#      labels = dwellings$id)

text(x = dwellings[locs, ]$east, 
     y = dwellings[locs, ]$north + 3,
     labels = dwellings[locs, ]$id)
```

You'll notice in the previous snippet of code that the `#` sign has been added to the first character space on lines 1, 2, & 3.  Adding the `#` sign enables you to comment out that line of code so R will ignore it.  In the above example, I have commented out the lines of code we produced earlier where we labeled all 10 houses, and followed it with out code that serves to label only the 3 units that were randomly selected.  At this point our plot should appear similar to the following image.

![Paths between 3 Identified Homes](images/rplot06.png)

Now instead of using a straight line, let's use a spline to represent a more continuous path betweem each of the selected locations along the persons travel path.  Comment out the previous `lines()` command and instead use the `xspline()` command to identify the path.  I will set the `shape = -1` in order to interpolate all points while crossing each dwelling unit.

```text
xspline(x = dwellings[locs, 2],
      y = dwellings[locs, 3],
      shape = -1,
      lty = 2)
```

![The Path of a Person en route between Homes](images/rplot07.png)

Finally, add a title to your plot using `title(main="A Person's path between Homes")`.

## Challenge Question

Create a similar plot as the one produced above, but instead meet the following specifications.

* Increase the minimum and maximum limits of your area from 1 to 1000 in both the `x` & `y` dimension.
* Randomly place 50 dwelling units throughout the 1000 x 1000 dimensioned area. Size each square appropriately.
* Randomly place 40 small circles \(trees\) throughout the 1000 x 1000 dimensioned area.  Set the radius of each circle to the same or approximately the same as the width of each home.
* Randomly place 12 large trees throughout the defined area, such that each tree has almost twice the radius as each home's width.
* Randomly select 7 homes from the 50 total, and use a dashed spline to describe the path between each labeled dwelling unit.
* Title your plot.

![One version of the plot produced by following the Challenge Question Specifications](images/rplot08.png)






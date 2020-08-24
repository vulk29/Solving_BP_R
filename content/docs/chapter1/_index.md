---
# Title, summary, and page position.
linktitle: Chapter 1
summary: R studio intro and basics.
weight: 1
icon: book-reader
icon_pack: fas

# Page metadata.
title: Chapter 1
date: "2018-09-09T00:00:00Z"
type: book  # Do not modify.
---

## Installing R and RStudio

What you're gonna need is to install the R software environment [here](https://mirrors.dotsrc.org/cran/) for your operating system.
  
![](https://eeecon.uibk.ac.at/~discdown/rprogramming/images/02/02_R_cran.jpg){width=50% height=50%}  
  
Click on the link to your operating system and click on the latest version to install, for mac it's the latest .pkg file on top of the list (4.0.0). If you don't know your operating system, go here: https://whatsmyos.com/

Then install RStudio, which is an integrated development environment or [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment). It is where you will spend most time with R, it has a welcoming layout and many functions that make R more accessible if you've only worked with graphical user interface tools for analyzing data, such as excel.  
To install RStudio, see https://rstudio.com/products/rstudio/download/#download for the free desktop version. Depending on whether you are a Mac or Windows user, the next steps will vary, but here is a step by step guide https://courses.edx.org/courses/UTAustinX/UT.7.01x/3T2014/56c5437b88fa43cf828bff5371c6a924/. 

Once you've installed RStudio you should see something like this.  
  
![](/assets/images/logo.png)


## Knowing your way around RStudio (Copyright (c) Data Carpentry)

Now that you have everything going, let's start by learning about [RStudio](https://www.rstudio.com/), which is an
Integrated Development Environment (IDE) for working with R.

The RStudio IDE open-source product is free under the
[Affero General Public License (AGPL) v3](https://www.gnu.org/licenses/agpl-3.0.en.html).
The RStudio IDE is also available with a commercial license and priority email
support from RStudio, Inc.

We will use the RStudio IDE to write code, navigate the files on our computer,
inspect the variables we create, and visualize the plots we generate. RStudio 
can also be used for other things (e.g., version control, developing packages, 
writing Shiny apps) that we will not cover during the workshop. 

One of the advantages of using RStudio is that all the information
you need to write code is available in a single window. Additionally, RStudio 
provides many shortcuts, autocompletion, and highlighting for the major file 
types you use while developing in R. RStudio makes typing easier and less
error-prone.


## Getting set up (Copyright (c) Data Carpentry)

It is good practice to keep a set of related data, analyses, and text
self-contained in a single folder called the **working directory**. All of the
scripts within this folder can then use *relative paths* to files. Relative paths
indicate where inside the project a file is located (as opposed to absolute paths, 
which point to where a file is on a specific computer). Working this way makes it
a lot easier to move your project around on your computer and share it with
others without having to directly modify file paths in the individual scripts.

RStudio provides a helpful set of tools to do this through its "Projects"
interface, which not only creates a working directory for you but also remembers
its location (allowing you to quickly navigate to it). The interface also 
(optionally) preserves custom settings and open files to make it easier to 
resume work after a break. 


## Create a new project (Copyright (c) Data Carpentry)

* Under the `File` menu, click on `New project`, choose `New directory`, then
  `New project`
* Enter a name for this new folder (or "directory") and choose a convenient
  location for it. This will be your **working directory** for the rest of the
  day (e.g., `~/data-carpentry`)
* Click on `Create project`
* Create a new file where we will type our scripts. Go to File > New File > R
  script. Click the save icon on your toolbar and save your script as
  "`script.R`".
  
## Organizing your working directory (Copyright (c) Data Carpentry)

Using a consistent folder structure across your projects will help keep things
organized and make it easy to find/file things in the future. This
can be especially helpful when you have multiple projects. In general, you might
create directories (folders) for **scripts**, **data**, and **documents**. Here
are some examples of suggested directories:

 - **`data/`** Use this folder to store your raw data and intermediate datasets. 
   For the sake of transparency and [provenance](https://en.wikipedia.org/wiki/Provenance), you
   should *always* keep a copy of your raw data accessible and do as much of
   your data cleanup and preprocessing programmatically (i.e., with scripts,
   rather than manually) as possible.
 - **`data_output/`** When you need to modify your raw data,
   it might be useful to store the modified versions of the datasets in a different folder.
 - **`documents/`** Used for outlines, drafts, and other
   text.
 - **`fig_output/`** This folder can store the graphics that are generated
   by your scripts.
 - **`scripts/`** A place to keep your R scripts for
   different analyses or plotting.

You may want additional directories or subdirectories depending on your project
needs, but these should form the backbone of your working directory.

<!-- ![Example of a working directory structure](../fig/working-directory-structure.png) -->

## The working directory (Copyright (c) Data Carpentry)

The working directory is an important concept to understand. It is the place
where R will look for and save files. When you write code for
your project, your scripts should refer to files in relation to the root of your working
directory and only to files within this structure.

Using RStudio projects makes this easy and ensures that your working directory
is set up properly. If you need to check it, you can use `getwd()`. If for some
reason your working directory is not what it should be, you can change it in the
RStudio interface by navigating in the file browser to where your working directory
should be, clicking on the blue gear icon "More", and selecting "Set As Working
Directory". Alternatively, you can use `setwd("/path/to/working/directory")` to
reset your working directory. However, your scripts should not include this line,
because it will fail on someone else's computer.

  
### The RStudio Interface  
Finally, let's take a quick tour of RStudio.

In the panel called Console you can play around with what R can do. One of the most important things to do first is to install packages which will be usefull and/or necessary for solving the problems in this course and beyond. 

To install your first package, paste this into the console field specified on the screnshot:
```
install.packages("dplyr")
```
Once the package has been installed, to use it, we will need to load it to use it 
```
library(dplyr)
```

In any learning any programming language your best weapon is __documentation__. In R studio you can view it directly in the help window in the lower left corner. To call the documentation and find out more about a function or package simply add a question mark before the name and run it. For example, one of the two functions we've used so far is `library`, let's find out more about it. 

```
?library
```
![](images/RStudioIntro2.png)

The help box reveals all the useful information about the function, such as the description, the arguments the function takes, examples of use, related functions and the package the funtion comes from. In the case of `library`, the package is `{base}`, a related function is `require`, the description yields that the function 'loads and attaches packages', the arguments include for example `quietly`, which is a boolean that controls whether library prints out that the attaching of the package was successfull or not. Under 'Usage', you can also see the what are the default values of the arguments, for example, see that `quietly` has a default of `FALSE` so `library` will print to alert you of a successful attachment, unless you explicitly tell it not to: 
```
library(stargazer, quietly = TRUE) # I attach stargazer which is a package for latex tables
```  

If you prefer to learn through videos, here is a comprehensive intro covering the bases by "How to R". It explains the interface of RStudio and the basic capabilities and is very accessible.

<iframe width="560" height="315" src="https://www.youtube.com/embed/lVKMsaWju8w" frameborder="0"  allowfullscreen ></iframe>
  
------

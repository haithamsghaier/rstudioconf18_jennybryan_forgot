Notes - Rstudio::conf 2018
================
Jessica Minnier
2/2/2018

<!------ Thanks for any contributions! Please edit the .Rmd, the .md is a byproduct of the .Rmd! --------->
Keynote - Di Cook - [slides](http://www.dicook.org/files/rstudio/)
==================================================================

-   "statistics starts once you have tidy data"
-   ggplot2 makes plots a type of statistic (function of the data)
-   compare data plots with null plots (i.e. `infer` package!)
-   people make inference based on plots without a firm foundation
-   roschach protocol: can you pick the real data from a field of null plots?
-   cookie monster teaches us about graphical inference: [video](https://youtu.be/rEHKm3Z1zUE)
-   `library(nullabor)` to calculate p-value = probability the data plot is "extreme", as well as power calculations
-   hypothesis for data plots comes from the type of plot being used, adding data to plot makes it a test statistic
-   hypotheses never come from looking at the data first!! only from underlying question
-   visual inference protocol gives some "teeth" to discoveries, quantify significance
-   if structure is not visible, prevent conclusions from small differences compared to differences seen in null data
-   apophenia = imagining things in plots like looking at the clouds for animals (avoid this with visual inference protocol)
-   we need a protocol for interactive graphics, how do we incorporate null plots to avoid incorrect inference?
-   variability in people's ability to detect visual differences---maybe there are super visual detectors

Shiny
=====

Joe Cheng - Scaling Shiny apps with async programming
-----------------------------------------------------

-   async lets shiny perform long-running tasks asynchronously, start task but don't wait around for result
-   `library(future)` wrap `future()` around code runs it in separate R process, freeing up original R process
-   `library(promises)` lets you access results from async tasks
-   these are not shiny-specific
-   need to chain an operation using `%...>%` onto a promise = promise pipe
-   **use promise pipe to "push subsequent computations into the future"**, delayed pipe, avoid waiting on other tasks
-   hooray nobody has to wait for serial execution
-   `install_github("rstudio/shiny@async")` - plz help testing!

Winston Chang - Developing robust shiny apps with regression testing
--------------------------------------------------------------------

-   manual testing takes a lot of time, is inconsistent, often forgotten, while automated testing is really hard due to the web browser, simulating interactions, graphical elements testing
-   `library(shinytest)` to the rescue
-   snapshot testing lets you "snapshot" states and save the script for testing
-   snapshot makes json if inputs, outputs, exported values; also screenshot
-   compare current states vs expected states = **shows a kind of diff viewer of the json**, as well as two screenshots with diffs highlighted (also slider), just like github!
-   basically any upgrading can cause issues, so run tests often!
-   limitations: recorder won't capture htmlwidgets (ie plotly or DT or leaflet), works best with standard shiny inputs and outputs; also, dynamic external data source may be harder to test

Alan Dipert - Make shiny fast by doing as little work as possible
-----------------------------------------------------------------

-   optimization loop method = benchmark -&gt; analyze -&gt; recommend (estimation of work/time needed) -&gt; optimize
-   analysis = identify the one slowest thing, optimize that first! use `profvis` (to view `Rprof` output)
-   beware `group_by()` since it can make other functions like `filter()` slower, since it has to iterate over the groups
-   plot caching is coming soon to shiny! `plotCache`
-   readRDS compresses by default so set `compress=FALSE` to make it faster

Sean Lopp - Scaling Shiny
-------------------------

-   load testing for shiny using existing tools turned out to be difficult due to "shiny magic"
-   Rstudio is working on tools to simulate heavy use of an app with a "real" load test -- development in progress!
-   discusses how to simulate load tests of many users, shows results on a dashboard
-   doing a live load test of 10,000 users (20 note cluster of c4.2xlarge 8 cores) --&gt; metrics dashboard shows \# visitors = 10k, and app laods and runs well! (impressive with this internet!)

Tidyverse
=========

Carson Sievert - Creating interactive web graphics suitable for exploratory data analysis - [slides](https://talks.cpsievert.me/20180202/)
------------------------------------------------------------------------------------------------------------------------------------------

-   plotly + crosstalk for interactive graphics that interact with each other
-   interactivity is only useful when we can iterate easily
-   "Worried about inference? See visual (Majumder et al 2013) and post-selection (Berk et al 2013) inference frameworks."
-   interactivity can augment exploration
-   use `highlight()` around `ggplotly` to highlight certain eleemnts/lines/shapes
-   these are standalone html that do not need web server, but can also work with shiny
-   if you have a lot of panels look into trelliscopejs which works with plotly
-   plotly.js handles the summarization/aggregation of data
-   example of using dendogram clustering to group data on other plots

Emily Riederer - tidycf: Turning analysis on its head by turning cashflows on their sides
-----------------------------------------------------------------------------------------

-   `library(tidycf)` package for tidying up cashflow data in business
-   tidy cashflows to streamline workflow to allow advanced analytics like bootstrapping error bars
-   "very opinionated R package"
-   empathy + empowerment + engagement = values learned from tidyverse and used in their design
-   empathy thinks about the user's experience
-   important to engage users with possiblity to contribute!

Max Kuhn - Modeling in the tidyverse
------------------------------------

-   goals of tidy modeling: promote tenets of the tidyverse (see manifesto), encourage empriical validation and good methodology, smooth out diverse interfaces, enable wider variety of methodologies
-   embrace resampling! protects against poor methodology
-   comparing models: use relevent loss functions, don't rely on p-values
-   basically thinking of loss functions as effect sizes
-   bayesian ROPE estimates = assess practical differences
-   you get more honest estimates from CV empirical estimates of prediction, than p-values from model output
-   smooth out diverse interfaces: can fit multiple kinds of models with one interface, not thinking about all the different packages you need
-   `library(parsnip)` for generic model specification, declaration of varaibles with `recipes`
-   caret might be the "least tidy" package on cran since so many options

Case Study
==========

JD Long - The unreasonable effectiveness of empathy
---------------------------------------------------

-   talking about "deep neural networks" inside of our head!
-   we each have an internal algorithm that gathers info and trains a model
-   children under 4 can't separate what they know vs what other people know
-   we don't remember learning empathy but we all did at some point
-   agile programming is an example of hacking empty, force us to think about the user's perspective
-   **data = people** in many cases!
-   our story we present in aggregate (i.e. maps, summary plots) is not the story of individuals, i.e. if we show a picture of a person or a plot showing individual lives **we evoke empathy, and the story is more real**
-   your business data is somebody's paycheck!
-   we are hardwired to have empathy in stories that are verbal narratives - Steven Jay Gould
-   other examples of "empathy hacks" -- i.e. ann taylor develops products for an imaginary ann taylor (with a house, dog, etc all described)
-   limitations: empathy doesn't work for "victimless" problems, may be biased toward certain groups

Julia Silge - Understanding PCA using Shiny and Stack Overflow data - [slides](https://speakerdeck.com/juliasilge/understanding-principal-component-analysis-using-stack-overflow-data)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   galaxy spectra PCA! you can actually learn stuff from it! but this is not our usual experience with PCA
-   now PCA for tags on stack overflow, PCA can look at many tags = "high dimensional data"
-   super high dimensional space made of up axes from all the tags. julia is somewher ein that space adn the people who are close to her in that space have similar search interests/tags.
-   `library(irlba)` has `prcomp_irlba()` that can deal with sparse matrices (made by `tidytext::cast_sparse()`!)
-   [machine learning flashcards!](https://machinelearningflashcards.com/)
-   can plot relative importance of each tags in each principle component, neat shiny app to do so! ([not public](https://twitter.com/juliasilge/status/959591825962016768), though, maybe soon)

Sandra Griffith - Accelerating cancer research with R
-----------------------------------------------------

-   deciding what program to use as a team, SAS vs R? need to cultivate the R culture
-   one reason to use R is the swag =) SAS lacking
-   rec for building up an R team: airbnb data science's "[using R packages and education to scale data science at airbnb](https://medium.com/airbnb-engineering/using-r-packages-and-education-to-scale-data-science-at-airbnb-906faa58e12d)" blog post
-   goal: grow an internal R package: `library(flatiron)` with database functions, internal identifier mapping, custom kaplan-meier surv curves (highchartR)
-   table 1: `library(compareGroups)`

Herman Sontrop - Developing and deploying large scale shiny applications
------------------------------------------------------------------------

-   use modules to self contain plots or interactivity, each serve a special purpose
-   HTMLtemplates for better CSS/html templates
-   use docker and octopus deploy for deployment

Packages
========

Giora Simchoni - Five packages in five weeks - from boredom to contribution via blogging [slides](http://giorasimchoni.com/rstudio_conf_2018_talk.html)
-------------------------------------------------------------------------------------------------------------------------------------------------------

-   Giora is not into DNA microarrays, but he IS into Beyonce
-   you need to start a blog! use blogdown
-   lots of great blog posts, including [ave mariah](http://giorasimchoni.com/2017/12/10/2017-12-10-ave-mariah/) = vocal range of pop singers
-   [gsimchoni/yrbss](https://github.com/gsimchoni/yrbss) no one should go through the pain Giora went through to get data from some crazy API with complex survey data, so he made a package (using H Parker, K Brobam, H Wickham, and Stack Overflow info)
-   `kandinsky` package: convert data frame to kandinsky-like painting
-   `mocap` package: helps interfacing with motion picture files, used for brain research!
-   `CastleOfR`: excercises with R as a game, escape from the castle
-   `songsim`: visualize song lyrics
-   `ssdkeras` (complicated), `ebayr` (search ebay), `ggwithimages` - make `geom_` to add images
-   the community needs YOU!

Jim Hester - You can make a package in 20 minutes [slides](http://rstd.io/rpkgs2018)
------------------------------------------------------------------------------------

-   packages can be for distribution but also just for *you*
-   if you can write an R function you can make a package
-   rOpenSci onboarding documentation has great package writing advice
-   slides have many good package writing refs
-   useful packages for development: devtools/roxygen2, usethis
-   demonstrates how to develop package from R script, live debugging and documenting
-   package plays notes and makes songs!
-   `usethis::use_package("audio")` adds packages to imports
-   make the readme very informative!!
-   `usethis::use_test()` creates all testing infrastructure and opens new test file
-   `covr::report()` shows the coverage of your tests

Joseph Rickert - What makes a great R package? [github](https://github.com/joseph-rickert/Rstudio-conf_2018)
------------------------------------------------------------------------------------------------------------

-   CRAN task views are valuable
-   packages can be public, personal, or professional
-   great R packages do something beautiful
-   J Rickert's "Hall of fame packages", these packages greatly contribute to R, statistics, computing, display of information, machine learning, reproducibility
-   they also have high reverse depends (\# of packages depending on the package), with multiple authors/collaborative

Mara Averick - Contributing to the tidyverse
--------------------------------------------

-   [sustainoss.org](https://sustainoss.org/): sustain open source software, bring together sustainers who make sure systems are healthy, don't have maintainer burnout
-   you don't need to write a package to contribute
-   contribute to what you use in your everday life
-   "the thoughtful user" - ask questions! on twitter, Rstudio community (figure out if you need to go to stackoverflow or go to issues on github), StackOverflow
-   thiago macieira 2012, "the art of problem solving" in [open advice: FOSS What we wish we knew when we started](http://open-advice.org/), edited by Lydia Pintscher
-   knowledge you need: know where the source is, i.e. .Rmd file not the html
-   learn from the source code, tidyverse source code is well written
-   contribute documentation! impossible (hard) for expert to reachieve beginner's mind
-   propose vignettes for specific use case
-   read tidyverse style guide
-   magic of `reprex`! making it easier to do is huge, help others learn to use it

Teaching
========

Ramnath Vaidyanathan - Data-driven curriculum development
---------------------------------------------------------

-   can we determine what causes students to abandon an exercise and learn from this? important predictors: instruction/course length, also program prerequisites completed
-   need content guidelines, provide instructor metrics

Colin Rundel - Kaggle in the classroom: using R and GitHub to run predictive modeling competitions [slides](https://github.com/rundel/Presentations/tree/master/RStudioConf2018)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   statistical computing for undergrads vs masters
-   use Rstudio server in the department, makes it much easier to distribute data and troubleshoot (not everyone is running things on their own computer)
-   core focus on reproducbility -- must be done on R markdown, turned in by github, one organization per class, one repo/team/assigment
-   continuous integration tools: automated grading framework, but this doens't work for larger open ended data analysis projects; use Wercker (less popular in Travis)
-   [rundel/ghclass](https://github.com/rundel/ghclass) - interfaces with day to day mechanical things in github, interfaces with `gh` package
-   use automatic testing -- check .Rmd runs, check they have the correct set of files at the end
-   `styler` or `lintr` can automatically clean up issues like indentations, automatic pull requests for formatting code
-   use rocker/tidyverse within wercker; as opposed to travis which outputs a long text file which are intimidating, wercker gives more friendly output, R scripts, etc
-   goal: automatic scoring for cleaning, modeling; diagnostics and useful feedback
-   leverage competitiveness = leaderboard
-   NYC open data - parking violations, super messy! good learning material; spacial data
-   use `plumber` package to make an API that interacts with wercker and SQLite backend

Chester Ismay - Something old, something new, something borrowed, something blue: Ways to teach data science (and learn it too!) [slides](http://bit.ly/rstudioconf18)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   introduce data and computation novices to data science, data modeling, statistical inference
-   freely available online textbook moderndive.com
-   teach ggplot2 on the chalkboard!
-   use tactile simulation to teach sampling distributions
-   don't just show code immediately, need tactile examples to show where it comes from (i.e. red and white balls in a bowl); bowl not an urn! \#statsjokes
-   datacamp gives you immediate feedback, interactive courses
-   moderndive uses bookdown, github, netlify
-   be nice to beginners!!

Marco Blume - Training an army of new data scientists \[given without slides\]
------------------------------------------------------------------------------

-   teach everyone in a company how to basic R, no more excel; including executives and database managers
-   teach only the very basics -- just a few functions, dplyr only (no tidyr or purrr or anything fancy)
-   no base r, base r doesn't exist, base r is for experts!!
-   teach ggplot but only basics, ggplot+geom, nothing fancy, if you want to be fancy use `facet_wrap`
-   emphasis on tidy data, subject matter experts now rejecting any data that is not tidy
-   R is like "ExcelPlus"
-   datacamp is key, people have to learn on their own
-   admin data in datacamp is brutal, so they wrote their own package to export it in tidy format
-   use tidyverse for people who have not done any data science, sell it as power excel, once they produce a markdown let them stop and teach them further only if they want to go further

Random observations
===================

-   Yihui's [xaringen package](https://github.com/yihui/xaringan) for slides, lots of examples at the talks and they all look awesome!
-   debugging in Rstudio
    -   [more info](https://support.rstudio.com/hc/en-us/articles/205612627-Debugging-with-RStudio?mobile_site=true)
    -   `debugonce()`: "use debugonce to stop in function; Use debug menu in Rstudio IDE, go to “on error” and set to “break in code"" \[from @robinson\_es tweet\](<https://twitter.com/robinson_es/status/959595036592635904>)
-   [coatless/errorist](https://github.com/coatless/errorist/blob/master/README.md): package can automatically search errors

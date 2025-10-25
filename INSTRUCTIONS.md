# Project FuelWatch


- [1. Data / Domain Knowledge](#1-data--domain-knowledge)
- [2. Overview Regions, Areas, and
  Brands](#2-overview-regions-areas-and-brands)
- [3. Price trends](#3-price-trends)
- [4. Characterization Price
  Dynamics](#4-characterization-price-dynamics)
- [5. Clustering](#5-clustering)
- [6. Potential explanations](#6-potential-explanations)
- [Build an extended data science project from
  here?](#build-an-extended-data-science-project-from-here)

In this homework you will explore **fuel prices over time in Western
Australia**.

Learning goals:

- **Data Downloading**: Learn to automate data collection from online
  sources while using scripts.
- **Data Preparation and .gitignore**: Combine multiple datasets into a
  single usable dataset. Learn that some files need not be in the GitHub
  repository but only on your local machine. Learn how you can make
  these vanish when you are in the *Source Control* tab.
- **Data Visualization**: Create clear visualizations on your own
  without the help of code snippets to explore fuel price trends by
  brand, region, and petrol station. Analyze patterns in fuel prices
  over time and across different regions.
- **Cluster Analysis:** Make a clustering using k-means clustering and
  analyze it.
- **Thinking about reasons for patterns**

There is a starter document `report.qmd` in the repository. This is for
your report. Preview it in HTML repeatedly, check that it looks good,
and commit intermediate versions to receive help and feedback.

The starter document includes some basic structure with YAML, headlines,
a bit of text, and one initial chunk, but you need to add your own code
cells independently: Instead of typing ```` ```{r} ```` yourself, learn
using the *Show All Commands* Command (Ctrl+Shift+P). In the box type
what you want, here “Code Cell” and see if you find the thing you need,
here “Quarto: Insert Code Cell”, and type enter. By this you can also
learn more keyboard short cuts, like Ctrl+Shift+I for the direct
insertion of a code cell.

Follow the steps below to complete the analysis. Do not forget to write
short introductory and concluding texts for all steps of analysis.

## 1. Data / Domain Knowledge

[Fuel Watch](https://www.fuelwatch.wa.gov.au) is a fuel monitoring
service created by the Government of Western Australia. It provides
information about fuel prices at the various petrol stations in Western
Australia. Historical fuel prices are provided in monthly chunks on the
[website](https://www.fuelwatch.wa.gov.au/retail/historic). On this site
you find links to csv-files with the data. Hint: Use the right-click
menu to copy the link address. That way you can find out that data files
are stored at URLs of the following form
`https://warsydprdstafuelwatch.blob.core.windows.net/historical-reports/FuelWatchRetail-MM-YYYY.csv`.

Part of the exercise is the systematic download of the data! Read
[Learning: Project workflow for data retrieval and
preparation](LEARNING.md#project-workflow-for-data-retrieval-and-preparation)
to get a taste of what common larger tasks in Data Science Projects you
can learn in this Homework Project.

1.  **Data Download:** You find the file `data_download.R`. This is an
    R-script loading the packages `tidyverse`. Write a script here which
    downloads the twelve recent monthly data from
    `FuelWatchRetail-MM-YYYY.csv` (October last year), …,
    `FuelWatchRetail-MM-YYYY.csv` (September this year). To that end, do
    the following in the script:
    - Create a date vector `dates` with the twelve dates YYYY-10-01 to
      YYY-09-01 (Replace YYYY by this year). Reformat it to the strings
      as needed for the URLs “MM-YYYY”. Read [Learning: Date-formats and
      date formatting as
      strings](LEARNING.md#date-formats-and-date-formatting-as-strings)
    - Create a string vector `urls` with twelve URLs in the form above.
      Read [Learning: Glue strings using
      variables](LEARNING.md#glue-strings-using-variables)
    - Create a string vector `filenames` with twelve file names in the
      form `FuelWatchRetail-MM-YYYY.csv`
    - Now, you could download the first file by calling
      `download.file(url = urls[1], destfile = filenames[1])`. However,
      do not do this twelve times! Instead: Write a loop around that
      function call like `for(i in 1:length(urls)) { ... }`.
    - Save the script and run it once. Check that all twelve files are
      in your project folder.

    So, you have a download script. A big advantage is: Now you can
    easily extend it for downloading data for more years!
2.  \*\*Update .gitignore*.* Goal: All the downloaded files should NOT
    be tracked by `git` and should not appear on GitHub! To that end you
    can use the file `.gitignore` in this repository (it should be
    already there, if not create it). `git` will ignore all file names
    listed here (each file in one line). Add all the file names here.
    Tip: You can use `*.csv` where `*` is a wildcard for any combination
    of letters. So, it excludes all CSV-files. In Positron: Check the
    Source Control Tap and observe that the files vanish in the Git tab
    after refreshing. You cannot add them anymore.
3.  **Data import.** In your report (`report.qmd`), in the code cell
    `packages-data`, load the tidyverse and import the data of all
    twelve files into one dataframe `data`. You can use the vectorized
    capabilities of `read_csv`! That means you can put in a vector of
    file names into its `file =` argument. To that end copy the code to
    create the filename-vector from the download script. Test your
    read-data code line and look at the column specification of
    `read_csv`. Do some pre-processing directly after importing:
    1.  *Remove Columns:* Do you see a variable `...11`? Check if it has
        useful information. If not remove it directly after reading by
        adding `` |> select(-`...11`) ``.
    2.  *Filter Rows:* In this Homework we only use the fuel product
        *ULP* (unleaded petrol). So you can filter `PROCUDT_DESCRIPTION`
        for `ULP` in the data import pipe. (You can include all other
        products if you want to investigate it further in your own
        project.)
    3.  *Transform the date column to date format:* Look at the column
        `PUBLISH_DATE`. What is its `class()`? Make it of class `Date`.
        Use `mutate()` and create a new variable `DATE` in date format.
        For creating dates from strings, use the appropriate function
        from the package
        [`lubridate`](https://lubridate.tidyverse.org/). Look at the
        References on the
        [`lubridate`](https://lubridate.tidyverse.org/)-website and find
        the appropriate one from the `ymd` family of functions.
    4.  *Renaming columns:* Shorten variable names using `rename`. In
        the following, it is assumed that you have the variables
        `REGION`, `AREA`, `BRAND`, `PRICE`

    You may return to this chunk to add further preprocessing and
    feature engineering steps to the pipe.
4.  **Gather some domain knowledge**. In the HTML you should see an
    image of the map of Western Australia. If not it is maybe because
    you are not connected to the internet. The image is referenced in
    the document as a URL and downloaded while rendering. Study the map
    and regions of Western Australia and find out with internet research
    how many people live in Western Australia and where the majority of
    the population lives. Write a short paragraph about this under the
    map.

## 2. Overview Regions, Areas, and Brands

Now, produce two visualizations, each visual in a different chunk.
Create human readable labels, choose good colors for categorical
variables and write 1-2 sentences before the chunk about what will be
shown, and 1-2 sentences after the chunk what is the most important
insight from the visualization.

1.  Visualize the number of data points in the **regions.** Make a bar
    chart. Put the labels on the y-axis and order by the number of data
    points starting with the largest region. Remember the
    [`forcats`](https://forcats.tidyverse.org) package and the function
    `fct_infreq` and `fct_rev`.
2.  Do a similar bar chart for the **brands**. Now, additionally color
    by **regions** such that a stacked bar chart is created (use the
    `fill` aesthetic). Learn positioning of the legend: Put the legend
    to the bottom of the visualization and bring the legend title
    position to the top of the legend (instead of the default left
    side).

In the concluding sentences describe where the largest number of data
points lie, which the two largest brands are and, answer the question if
brands tend to focus on different regions or mostly competing in mot
regions.

## 3. Price trends

1.  Start the section by writing a sentence which describes the overall
    average price.
2.  Make three line plots in three different chunks:
    1.  Summarize a variable `AVG_PRICE` over all `DATE`s and plot the
        **average price over the twelve month**. Describe what pattern
        you see! Distinguish between long term changes and regular
        short-term oscillations. When there are regular oscillation,
        what is the time interval of one cycle in the oscillation?
    2.  Do the the same computation of `AVG_PRICE` but aggregating over
        the seven **weekdays** only. To that end, return to the
        pre-processing pipe in the first chunk and add a `mutate` to
        create the new feature `WEEKDAY` (use the `lubridate` function
        `wday()` with `label = TRUE`. Note: When you want to do a
        `geom_line` plot with the new `WEEKDAY` variable you may need to
        specify the aesthetic `group = 1`, otherwise ggplot will
        consider each weekday as one group and not connect them.
        Describe what the most expensive and what the cheapest day is.
    3.  Go back to `AVG_PRICE` per `DATE` but aggregate also per
        `REGION`. Then **facetted the plot by region** such that you
        have ten subplots in two columns. Describe in which regions
        there are oscillations and how this is related to the
        metropolitan area in Western Australia.

## 4. Characterization Price Dynamics

In the following you shall characterization different types of price
dynamics by the mean and standard deviation of the daily prices over the
twelve months. To that end, only petrol stations should be considered
which have prices at all days.

1.  At the beginning of the section: Write a short **description about
    the selection of data petrol stations and the new features** used in
    this sections (see above). Note: Better write it after doing the
    next tasks in this section.
2.  Return to the **per-processing pipe** in the first chunks and add a
    new `mutate` line for three new features:
    1.  `N_OBS_TRADING_NAME` which counts the number of observations for
        each `TRADING_NAME` (marking one petrol station). Use the
        function `n()` to count and `.by = c(TRADING_NAME, BRAND)` to
        group by petrol station under the same brand.  
        **Side quest** (no need to write about it in the report): Think
        about grouping by `TRADING_NAME` only and by `TRADING_NAME` and
        `BRAND`. Can it be different? Can a petrol station be run by
        different brands? What does it mean? Are there cases in the
        data?
    2.  `MEAN_PRIZE` which computes the mean prize of one petrol station
        over all days. Create this column in the same `mutate` command
        because it uses the same `.by` grouping.
    3.  `SD_PRIZE` which computes the standard deviation of the prize of
        one petrol station over all days. Create this column in the same
        `mutate` command.
3.  Make three visualization each in a new chunk.
    1.  First create a new dataframe `data_stations` based on `data` in
        which you filter only data for those petrol station
        (`TRADING_NAME`) which have observations for all days, select
        the variables `REGION`, `TRADING_NAME`, `MEAN_PRICE`,
        `SD_PRICE`, and `BRAND`, and finally reduce the size by using
        `distinct()`. The latter will reduce the data frame to distinct
        rows and because all selected variables are the same for each
        `DATE` only one observation per station remains.  
        Then make a new line to make a ggplot based on `data_stations`
        which shows a **histogram** of `SD_PRICE`. Select a good
        bandwidth (`bw`). Describe the histogram. You can interpret it
        as a [bimodal
        distribution](https://en.wikipedia.org/w/index.php?title=Bimodal_distribution&redirect=yes).
        Where are the two typical values of standard deviations for the
        two modes? Why do we see bimodality and not a “normal” bell
        shape? Can it be related to the characterization of price trend
        patterns? Think about a time series with weekly oscillation, a
        time series with a smoother shape and a constant price time
        series. Which has the largest standard deviationss, which the
        smnallest?
    2.  Make a **scatter plot of stations** from `data_stations` where
        you put `MEAN_PRICE` on the x-axis, `SD_PRICE` on the y-axis and
        color by `REGION`. Below the plot answer four questions. To that
        end, create another dataframe `data_stations_metro` where you
        filter from `data_stations` only the stations with are in the
        region “Metro”. Make an unordered list (see Markdown Quick
        Reference) that answer these for questions.
        1.  In which regions lie the petrol stations with high standard
            deviation?
        2.  In which regions are the highest prices? What characterizes
            that region with respect to the metropolitan area?
        3.  What is the correlation of mean and standard deviation over
            all stations? Use an inline-citation
            `` `r cor(data_stations$MEAN_PRICE, data_stations$SD_PRICE)` ``.
            Interpret the correlation and provide a potential
            explanation.
        4.  What is the correlation of mean and standard deviation over
            the stations in the “Metro” region? Use an inline-citation
            using `data_station_metro`. Interpret the correlation and
            provide a potential explanation.
    3.  Make a **scatter plot of brands** with respect to the average of
        their petrol stations’ mean and standard deviations of prices.
        To that end, start with `data_stations` and `summarize`
        `.by = BRAND` and create the variables `AVG_MEAN_PRICE` and
        `AVG_SD_PRICE` by computing the mean and `N` using `n()` to
        count the petrol stations per brand. Then pipe this into a
        `ggplot` using `AVG_MEAN_PRICE` and `AVG_SD_PRICE` for `x` and
        `y` and `N` as `size`. For `geom_point` use additionally
        `alpha = 0.3` for the cases of over-plotting. Also add a
        `geom_text` with `label = BRAND` to show the brand names in the
        plot. For scaling use `scale_size_area` and put `max_size = 10`
        and then play with the value such that the size differences can
        be well seen.  
        Below the plot answer the following question: What do you think?
        Do larger brands or smaller brands drive the oscillations?

## 5. Clustering

In this section, you will compute a clustering of the petrol stations
for which data exists for 366 days under the same brand. That means you
compute a new feature with cluster labels indicating to which cluster of
trend patterns the petrol station belongs. Use `kmeans`-clustering and
explore what a good number of $k$ centers. Then you plot the average
price trend for each cluster and make two tables counting the how many
petrol stations of each cluster are in each region (Table 1) and under
each brand (Table 2).

1.  Write a **description about the clustering** you performed, how
    stable different clustering runs were for different $k$s and why you
    selected your final $k$. Note: Better write this paragraph after
    doing all the other tasks in this section!
2.  Make a chunk where you create two dataframes, a `kmeans` object, and
    make one **visualization of the clustering**:
    1.  Create a *dataframe* `data_stations_days_as_features` from
        `data`, where you filter those stations which have 366
        observations under the same brand (use your already computed
        feature `N_OBS_PER_TRADING_NAME`), then select `TRADING_NAME`,
        `REGION`, `BRAND`, `MEAN_PRICE`, `SD_PRICE`, `DATE`, and
        `PRICE`, and then use `pivot_wider` to create a feature for each
        of the 366 days, by taking the column names from `DATE` and the
        values from `PRICE`. (Check that you new data frame has the same
        number of rows than `data_stations`.)
    2.  Create the *`kmeans` object*. Create `kmeans_stations` by
        starting with `data_stations_days_as_features`, then select only
        the 366 day features and pipe this dataframe into the base R
        command `kmeans` specifying $k$ as `centers = 3`. (Later, you
        will explore different values for $k$ and decide for one).  
        **Sidequest:** You now already how to explore complex objects
        from model output. Explore the object `kmeans_stations` and
        learn what the sub-elements are.
    3.  Create a *dataframe* `data_stations_cluster` starting from
        `data_stations_days_as_features`, selecting only `TRADING_NAME`,
        `REGION`, `BRAND`, `MEAN_PRICE`, `SD_PRICE`, `DATE`, and `PRICE`
        (and not the day features) and add a new columns with the
        cluster labels by adding
        `|> mutate(CLUSTER = factor(kmeans_stations$cluster))` at the
        end of the pipe. (Note, use `factor` such that the cluster
        labels are not treated as continuous variables when you map the
        two a color aesthetic. )
    4.  Use data `data_stations_cluster` to make a *scatter plot*
        coloring by `CLUSTER` using the already use variables
        `MEAN_PRICE` and `SD_PRICE` as `x` and `y`.
3.  In a new chunk, make a visualization of the average prize over the
    twelve month grouped by `CLUSTER`. This shall show the price
    dynamics the different clusters represent. To that end you need to
    append the `CLUSTER` labels to the original dataframe `data`. Start
    your visualization pipe with `data`, filter for those stations with
    366 days under the same brand, `left_join` `data_stations_clusters`
    and then summarize `AVG_PRICE` as the mean price grouping by
    `CLUSTER` and `DATE`. Pipe this into a `ggplot` for the typical line
    plot with `color = CLUSTER`.
4.  **Exploration of different number of clusters** $k$ by running the
    above several times for the values $k = 2,3,4,5$ and observing how
    the clusters look in the two visualizations above. Observe how
    different the clusterings turn out in different runs for different
    $k$. Important: A color change because of different numerical values
    is not a meaningful difference when the groups are the same!  
    Decide for one value of $k$ which gives you most stable and most
    meaningful (looking at the trend pattern) clusters. Go back to
    task 1. in this section and describe your insights from exploring
    different $k$, which $k$ you select and why.  
    Once $k$ is fixed go to the chunk where you compute the `kmeans`
    object and add `set.seed(0)` as the first line. This fixes all
    random number for the computation. That means, when ever you render,
    the outcome will be the same. Test a few (arbitrary) numbers as
    seeds instead of 0, and keep one for which the trend patterns are
    most meaningfully different.  
5.  Write a numbered list with as many items as you have clusters and
    briefly **describe the trend pattern of each cluster** with respect
    to the average price level, the amount of oscillations (regular
    patterns) and fluctuations (general longer term changes).
6.  Create two *great tables* using the [`gt`
    package](https://gt.rstudio.com).  
    (Probably, you need to install it first. Then add `library(gt)` in
    your very first chunk where you also load the tidyverse.)
    1.  The first table shall report the number of petrol stations with
        the **regions in rows and a column for each cluster**. Create a
        pipe ending with the creating of a `gt` object with colored
        cells. Start the pipe with `data_stations_cluster` and `count`
        by `CLUSTER` and `REGION`. Then continue with reshaping the
        dataframe such that a columns is created for each cluster (use
        `pivot_longer` taking names from `CLUSTER` and values from `n`,
        as produced by `count`). Then pipe this into

    ``` r
    gt() |> 
    data_color(-REGION, palette = "Oranges", direction = "columns")
    ```

    and test how it looks. In the `direction` argument you can replace
    `"column"` by `"row"`. Test both and understand how they they color
    the cells. The purpose of the table should be to show which cluster
    is dominating in each region. Select `"row"` or `"column"` such that
    this purpose it best fulfilled.
    2.  The second table should be similar to the first, just for brands
        instead of regions.

## 6. Potential explanations

Write a paragraph where you provide some potential explanations for the
different trend patterns. Note that there is no price regulation in
Western Australia. So, there is always price competition because
customers can fill up where they want. The government provides the
portal FuelWatch for price transparency. So, customers as well as
stations and companies can monitor price trends easily.

Treat these questions:

- Why do we have generally higher and lower prices somewhere?
- Why do the oscillations happen?
- Where do oscillation happen? Why there?
- Do stations with oscillations of prices realize average higher prices
  in their region? If yes, do you think they also get higher earnings
  compared to stations without oscillations? Can we answer this question
  from the data we have? If not, what would we need to know to answer
  it.

## Build an extended data science project from here?

These are some ideas, they are not part of the Homework Project.

- Extend the analysis to more years. FuelWatch was established 2001!
  - Are the oscillations there from the start?
  - Do they spread regionally?
  - Was it always the some day when prices jumped up?
- Track stations over time.
  - Do stations change the trend patterns?
  - Probably some stations drop out and new appear or change brand. Is
    this related trend patterns?
- Improve the classification of trend patterns. Up to now the clustering
  does not take into account if days are close to each other. Create new
  features, find a direct way to classify different patterns, or test
  different unsupervised clustering methods.
- Relate the long-term changes to prices for crude oil. To that end,
  find new data and check out where Western Australia imports fuel from.

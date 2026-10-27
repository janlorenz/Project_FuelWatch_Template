# Project FuelWatch


- [1. Data / Domain Knowledge](#1-data--domain-knowledge)
- [2. Overview Regions, Areas, and
  Brands](#2-overview-regions-areas-and-brands)
- [3. Price trends](#3-price-trends)
- [4. Characterization Price
  Dynamics](#4-characterization-price-dynamics)
- [5. Clustering](#5-clustering)
- [6. Potential explanations](#6-potential-explanations)
- [Possible extensions](#possible-extensions)

In this homework you will explore **fuel prices over time in Western
Australia**.

Learning goals:

- **Data Downloading**: Learn to automate data collection from online
  sources using scripts.
- **Data Preparation and .gitignore**: Combine multiple datasets into a
  single usable dataset. Learn that some files need not be in the GitHub
  repository but only on your local machine. Learn how you can make
  these vanish from the *Source Control* tab with `.gitignore`.
- **Data Visualization**: Create clear visualizations on your own
  without the help of code snippets to explore fuel price trends by
  brand, region, and petrol station. Analyze patterns in fuel prices
  over time and across different regions.
- **Cluster Analysis:** Make a clustering using k-means clustering and
  analyze it.
- **Think about reasons for the patterns** (see Section 6)

There is a starter document `report.qmd` in the repository. This is for
your report. Preview it in HTML repeatedly, check that it looks good,
and commit intermediate versions to receive help and feedback.

The starter document includes some basic structure with YAML, headlines,
a bit of text, and one initial cell. You need to add your own code
cells. Instead of typing ```` ```{r} ```` yourself, learn using the
*Show All Commands* command (Ctrl+Shift+P). In the box type what you
want, here “Code Cell” and see if you find the thing you need, here
“Quarto: Insert Code Cell”, and press Enter. By this you can also learn
more keyboard shortcuts, like Ctrl+Shift+I for the direct insertion of a
code cell.

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
to see which larger common tasks in data science projects you can learn
in this homework project.

1.  **Data Download:** You find the file `data_download.R`. This is an R
    script loading the packages `tidyverse`. Write a script here which
    downloads the recent monthly data for the last year from the URL
    form given above. The files are named `FuelWatchRetail-MM-YYYY.csv`,
    from October last year to September this year. To that end, do the
    following in the script:
    - Create a date vector `dates` with the dates from (YYYY-1)-10-01 to
      YYYY-09-01, where YYYY is this year. Reformat them as strings in
      the form “MM-YYYY” needed for the URLs. Read [Learning:
      Date-formats and date formatting as
      strings](LEARNING.md#date-formats-and-date-formatting-as-strings)
    - Create a string vector `urls` with twelve URLs in the form above.
      Read [Learning: Glue strings using
      variables](LEARNING.md#glue-strings-using-variables)
    - Create a string vector `filenames` with file names in the form
      `FuelWatchRetail-MM-YYYY.csv`
    - Now, you could download the first file by calling
      `download.file(url = urls[1], destfile = filenames[1])`. However,
      do not do this twelve times! Instead: Write a loop around that
      function call like `for(i in 1:length(urls)) { ... }`. (You can
      also use the function `purrr::walk()`)
    - Save the script and run it once. Check that all files are in your
      project folder.

    So, you have a download script. A big advantage is: Now you can
    easily extend it for downloading data for more years!
2.  **Update .gitignore.** Goal: All the downloaded files should NOT be
    tracked by `git` and should not appear on GitHub! To that end you
    can use the file `.gitignore` in this repository (it should be
    already there, if not create it). `git` will ignore all file names
    listed here (each file in one line). Add the names of all downloaded
    files here. Tip: You can use `*.csv` where `*` is a wildcard for any
    combination of characters. So, it excludes all CSV-files. In
    Positron: Check the Source Control tab and observe that the files
    vanish from it after refreshing. You cannot add them anymore unless
    you delete or comment the line in `.gitignore`.
3.  **Data import.** In your report (`report.qmd`), in the code cell
    `packages-data`, load the tidyverse and import the data of all
    twelve files into one dataframe `data`. You can use the vectorized
    capabilities of `read_csv`! That means you can put in a vector of
    file names into its `file =` argument. To that end, copy the code to
    create the filename-vector from the download script. Test your
    read-data code line and look at the column specification of
    `read_csv`. Do some pre-processing directly after importing:
    1.  *Filter Rows:* In this homework we only use the fuel product
        *ULP* (unleaded petrol). So you can filter `PRODUCT_DESCRIPTION`
        for `ULP` in the data import pipe. (You can include all other
        products if you want to investigate it further in your own
        project.)
    2.  *Transform the date column to date format:* Look at the column
        `PUBLISH_DATE`. What is its `class()`? Make it of class `Date`.
        Use `mutate()` and create a new variable `DATE` in date format.
        For creating dates from strings, use the appropriate function
        from the package
        [`lubridate`](https://lubridate.tidyverse.org/). Look at the
        reference page of the
        [`lubridate`](https://lubridate.tidyverse.org/)-website and find
        the appropriate function from the `ymd` family.
    3.  *Renaming columns:* Shorten variable names using `rename`. In
        the following, it is assumed that you have the variables `DATE`,
        `REGION`, `AREA`, `BRAND`, and `PRICE`

    You may return to this chunk to add further preprocessing and
    feature engineering steps to the pipe.
4.  **Gather some domain knowledge**. In the HTML you should see an
    image of the map of Western Australia. If not, it may be because you
    are not connected to the internet. The image is referenced in the
    document as a URL and downloaded while rendering. Study the map and
    regions of Western Australia and use internet research to find out
    how many people live in Western Australia and where the majority of
    the population lives. What regions are part of metropolitan areas?
    Write a short paragraph about this under the map.

## 2. Overview Regions, Areas, and Brands

Now, produce two visualizations, each in a separate chunk. Create human
readable labels, choose good colors for categorical variables and write
1-2 sentences before the chunk about what will be shown, and 1-2
sentences after the chunk stating the most important insight from the
visualization.

1.  Visualize the number of data points in the **regions.** Make a bar
    chart. Put the labels on the y-axis and order by the number of data
    points starting with the largest region. Remember the
    [`forcats`](https://forcats.tidyverse.org) package and the function
    `fct_infreq` and `fct_rev`.
2.  Do a similar bar chart for the **brands**. Now, additionally color
    by **regions** such that a stacked bar chart is created (use the
    `fill` aesthetic). Learn positioning of the legend: Put the legend
    at the bottom of the visualization and move the legend title above
    the legend (instead of the default left side).

In the concluding sentences describe where the largest number of data
points lie and which the two largest brands are. Answer whether brands
tend to focus on different regions or mostly compete in most regions.

## 3. Price trends

In this section, we speak of **oscillations** and **fluctuations**.
Oscillations are regular, up-and-down movements, and **fluctuations**
are changes without a regular pattern.

1.  Start the section by writing a sentence which describes the overall
    average price.
2.  Make three line plots in three different chunks:
    1.  Summarize `PRICE` as `AVG_PRICE` over all `DATE`s and plot the
        **average price over the months**. Describe the pattern you see.
        Distinguish between long-term changes and regular short-term
        oscillations. If there are regular oscillations, what is the
        time interval of one cycle?
    2.  Do the same computation of `AVG_PRICE` but aggregating over the
        seven **weekdays** only. To that end, return to the
        pre-processing pipe in the first chunk and add a `mutate` to
        create the new feature `WEEKDAY` (use the `lubridate` function
        `wday()` with `label = TRUE`). Note: When you want to do a
        `geom_line` plot with the new `WEEKDAY` variable you may need to
        specify the aesthetic `group = 1`, otherwise ggplot will
        consider each weekday as one group and not connect them.
        Describe which day is the most expensive and which is the
        cheapest.
    3.  Go back to `AVG_PRICE` per `DATE` but aggregate also per
        `REGION`. Then **facet the plot by region** such that you have
        ten subplots in two columns. Describe in which regions there are
        oscillations and how this is related to the metropolitan area in
        Western Australia.

## 4. Characterization Price Dynamics

In the following you shall characterize different types of price
dynamics by the mean and standard deviation of the daily prices over the
months. To that end, only petrol stations that have prices on all days
should be considered.

1.  Place it at the beginning of the section: write a short
    **description of the selection of petrol stations and the new
    features** used in this section (features are described below).
    Draft it after doing the next tasks in this section.
2.  Return to the **pre-processing pipe** in the first chunk and add a
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
    2.  `MEAN_PRICE` which computes the mean price of one petrol station
        over all days. Create this column in the same `mutate` command
        because it uses the same `.by` grouping.
    3.  `SD_PRICE` which computes the standard deviation of the price of
        one petrol station over all days. Create this column in the same
        `mutate` command.
3.  Make three visualizations, each in a new chunk.
    1.  First create a new dataframe `data_stations` based on `data` in
        which you filter only data for those petrol stations
        (`TRADING_NAME`) that have observations for all days, select the
        variables `REGION`, `TRADING_NAME`, `MEAN_PRICE`, `SD_PRICE`,
        and `BRAND`, and finally reduce the data frame with
        `distinct()`. This keeps only distinct rows: because all
        selected variables are the same for each `DATE` only one
        observation per station remains.  
        Then add a ggplot based on `data_stations` which shows a
        **histogram** of `SD_PRICE`. Choose a suitable bandwidth (`bw`).
        Describe the histogram. You can interpret it as a [bimodal
        distribution](https://en.wikipedia.org/w/index.php?title=Bimodal_distribution&redirect=yes).
        Where are the two typical values of standard deviations for the
        two modes? Why do we see bimodality and not a “normal” bell
        shape? Can it be related to the characterization of price trend
        patterns? Think about a time series with weekly oscillation, a
        time series with a smoother shape and a constant price time
        series. Which has the largest standard deviations and which has
        the smallest?
    2.  Make a **scatter plot of stations** from `data_stations` where
        you put `MEAN_PRICE` on the x-axis, `SD_PRICE` on the y-axis and
        color by `REGION`. Below the plot answer four questions. To that
        end, create another dataframe `data_stations_metro` where you
        filter from `data_stations` only the stations that are in the
        region “Metro”. Make an unordered list that answers these four
        questions.
        1.  In which regions lie the petrol stations with high standard
            deviation?
        2.  In which regions are the highest prices? What characterizes
            that region with respect to the metropolitan area?
        3.  What is the correlation of mean and standard deviation over
            all stations? Use inline R code
            `` `r cor(data_stations$MEAN_PRICE, data_stations$SD_PRICE)` ``.
            Interpret the correlation and provide a potential
            explanation.
        4.  What is the correlation of mean and standard deviation over
            the stations in the “Metro” region? Use inline R code using
            `data_stations_metro`. Interpret the correlation and provide
            a potential explanation.
    3.  Make a **scatter plot of brands** with respect to the average of
        their petrol stations’ mean and standard deviations of prices.
        To that end, start with `data_stations` and `summarize`
        `.by = BRAND` and create the variables `AVG_MEAN_PRICE` and
        `AVG_SD_PRICE` as the means of `MEAN_PRICE` and `SD_PRICE`; `N`
        counts the petrol stations per brand using `n()`. Then pipe this
        into a `ggplot` using `AVG_MEAN_PRICE` and `AVG_SD_PRICE` for
        `x` and `y` and `N` as `size`. For `geom_point` use additionally
        `alpha = 0.3` to handle over-plotting. Also add a `geom_text`
        with `label = BRAND` to show the brand names in the plot. Use
        `scale_size_area` with `max_size = 10` and adjust this value
        until differences in size are clearly visible.  
        Below the plot answer the following question: What do you think?
        Do larger or smaller brands drive the oscillations (i.e., show
        larger standard deviations of price)?

## 5. Clustering

In this section, you will compute a clustering of the petrol stations
for which data exists for all days under the same brand. That means you
compute a new feature with cluster labels indicating to which cluster of
trend patterns the petrol station belongs. Use `kmeans` clustering and
explore a good number of centers $k$. Then you plot the average price
trend for each cluster and make two tables counting how many petrol
stations of each cluster are in each region (Table 1) and under each
brand (Table 2).

1.  Write a **description about the clustering** you performed, how
    stable different clustering runs were for different values of $k$
    and why you selected your final $k$. Note: Better write this
    paragraph after doing all the other tasks in this section!
2.  Make a chunk where you create two dataframes, a `kmeans` object, and
    make one **visualization of the clustering**:
    1.  Create a *dataframe* `data_stations_days_as_features` from
        `data`, where you filter those stations which have observations
        for all days under the same brand (use your already computed
        feature `N_OBS_TRADING_NAME`), then select `TRADING_NAME`,
        `REGION`, `BRAND`, `MEAN_PRICE`, `SD_PRICE`, `DATE`, and
        `PRICE`, and then use `pivot_wider` to create a feature for each
        day, by taking the new column names from `DATE` and the values
        from `PRICE`. (Check that your new data frame has the same
        number of rows as `data_stations`.)
    2.  Create the *`kmeans` object*. Create `kmeans_stations` by
        starting with `data_stations_days_as_features`, then select only
        the day features and pipe this data frame into the base R
        command `kmeans` specifying $k$ as `centers = 3`. (Later, you
        will explore different values for $k$ and decide for one).  
        **Side quest:** You already know how to explore complex objects
        from model output. Explore the object `kmeans_stations` and
        learn what the sub-elements are.
    3.  Create a *dataframe* `data_stations_cluster` starting from
        `data_stations_days_as_features`, selecting only `TRADING_NAME`,
        `REGION`, `BRAND`, `MEAN_PRICE`, `SD_PRICE`, `DATE`, and `PRICE`
        (and not the day features) and add a new column with the cluster
        labels by adding
        `|> mutate(CLUSTER = factor(kmeans_stations$cluster))` at the
        end of the pipe. (Note, use `factor` such that the cluster
        labels are not treated as continuous variables when you map it
        to a color aesthetic.)
    4.  Use the data `data_stations_cluster` to make a *scatter plot*
        coloring by `CLUSTER` using the already used variables
        `MEAN_PRICE` and `SD_PRICE` as `x` and `y`.
3.  In a new chunk, make a visualization of the average price over the
    months grouped by `CLUSTER`. This shall show the price dynamics the
    different clusters represent. To that end you need to append the
    `CLUSTER` labels to the original dataframe `data`. Start your
    visualization pipe with `data`, filter for those stations with data
    for all days under the same brand, `left_join`
    `data_stations_cluster` by `TRADING_NAME`, `BRAND`, and `DATE`, and
    then summarize `AVG_PRICE` as the mean price grouping by `CLUSTER`
    and `DATE`. Pipe this into a `ggplot` for the typical line plot with
    `color = CLUSTER`.
4.  **Exploration of different number of clusters** $k$ by running the
    above several times for the values $k = 2, 3, 4, 5$ and observing
    how the clusters look in the two visualizations above. Observe how
    different the clusterings turn out in different runs for different
    $k$. Important: a change in color caused only by different cluster
    labels is not a meaningful difference when the groups are the
    same!  
    Decide on one value of $k$ that gives the most stable and most
    meaningful clusters (judged by the trend patterns). Go back to task
    1 in this section and describe what you learned from exploring
    different values of $k$, which $k$ you chose, and why.  
    Once $k$ is fixed, go to the chunk where you compute the `kmeans`
    object and add `set.seed(0)` as the first line. This fixes all
    random numbers for the computation, so whenever you render, the
    outcome will be the same. Check whether the clusters are stable
    under a few other arbitrary seeds, and mention the result in your
    description from task 1.  
5.  Write a numbered list with as many items as you have clusters and
    briefly **describe the trend pattern of each cluster** with respect
    to the average price level, the amount of oscillations and
    fluctuations (as defined in Section 3).
6.  Create two *tables* using the [`gt`
    package](https://gt.rstudio.com).  
    (You probably need to install it first. Then add `library(gt)` in
    your very first chunk where you also load the tidyverse.)
    1.  The first table shall report the number of petrol stations with
        the **regions in rows and a column for each cluster**. Create a
        pipe ending with creating a `gt` object with colored cells.
        Start the pipe with `data_stations_cluster` and `count` by
        `CLUSTER` and `REGION`. Then continue with reshaping the
        dataframe such that a column is created for each cluster (use
        `pivot_wider` taking names from `CLUSTER` and values from `n`,
        as produced by `count`). Then pipe this into

    ``` r
    gt() |> 
    data_color(-REGION, palette = "Oranges", direction = "column")
    ```

    and test how it looks. In the `direction` argument you can replace
    `"column"` by `"row"`. Test both and understand how they color the
    cells. The purpose of the table should be to show which cluster is
    dominating in each region. Select `"row"` or `"column"` such that
    this purpose is best fulfilled.
    2.  The second table should be similar to the first, just for brands
        instead of regions.

## 6. Potential explanations

Write a paragraph where you provide some potential explanations for the
different trend patterns. Note the market context: There is no hard
price regulation in Western Australia, and customers can fill up where
and when they want, so price competition is likely. The government
provides the portal FuelWatch for price transparency. Customers as well
as stations and companies can therefore monitor price trends easily.

Address these questions:

- Try to distinguish regular patterns and patterns triggered by external
  shocks. If there are external shocks describe them.
- Why are prices generally higher in some places and lower in others?
- Why do the regular oscillations happen?
- Where do oscillations occur? Why there?
- Do stations with oscillating prices achieve higher average prices in
  their region? If so, do you think they also get higher earnings
  compared to stations without oscillations? Can we answer these
  questions from the data we have? If not, what would we need to know to
  answer them?

## Possible extensions

These are some ideas. They are not part of the homework project.

- Extend the analysis to more years. FuelWatch was established in 2001.
  - Are the oscillations there from the start?
  - Do they spread regionally?
  - Was it always the same day when prices jumped up?
- Track stations over time.
  - Do stations change the trend patterns?
  - Probably some stations drop out and new appear or change brand. Is
    this related to trend patterns?
- Improve the classification of trend patterns. Up to now the clustering
  does not take into account whether days are close to each other.
  Create new features, find a direct way to classify different patterns,
  or test different unsupervised clustering methods.
- Relate the long-term changes to prices for crude oil. To that end,
  find new data and check out where Western Australia imports fuel from.

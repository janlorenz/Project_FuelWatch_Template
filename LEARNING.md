# LEARNING


- [Project workflow for data retrieval and
  preparation](#project-workflow-for-data-retrieval-and-preparation)
- [Date-formats and date formatting as
  strings](#date-formats-and-date-formatting-as-strings)
- [Glue strings using variables](#glue-strings-using-variables)

Use the opportunity in this Homework Project to learn a bit more about
some concepts and tools used here.

## Project workflow for data retrieval and preparation

Larger data science projects often include automatic or semi-automatic
**data retrieval**. Once raw data is in place in the project folder, it
often needs some **data preparation** to create the dataframes for the
next steps of data analysis. Data preparation steps can be joining or
binding several data frames imported from raw data files; some
transformations and cleaning, like removing some columns and changing
the data types of variables, for example, producing dates and factors.

In a **small project**

- You download or copy the data file manually into the project folder.
- In the initial code cell of your Markdown file you import the data in
  and pipe it through some data transformations and store it in a
  dataframe.

For examples:

``` r
data <- read_csv("rawdata-file.csv") |> 
  select(-column_to_remove) |> 
  mutate(Date = as_date(Date))
```

This can create the following problems:

- To make your work reproducible, your readers need to be able to
  reproduce your analysis. So, they need the data. However, including
  large dataset in a git-repository may be discouraged. One way is to
  not `git commit` the raw data, but provide a documentation how the
  user can download the data and information where to put it. An even
  better way is to write a *data retrieval script*! However, a data
  retrieval script should not be done every time you render the
  document. So, you better put it into a different file which you (and
  your reproducing readers) only need to execute once.
- The data import and preparation may be computationally very time
  consuming. So, you may not want to have to spend that time every time
  again when you (or your reproducing readers) render the report. \*So,
  better run a data retrieval and data preparation script an save a new
  data file which can be imported in the Markdown file much faster!

In a **large project** you can think of a structure like this:

- A directory `data_raw` and either
  - the raw data files
  - a `data_download` script which would download the data and put it
    into the `data_raw` directory, or
  - a documentation how the raw data can be put into the `data_raw`
    directory
- A directory `data_prepared` and a script `data_preparation` which
  imports the raw data, creates dataframes for usage in the data
  analysis, and exports it into data files in the directory
  `data_prepared`

In this Homework Project you get a *taste* of this. Whenever you start a
new project, think about the data retrieval and preparation workflow.
When a project grows, there me be a time where you should reorganize and
streamline the data retrieval and preparation workflow.

## Date-formats and date formatting as strings

R has a class “Date”. The ISO standard for writing dates as a string is
“YYYY-MM-DD” (so “2024-10-01” for the 1st of October 2024). The base-R
function to create a date from the string is `as.Date` in tidyverse’s
library lubridate the analog function is `as_date`. Test
`d <- as.Date("2024-10-01")` and then `class(d)` and `as.numeric(d)` to
recall how dates are stored.

Remember the function `seq` for sequence generation? (Test
`seq(from = 1, to = 100, by = 5)` to remember.) This also works for
dates, where you can put a start and an end date into `from =` and
`to =`. A nice thing is you can also put a character string like “day”,
“week” or “month” into `by =`. (Call `?seq.Date` and read the details.)
With this you should be able to create the required vector `dates`.

For the URLs we need the dates in the form “MM-YYYY”. You can use the
`format` function for this. The format function can be used for many
purposes of *pretty printing*. For example you can use it to print
numbers with big marks, decimal marks and digits. Try
`format(1234.567, big.mark = ",", decimal.mark = ".", digits = 5)`. For
dates you can call `?format.Date` and read the details. For our purpose
`format(dates, format = "%m-%Y")` should work.

## Glue strings using variables

A common task in data science scripting is to create longer strings
which are partly composed of variables. Often the variables are vectors
and we want to have a vector of strings. The tidyverse version is with
the function `str_glue`. The function `str_glue()` takes a string but
interprets expressions enclosed by braces `{}` as R code. And it works
vectorized! Test `str_glue("The day is {dates}.")`.

The base-R commands for this are `paste` and `paste0` and they are also
good. Test `paste("The day is", dates, ".")` and
`paste0("The day is ", dates, ".")`.

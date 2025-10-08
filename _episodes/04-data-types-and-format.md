---
title: Data Types and Formats
teaching: 20
exercises: 25
questions:
    - "What types of data can be contained in a DataFrame?"
    - "Why is the data type important?"
objectives:
    - "Describe how information is stored in a Python DataFrame."
    - "Define the two main types of data in Python: text and numerics."
    - "Examine the structure of a DataFrame."
    - "Modify the format of values in a DataFrame."
    - "Describe how data types impact operations."
    - "Define, manipulate, and interconvert integers and floats in Python."
    - "Analyze datasets having missing/null values (NaN values)."
    - "Write manipulated data to a file."
keypoints:
    - "Pandas uses other names for data types than Python, for example: `object` for textual data."
    - "A column in a DataFrame can only have one data type."
    - "The data type in a DataFrame’s single column can be checked using `dtype`."
    - "Make conscious decisions about how to manage missing data."
    - "A DataFrame can be saved to a CSV file using the `to_csv` function."
---

The format of individual columns and rows will impact analysis performed on a
dataset read into Python. For example, you can't perform mathematical
calculations on a string (text formatted data). This might seem obvious,
however sometimes numeric values are read into Python as strings. In this
situation, when you then try to perform calculations on the string-formatted
numeric data, you get an error.

In this lesson we will review ways to explore and better understand the
structure and format of our data.

## Types of Data

How information is stored in a
DataFrame or a Python object affects what we can do with it and the outputs of
calculations as well. There are two main types of data that we will explore in
this lesson: numeric and text data types.

## Numeric Data Types

Numeric data types include integers and floats. A **floating point** (known as a
float) number has decimal points even if that decimal point value is 0. For
example: 1.13, 2.0, 1234.345. If we have a column that contains both integers and
floating point numbers, Pandas will assign the entire column to the float data
type so the decimal points are not lost.

An **integer** will never have a decimal point. Thus if we wanted to store 1.13 as
an integer it would be stored as 1. Similarly, 1234.345 would be stored as 1234. You
will often see the data type `Int64` in Python which stands for 64 bit integer. The 64
refers to the memory allocated to store data in each cell which effectively
relates to how many digits it can store in each "cell". Allocating space ahead of time
allows computers to optimize storage and processing efficiency.

## Text Data Type

Text data type is known as Strings in Python, or Objects in Pandas. Strings can
contain numbers and / or characters. For example, a string might be a word, a
sentence, or several sentences. A Pandas object might also be a plot name like
'plot1'. A string can also contain or consist of numbers. For instance, '1234'
could be stored as a string, as could '10.23'. However **strings that contain
numbers can not be used for mathematical operations**!

Pandas and base Python use slightly different names for data types. More on this
is in the table below:

| Pandas Type | Native Python Type | Description |
|-------------|--------------------|-------------|
| object | string | The most general dtype. Will be assigned to your column if column has mixed types (numbers and strings). |
| int64  | int | Numeric characters. 64 refers to the memory allocated to hold this character. |
| float64 | float | Numeric characters with decimals. If a column contains numbers and NaNs (see below), pandas will default to float64, in case your missing value has a decimal. |
| datetime64, timedelta[ns] | N/A (but see the [datetime] module in Python's standard library) | Values meant to hold time data. Look into these for time series experiments. |

[datetime]: http://doc.python.org/2/library/datetime.html


## Checking the format of our data

Now that we're armed with a basic understanding of numeric and text data
types, let's explore the format of our wave data. We'll be working with the
same `waves.csv` dataset that we've used in previous lessons. If you've started a new
notebook, you'll need to load Pandas and the dataset again:

~~~
# Make sure pandas is loaded
import pandas as pd

# Note that pd.read_csv is used because we imported pandas as pd
waves_df = pd.read_csv("data/waves.csv")
~~~
{: .language-python}

Remember that we can check the type of an object like this:

~~~
type(waves_df)
~~~
{: .language-python}

~~~
pandas.core.frame.DataFrame
~~~
{: .output}

Next, let's look at the structure of our waves data. In Pandas, we can check
the type of one column in a DataFrame using the syntax
`dataFrameName[column_name].dtype`:

~~~
waves_df['Name'].dtype
~~~
{: .language-python}

~~~
dtype('O')
~~~
{: .output}

A type 'O' just stands for "object" which in Pandas' world is a string
(text).

~~~
waves_df['record_id'].dtype
~~~
{: .language-python}

~~~
dtype('int64')
~~~
{: .output}

The type `int64` tells us that Python is storing each value within this column
as a 64 bit integer. We can use the `dat.dtypes` command to view the data type
for each column in a DataFrame (all at once).

~~~
waves_df.dtypes
~~~
{: .language-python}

which **returns**:

~~~
record_id           int64
buoy_id             int64
Name               object
Date               object
Tz                float64
Peak Direction    float64
Tpeak             float64
Wave Height       float64
Temperature       float64
Spread            float64
Operations         object
Seastate           object
Quadrant           object
dtype: object
~~~
{: .language-python }

Note that some of the columns in our wave data are of type `int64`. This means
that they are 64 bit integers. Others are floating point value
which means they contains decimals. The 'Name', 'Operations',	'Seastate', 
and	'Quadrant' columns are objects which contain strings.

## Working With Integers and Floats

So we've learned that computers store numbers in one of two ways: as integers or
as floating-point numbers (or floats). Integers are the numbers we usually count
with. Floats have fractional parts (decimal places).  Let's next consider how
the data type can impact mathematical operations on our data. Addition,
subtraction, division and multiplication work on floats and integers as we'd expect.

~~~
print(5+5)
~~~
{: .language-python}

~~~
10
~~~
{: .output}

~~~
print(24-4)
~~~
{: .language-python}

~~~
20
~~~
{: .output}

If we divide one integer by another, we get a float.
The result on Python 3 is different than in Python 2, where the result is an
integer (integer division).

~~~
print(5/9)
~~~
{: .language-python}

~~~
0.5555555555555556
~~~
{: .output}

~~~
print(10/3)
~~~
{: .language-python}

~~~
3.3333333333333335
~~~
{: .output}

We can also convert a floating point number to an integer or an integer to
floating point number. Notice that Python by default rounds down when it
converts from floating point to integer.

~~~
# Convert a to an integer
a = 7.83
int(a)
~~~
{: .language-python}

~~~
7
~~~
{: .output}

~~~
# Convert b to a float
b = 7
float(b)
~~~
{: .language-python}

~~~
7.0
~~~
{: .output}

## Working with dates

You've probably noticed that one of the columns in our `waves_df` DataFrame represents the time at which the measurement was taken. As with all other non-numeric types, Pandas automatically set the type of
this column as `Object`. However, because we know it's a date, we can cast is a Date type. For the purposes of this section, let's create a new Pandas Series of the Date values:

~~~
dates = waves_df["Date"]
~~~
{: .language-python}

We can use the `to_datetime` function to convert the values in this Series to a Date type:

~~~
# note that we're overwriting the variable we created
dates = pd.to_datetime(dates, format="%d/%m/%Y %H:%M")
~~~
{: .language-python}

What does the value given to the `format` argument mean? Because there is no consistent way of specifying dates, Python has a set of codes to specify the elements. We use these codes to tell Python the format
of the date we want to convert. The full list of codes is at https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes, but we're using:

 - %d : Day of the month as a zero-padded decimal number.
 - %m : Month as a zero-padded decimal number.
 - %Y : Year with century as a decimal number.
 - %H : Hour (24-hour clock) as a zero-padded decimal number.
 - %M : Minute as a zero-padded decimal number.

Let's take an individual value and see some of the things we can do with it

~~~
date1 = dates.iloc[14]
~~~

 - We'll look at indexing more in the next episode.

We can see that it's now of a DateTime type:

~~~
type(date1)
~~~
{: .language-python}

~~~
pandas._libs.tslibs.timestamps.Timestamp
~~~
{: .output}

We can now take advantage of Pandas' (and Python's) powerful methods of dealing with dates, which we couldn't have easily done while it was a String. For example:

 - The day of the week the measurement was taken (indexed from Monday being 0):

~~~
# note that this is a statement, so no brackets
date1.day_of_week
~~~
{: .language-python}

 - The name of the day of the week:

~~~
# note that this is a function, so there are brackets
date1.day_name()
~~~
{: .language-python}

 - We can also determine the day of the year:

~~~
date1.day_of_year
~~~
{: .language-python}

This is a convenient place to highlight that the `apply` method is one way to run a function on every element of a Pandas data structure, without needing to write a loop. For example, to get the length of 
the Buoy Station Names, we can write:

~~~
waves_df["Names"].apply(len)
~~~
{: .language-python}

which will return

~~~
0       31
1       24
2       27
3       16
4        7
        ..
2068    16
2069    16
2070    16
2071    16
2072    16
Name: Name, Length: 2073, dtype: int64
~~~
{: .output}

Similarly, we can create a new Series which contains the day of the week all of the measurements were taken on:

~~~
days_of_measurements = dates.apply(pd.Timestamp.day_name)
~~~
{: .language-python}

However, note that we have to give the full, _qualified_ name of the function - this is something we determine from the documentation (e.g. https://pandas.pydata.org/docs/reference/api/pandas.Timestamp.day_name.html).

Are there any days of the week that measurements weren't taken on? We can either look at the unique string values, or the result of `nunique` which we saw earlier:

~~~
days_of_measurements = dates.apply(pd.Timestamp.day_name)
print(days_of_measurements)
print(len(days_of_measurements.unique()))
print(days_of_measurements.nunique())
~~~
{: .language-python}

If we want to do anything more complex with dates, we may need to use Python's functions (the Pandas functions are mostly convenience functions for some of the underlying Python equivalent ones).
Looking again at the DateTime codes, we can see that `%a` will give us the short version of the day of the week. The DateTime Library has a function for formatting DateTime objects: `datetime.datetime.strftime`,
but now we need to give as argument to the function we're going to use in `apply`. The `args` argument allows us to do this:

~~~
# need to import the DateTime library
import datetime
dates.apply(datetime.datetime.strftime, args=("%a",))
~~~
{: .language-python}

>## Watch out for tuples!
> _Tuples_ are data structure similar to a list, but are _immutable_. They are created using parentheses, with items separated by commas: 
> `my_tuple = (1, 2, 3)`
> However, putting parentheses around a single object does not make it a tuple! Creating a tuple of length 1 still needs a trailing comma.
> Test these: `type(("a"))` and `type(("a",))`.
> The `args` argument of `apply` expects a tuple, so if there's only one argument to give we need to use the trailing comma. 
{: .callout}

We can also find the time differences between two dates - Pandas (and Python) refer to these as _Time Deltas_. We can take the difference between two timestamps, and Python will
automatically create a TimeDelta for us:

~~~
date2 = dates.iloc[15]
time_diff = date2 - date1
print(time_diff)
print(type(time_diff))
~~~
{: .language-python}

~~~
Timedelta('0 days 00:30:00')
pandas._libs.tslibs.timedeltas.Timedelta
~~~
{: .output}

> ## Rounding
> Using the `apply` function, round the values in the Wave Height column to the nearest whole number and store the resulting Series in a new variable called `rounded_heights`.
> What would you need to change to round to 2 decimal place?
>
> > ## Solution
> > ~~~
> > rounded_heights = waves_df["Wave Height"].apply(round)
> > waves_df["Wave Height"].apply(round, args=(2,))
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

> ## Exploring Timedeltas 
> Have a look at the Pandas Timedelta documentation. How could you print only the minutes difference from our `time_diff` variable? 
>
> > ## Solution
> > There are 2 ways
> > ~~~
> > print(time_diff.components.minutes)
> > print(time_diff.seconds/60)
> > ~~~
> > {: .language-python}
> >
> > Note that the values in the `components` attribute aren't for the total delta, only for that proportion; e.g. a time delta of 1 day and 30 seconds would return
> >
> > ~~~
> > Components(days=1, hours=0, minutes=0, seconds=30, milliseconds=0, microseconds=0, nanoseconds=0)
> > ~~~
> > {: .language-python}
> >
> {: .solution}
{: .challenge}

## Working With Our Wave Data

Getting back to our data, we can modify the format of values within our data, if
we want. For instance, we could convert the `record_id` field to floating point
values.

~~~
# Convert the record_id field from an integer to a float
waves_df['record_id'] = waves_df['record_id'].astype('float64')
waves_df['record_id'].dtype
~~~
{: .language-python}

~~~
dtype('float64')
~~~
{: .output}

> ## Changing Types
>
> Try converting the column `buoy_id` to floats using
>
> ~~~
> waves_df.buoy_id.astype("float")
> ~~~
> {: .language-python}
>
> Next try converting `Temperature` to an integer. What goes wrong here? What is Pandas telling you?
> We will talk about some solutions to this in the section below.
>> ## Solution 
>> 
>> Converting the `buoy` column to floats returns
>> ~~~
>> 0       14.0
>> 1        7.0
>> 2        5.0
>> 3        3.0
>> 4       10.0
>>         ... 
>> 2068    16.0
>> 2069    16.0
>> 2070    16.0
>> 2071    16.0
>> 2072    16.0
>> Name: buoy_id, Length: 2073, dtype: float64 
>> ~~~
>> {: .output}
>> So we can see that we can convert a whole column of _int_ values to floating point values. Converting floating point values to ints works in the same way.
>> However, this only works _if_ all there's a value in
>> every row. When we try this with the Temperature column:
>> ~~~
>> waves_df.Temperature.astype("int")
>> ~~~
>> {: .language-python}
>>
>> We get an error, with the pertinent line in the error (the last one) being:
>>
>> ~~~
>> ValueError: Cannot convert NA to integer
>> ~~~
>> {: .output}
>>
>> This happens because some of the values in the Temperature column are None values, and the `astype` function can't convert this type of value.
> {: .solution}
{: .challenge}

## Missing Data Values - NaN

What happened in the last challenge activity? Notice that this throws a value error:
`ValueError: Cannot convert NA to integer`. If we look at the `Temperature` column in the waves
data we notice that there are NaN (**N**ot **a** **N**umber) values. **NaN** values are undefined
values that cannot be represented mathematically. Pandas, for example, will read
an empty cell in a CSV or Excel sheet as a NaN. NaNs have some desirable properties: if we
were to average the `Temperature` column without replacing our NaNs, Python would know to skip
over those cells.

~~~
waves_df['Temperature'].mean()
~~~
{: .language-python}

~~~
12.872890559732667
~~~
{: .output}

Dealing with missing data values is always a challenge. It's sometimes hard to
know why values are missing - was it because of a data entry error? Or data that
someone was unable to collect? Should the value be 0? We need to know how
missing values are represented in the dataset in order to make good decisions.
If we're lucky, we have some metadata that will tell us more about how null
values were handled.

For instance, in some disciplines, like Remote Sensing, missing data values are
often defined as -9999. Having a bunch of -9999 values in your data could really
alter numeric calculations. Often in spreadsheets, cells are left empty where no
data are available. Pandas will, by default, replace those missing values with
NaN. However it is good practice to get in the habit of intentionally marking
cells that have no data, with a no data value! That way there are no questions
in the future when you (or someone else) explores your data.

### Where Are the NaNs?

Let's explore the NaN values in our data a bit further. Using the tools we
learned in lesson 02, we can figure out how many rows contain NaN values for
Temperature. We can also create a new subset from our data that only contains rows
with Temperature values > 0 (i.e. select meaningful seawater temperature values):

In our case, all the Temperature values are above zero. You can verify this by either trying to
select all rows that have temperatures less than or equal to zero (which returns an empty data frame):

```
waves_df[waves_df.Temperature <= 0]
```
{: .language-python}

or, by seeing that the number of rows that have values above zero (1197) added to the
number of rows with NaN values (876) is equal to the total number of rows in the
original data frame (2073).

We can replace all NaN values with zeroes using the `.fillna()` method (we might want to
make a copy of the data so we don't lose our work):

~~~
df1 = waves_df.copy()
# Fill all NaN values with 0
df1['Temperature'] = df1['Temperature'].fillna(0)
~~~
{: .language-python}

However NaN and 0 yield different analysis results. The mean value when NaN
values are replaced with 0 is different from when NaN values are simply thrown
out or ignored.

~~~
df1['Temperature'].mean()
~~~
{: .language-python}

~~~
7.4331162566329
~~~
{: .output}

This sounds like it could be a 'real' temperature value, but the answer is biased 'low' 
because we have included a load of erroneous zeros - instead of using NaNs for our missing values.

We can fill NaN values with any value that we chose. The code below fills all
NaN values with a mean for all Temperature values. Let's first create another copy of our data.

~~~
df2 = waves_df.copy()
df2['Temperature'] = df2['Temperature'].fillna(waves_df['Temperature'].mean())
~~~
{: .language-python}

We could also chose to create a subset of our data, only keeping rows that do
not contain NaN values.

Our mean now looks more sensible again:

~~~
df2['Temperature'].mean()
~~~
{: .language-python}

~~~
12.872890559732667
~~~
{: .output}

The point is to make conscious decisions about how to manage missing data. This
is where we think about how our data will be used and how these values will
impact the scientific conclusions made from the data.

Python gives us all of the tools that we need to account for these issues. We
just need to be cautious about how the decisions that we make impact scientific
results.

> ## Counting
> Count the number of missing values per column.
>
> **Hint**
> The method `.count()` gives you the number of non-NA observations per column.
> Try looking to the `.isnull()` method.
>
>> ## Solution
>> ~~~
>> for c in waves_df.columns:
>>     print(c, len(waves_df[waves_df[c].isnull()]))
>> ~~~
>> {: .language-python}
>>
>> Or, since we've been using the `pd.isnull` function so far:
>>
>> ~~~
>> for c in waves_df.columns:
>>     print(c, len(waves_df[pd.isnull(waves_df[c])]))
>> ~~~
>> {: .language-python}
>>
>> It's also possible to use function chaining:
>>
>> ~~~
>> waves_df.isnull().sum()
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

The answer to the previous challenge shows there's often more than one way to use the _same_ function - in this case we
can call `.isnull()` on a DataFrame, or pass a DataFrame to it as an argument. In most cases, you will need to read the
documentation to find out how to use functions.

## Writing Out Data to CSV

We've learned about using manipulating data to get desired outputs. But we've also discussed
keeping data that has been manipulated separate from our raw data. Something we might be interested
in doing is working with only the columns that have full data. First, let's reload the data so
we're not mixing up all of our previous manipulations.

~~~
waves_df = pd.read_csv("data/waves.csv")
~~~
{: .language-python}

Next, let's drop all the rows that contain missing values. We will use the command `dropna`.
By default, dropna removes rows that contain missing data for even just one column.

~~~
df_na = waves_df.dropna()
~~~
{: .language-python}

If you now type `df_na`, you should observe that the resulting DataFrame has 692 rows
and 13 columns, much smaller than the 2073 row original.

We can now use the `to_csv` command to export a DataFrame in CSV format. Note that the code
below will by default save the data into the current working directory. We can
save it to a different folder by adding the foldername and a slash before the filename:
`df.to_csv('foldername/out.csv')`. We use 'index=False' so that
pandas doesn't include the index number for each line.

~~~
# Write DataFrame to CSV
df_na.to_csv('data_output/waves_complete.csv', index=False)
~~~
{: .language-python}

We will use this data file later in the workshop. Check out your working directory to make
sure the CSV wrote out properly, and that you can open it! If you want, try to bring it
back into Python to make sure it imports properly.

{% include links.md %}


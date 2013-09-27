---
layout: post
title: "'Reading CSV data into R'"
date: 2013-09-27 12:27
comments: true
categories: R, data, import
---

There are a variety of ways to import data into R.

I most often work with CSV files.  In my experience, getting the file path
correct is the trickiest part of bringing data into R.  I'm going to show you how
this works by first writing a piece of data to a CSV file and then we'll bring it back
in.

The `data()` function in R allows access to tons of toy datasets.  R programmers
typically use these to [create reproducible examples](http://stackoverflow.com/questions/5963269/how-to-make-a-great-r-reproducible-example) on sites like StackOverflow.

{% codeblock lang:r %}
data(mtcars) # first I get some toy data, so now we have access to the mtcars data.frame

head(mtcars) # Let's take a look

                  mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1 4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1 4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1 4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0 3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0 3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0 3    1

str(mtcars) # we can see how each variable is coded here: num, chr, list, int, etc.

'data.frame':32 obs. of  11 variables:
  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
  $ disp: num  160 160 108 258 360 ...
  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
  $ qsec: num  16.5 17 18.6 19.4 17 ...
  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
{% endcodeblock %}

So we have a `data.frame` with 32 observations and 11 variables.  Now let's
use `write.csv()`. First we'll take a look at the inputs for the function. We
can do this by putting a `?` in front of our empty function and running it with
`R`.

{% codeblock lang:r %}
?write.csv # put a ? in front of any function to see its help page.
{% endcodeblock %}

We're brought to [this page](http://stat.ethz.ch/R-manual/R-devel/library/utils/html/write.table.html) which shows all the options we can use.  For now, we're only going to use the first two arguments (or parameters, which ever you prefer): `x`, some object and `file= ""`.

If you use Linux, Mac, or Unix:
{% codeblock lang:r %}
write.csv(mtcars, file = "~/mtcars.csv") # <-- that file path would tell a Unix
system to save the CSV in the home directory.
{% endcodeblock %}

If you use Windows (Windows is not the OS you want to be programming R in, but
it's okay to start on):
{% codeblock lang:r %}
write.csv(mtcars, file = "C:/Users/Chris/Documents/mtcars.csv")
{% endcodeblock %}

Notice the direction of the backslashes in the Windows path?  It's the opposite
of how Windows does it.  The `\` character is considered special in R.  You can
simply convert all the `\` backslashes that Windows shows you in a filepath to
`/` forward slashes.  If you wanted to get fancy, you could actually put two
backslashes, like this instead of single forward slashes `C:\\temp\\some-file.csv`.

Both of these would work, `C:\\temp\\some-file.csv` and `C:/temp/some-file.csv`,
while `C:\temp\some-file.csv` will not.

Now that we know the exact path of the file, let's read it back in to R.

{% codeblock lang:r %}
my_data_frame <- read.csv("~/mtcars.csv")
{% endcodeblock %}

or for Windows:

{% codeblock lang:r %}
my_data_frame <- read.csv("C:/Users/Chris/Documents/mtcars.csv")
{% endcodeblock %}

I'd encourage you to read [the documentation](http://stat.ethz.ch/R-manual/R-devel/library/utils/html/read.table.html) on the `read.csv()`.

You'll notice that when we take a look at the imported data the variables are
coded differently.  This is because `read.csv()` looks at your data and makes a
guess about which type they should be: `character`, `numeric`, `int`, `factor`,
etc.  Commonly you'll want to control this with options like `stringsAsFactors =
FALSE`.

{% codeblock lang:r %}
'data.frame':32 obs. of  12 variables:
   $ X   : Factor w/ 32 levels "AMC Javelin",..: 18 19 5 13 14 31 7 21 20 22 ...
   $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
   $ cyl : int  6 6 4 6 8 6 8 4 4 6 ...
   $ disp: num  160 160 108 258 360 ...
   $ hp  : int  110 110 93 110 175 105 245 62 95 123 ...
   $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
   $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
   $ qsec: num  16.5 17 18.6 19.4 17 ...
   $ vs  : int  0 0 1 1 0 1 0 1 1 1 ...
   $ am  : int  1 1 1 0 0 0 0 0 0 0 ...
   $ gear: int  4 4 4 3 3 3 3 4 4 4 ...
   $ carb: int  4 4 1 1 2 1 4 2 2 4 ...
{% endcodeblock %}

That's all for now.  Next time we'll do some basic visualization of this
dataset.








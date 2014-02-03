---
layout: post
title: "Are academics adopting Github?"
date: 2013-12-15 20:27
comments: true
categories: academia, git, github, version control, R, Python
description: Version control like git and github is becoming an important tool for academics.
---

As the partner of one of the "Piled Higher, and Deeper" (a Ph.D student) crowd and also a data
scientist, I couldn't help but introduce the 'git-way' to Gentry.

She's a geographer at Queen's university.  **Like me, she's tech savvy, but coding
is new to her.  Human geography, her field, is largely qualitative like the rest
of the humanities.**

[{% imgcap right http://i.imgur.com/PDQx8tf.jpg 350 550 "An academic's console command cheat sheet" %}](http://i.imgur.com/PDQx8tf.jpg)

At the end of her previous degree, I helped her cope with MS Word as it struggled under the weight of a 100+ page thesis.  We expanded the memory on her laptop.  She had "sub-docs" everywhere.  Bits and pieces of thoughts  put to the side, worked on independently, temporarily stricken.  But how to keep track of it all?

I was learning Git at the same time on my path from statistician to data scientist.

A year later, I've come to use git and Github on a daily-basis.  I've moved
from an academic statistician to a commercial data scientist.  I work with a
team of developers, and the Git-way has become really the only way for keeping track
of my projects.

Now, Gentry is working on her dissertation.  As a first step into coding, she's been learning LaTeX.  She's sold because she can annotate her own dissertation with comments.

For those unfamiliar, LaTeX is a typsetting language. It creates beautiful documents with high-level style control. I recommend it to anyone that's ever tried to do formatting in
MS Word. :-)

LaTeX is also largely confined to academia.  I'm not saying they don't exist,
but I haven't met anyone that uses LaTeX who is not also an academic.

**Now that Gentry was coding her dissertation, using Git and Github were open to
her as tools.**

I commonly relate Github to my non-programming friends by telling them it's the
Facebook of code and programmers.  People instantly get this analogy.  "Oh, so
it lets you be social and interact?"

"Precisely, but with code."

Github is perfect for academics.  If they can use it by coding with LaTeX, even
non-quantitative academics can work socially and openly (which is possibly more
important).

Gentry's sold and she's been been learning Git with my co-worker Tommy Morgan.  Tommy
teaches an [excellent lesson on Git at Treehouse](http://teamtreehouse.com/library/git-basics).  As I was speaking with the Co-Founder Ryan Carson last week, I wondered out loud, "how many academics out there are using Git (and Github)?"

Today, you get the benefit of that curiousity. **Are academics adopting
Github?**

Assuming that LaTeX is a good proxy for academic use of Github, **yes, academics
have been creating Github repositories at basically an exponential rate since
the beginning of 2011** and the pace has really picked up since August 2013.
(makes sense given that it's the school year).

{% img center http://i.imgur.com/bVA4Pa9.png %}

...and on a log-scale (notice that the growth is linear on this scale), that means
it's exponential growth since the beginning of 2011.

{% img center http://i.imgur.com/hmI9mpb.png %}

This is just a raw look at the number of LaTeX repositories on Github by month.
But I don't think the data needs to be spoiled by over-analysis.  As a next
step, I'd like to try forecasting the growth rate of these repositories by using
the growth rates of other repositories.

The idea of git and Github is so powerful, that I bet the academic community
falls in love as much as the programming world has.  I also predict that those
academics who adopt first will have a sigificant competitive advantage over
their peers.  This is in an increasingly competitive job market where
Ph.D-candidate after candidate hit a brick wall when searching for a
tenure-track position.

This data scientist is very proud of his human geographer for using version
control in the place of snippets of MS Word documents.

For those interested, the code I used to do this analysis was a mix of Python
and R.  Here's the [Github repo](https://github.com/statwonk/academic-github-adoption), and relevant code snippets:

{% codeblock lang:python %}
import requests
import json
import csv
import time

times = ["2010", "2011", "2012"]
for i in range(0, 12):
  times.append(str(str(2013) + "-" + str(i + 1)))

df = {}
for j in times:
    for i in range(1, 10):
        # print "Sleeping 15 seconds ... "
        time.sleep(60/4)
        r = requests.get('https://api.github.com/search/repositories?q=language:latex&' + \
                         'page=' + str(i) + '&per_page=100' + \
                         '&created=' + str(j))
        if(r.ok):
            repoItem = json.loads(r.content)
            print "page " + str(i) + ", year " + str(j)
            for repo in repoItem['items']:
                df[repo['name']] = repo['created_at']
        else: print "oh nos! Page " + str(i) + " has a problem"

with open('data.csv', 'wb') as csvfile:
    writer = csv.writer(csvfile, delimiter= ',')
    for i in df.keys():
        writer.writerow((i, df[i]))
{% endcodeblock %}

... admittedly pretty basic.  Just learning Python! Plotting was done in R's
```ggplot2``` library.

{% codeblock lang:r %}
library(ggplot2)
library(lubridate)
library(ggthemes)

df <- read.csv("data.csv", header = F)
names(df) <- c("repo", "created_at")
df$created_at <- as.POSIXct(df$created_at)
df <- df[df$created_at < max(df$created_at) - days(14), ]

ggplot(df, aes(x = created_at)) +
  scale_y_continuous(breaks = seq(0, 150, 25)) +
  geom_hline(yintercept = 25, colour = "red", linetype="dotted") +
  geom_hline(yintercept = 50, colour = "red", linetype="dotted") +
  geom_hline(yintercept = 75, colour = "red", linetype="dotted") +
  geom_hline(yintercept = 100, colour = "red", linetype="dotted") +
  geom_hline(yintercept = 125, colour = "red", linetype="dotted") +
  geom_histogram(colour = "black", fill = "tan", binwidth = 60*60*24*30.5, stat = "bin") +
  xlab("") +
  ylab("") +
  ggtitle("Public Github Repositories\nwritten in LaTeX per Month") +
  theme_solarized(light = F) +
  theme(axis.text.x = element_text(size = 32),
        axis.text.y = element_text(size = 28),
        plot.title = element_text(size = 30))

ggplot(df, aes(x = created_at)) +
  scale_y_log10() +
  geom_hline(yintercept = 10, colour = "red", linetype="dotted") +
  geom_hline(yintercept = 100, colour = "red", linetype="dotted") +
  geom_histogram(colour = "black", fill = "tan", binwidth = 60*60*24*30.5, stat = "bin") +
  xlab("") +
  ylab("(Log scale)") +
  ggtitle("Public Github Repositories\nwritten in LaTeX per Month") +
  theme_solarized(light = F) +
  theme(axis.title.y = element_text(size = 25),
        axis.text.x = element_text(size = 32),
        axis.text.y = element_text(size = 28),
        plot.title = element_text(size = 30))
{% endcodeblock %}

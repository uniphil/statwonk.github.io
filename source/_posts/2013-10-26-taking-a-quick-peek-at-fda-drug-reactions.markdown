---
layout: post
title: "'Taking a quick peek at FDA adverse drug reactions"
date: 2013-10-26 00:07
comments: true
categories: medicine, statistics, R, FDA
---
About a week ago my doctor put me on a new medication.  The drug is one that is known to have some unpleasant effects as you get used to it.  I was curious to see what the reported adverse reactions (to the U.S. FDA) were, so I decided to take a look at the data.

The data actually wasn't very easy to find, but here's the current location: [http://www.fda.gov/Drugs/GuidanceComplianceRegulatoryInformation/Surveillance/AdverseDrugEffects/ucm082193.htm](http://www.fda.gov/Drugs/GuidanceComplianceRegulatoryInformation/Surveillance/AdverseDrugEffects/ucm082193.htm).  

Wow, look at the SEO value of that URL!

It's pretty unbelievable to me that the latest data is now more than 10 months old. We'll have to see what the FDA says,

<blockquote class="twitter-tweet tw-center-align"><p><a
href="https://twitter.com/FDA_Drug_Info">@FDA_Drug_Info</a> This info seems
stale, what&#39;s going on? <a
href="http://t.co/fVAkiJzS9i">http://t.co/fVAkiJzS9i</a></p>&mdash; Christopher
Peters (@statwonk) <a
href="https://twitter.com/statwonk/statuses/393945145030094848">October 26,
2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

So I took a look at the drug reaction info and the top twenty reactions seemed relatively benign, so I feel a bit better.  It's a pretty simple way to go about things, but I'd encourage you to take a look at some [FDA studies of drugs you take](http://www.accessdata.fda.gov/scripts/cder/drugsatfda/index.cfm).  The few I've looked at are appallingly deficient.  Where are the power studies, yo?  If a study has a 5% likelihood of detecting a 20% increase in heart attack, you'd never know -- that's why you should NOT blindly trust p-values > 0.05.  It doesn't mean that there isn't an effect and the study may not have had a good chance of detecting the increased risk from the get-go.

You can find my little script on [Github here](https://github.com/statwonk/FDA-adverse-drug-reactions). 

I would love to collaborate with someone on this, let's dig into this data!  

Perhaps data mine to find not well known drug interactions?

{% codeblock lang:r %}
# Author: Christopher Peters
# Twitter: @statwonk
# Analysis of adverse reported drug events from FDA database

# Data found here: http://www.fda.gov/Drugs/GuidanceComplianceRegulatoryInformation/Surveillance/AdverseDrugEffects/ucm082193.htm

patient <- read.table("~/Downloads/faers_ascii_2012q4/ascii/demo12q4.txt", sep = "$", header = T, fill = T, quote = "")
drug <- read.table("~/Downloads/faers_ascii_2012q4/ascii/drug12q4.txt", sep = "$", header = T, fill = T, quote = "")
reaction <- read.table("~/Downloads/faers_ascii_2012q4/ascii/reac12q4.txt", sep = "$", header = T, fill = T, quote = "")
outcomes <- read.table("~/Downloads/faers_ascii_2012q4/ascii/outc12q4.txt", sep = "$", header = T, fill = T, quote = "")

# You can find individual drugs and their reported adverse by specifying there names below

# There are commonly many names for a drug, see below where I put three grepl statements
# with text like, "put drug brand name here", you can replace this with drug brand names
# you can add or remote grepl statements depending on the number of brand names you want to look over.
df <- drug[(grepl("put drug brand name 1 here", drug$drugname, ignore.case = T) | # drug is likely to be entered as many different brand names, use this to capture them individually
              grepl("put drug brand name 2 here", drug$drugname, ignore.case = T) | # enter drug names here, add or remove grepl() as needed with "or" statements \
              grepl("put drug brand name 3 here ... add more grepl statements as necessary", drug$drugname, ignore.case = T)) & drug$drug_seq == 1, ] # drug seq 1 == suspect drug of many possible that patient is taking

df <- merge(df, reaction, by = "primaryid") # let's merge the drug file with reactions
df <- merge(df, outcomes, by = "primaryid") # we'll bring in outcomes, too
df2 <- as.data.frame(table(df$pt, df$outc_code)) # count the instances of reactions and their outcomes
names(df2) <- c("reaction", "outcome", "count")
df2 <- df2[order(df2$count, decreasing = T), ]
head(df2, 20) # top 20 reactions
{% endcodeblock %}


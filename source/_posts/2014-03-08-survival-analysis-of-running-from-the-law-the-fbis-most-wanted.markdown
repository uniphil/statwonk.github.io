---
layout: post
title: You've just been added to the FBI's Ten Most Wanted List, how long will you survive?
date: 2014-03-08 19:28
comments: true
categories:
- survival analysis
---
My best guess is,
{% blockquote %}
613 days, but there's a 50-50 shot you'll be caught within 2.7 months.
{% endblockquote %}

Now I'll give some background and show you how I arrived at that number.

The FBI's Ten Most Wanted Fugitives List has publicized America's deadliest criminals for nearly 65 years, since March 14th, 1950.  The list is available nearly [everywhere](http://www.fbi.gov/wanted), from America's post offices to the streets of Venezuela, criminals have been noticed and captured.  The list makes life difficult for fugitives and some end up collaborating with the FBI as criminal informants.  In other cases, the FBI has removed fugitives when charges were dismissed or the critera for being on the list was not met (in their eyes).

The list is interesting to me because it's all about durations.  

{% blockquote %}
-  How long will a fugitive remain free once they're added to the list?

-  How does our risk of getting captured change over time after being added to the list?

-  What's the probability that we're caught over the next week, two weeks, year after being added to the list?
{% endblockquote %}

Analysis of durations falls squarely on the survival analysis statistical toolset.  The survival framework was designed ground-up to deal with these types of problems.  You might be thinking, 'Woah, this guy just took a simple set of questions probably solvable with Excel's Average function and made it sound complicated.' But if that's your tool as a fugitive, you'll underestimate the length free and perhaps act reckless in your escape.

Let's take the average lenth of time-to-capture simply by averaging the duration from being added to the list to capture.

{% gist 9448932 %}

Currently that average function says 402 days free. Let's get a distribution of this statistic by boostrapping.  For those unfamiliar, bootstrapping means to take a sample of the 491 fugutives <a href="http://en.wikipedia.org/wiki/Sampling_(statistics)#Replacement_of_selected_units)">with replacement</a>, say 30% of the 491, calculate the mean, save it, and repeat say 1,000 times. You end up with a distribution for your mean statistic. 

{% img center http://i.imgur.com/F4Xqubg.png %}

You can see that the statistic is centered around 402 days.  The 5% and 95% percentiles are 309 and 512 days, respectively.  So 402 +/- 101 days is your guess by this method.

What about the fact that we left 12 fugitives our of our analysis?  If we say that today is their "duration free", then we're assuming they're all captured today.  But they're not, we can do better.

I start by marking each fugitive as caught or not caught. In SA-speak this is censoring.  You're 'censored' if you haven't been caught and you've 'died' if you've been caught. Survival analysis uses simplifying language so that all the statisticians can communicate such that problem-specific language is abstracted. Another label for 'died' is 'failure.' This can lead to funny situations like me calling a fugitive being caught a 'death.'  Why death?  Well much of survival analysis was developed with medical trials in mind.

Once I've coded the fugitives, I measure the time from being put on the list to time caught OR today.  At that point I can use the Kaplan-Meier estimator (KM estimator) to get a non-parametric estimate of the mean that includes all of our data.

The KM estimator calculates the share that were captured for each duration in our dataset, of those that could have been captured. 

{% codeblock %}
df <- df[!is.na(df$censor) & df$days_to_capture >= 0, ]

# install.packages("survival"); library(survival)
print(fit <- survfit(Surv(days_to_capture, censor) ~ 1, data = df), rmean = "common")
Call: survfit(formula = Surv(days_to_capture, censor) ~ 1, data = df)

   records      n.max    n.start     events     **rmean** se(rmean)     median    0.95LCL    0.95UCL 
          491        491        491        479        613         92         84         69        106 
	      * restricted mean with upper limit =  15895 
{% endcodeblock%}

So we have 491 records.  Out of the 500 on the list, nine fugitives were captured before their listings were publicized.  I've removed these because we're playing that part of the fugitive and it's more fun to assume you're free when the publication happens.

The key is that our mean here is 613 days, more than 50 percent longer than our estimate that didn't take into account censoring (current fugitives).  Also our 5% to 95% confidence interval is wider, 368 days instead of 202 days.  That makese sense because we're including some long-free fugitives now.  By itself, it should make us suspicious that the confidence interval of the simple mean is too narrow, which would lead us to believe the mean more precise than it really is at estimating time free.

{% img center http://i.imgur.com/5krTTdB.png %}

Wow, your odds of surviving in the wild after 10 years is pretty minisucle.  We can see our mean-time estimate of 613 days depicted as a vertical <span style="color:red">red line</span>.

{% img center http://i.imgur.com/AyXmo6e.png %}

The cool thing about the survival framework is that you have many ways you can pivot.  There's all kinds of alternative interpretations that can be derived.  For example, by taking the probability of being caught over the survival curve yields a hazard rate:

{% img center http://i.imgur.com/v3oMzIb.png %}

It may not seem sexy, but it's a k-means nearest-neighbor smoothed hazard function fit to our data. It's interpretation is neat -- instananeous likelihood of being caught. See it fall from 0.025 to 0.005 in about three months? That means in just three months your risk of being caught falls 80%!  Also notice how flat it is after 90 days, your risk of being caught basically stays the same after a few months of running.

We can also take the integral of this hazard function and figure out cumulative risk of being caught.

{% img center http://i.imgur.com/slMPDsL.png %}

The cumulative hazard function (CHF) measures rate of risk. Think Sonic the Hedgehog, or Mario without a mushroom.  Getting caught is like dying in those games, you're reset.  Die in Sonic?  Boom, new Sonic.  That's what the graph above shows for a typical fugtive.  So by day 1,000, you're already likely to have been captured twice (and reset twice).

This is how the full CHF looks through the [longest fugitive](http://en.wikipedia.org/wiki/Leo_Burt), out at 15,000+ days.[^1]  So whereas most fugitives would have been caught four times over, he's been going strong:

{% img center http://i.imgur.com/w5p76Lr.png %}

It's common in survival analysis to fit parametric distributions to survival data.  If you're able to find a model that fits well, it can be used for inference or prediction. 

{% img center http://i.imgur.com/I38qSLR.png %}

This data is best fit by a [Log-normal](http://en.wikipedia.org/wiki/Log-normal_distribution) distribution.  Behind the Weibull distribution, the log-normal is probably second most-popular.

If we look at the log-normal by itself, the fit looks pretty good!  Notice again the small crosses that represent fugitives currently on the run?

{% img center http://i.imgur.com/nhZpLdf.png %}

{% codeblock %}
print(fit2 <- fitdistcens(censdata = df[df$left > 0, c("left", "right")], 
      "lnorm"))

Fitting of the distribution ' lnorm ' on censored data by maximum likelihood 
Parameters:
        estimate
	meanlog 4.430638  # exponentiate to get ~84 days
	sdlog   2.149077
{% endcodeblock%}

Now that we have our model, let's figure out the probability that you evade capture for a set of days:

{% codeblock %}
quantile(fit2)
Estimated quantiles for each specified probability (censored data)
                     p=0.1    p=0.2    p=0.3   p=0.4  p=0.5    p=0.6   p=0.7    p=0.8    p=0.9
	    estimate 5.346729 13.76225 27.21208 48.7242 83.985 144.7634 259.204 512.5239 1319.214
{% endcodeblock %}

Each of the estimates above is in days and corresponds to the probability you're caught.  It may seems strange, but I'm stating the probabilities upfront: 10% to 90% by increments of 10%.

So the probability that you're caught within 5.34 days is about 10 percent.  Using another probability, there's a 90 percent chance that you're caught within 1,319 days (about 3.6 years).

What strikes me about this is that there is a 1 in 5 shot you're caught within two weeks. After another two weeks it rises to a 1-in-3 shot you're caught.  The first two weeks are very much so the most crucial to the fugitive.  The first two weeks is equivalent to the 16 weeks after in terms of risk.

So there you have it!  The FBI's Ten Most Wanted list as analyzed with survival analysis.

[^1]: The FBI removes fugitives, but I don't think that's really a fair measure.  I'm after a measure of my likelihood to get locked up once I'm put on the list.  It's hard to believe that the pressure the List creates simply disappears after a fugitive is removed. Apparently Leo is in Canada, where I live, yikes!  He's been free for 15,892 days or over 43 years!

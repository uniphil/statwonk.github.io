---
layout: post
title: "P-value does not stand for profit."
date: 2013-10-06 16:26
comments: true
categories: statistics, business, data science 
---

With the data science revolution, a whole new round of statisticans will be thrust into dealing with business problems.  What they're bound to find is that their beloved p-value is not going to stand up to the market as it does in academic journals.  For the past thirty years, the computational cost of the simple t-test has fallen dramatically, while the academic reward of publication has stayed the same.  The result is that more newly minted analysts than ever are confused by the blank stare they receive from their manager when they tell the manager, "these two samples are statistically significantly different at the 5% level."  The manager asks, "{% raw %}<span style="background-color:#ffe026">so what? How much profit will we make?</span>{% endraw %}" To which the statistician replies, "{% raw %}<span style="background-color:#ffe026">the p-value is less than 5%.</span>{% endraw %}"


{% raw %}<span style="background-color:#ffe026"></span>{% endraw %}
Here's one simple example. Suppose I have two site variations I want to test.  I'd like to test a new red button against our old green button.  I'm interested to know if the red button outperforms green with respect to clicks.

We'll simulate this with the `R` language.  We can draw a `0` or `1` with known probabilities to model our buttons.  I'll set the conversion rate for the green button at 3% and 5% for our new red button, a 66% percent improvement.

{% codeblock lang:r %}

old_green_button <- rbinom(n = 100000, 1, p = 0.03)
new_red_button <- rbinom(n = 100000, 1, p = 0.05)

prop.test(table(old_green_button, new_red_button))
{% endcodeblock %}

In the code above, I first randomly generate `100k` binomial outcomes (really bernoulli r.v.) for each button. Remember, the conversion rates are 3% and 5%, respectively.  Next, I apply the Chi-square test for equality of proportions.  This is the standard statistical test for testing if two proportions (conv. rates) come from the same population.  That is, should I expect these two buttons to yield a different conversion rate (and profit) going forward?

Below are the results from one run, and despite the new red button being 66%
better than the green button, our p-value sits at 90%, well above the
traditional statistician's 5% p-value cutoff.

{% codeblock lang:r %}

2-sample test for equality of proportions with continuity correction

data:  table(old_green_button, new_red_button)
X-squared = 0.0185, df = 1, p-value = 0.8919
alternative hypothesis: two.sided
95 percent confidence interval:
 -0.008662492  0.007240556
 sample estimates:
    prop 1    prop 2 
    0.9498581 0.9505691 
{% endcodeblock %}

Now, I'll repeat this experiment 1000 times and show you how the p-value
shakes out.

{% codeblock lang:r %}
library(ggplot2)
library(ggthemes)

times <- 1000
p_values <- rep(NA, times)
for(i in 1:times){
  set.seed(i)
  old_green_button <- rbinom(n = 100000, 1, p = 0.03)
  new_red_button <- rbinom(100000, 1, p = 0.05)
  p_values[i] <- prop.test(table(old_green_button, 
                                 new_red_button))$p.value
  
  print(i)
}

df <- as.data.frame(p_values)
ggplot(df, aes(x = p_values)) +
  geom_density(fill = "grey50", alpha = 0.5) +
  geom_vline(xintercept = 0.05, colour = "red", size = 2) +
  ggtitle("Only 6% of p-values are less than 0.05") +
    ylab("Density") +
    xlab("P-values") +
  theme_few() +
  theme(axis.text = element_text(size = 15),
        plot.title = element_text(size = 20))
{% endcodeblock %}

The result shows that only 6% of p-values will be less than 0.05%!  The
sacred statistical decision rule would have you leave a 66% percent
conversion rate increase on the table and this is why p-value does not stand for profit.

<img src="http://i.imgur.com/9vl6z1x.png"></img>

In my next post, I'll analyze exactly why using p-values above other
methods leads the data scientist astray.  I'll introduce how I use Bayesian
analysis to avoid the p-value pitfall.


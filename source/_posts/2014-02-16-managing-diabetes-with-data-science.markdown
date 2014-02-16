---
layout: post
title: "Managing diabetes with data science"
date: 2014-02-16 11:13
comments: true
categories: 
---

As I've [written about before](http://statwonk.github.io/blog/2014/01/05/visualizing-diabetes-data/), my partner has Type 1 diabetes.  The disease is incredibly complex and its manifestation is the product of many genetic and environmental factors.  The way it basically works is called "basal / bolus".  Each non-diabetic person has a baseline rate of insulin (basal) produced by their pancreas.  When that person eats the pancreas gives a burst of insulin called a bolus.

The diabetic tries to emulate this process either with needles, pills, or an insulin pump. The side, [Diabetes Manager](http://diabetesmanager.pbworks.com/w/page/17680318/The%20Management%20of%20Type%201%20Diabetes%20) has a great pictoral representation of the process:

{% img center http://i.imgur.com/cfu1OQZ.png %}

With an insulin pump there are a number of levers you can toggle to try and emulate the process above.

**For example:**

1.  The basal rate (a little bit of insulin every 15 minutes).
2.  The size of the bolus given the size of the meal.
3.  Since boluses can be stacked (think several course meal), the rate that they have an effect is an input parameter.
4.  The degree to which your body responds to insulin (sensitivity), which determines the size of "corrections" necessary given blood glucose vs. target.

There are other levers as well, but these are the main ones.

Normally a diabetic would take their data to an endocrinologist that would change these levers every three months. However, we've found that three months is much too long of a period to wait for adjustments.  The body's chemistry is complex, if one is stressed rates may need to go up, if one is eating more of a low-carb diet they may need to come down.

This is where my expertise can be helpful.  My partner's pump records all kinds of measurements, one of which is every blood glucose reading taken.

One way I've found helpful in looking at the data is a cross-section on hour of the day.  Diabetic patterns move widly intra-day but there tends to be patterns over time. If you're "high" in the morning today, you're likely to be high in the morning tomorrow without some kind of intervention.  Or it could be a fluke.

The following graph shows the last three month's of my partner's blood glucose data.  It shows a boxplot for each hour of the data and the actual data plotted behind. The desired range is between the horizontal red lines (70 - 200)  Also I added a blue smoother.  Each element has a purpose.  The data makes sure that I know where the data is sparse and boxplots may be less reliable.  The boxplots show me where 50% of the datais contained and gives me an idea of variance by hour.  Finally the blue smoother keeps me focused on the underlying trend:

{% img center http://i.imgur.com/AgfoJOC.png %}

It's really amazing how the eyes can pick out patterns without numeric statistics.  The boxplots above definitely use statistics, but they show the results in a visual way that makes things much clearer than I think a table would.

This is where I need to insert my 'fairness explaination'.  You may notice that my partner has some readings out of range (below 70 or 200 mg/dL). No diabetic can manage all of the factors they're faced with in a perfect manner.  No person is perfect. However, my partner by conventional medical standards is an A+ patient.

**Recommendations**

Given the data above, we plan to make a 10% - 15% increase in her basal rate starting 17h (5pm) to about 3h (3am). We want to make sure that we give plenty of time for this high amount to burn off before 7h (7am), when hypoglycemic events (< 70) tend to start. Also, spikes after 5pm and 9pm are likely due to snacks, this indicates we need to increase the bolus size at this part of the day.

So there you have it!  This is a view into how a data scientist and Type 1 get along to manage diabetes!

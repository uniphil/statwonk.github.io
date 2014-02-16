---
layout: post
title: "How statisticians lost their business mojo"
date: 2013-10-11 09:45
comments: true
categories: 
---

The manufacturers of Japan will tell you of W. Edwards Deming.  His famous 1950s lectures in Japan popularized quality control and continuous improvement. Six sigma strategy is born out of Deming's vision.

{% img left /images/deming.gif 200 250 Deming %}

Consider when you drink your Guiness beer, world reknown, that it's recipe was
perfected by a statistician, W. S. Gosset.  (He created the t-distribution in
1909 to do this).

<span style="font-weight:bold">These statisticians had an amazing sense of business mojo.</span>

Since the 1960s, we [statisticians] have lost this mojo.  And the reason is that
they've followed R.A. Fisher's significance testing orthodoxy without question.  Fisher created the p-value we know today and many
think of as synonymous with statistics.  P-value measures precision, not effect size.  That's it, that's why our business mojo is gone. P-values are the gate keeper to academic publication, NOT effect sizes.

Effect size is "oomph", "mojo", "level of profit", "return on investment."  Yet
statistician's obsession with the p-value and precision alone leads to
hilariously inept results.

Take this extract from a leading journal of management science,

{% blockquote Thomas J. Douglas and Lawrence D. Fredendall, Decision Sciences 35 (3 summer 2004) %}
Our first hypothesis suggested that visionary leadership was related to higher
levels of internal and external cooperation.  We used two measures to represent
internal and external cooperation, quality philosophy and supplier cooperation.
Top management team involvement, our measure of visionary leadership, was
significantly related to both quality philosophy (t = 10.80, p < .001) and
supplier involvement (t = 7.59, p < .001). <span style="background-color: #ffe026">Therefore, Hypothesis I is supported.</span>
{% endblockquote %}

Notice that there's zero mention of effect size?  Just that the p-values are
less than 0.05.  Ironically this article is titled, "Evaluating the Deming
Management Model of Total Quality in Services."  So how much did Deming's model
contribute to internal and external cooperation?  The authors never say.  They
technically report it in one table, but never mention it as a conclusion of
worth.

This is what Deming says about significance testing,

{% blockquote Deming http://books.google.com/books?id=JWLIRr_ROgAC&pg=PA116&lpg=PA116&dq=avoid+passages+in+books+that+treat+confidence+intervals+and+tests+ofsignificance,+as+such+calculations+have+no+appliction+in+analytic+problems+in+science+and+industry&source=bl&ots=GvsUzNkqEf&sig=B0otz7pBntQaNhEocw4hFaWBl_U&hl=en&sa=X&ei=KjJYUtjIEMTgyQGthICADw&redir_esc=y#v=onepage&q=avoid%20passages%20in%20books%20that%20treat%20confidence%20intervals%20and%20tests%20ofsignificance%2C%20as%20such%20calculations%20have%20no%20appliction%20in%20analytic%20problems%20in%20science%20and%20industry&f=false 1982 %}
Ther are many other books on so-called quality control [Deming wrote].  Each
book has something good in it, and nearly every author is a friend and colleague
of mine.  Most of the books nevertheless contain bear traps, such as reject
limits, ... areas under the normal curve, acceptance sampling ... The student
 should also <span style="background-color: #ffe026">avoid passages in books that treat confidence intervals and tests of
significance, as such calculations have no application in analytic problems in science and industry.</span>
{% endblockquote %}

<span style="background-color:#ffe026">So how can statisticians get their business mojo back?</span>

As [Stephen T. Ziliak and Deirdre McCloskey](http://www.deirdremccloskey.com/docs/jsm.pdf) note, "Deming himself asked of any service or product how in the eyes of the user it could be improved. No matter."

Fisher (p-value inventor) was very much against using measures of
productivity, profit, reliability, etc. in science. He states the difference,

{% blockquote Fisher http://www.phil.vt.edu/dmayo/personal_website/Fisher-1955.pdf 1955 %}
I am casting no contempt on acceptance procedures [read effect size, read profit, revenue, cost reduction], and I am thankful, whenever I travel by air, that the high level of precision and reliability required can really be achieved by such means.  But the logical differences between such an operation and the work of scientific discovery by physical or biological experimentation seem to me so wide that the analogy between them is not helpful, and the identification of the two sorts of operation is decidedly misleading.
{% endblockquote %}

<span style="background-color: #ffe026">So Fisher is very quietly telling us, yes, my methods are not for
business.  They're for science.</span>  The massive implications of that for past research aside, what can we replace Fisher's methods with?

We want to apply newer more sophisticated methods that respect effect sizes, and will lead us to large gains for the company.  If we have to go back over the mechanism with Fisherian testing to enhance precision, fine.  Search for large effects first as Deming's philosophy suggests.

We should also listen to Leo Brieman when he suggests, "we can move away from exclusive dependence on data models and adopt a more diverse set of tools."  In his seminal,"[Statistical Modeling: The Two Cultures](http://projecteuclid.org/DPubS?service=UI&version=1.0&verb=Display&handle=euclid.ss/1009213726)," he proposes statisticians be humble in the face of problems.  Refrain from imposing linearity and variable selection by tests of significance (p-values).  Instead minimize rates of error in held out test set data using a training set.  This opens up a HUGE new space to do analysis, all without p-values! Non-parametric methods like [`random-forest`](http://stackoverflow.com/questions/tagged/random-forest), SVM, boosting, bagging, sophisticated loss functions (read maximizing revenue with respect to minimizing cost, profit!).

There are other methods, too, such as Bayesian inference.  These methods are really great for giving you degrees of a belief in a hypothesis.  If you're working on marketing problems, like I am, then you know pretty well ahead of time what to expect in terms of a conversion rate.  And if you use Chi-square or t-tests to choose winners, you know that your experiments commonly require 200k plus participants to achieve statistical significance (p-value < .05) even for a 20% increase in conversion rate.  Fishers methods are much much too coarse for web app startup companies.  Instead, we're more accepting of Type 1 error, which for us is that a marketing campaign under performs.  We are also looking for ostensibly the best user experience possible, or increase in profit, something meaninful in effect size: a game changer.  Amazingly, Fisher will miss these game changers.  Bayes methods allow us to use our prior knowledge that conversion rates range between 1 - 5% (and follow a beta distribution) to get much more sensitive tools.

<span style="font-weight:bold">Summary:</span>

1.  <span style="background-color:#ffe026">Don't use p-values and significance testing in business.</span>
2.  Use algoritmic methods that minimize or maximize something that matters to you.
3.  Focus on the effect size (Deming).  Let's be certain of having a meaningful effect on our customer's experience, not just certainty about some effect regardless of of the size (Fisher).

Who am I to critize 70+ years of statistical application?  I'm a data
scientist, a business statistician who tried to use Fisherian methods in
business.  I have quickly learned that they're nearly useless here.
Instead, there's a whole new set of useful tools that open up to the
business analyst that realizes significance testing is nearly worthless.
We'll no longer be plagued with a lack of observations.  W.S. Gosset was able to
improve Guinness beer sometimes 3 obsevations at a time.  No longer will the
practicioners (marketers, management, stakeholders) scratch their head when you
come back with a 30% a/b test lift and a p-value of 0.60.  Neither will they be
fluxomed when you say, "look, the p-value is less than .05, we have a
relationship here!"  So, what?  If you are very certain that you convert 2 more
users out of 1 billion clicks, who cares?  What Deming, Gossett, and now myself
are looking for are large effect sizes, even if they vary!

I have to give huge thanks to Stephen T. Ziliak and Deirdre N. McCloskey for
nurturing my thoughts in this area, who's book, "[The Cult of Statistical Significance](http://projecteuclid.org/DPubS?service=UI&version=1.0&verb=Display&handle=euclid.ss/1009213726)" should revolutionize scientific statistical application, even if it fails to do so like those before those authors.

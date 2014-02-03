---
layout: post
title: Goodbye scripts, hello R packages!
date: 2013-11-03 20:21
comments: true
categories: R, programming, development, modularization
description: Moving from long R scripts to consice and DRY R code requires using R packages.  Here I document my first interaction with creating packages.
---


This weekend I reached the breaking point with my R codebase.  I've heard it said before that the best thing about R is that it was created by statisticians and the worst thing about R is that was created by statisticians.

R is a domain-specific language.  My perspective as a student of R, is that the culture is heavily oriented towards writing scripts.  Academic statisticians as a culture have, in my opinion, very poor programming skills.  There are exceptions like Hadley Wickham, but it appears even he has been forced to leave academia.  Probably because his game changing R packages weren't recognized in proportion to the mindless publication of p-values or yet another Fisherian SAS implementation.  I could be wrong, is it a sabbatical? are his R packages recognized as world-class by his collegues?

My own academic experience was a mixed bag.  I had an absolutely wonderful advisor that programmed in R (and a couple of other professors, too).  The rest of the department were using SAS.  If you use R (and have used SAS), I don't have to tell you what that meant for me in terms of programming education in school.

I've been on my own when it comes to learning the fundamentals of modern programming.  Having only used procedural languages (R [mostly], SAS, and Stata), the transition to object-orientation was both difficult and a breath of fresh air.

I understood that having hundreds and hundreds of scripts meant that I was effectively re-writting the wheel on a daily basis.  I want to escape from that. Luckily, I work for an [amazing company](http://teamtreehouse.com) that happens to be in the tech. education business.

It wasn't until a couple of coworkers suggested I learn Ruby (Treehouse rocks for learning this language by the way!), that I really began to engage object-orientation and modularization on a daily-basis.  "Wait, you mean if I define this in a class with a given namespace, I can call the method from anywhere?"

R is also an object-oriented language, but it's application seems fairly limited and you're usually crossing your fingers in hopes that a package maintainer has thought to overload the ``` plot() ``` or ``` summary() ``` functions.

Modularization appears to come largely through ``` source() ``` and R packages.

To me, package development has always seemed intimidating.  I've had several false starts.  That was made more likely by my past use of Windows as a development OS.  Bad bad bad choice.  For the past six months, I've moved to Ubuntu (vim and tmux, and I have to give huge credit to [RStudio](http://www.rstudio.com/) even though I'm moving away from it) and not looked back.

Today, in my frustration I decided to give package creation another shot.  And it worked!  To my delight, I was able to create the package, add some functions, compile it, load it, and start using my code in a clean re-usable and modular way.  Success!

I have to give almost exclusive credit to Hadley Wickham and his super clear explaination of [package authoring basics.](http://adv-r.had.co.nz/Package-basics.html)

Hadley has built some amazing package development tools, like [devtools](https://github.com/hadley/devtools) which proports to make the package structure easier to use than not (and I believe it).  He actually says in the documentation that he'll send you a handwritten apology if you piss off the R-core team due to a ``` devtools ``` bug.  Sounds like a goal. :-)  He also has created a package for [test-driven development](https://github.com/hadley/testthat) in R.  Another aspect of Ruby that I've loved, but never used in R.  And I'm not alone, I know that I'm not.  I've never met another R programmer face-to-face that uses unit tests.  Looking forward to it!  Finally, Hadley completes the grand slam (with Peter Danenberg and Manuel Eugster) with the ``` roxygen2 ``` [package](https://github.com/klutometis/roxygen), which allows package developers to write self-documenting code.

So today I'm feeling great about getting my first package off the ground. I've already spent a couple of hours breaking my old scripts into modules.  I'm calling the package ``` treehouser ``` since it's for work, and follow's Hadley's naming convention.  I'm hugely greatful to Hadley (I've already gotten an amazing amount of milliage out of ``` ggplot2 ```).  Thank you!

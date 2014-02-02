---
layout: post
title: "Dipping my toes in the JavaScript and Node.js waters"
date: 2014-02-02 11:03
comments: true
categories: node JavaScript R CoffeeScript closure first-class functions 
---
I've recently been attracted to learning JavaScript by way of D3.js.  We all know from the Hacker News posts that 'ole JS has seen a huge revitalization and if you want to push data around with D3, you might as well take advantage of the powerful language its build on. So I started with [Crockford's](http://crockford.com) *JavaScript: The Good Parts.*

Before reading the book, JavaScript was a means to take care of incidental calculations that may need to be done when creating graphs.  Dates are always a pain in the ass, so hey, "I might want some JS ability to work with them."

Instead, I'm now looking at JavaScript as a potential alternative to R in certain spaces. Having the ability to jump from language to language (maybe even tie them together with a shell script) is a huge boon for data science work.  Each language has it's pros and cons and usually some mix of them is the best most efficient path from raw data to inights.

Only recently did I even realize that the browser is a compiler while reading up on Google's [V8 engine](https://code.google.com/p/v8/) and Mozilla's [Gecko engine](http://en.wikipedia.org/wiki/Gecko_(layout_engine\)). An aquantance at a recent party put it, " ... Google put tons of work into V8, it's a very efficient and speedy." This is pretty exciting in that it turns the browser from a space simply to display data to a computing engine.

For example, suppose you want to implement a marketing attribution model. In order to do this, you want to use a Cox Proportional Hazard model to calculate conversion rate liklihoods. Are you going to download the data, apply the model in R, create weights, upload to the database, and then analyze?  You could, or you could fit that Cox PH model with JavaScript on-the-fly and be done with it.  [This person did just that and you can checkout the implementation for yourself](http://statpages.org/prophaz.html):

{% img center http://i.imgur.com/BjZYck0.png %}

My usual development goto is a Sinatra / rack-app or maybe even full-fledge rails app.  Whether it's D3, a simulation, or something else, a rails app is usually overkill.  [Node.js](http://nodejs.org) makes even Sinatra look like overkill for JS development.  Node allows you to easily spin up a server to host JavaScript (and other things):
<br>

Here's a hello world example, with Node installed, just run it with ```node your-filename.js```
{% codeblock lang:javascript %}
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('Hello World\n');
    }).listen(1337, '127.0.0.1');
    console.log('Server running at http://127.0.0.1:1337/');
{% endcodeblock %}

Working with just JavaScript? Use vim and the console with ```node some-script.js```.

As I mentioned, I've been working through [Douglas Crockford's](http://crockford.com) [JavaScript: The Good Parts](http://www.amazon.com/exec/obidos/ASIN/0596517742/wrrrldwideweb).  The design patterns just seem to fit my brain.  I've been using closures (ineffectively) for years now.  It turns out that they can be very powerful in conjunction with R or JavaScript's first-class functions. 

[First-class functions](http://stackoverflow.com/questions/705173/what-is-meant-by-first-class-object) and [closures](http://javascript.crockford.com/private.html) are also features of R, my native language. 

Bootstrapping is something that data scientists do commonly.  Basically instead of having a statistic like a mean, it gives us the distribution of that statistic. Bootstrapping isn't hard, it could be as simple as sub-sampling data 100 times and calculating the mean each time. That collection of means will give you a good idea how the true mean of the population is distributed. The hard thing about bootstrapping is keeping track of all of the parts.  You might want to bootstrap means, median, upper and lower confidence intervals, etc.  There's a lot to keep track of at times!

I'll usually do something like this in R:

{% codeblock lang:r %}
nboot <- 100 # 100 bootstrap means
boot_means <- rep(NA, nboot)

data <- runif(1000)
for(i in 1:nboot) {
  boot_means[i] <- data[sample(data, 10, replace = T), ] 
}
{% endcodeblock%}

And that looks pretty simple, but it's just one statistic.

Below is a bootstrapping example showing the use of closures and first-class functions in R from the [Left Censored](http://leftcensored.skepsi.net/2012/12/02/closures-in-r-a-useful-abstraction/) blog

{% codeblock lang:r %}
make_container <- function(n) {
  x <- numeric(n)
  i <- 1
  
  function(value=NULL) {
    if (is.null(value)) {
      return(x)
    }
    else {
      x[i] <<- value
      i <<- i + 1
    } 
  }
}

nboot <- 10
sample_size <- 10
bootmeans <- make_container(nboot)

for (i in 1:nboot) {
  bootmeans(mean(runif(sample_size, 0, 100)))
}
round(bootmeans(), 0)
 [1] 37 66 54 44 57 50 55 64 65 55
{% endcodeblock %}

This is cool because the bootstrappng pattern is factored out into the ```make_container``` function.  The ```function(value=NULL)``` is a first-class lambda in R, but it looks like a classic first-class JS lambda.

Now lets look at a similar implementation in JavaScript:

{% codeblock lang:javascript %}
var make_container = function (n) {
  var that = this, // for namespacing, thanks Crockford!   
      x = [],
      i = 0;
  return function (value) { // example of first-class functions! 
    if (value === null) {
      return x;
    } else {
        that.x[i] = value;
	that.i += 1;
    };
  return x.slice(0, x.length - 1);
  };
};

var getMean = function(x) {
  var sum = 0;
  for(var i = 0; i < x.length; i++) { sum += parseInt(x[i]); };
  var avg = sum/x.length;
  return Math.floor(avg);
}, // amazingly, I couldn't find a JS mean() function
  nboot = 10, // x bootstrapped means
  data = [],
  sample_size = 100; // how many obs in each sample?
  get_data = function() { // function to pull data
    for(var j = 0; j < sample_size; j++) {
      data[j] = Math.floor(Math.random() * 100);
    };
    return data;
  }

// Let's just look at one sample mean
console.log("arithmetic mean: " + getMean(get_data()));

var bootmeans = make_container(nboot);
for(var i = 0; i < nboot; i++) {
  bootmeans(getMean(get_data()));
}

// Now x bootstrapped means
console.log(bootmeans())
{% endcodeblock %}

It's much longer and really dirty, but hey, it's in JavaScript! I also have a nice interpreter / host with Node.js. [It's not an exact copy, **JS doesn't have nice statistical functions like R, such as mean!!!!**]

I'm also interested in how I can leverage learning JavaScript [**given how similar it is to R**](http://www.yaksis.com/posts/coffeescript-for-r.html). As that author shows, the following code block is 100% valide JavaScript and R:

{% codeblock lang:javascript %}
fib = function(n) {
  if (n==0) {
    return (0)
  } else if (n==1) {
    return (1)
  } else {
    return (fib(n-1) + fib(n-2))
  }
}
{% endcodeblock %}

For now I'm just excited that I have a space beyond the web worker to throw data around!

<!--
id: 31072123211
link: http://blog.olgabotvinnik.com/post/31072123211/logistic-regression-tutorial
slug: logistic-regression-tutorial
date: Fri Sep 07 2012 13:17:00 GMT-0700 (PDT)
raw: {"blog_name":"sciencemeetproductivity","id":31072123211,"post_url":"http://blog.olgabotvinnik.com/post/31072123211/logistic-regression-tutorial","slug":"logistic-regression-tutorial","type":"text","date":"2012-09-07 20:17:00 GMT","timestamp":1347049020,"state":"published","format":"markdown","reblog_key":"4TbsoDNl","tags":["R","logistic regression","tutorial","star trek"],"short_url":"http://tmblr.co/ZStENuSy2lbB","highlighted":[],"note_count":1,"title":"1, 2, 3 ... logistic regression!","body":"<h2>R Tutorial in Logistic Regression</h2>\n\n<p>Recently I read a <a href=\"http://www.nature.com/msb/journal/v3/n1/full/msb4100180.html\" target=\"_blank\">paper</a> \nthat used logistic regression in its methods\nand realized I had no idea what logistic regression <em>is</em>, so I made this tutorial\nfor myself and others to learn about it. I wanted to create a tutorial that created \nrandom data that anyone could use, and taught you a bit about R in the process. This \nis my first tutorial and feedback is encouraged.</p>\n\n<p>This tutorial on the statistical machine-learning method known as Logistic\nRegression is implemented in <a href=\"http://r-project.org\" target=\"_blank\">R</a>. Familiarity with programming\nsyntax, such as vector indexing, is assumed but I tried to make this tutorial\nas accessible as possible for those with less programming experience.</p>\n\n<h1>What in Merlin&#8217;s beard is <em>logistic regression?</em></h1>\n\n<h2>First, what is <em>regression</em>?</h2>\n\n<p>You may have heard of &#8220;linear regression,&#8221; which fits a $y=mx+b$ line to a set of points, such as in the figure below. I&#8217;m not going to go into the details of linear regression, but you can <a href=\"http://www.stat.yale.edu/Courses/1997-98/101/linreg.htm\" target=\"_blank\">read about it</a> if you like.</p>\n\n<p><img src=\"http://media.tumblr.com/tumblr_mdorakn4cw1rw6gvj.png\" alt=\"Linear regression matches a best-fitting line to a set of data.\"/></p>\n\n<!-- Add description of Logit function -->\n\n<p>Logistic regression is similar, but instead of having two continuous variables as above, one of the variables is binary (0 or 1) and the other is continuous. But we&#8217;re not fitting a line, we&#8217;re fitting a <em>logit</em>, which is a totally different function that estimates probability for a continuous variable rather than assigns classification. For example, we could predict the probability\nof cancer relapse given some gene expression values. Or the probability of a heart attack\ngiven a person&#8217;s cholesterol levels. We would base these probabilities on\npreviously encountered data, a method of machine learning known as \n&#8220;Supervised Learning.&#8221; You tell the computer what the answer is for many examples, and then the computer has to guess whether the outcome is 0 or 1.</p>\n\n<p>Think of it this way: you look at hundreds of values representing the number of \n<em>Star Trek: The Next Generation</em> episodes a person has watched, with a teacher \npeering over your shoulder, and he tells you whether each value is associated \nwith attending a <em>Star Trek</em> convention or not. Then the teacher leaves, and\nyou&#8217;re given a new number of <em>Star Trek: TNG</em> episodes, $x$, and you have to give a \nprobability $p$ whether the person who has seen this many episodes has been to a \nconvention. The probability of $x$ episodes watched and no convention attendance\nis $1-p$, so by knowing $p$, we also know $1-p$ and we only need to specify one value.</p>\n\n<h2>Regression is not the same thing as classification</h2>\n\n<p>This is an important distinction between classification and regression. If we were\ndoing classification, we would just say whether the person who has seen $x$ number \nof episodes has gone to a convention or not. But\nwe don&#8217;t always want a binary outcome. The cholesterol-heart attack association is a \ngreat example of this. You probably don&#8217;t want to tell a patient, &#8220;You <em>will</em> have a \nheart attack,&#8221; but rather say &#8220;You have an $x$% probability of having a heart\nattack, given your current cholesterol levels.&#8221; Same goes for cancer relapse. A\nbinary yes/no answer can obscure the data. Also, a probability of 0 is essentially \nimpossible. Could you truly, definitively, say that a person will <em>never</em> have a \nheart attack? Not until their death, but then all possible heart attack events have\nalready been observed. Sorry for the gruesome-ness.</p>\n\n<h2>So what about this <em>logisitic</em> thing?</h2>\n\n<h3>First, we have to talk about <em>odds</em></h3>\n\n<p>We talked about predicting the probability of some event. But we&#8217;re going to use \n<em>odds</em> instead. If the probability of having attended a <em>Star Trek: TNG</em> convention\nis $p$, then the odds are:</p>\n\n<p>$$\\text{odds} = \\frac{\\text{probability of event ocurring}}{\\text{probability of event not occuring}} = \\frac{p}{1-p}$$</p>\n\n<p>Say after watching a certain number of episodes, the probability of attending a \nconvention is $p=0.7$. Then the odds of attending a convention are</p>\n\n<p>$$\\frac{0.7}{0.3} = 2.333\\ldots$$</p>\n\n<p>But the odds of not attending a convention are</p>\n\n<p>$$\\frac{0.3}{0.7} = 0.4285\\ldots$$</p>\n\n<p>As you can see, odds and probability are not the same. Odds can be greater than 1, and probability must be between 0 and 1.</p>\n\n<h3>Next, we have to define <em>log-odds</em></h3>\n\n<p>It would be nice if these odds were somehow symmetric and we could intuitively make \ninferences just by looking at the value. This is where the logarithm comes in. If we \ntake the <a href=\"http://en.wikipedia.org/wiki/Natural_logarithm\" target=\"_blank\">natural log</a> (ln), aka \n$\\log_e$ of the odds of attending a convention, we get</p>\n\n<p>$$\\ln \\frac{0.7}{0.3} = 0.8472\\ldots$$</p>\n\n<p>And the log-odds of <em>not</em> attending a convention:</p>\n\n<p>$$\\frac{0.3}{0.7} = -0.8472\\ldots$$</p>\n\n<p>The opposite of attending a convention is the negative of attending a convention! \nNow let&#8217;s stop and think. <em>Why</em> is this? This is because the natural log of 1 is 0,\nso it intersects at 0 as in the plot below. Then the natural log of anything smaller \nthan 1 is negative, approaching negative infinity as the number gets closer and \ncloser to 0. The natural log of anything larger than 1 is positive, and approaches \npositive infinity as the number gets larger and larger, but slower than we approach \nnegative infinity from the other side.</p>\n\n<p><img src=\"http://media.tumblr.com/tumblr_mdorb7uHwT1rw6gvj.png\" alt=\"Natural log function\"/></p>\n\n<p>This symmetry is helpful to guide our intuition about events occurring or not \noccuring.</p>\n\n<p><em>FYI:</em> We used the natural logarithm here because that&#8217;s the standard. You could\nreally use any log base you like, even $\\log_\\pi$ would do, but it&#8217;s just not\nused that often.</p>\n\n<h3>From log-odds to <em>logits</em></h3>\n\n<p>The log-odds are also called the <em>logit</em> of the probability:</p>\n\n<p>$$ \\log(\\text{odds}) = \\text{logit}(p) = \\ln\\left( \\frac{p}{1-p} \\right) $$</p>\n\n<p>In logistic regression, we set</p>\n\n<p>$$\\text{logit}(p) = a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n$$.</p>\n\n<p>Where $n$ is the number of independent variables you have. In our case, we just have one independent variable, $x = x_1$, the number of <em>Star Trek: TNG</em> episodes viewed. But we could add to the complexity and also have $x_2$ be the number of <em>Star Trek: The Original Series</em> (<a href=\"http://mashable.com/2012/09/07/google-star-trek/\" target=\"_blank\">happy 46th anniversary!</a>) episodes viewed, $x_3$ be number of <a href=\"http://en.memory-alpha.org/wiki/Latinum\" target=\"_blank\">latinum strips</a> owned, and $x_4$ be <a href=\"http://www.amazon.com/Star-Trek-Conversational-Marc-Okrand/dp/0671797395\" target=\"_blank\">Klingon language proficiency</a>. But we&#8217;re going to keep it simple and stick to our one $x$.</p>\n\n<p>Now we could do regular ol&#8217; linear regression on this, but there are several issues.</p>\n\n<ol><li>We have a binary variable for our dependent variable (aka the $y$), and linear regression assumes a continuous distribution for the dependent variable. With probability as the output, we cannot have values less than 0 or greater than 1, and linear regression does not conform to this.</li>\n<li>The variance of $y$ is not constant across values of $x$. The variance is $p(1-p) = pq$ ($q$ is short for $1-p$), and if we have a probability of $0.50$ of attending a <em>Star Trek</em> convention, then we have $0.5\\times 0.5 = 0.25$ odds. But if we have $p=0.9$, then the variance $pq = 0.9\\times 0.1 = 0.09$, and $0.25 \\neq 0.09$. Thus the assumption that variance of $y$ is constant across values of $x$ is invalid. This is also called <a href=\"http://blog.minitab.com/blog/statistics-and-quality-data-analysis/dont-be-a-victim-of-statistical-hippopotomonstrosesquipedaliophobia\" target=\"_blank\">homoscedasticity</a>. </li>\n<li>If you want to do significance testing (you maybe familiar with <em>p</em>-values or <em>R</em>-values to describe how significant a result is), the assumption that the errors of prediction, $Y-Y^\\prime$ are <a href=\"http://en.wikipedia.org/wiki/Normal_distribution\" target=\"_blank\">normally distributed</a> doesn&#8217;t work. $Y$ only takes on the values 0 and 1, so there&#8217;s no way you can create a continuous distribution such as a normal distribution with just two values.</li>\n</ol><p>Logits are not very intuitive as a unit, even though they are very helpful when looking at odds. But we need to get back to probabilities so we can give a probability of convention attendance. We&#8217;ll have to exponentiate both sides to get rid of the natural log. I&#8217;m going to keep the long-form version of the equation so you can see how it works for more variables.</p>\n\n<p>$$\n\\begin{align}\n\\text{logit}(p) &amp;= \\ln\\left( \\frac{p}{1-p} \\right) = a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n\\newline\n\\frac{p}{1-p} &amp;= e^{a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n} \\newline\np &amp;= (1-p)e^{a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n} \\newline\np + p(e^{-(a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n)}) &amp;= 1 \\newline\n1 + e^{`(a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n)} &amp;= \\frac{1}{p}\\newline\n\\frac{1}{1+e^{-(a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n)}} &amp;= p\n\\end{align}\n$$</p>\n\n<p>We can multiply the fraction by a clever form of 1, $e^{(a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n)}/e^{(a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n)}$ to get</p>\n\n<p>$$\np = \\frac{e^{(a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n)}}{1+e^{(a_0 + a_1x_1 + a_2x_2, \\ldots , a_nx_n)}}.\n$$</p>\n\n<p>If we set $a_1 = 1$ and all other $a_0, a_2, a_3, \\ldots = 0$, then we get the general logit cuve below</p>\n\n<p><img src=\"http://media.tumblr.com/tumblr_mdorbq7Ixx1rw6gvj.png\" alt=\"The logit curve ranges from negative infinity to positive infinity on the x-axis, and from 0 to 1 on the y-axis.\"/></p>\n\n<p>So now we have a way of creating a predictor of the probability of a binary variable from a continuous variable. Now let&#8217;s get started with doing regression.</p>\n\n<p><em>PS, if you want to get math typesetting (aka LaTeX) on your blog, I recommend \n<strong>against</strong> searching for &#8220;tumblr latex&#8221; and instead checking out <a href=\"http://checkmyworking.com/2012/01/how-to-get-beautifully-typeset-maths-on-your-blog/\" target=\"_blank\">this tutorial</a> on installing MathJax on your blog, tumblr or not.</em></p>\n\n<h1>Step 1: Get binary data.</h1>\n\n<h2>Initialize a vector of 0&#8217;s</h2>\n\n<p>For this case, we are going to make up some binary data. According to <a href=\"http://www.statgun.com/tutorials/logistic-regression.html\" target=\"_blank\">a nice explanation of logistic regression</a>,\nlogistic regression requires at least 50 data points per classification. Meaning, \n50+ 0&#8217;s and 50+ 1&#8217;s. There are 178 total <em>Star Trek: TNG</em> episodes and lots of bonus \nfeatures and interviews on the DVDs, so let&#8217;s round up to 200 total <em>TNG</em> pieces of \nvideo. In R, <code>rep</code> is the function to create a vector of repeated values. The\nfirst argument is what you want repeated, and the second argument is how\nmany times you want to repeat it. You could even have a vector in the first \nargument, such as <code>rep(c(\"red\", \"blue\"), 30)</code>, to get a vector of alternating \nstrings, &#8220;red&#8221; and &#8220;blue.&#8221; We could have also made the same vector\nwith <code>classification = vector(length=200, mode=\"numeric\")</code>, which initializes\nthe numeric values at 0, but I decided to do it with <code>rep</code> instead.</p>\n\n<pre><code>classification = rep(0,200)\n</code></pre>\n\n<p>EDIT: I&#8217;ve been informed that at 50+ data points is not always necessary, so take the rule with a grain of salt.</p>\n\n<h2>Set a random seed</h2>\n\n<p>Let&#8217;s set the random seed so we will get consistently the same results, no\nmatter how many times we run the code. No random number generator is truly random,\nand part of the semi-randomness is starting with a seed number, and then doing \nall kinds of transformations to then get a &#8220;random&#8221; number. So if we make sure\nwe are always using the same seed, then we will always get the same set of\n&#8220;random&#8221; numbers.</p>\n\n<pre><code>set.seed(100)\n</code></pre>\n\n<h2>Randomly add 1s to the last part of the vector</h2>\n\n<p>Now let&#8217;s randomly add a bunch of 1s to the vector, ramping up the probability of \ngetting a one (with hyperbolic growth, if you&#8217;re being technical) as we get to \nthe end of the vector. Let&#8217;s be interesting and add some noise to the data so \nwe don&#8217;t have not do an equal 50/50 split. We&#8217;ll use a randomly-generated\nprobability distribution to assign 0 or 1 to the last 125 values. This way, our\ndata is a little more realistic and less clean.</p>\n\n<p>FYI, <code>runif</code> is the <a href=\"http://en.wikipedia.org/wiki/Uniform_distribution_(continuous)\" target=\"_blank\">uniform distribution function</a>\nin R, which has an equal probability of obtaining every value between 0 and 1.\nIt is useful in this assignment because I know that the odds are 50/50 that\nthe value will be greater than 0.5, so I increased the threshold to &gt;0.6\nand added a multiplicative factor, which varies from 1 to 125. This way, the\nvalues near the beginning of the vector have close to a 60/40 chance of being\nless than 0.6, but by the end, they are almost certainly all above 0.6.\nTake a look below.</p>\n\n<p>The multiplicative factor:\nFYI, the <code>str</code> function is a method of previewing the first few values of\nvariables without seeing everything at once. It can be handy. Another method is\n<code>head</code>, which like the *NIX head, shows the first few values, but it can\nbe annoying if you have a matrix with a large number of columns, say over 100. Then\nyou get the first 10 values for all 100 columns, which takes up a lot of space. <code>str</code>\ncan be more convenient in that case.</p>\n\n<pre><code>&gt; str(125/125:1)\n num [1:125] 1 1.01 1.02 1.02 1.03 ...\n</code></pre>\n\n<p>Randomly generated values in the uniform distribution:\n(Notice that I re-set the random number seed. This is because after\na random seed is used, the random number generator advances to the next seed.)</p>\n\n<pre><code>&gt; set.seed(100); str(runif(125))\n num [1:125] 0.3078 0.2577 0.5523 0.0564 0.4685 ...\n</code></pre>\n\n<p>Multiplicative factor times the uniform distribution vector:\n(For those Linear Algebra lovers out there, we don&#8217;t get a matrix because by \ndefault, R multiplies vectors on an element-by-element basis. If you want\nthe usual vector product, aka the cross product, do <code>vector1 %*% vector2</code>)</p>\n\n<pre><code>&gt; set.seed(100); str(runif(125)*(125/125:1))\n num [1:125] 0.3078 0.2598 0.5613 0.0578 0.484 ...\n</code></pre>\n\n<p>Now we can use <code>ifelse</code> to assign a 0 or 1 based on whether the vector we just\ncreated is greater than 0.6 or not.</p>\n\n<pre><code>set.seed(100); classification[76:200] = ifelse(runif(125)*(125/125:1) &gt; 0.6, 1, 0)\n</code></pre>\n\n<h2>The final binary vector</h2>\n\n<p>This is what I get for the binary classification vector. Keep in mind that we only \nadded 1&#8217;s to the last 125 values, or from index 76 and up.</p>\n\n<pre><code>&gt; classification\n  [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0\n [38] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0\n [75] 0 0 0 0 0 0 0 1 0 0 0 1 1 0 0 1 1 0 0 0 1 1 1 1 1 0 0 1 1 1 0 1 1 0 1 1 1\n[112] 0 1 1 0 0 1 1 1 1 1 1 1 0 0 0 0 0 0 1 0 0 0 1 0 1 1 1 1 1 1 1 1 0 1 1 1 1\n[149] 1 1 1 1 1 1 0 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 0 0 1 1 1 0 1 1 1 1 1 0 1\n[186] 1 1 1 1 1 0 1 1 1 1 1 1 1 1 1\n</code></pre>\n\n<h1>Step 2: Get some continuous data.</h1>\n\n<p>We&#8217;re going to make up continuous data similarly to how we created the binary \nclassification vector. This time, we set the next seed so we&#8217;d get different \nnumbers, and multiplied a uniform distribution by a linear function. Now this isn&#8217;t\nquite enough to make most of the 1&#8217;s be in the upper numbers and most of the 0&#8217;s\nbe in the lower numbers, so we add a linearly increasing amount of Gaussian noise\nto the values.</p>\n\n<pre><code>set.seed(101); continuous = runif(200)*1:200+rnorm(200,mean=50,sd=5)*1:200\n</code></pre>\n\n<p>My continuous vector looks like this:</p>\n\n<pre><code>&gt; str(continuous)\n num [1:200] 51.7 94.2 184.1 226.1 269.9 ...\n</code></pre>\n\n<p>As you can see, we have values larger than 200, but we assumed there are 200 pieces of <em>Star Trek: TNG</em> video media to watch, so we need to normalize it by the max. Now our maximum value is 1, so we multiply by 200 to get a maximum value of 200.</p>\n\n<pre><code>&gt; continuous = continuous/max(continuous) * 200\n</code></pre>\n\n<h2>What does our data look like?</h2>\n\n<p>Now if we plot the continuous vector vs the binary vector, we&#8217;ll see that in general,\nthe 0&#8217;s occur in the lower end of the continuous spectrum, and the 1&#8217;s occur in the\nhigher end of the continuous spectrum.</p>\n\n<!-- fix data because continuous goes up to 10,000 -->\n\n<pre><code>png(\"~/workspace/tutorials/logistic_regression_points.png\")\nplot(continuous, classification, pch=20, col=\"blue\")\ndev.off()\n</code></pre>\n\n<p><img src=\"http://media.tumblr.com/tumblr_mdorcfiIYs1rw6gvj.png\" alt=\"The continuous vector on the x-axis vs the binary vector in the y-axis. Notice that most of the values classified as 0 appear with lower continuous values, and the values classified as 1 appear with higher continuous values.\"/></p>\n\n<p>So according to our data, there&#8217;s a lot of people who have seen <em>most</em> of <em>Star Trek:TNG</em> and have been to a convention, but there are a few individuals who have seen most of it and yet have not been a convention.</p>\n\n<h1>Step 3: Do logistic regression!</h1>\n\n<p>Now you can use generalized linear models to fit to the data. Note: This is one of many methods in R to do logistic regression, and I chose to do the simplest version. Note that <code>classification ~ continuous</code> is shorthand for <code>y ~ x</code>, or as the R people say, <code>response ~ terms</code>. If you had more data points, like Klingon language profiency, you would add this variable by using the <code>cbind</code> or &#8220;column bind&#8221; function which would create a 2-column matrix from the two vectors: <code>glm(classification ~ cbind(continuous, klingon), ...)</code>.</p>\n\n<pre><code>glm.out = glm(classification ~ continuous, family=binomial(logit))\npng(\"~/workspace/tutorials/logistic_fitted.png\")\nplot(continuous, glm.out$fitted, type=\"l\", col=\"red\", lwd=1, main=\"Logistic function fitted to classification data\\nvia a generalized linear model\")\npoints(continuous, classification, col=\"blue\", pch=20)\ndev.off()\n</code></pre>\n\n<p>The plot of the fitted curve looks like:</p>\n\n<p><img src=\"http://media.tumblr.com/tumblr_mdorcxMktg1rw6gvj.png\" alt=\"Logistic curve fitted to data.\"/></p>\n\n<h2>So what actually happened here?</h2>\n\n<p>The <code>glm</code> function found the values of $a_0$ and $a_1$ that minimize the error term. I&#8217;m not qualified to describe <code>glm</code> in gory detail, or all the other possible methods of calculating logistic regression, but <a href=\"http://www.omidrouhani.com/research/logisticregression/html/logisticregression.htm#_Toc147483467\" target=\"_blank\">this site</a> gives the mathematics the justice it deserves. You will need to be familiar with linear algebra to fully understand the arguments.</p>\n\n<h1>FAQ</h1>\n\n<h2>What if your classification is 1 for the low values and 0 for the high values?</h2>\n\n<p>You can still use the same steps, but your logit curve will be flipped along the $y$-axis.</p>\n\n<h2>Have you attended a <em>Star Trek</em> convention?</h2>\n\n<p>No, I have not. But I thought it would be a fun binary variable.</p>\n\n<h1>References</h1>\n\n<p>I could not have done this without the help of many different sources.</p>\n\n<ul><li><a href=\"http://luna.cas.usf.edu/~mbrannic/files/regression/Logistic.html\" target=\"_blank\">http://luna.cas.usf.edu/~mbrannic/files/regression/Logistic.html</a> - the first thing I read, and the primary source for the explanation of logistic regression in the first section (<em>What in Merlin&#8217;s beard is logistic regression?</em>). Highly recommended introduction to logistic regression.</li>\n<li><a href=\"http://en.wikipedia.org/wiki/Logistic_regression\" target=\"_blank\">http://en.wikipedia.org/wiki/Logistic_regression</a> - Wikipedia is a great place to start, but it dove into the technical aspects a little too quickly for my taste.</li>\n<li><a href=\"http://www.statgun.com/tutorials/logistic-regression.html\" target=\"_blank\">http://www.statgun.com/tutorials/logistic-regression.html</a> - a great explanation of logistic regression in general.</li>\n<li><a href=\"http://ww2.coastal.edu/kingw/statistics/R-tutorials/logistic.html\" target=\"_blank\">http://ww2.coastal.edu/kingw/statistics/R-tutorials/logistic.html</a> - used R to explain logistic regression of a particular dataset.</li>\n<li><a href=\"http://www.omidrouhani.com/research/logisticregression/html/logisticregression.htm\" target=\"_blank\">http://www.omidrouhani.com/research/logisticregression/html/logisticregression.htm</a> - mathematical derivation of logistic regression</li>\n<li><a href=\"http://ww2.coastal.edu/kingw/statistics/R-tutorials/logistic.html\" target=\"_blank\">http://ww2.coastal.edu/kingw/statistics/R-tutorials/logistic.html</a> - explains logistic regression and odds, and implements logistic regression on a particular dataset.</li>\n</ul>"}
publish: 2012-09-07
tags: R, logistic regression, tutorial, star trek
title: 1, 2, 3 ... logistic regression!
-->


1, 2, 3 ... logistic regression!
================================

R Tutorial in Logistic Regression
---------------------------------

Recently I read a
[paper](http://www.nature.com/msb/journal/v3/n1/full/msb4100180.html)
that used logistic regression in its methods and realized I had no idea
what logistic regression *is*, so I made this tutorial for myself and
others to learn about it. I wanted to create a tutorial that created
random data that anyone could use, and taught you a bit about R in the
process. This is my first tutorial and feedback is encouraged.

This tutorial on the statistical machine-learning method known as
Logistic Regression is implemented in [R](http://r-project.org).
Familiarity with programming syntax, such as vector indexing, is assumed
but I tried to make this tutorial as accessible as possible for those
with less programming experience.

What in Merlin’s beard is *logistic regression?*
================================================

First, what is *regression*?
----------------------------

You may have heard of “linear regression,” which fits a \$y=mx+b\$ line
to a set of points, such as in the figure below. I’m not going to go
into the details of linear regression, but you can [read about
it](http://www.stat.yale.edu/Courses/1997-98/101/linreg.htm) if you
like.

![Linear regression matches a best-fitting line to a set of
data.](http://media.tumblr.com/tumblr_mdorakn4cw1rw6gvj.png)

Logistic regression is similar, but instead of having two continuous
variables as above, one of the variables is binary (0 or 1) and the
other is continuous. But we’re not fitting a line, we’re fitting a
*logit*, which is a totally different function that estimates
probability for a continuous variable rather than assigns
classification. For example, we could predict the probability of cancer
relapse given some gene expression values. Or the probability of a heart
attack given a person’s cholesterol levels. We would base these
probabilities on previously encountered data, a method of machine
learning known as “Supervised Learning.” You tell the computer what the
answer is for many examples, and then the computer has to guess whether
the outcome is 0 or 1.

Think of it this way: you look at hundreds of values representing the
number of *Star Trek: The Next Generation* episodes a person has
watched, with a teacher peering over your shoulder, and he tells you
whether each value is associated with attending a *Star Trek* convention
or not. Then the teacher leaves, and you’re given a new number of *Star
Trek: TNG* episodes, \$x\$, and you have to give a probability \$p\$
whether the person who has seen this many episodes has been to a
convention. The probability of \$x\$ episodes watched and no convention
attendance is \$1-p\$, so by knowing \$p\$, we also know \$1-p\$ and we
only need to specify one value.

Regression is not the same thing as classification
--------------------------------------------------

This is an important distinction between classification and regression.
If we were doing classification, we would just say whether the person
who has seen \$x\$ number of episodes has gone to a convention or not.
But we don’t always want a binary outcome. The cholesterol-heart attack
association is a great example of this. You probably don’t want to tell
a patient, “You *will* have a heart attack,” but rather say “You have an
\$x\$% probability of having a heart attack, given your current
cholesterol levels.” Same goes for cancer relapse. A binary yes/no
answer can obscure the data. Also, a probability of 0 is essentially
impossible. Could you truly, definitively, say that a person will
*never* have a heart attack? Not until their death, but then all
possible heart attack events have already been observed. Sorry for the
gruesome-ness.

So what about this *logisitic* thing?
-------------------------------------

### First, we have to talk about *odds*

We talked about predicting the probability of some event. But we’re
going to use *odds* instead. If the probability of having attended a
*Star Trek: TNG* convention is \$p\$, then the odds are:

\$\$\\text{odds} = \\frac{\\text{probability of event
ocurring}}{\\text{probability of event not occuring}} =
\\frac{p}{1-p}\$\$

Say after watching a certain number of episodes, the probability of
attending a convention is \$p=0.7\$. Then the odds of attending a
convention are

\$\$\\frac{0.7}{0.3} = 2.333\\ldots\$\$

But the odds of not attending a convention are

\$\$\\frac{0.3}{0.7} = 0.4285\\ldots\$\$

As you can see, odds and probability are not the same. Odds can be
greater than 1, and probability must be between 0 and 1.

### Next, we have to define *log-odds*

It would be nice if these odds were somehow symmetric and we could
intuitively make inferences just by looking at the value. This is where
the logarithm comes in. If we take the [natural
log](http://en.wikipedia.org/wiki/Natural_logarithm) (ln), aka
\$\\log\_e\$ of the odds of attending a convention, we get

\$\$\\ln \\frac{0.7}{0.3} = 0.8472\\ldots\$\$

And the log-odds of *not* attending a convention:

\$\$\\frac{0.3}{0.7} = -0.8472\\ldots\$\$

The opposite of attending a convention is the negative of attending a
convention! Now let’s stop and think. *Why* is this? This is because the
natural log of 1 is 0, so it intersects at 0 as in the plot below. Then
the natural log of anything smaller than 1 is negative, approaching
negative infinity as the number gets closer and closer to 0. The natural
log of anything larger than 1 is positive, and approaches positive
infinity as the number gets larger and larger, but slower than we
approach negative infinity from the other side.

![Natural log
function](http://media.tumblr.com/tumblr_mdorb7uHwT1rw6gvj.png)

This symmetry is helpful to guide our intuition about events occurring
or not occuring.

*FYI:* We used the natural logarithm here because that’s the standard.
You could really use any log base you like, even \$\\log\_\\pi\$ would
do, but it’s just not used that often.

### From log-odds to *logits*

The log-odds are also called the *logit* of the probability:

\$\$ \\log(\\text{odds}) = \\text{logit}(p) = \\ln\\left( \\frac{p}{1-p}
\\right) \$\$

In logistic regression, we set

\$\$\\text{logit}(p) = a\_0 + a\_1x\_1 + a\_2x\_2, \\ldots ,
a\_nx\_n\$\$.

Where \$n\$ is the number of independent variables you have. In our
case, we just have one independent variable, \$x = x\_1\$, the number of
*Star Trek: TNG* episodes viewed. But we could add to the complexity and
also have \$x\_2\$ be the number of *Star Trek: The Original Series*
([happy 46th
anniversary!](http://mashable.com/2012/09/07/google-star-trek/))
episodes viewed, \$x\_3\$ be number of [latinum
strips](http://en.memory-alpha.org/wiki/Latinum) owned, and \$x\_4\$ be
[Klingon language
proficiency](http://www.amazon.com/Star-Trek-Conversational-Marc-Okrand/dp/0671797395).
But we’re going to keep it simple and stick to our one \$x\$.

Now we could do regular ol’ linear regression on this, but there are
several issues.

1.  We have a binary variable for our dependent variable (aka the
    \$y\$), and linear regression assumes a continuous distribution for
    the dependent variable. With probability as the output, we cannot
    have values less than 0 or greater than 1, and linear regression
    does not conform to this.
2.  The variance of \$y\$ is not constant across values of \$x\$. The
    variance is \$p(1-p) = pq\$ (\$q\$ is short for \$1-p\$), and if we
    have a probability of \$0.50\$ of attending a *Star Trek*
    convention, then we have \$0.5\\times 0.5 = 0.25\$ odds. But if we
    have \$p=0.9\$, then the variance \$pq = 0.9\\times 0.1 = 0.09\$,
    and \$0.25 \\neq 0.09\$. Thus the assumption that variance of \$y\$
    is constant across values of \$x\$ is invalid. This is also called
    [homoscedasticity](http://blog.minitab.com/blog/statistics-and-quality-data-analysis/dont-be-a-victim-of-statistical-hippopotomonstrosesquipedaliophobia).
3.  If you want to do significance testing (you maybe familiar with
    *p*-values or *R*-values to describe how significant a result is),
    the assumption that the errors of prediction, \$Y-Y\^\\prime\$ are
    [normally
    distributed](http://en.wikipedia.org/wiki/Normal_distribution)
    doesn’t work. \$Y\$ only takes on the values 0 and 1, so there’s no
    way you can create a continuous distribution such as a normal
    distribution with just two values.

Logits are not very intuitive as a unit, even though they are very
helpful when looking at odds. But we need to get back to probabilities
so we can give a probability of convention attendance. We’ll have to
exponentiate both sides to get rid of the natural log. I’m going to keep
the long-form version of the equation so you can see how it works for
more variables.

\$\$ \\begin{align} \\text{logit}(p) &= \\ln\\left( \\frac{p}{1-p}
\\right) = a\_0 + a\_1x\_1 + a\_2x\_2, \\ldots , a\_nx\_n\\newline
\\frac{p}{1-p} &= e\^{a\_0 + a\_1x\_1 + a\_2x\_2, \\ldots , a\_nx\_n}
\\newline p &= (1-p)e\^{a\_0 + a\_1x\_1 + a\_2x\_2, \\ldots , a\_nx\_n}
\\newline p + p(e\^{-(a\_0 + a\_1x\_1 + a\_2x\_2, \\ldots , a\_nx\_n)})
&= 1 \\newline 1 + e\^{\`(a\_0 + a\_1x\_1 + a\_2x\_2, \\ldots ,
a\_nx\_n)} &= \\frac{1}{p}\\newline \\frac{1}{1+e\^{-(a\_0 + a\_1x\_1 +
a\_2x\_2, \\ldots , a\_nx\_n)}} &= p \\end{align} \$\$

We can multiply the fraction by a clever form of 1, \$e\^{(a\_0 +
a\_1x\_1 + a\_2x\_2, \\ldots , a\_nx\_n)}/e\^{(a\_0 + a\_1x\_1 +
a\_2x\_2, \\ldots , a\_nx\_n)}\$ to get

\$\$ p = \\frac{e\^{(a\_0 + a\_1x\_1 + a\_2x\_2, \\ldots ,
a\_nx\_n)}}{1+e\^{(a\_0 + a\_1x\_1 + a\_2x\_2, \\ldots , a\_nx\_n)}}.
\$\$

If we set \$a\_1 = 1\$ and all other \$a\_0, a\_2, a\_3, \\ldots = 0\$,
then we get the general logit cuve below

![The logit curve ranges from negative infinity to positive infinity on
the x-axis, and from 0 to 1 on the
y-axis.](http://media.tumblr.com/tumblr_mdorbq7Ixx1rw6gvj.png)

So now we have a way of creating a predictor of the probability of a
binary variable from a continuous variable. Now let’s get started with
doing regression.

*PS, if you want to get math typesetting (aka LaTeX) on your blog, I
recommend **against** searching for “tumblr latex” and instead checking
out [this
tutorial](http://checkmyworking.com/2012/01/how-to-get-beautifully-typeset-maths-on-your-blog/)
on installing MathJax on your blog, tumblr or not.*

Step 1: Get binary data.
========================

Initialize a vector of 0’s
--------------------------

For this case, we are going to make up some binary data. According to [a
nice explanation of logistic
regression](http://www.statgun.com/tutorials/logistic-regression.html),
logistic regression requires at least 50 data points per classification.
Meaning, 50+ 0’s and 50+ 1’s. There are 178 total *Star Trek: TNG*
episodes and lots of bonus features and interviews on the DVDs, so let’s
round up to 200 total *TNG* pieces of video. In R, `rep` is the function
to create a vector of repeated values. The first argument is what you
want repeated, and the second argument is how many times you want to
repeat it. You could even have a vector in the first argument, such as
`rep(c("red", "blue"), 30)`, to get a vector of alternating strings,
“red” and “blue.” We could have also made the same vector with
`classification = vector(length=200, mode="numeric")`, which initializes
the numeric values at 0, but I decided to do it with `rep` instead.

    classification = rep(0,200)

EDIT: I’ve been informed that at 50+ data points is not always
necessary, so take the rule with a grain of salt.

Set a random seed
-----------------

Let’s set the random seed so we will get consistently the same results,
no matter how many times we run the code. No random number generator is
truly random, and part of the semi-randomness is starting with a seed
number, and then doing all kinds of transformations to then get a
“random” number. So if we make sure we are always using the same seed,
then we will always get the same set of “random” numbers.

    set.seed(100)

Randomly add 1s to the last part of the vector
----------------------------------------------

Now let’s randomly add a bunch of 1s to the vector, ramping up the
probability of getting a one (with hyperbolic growth, if you’re being
technical) as we get to the end of the vector. Let’s be interesting and
add some noise to the data so we don’t have not do an equal 50/50 split.
We’ll use a randomly-generated probability distribution to assign 0 or 1
to the last 125 values. This way, our data is a little more realistic
and less clean.

FYI, `runif` is the [uniform distribution
function](http://en.wikipedia.org/wiki/Uniform_distribution_(continuous))
in R, which has an equal probability of obtaining every value between 0
and 1. It is useful in this assignment because I know that the odds are
50/50 that the value will be greater than 0.5, so I increased the
threshold to \>0.6 and added a multiplicative factor, which varies from
1 to 125. This way, the values near the beginning of the vector have
close to a 60/40 chance of being less than 0.6, but by the end, they are
almost certainly all above 0.6. Take a look below.

The multiplicative factor: FYI, the `str` function is a method of
previewing the first few values of variables without seeing everything
at once. It can be handy. Another method is `head`, which like the \*NIX
head, shows the first few values, but it can be annoying if you have a
matrix with a large number of columns, say over 100. Then you get the
first 10 values for all 100 columns, which takes up a lot of space.
`str` can be more convenient in that case.

    > str(125/125:1)
     num [1:125] 1 1.01 1.02 1.02 1.03 ...

Randomly generated values in the uniform distribution: (Notice that I
re-set the random number seed. This is because after a random seed is
used, the random number generator advances to the next seed.)

    > set.seed(100); str(runif(125))
     num [1:125] 0.3078 0.2577 0.5523 0.0564 0.4685 ...

Multiplicative factor times the uniform distribution vector: (For those
Linear Algebra lovers out there, we don’t get a matrix because by
default, R multiplies vectors on an element-by-element basis. If you
want the usual vector product, aka the cross product, do
`vector1 %*% vector2`)

    > set.seed(100); str(runif(125)*(125/125:1))
     num [1:125] 0.3078 0.2598 0.5613 0.0578 0.484 ...

Now we can use `ifelse` to assign a 0 or 1 based on whether the vector
we just created is greater than 0.6 or not.

    set.seed(100); classification[76:200] = ifelse(runif(125)*(125/125:1) > 0.6, 1, 0)

The final binary vector
-----------------------

This is what I get for the binary classification vector. Keep in mind
that we only added 1’s to the last 125 values, or from index 76 and up.

    > classification
      [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
     [38] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
     [75] 0 0 0 0 0 0 0 1 0 0 0 1 1 0 0 1 1 0 0 0 1 1 1 1 1 0 0 1 1 1 0 1 1 0 1 1 1
    [112] 0 1 1 0 0 1 1 1 1 1 1 1 0 0 0 0 0 0 1 0 0 0 1 0 1 1 1 1 1 1 1 1 0 1 1 1 1
    [149] 1 1 1 1 1 1 0 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 0 0 1 1 1 0 1 1 1 1 1 0 1
    [186] 1 1 1 1 1 0 1 1 1 1 1 1 1 1 1

Step 2: Get some continuous data.
=================================

We’re going to make up continuous data similarly to how we created the
binary classification vector. This time, we set the next seed so we’d
get different numbers, and multiplied a uniform distribution by a linear
function. Now this isn’t quite enough to make most of the 1’s be in the
upper numbers and most of the 0’s be in the lower numbers, so we add a
linearly increasing amount of Gaussian noise to the values.

    set.seed(101); continuous = runif(200)*1:200+rnorm(200,mean=50,sd=5)*1:200

My continuous vector looks like this:

    > str(continuous)
     num [1:200] 51.7 94.2 184.1 226.1 269.9 ...

As you can see, we have values larger than 200, but we assumed there are
200 pieces of *Star Trek: TNG* video media to watch, so we need to
normalize it by the max. Now our maximum value is 1, so we multiply by
200 to get a maximum value of 200.

    > continuous = continuous/max(continuous) * 200

What does our data look like?
-----------------------------

Now if we plot the continuous vector vs the binary vector, we’ll see
that in general, the 0’s occur in the lower end of the continuous
spectrum, and the 1’s occur in the higher end of the continuous
spectrum.

    png("~/workspace/tutorials/logistic_regression_points.png")
    plot(continuous, classification, pch=20, col="blue")
    dev.off()

![The continuous vector on the x-axis vs the binary vector in the
y-axis. Notice that most of the values classified as 0 appear with lower
continuous values, and the values classified as 1 appear with higher
continuous
values.](http://media.tumblr.com/tumblr_mdorcfiIYs1rw6gvj.png)

So according to our data, there’s a lot of people who have seen *most*
of *Star Trek:TNG* and have been to a convention, but there are a few
individuals who have seen most of it and yet have not been a convention.

Step 3: Do logistic regression!
===============================

Now you can use generalized linear models to fit to the data. Note: This
is one of many methods in R to do logistic regression, and I chose to do
the simplest version. Note that `classification ~ continuous` is
shorthand for `y ~ x`, or as the R people say, `response ~ terms`. If
you had more data points, like Klingon language profiency, you would add
this variable by using the `cbind` or “column bind” function which would
create a 2-column matrix from the two vectors:
`glm(classification ~ cbind(continuous, klingon), ...)`.

    glm.out = glm(classification ~ continuous, family=binomial(logit))
    png("~/workspace/tutorials/logistic_fitted.png")
    plot(continuous, glm.out$fitted, type="l", col="red", lwd=1, main="Logistic function fitted to classification data\nvia a generalized linear model")
    points(continuous, classification, col="blue", pch=20)
    dev.off()

The plot of the fitted curve looks like:

![Logistic curve fitted to
data.](http://media.tumblr.com/tumblr_mdorcxMktg1rw6gvj.png)

So what actually happened here?
-------------------------------

The `glm` function found the values of \$a\_0\$ and \$a\_1\$ that
minimize the error term. I’m not qualified to describe `glm` in gory
detail, or all the other possible methods of calculating logistic
regression, but [this
site](http://www.omidrouhani.com/research/logisticregression/html/logisticregression.htm#_Toc147483467)
gives the mathematics the justice it deserves. You will need to be
familiar with linear algebra to fully understand the arguments.

FAQ
===

What if your classification is 1 for the low values and 0 for the high values?
------------------------------------------------------------------------------

You can still use the same steps, but your logit curve will be flipped
along the \$y\$-axis.

Have you attended a *Star Trek* convention?
-------------------------------------------

No, I have not. But I thought it would be a fun binary variable.

References
==========

I could not have done this without the help of many different sources.

-   [http://luna.cas.usf.edu/\~mbrannic/files/regression/Logistic.html](http://luna.cas.usf.edu/~mbrannic/files/regression/Logistic.html)
    - the first thing I read, and the primary source for the explanation
    of logistic regression in the first section (*What in Merlin’s beard
    is logistic regression?*). Highly recommended introduction to
    logistic regression.
-   [http://en.wikipedia.org/wiki/Logistic\_regression](http://en.wikipedia.org/wiki/Logistic_regression)
    - Wikipedia is a great place to start, but it dove into the
    technical aspects a little too quickly for my taste.
-   [http://www.statgun.com/tutorials/logistic-regression.html](http://www.statgun.com/tutorials/logistic-regression.html)
    - a great explanation of logistic regression in general.
-   [http://ww2.coastal.edu/kingw/statistics/R-tutorials/logistic.html](http://ww2.coastal.edu/kingw/statistics/R-tutorials/logistic.html)
    - used R to explain logistic regression of a particular dataset.
-   [http://www.omidrouhani.com/research/logisticregression/html/logisticregression.htm](http://www.omidrouhani.com/research/logisticregression/html/logisticregression.htm)
    - mathematical derivation of logistic regression
-   [http://ww2.coastal.edu/kingw/statistics/R-tutorials/logistic.html](http://ww2.coastal.edu/kingw/statistics/R-tutorials/logistic.html)
    - explains logistic regression and odds, and implements logistic
    regression on a particular dataset.



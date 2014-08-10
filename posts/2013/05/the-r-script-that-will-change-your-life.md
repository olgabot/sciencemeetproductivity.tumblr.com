<!--
id: 49520982242
link: http://blog.olgabotvinnik.com/post/49520982242/the-r-script-that-will-change-your-life
slug: the-r-script-that-will-change-your-life
date: Fri May 03 2013 10:01:30 GMT-0700 (PDT)
raw: {"blog_name":"sciencemeetproductivity","id":49520982242,"post_url":"http://blog.olgabotvinnik.com/post/49520982242/the-r-script-that-will-change-your-life","slug":"the-r-script-that-will-change-your-life","type":"text","date":"2013-05-03 17:01:30 GMT","timestamp":1367600490,"state":"published","format":"markdown","reblog_key":"OATWMWwu","tags":["R","programming","Rprofile","automation","updating packages"],"short_url":"http://tmblr.co/ZStENuk7hZZY","highlighted":[],"note_count":3,"title":"The R script that will change your life","body":"<p>If you use the <a href=\"http://www.r-project.org/\" target=\"_blank\"><code>R</code></a> programming language, you probably know how much of a pain it is to keep your packages updated. You&#8217;ve run <code>update.packages(...)</code> on the few that you want to keep up to date, but it&#8217;s a pain in the neck to do that for every package, every time. Thankfully, where there&#8217;s a will, there&#8217;s a way!</p>\n\n<p>When <code>R</code> starts up, it looks at your <code>.Rprofile</code> file (if you have one), and runs it. Mine looks like this:</p>\n\n<pre><code>#!/usr/bin/Rscript\n\noptions(\"repos\"=\"http://cran.stat.ucla.edu/\")\nlibrary(utils)\nupdate.packages(ask = FALSE)\nmy.packages = c(\"CvM2SL2Test\", \"MASS\", \"verification\", \"gtools\", \"ROCR\",\n        \"RColorBrewer\", \"heatmap.plus\", \"gmodels\", \"gplots\",\n        \"profr\", \"proftools\",\n        \"colorRamps\")\nto.download = which(!my.packages %in% rownames(installed.packages()))\nif( length(to.download) &gt; 0){\n    install.packages(my.packages[to.download], clean=TRUE, dependencies=TRUE)\n}\n</code></pre>\n\n<p>This script has three awesome features:</p>\n\n<ol><li>It will update ALL the packages I have without asking.</li>\n<li>It has an A-list of packages that I always want to have.</li>\n<li>It iterates over the A-list and makes sure they&#8217;re updated.</li>\n</ol><p>It may be a little redundant, but having a few fail-safes never hurt anyone.</p>\n\n<p>Feel free to steal this <code>.Rprofile</code> for your own usage.</p>"}
publish: 2013-05-03
tags: R, programming, Rprofile, automation, updating packages
title: The R script that will change your life
-->


The R script that will change your life
=======================================

If you use the [`R`](http://www.r-project.org/) programming language,
you probably know how much of a pain it is to keep your packages
updated. You’ve run `update.packages(...)` on the few that you want to
keep up to date, but it’s a pain in the neck to do that for every
package, every time. Thankfully, where there’s a will, there’s a way!

When `R` starts up, it looks at your `.Rprofile` file (if you have one),
and runs it. Mine looks like this:

    #!/usr/bin/Rscript

    options("repos"="http://cran.stat.ucla.edu/")
    library(utils)
    update.packages(ask = FALSE)
    my.packages = c("CvM2SL2Test", "MASS", "verification", "gtools", "ROCR",
            "RColorBrewer", "heatmap.plus", "gmodels", "gplots",
            "profr", "proftools",
            "colorRamps")
    to.download = which(!my.packages %in% rownames(installed.packages()))
    if( length(to.download) > 0){
        install.packages(my.packages[to.download], clean=TRUE, dependencies=TRUE)
    }

This script has three awesome features:

1.  It will update ALL the packages I have without asking.
2.  It has an A-list of packages that I always want to have.
3.  It iterates over the A-list and makes sure they’re updated.

It may be a little redundant, but having a few fail-safes never hurt
anyone.

Feel free to steal this `.Rprofile` for your own usage.


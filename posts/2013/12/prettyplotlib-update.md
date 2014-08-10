<!--
id: 70959694038
link: http://blog.olgabotvinnik.com/post/70959694038/prettyplotlib-update
slug: prettyplotlib-update
date: Mon Dec 23 2013 18:06:00 GMT-0800 (PST)
raw: {"blog_name":"sciencemeetproductivity","id":70959694038,"post_url":"http://blog.olgabotvinnik.com/post/70959694038/prettyplotlib-update","slug":"prettyplotlib-update","type":"text","date":"2013-12-24 02:06:00 GMT","timestamp":1387850760,"state":"published","format":"markdown","reblog_key":"aRpQXLVD","tags":["python","dataviz","prettyplotlib","pydata"],"short_url":"http://tmblr.co/ZStENu125Xm3M","highlighted":[],"note_count":1,"title":"Prettyplotlib update!","body":"<p>Check it out: <a href=\"http://nbviewer.ipython.org/github/olgabot/prettyplotlib/blob/master/ipython_notebooks/Examples%20of%20everything%20pretty%20and%20plotted!.ipynb?create=1\" target=\"_blank\">http://nbviewer.ipython.org/github/olgabot/prettyplotlib/blob/master/ipython_notebooks/Examples%20of%20everything%20pretty%20and%20plotted!.ipynb?create=1</a></p>\n\n<p>Major changes:</p>\n\n<ul><li>Don&#8217;t have to supply <code>ax</code> object to everything</li>\n<li>All functions return an <code>ax</code> object (let me know if this is not true!)</li>\n<li>Added <code>fill_between</code> and <code>fill_betweenx</code></li>\n<li><code>pcolormesh</code> accepts <code>center_value</code> keyword argument (&#8216;kwarg&#8217;) to re-center diverging colormaps</li>\n<li>Don&#8217;t change <code>rcParams</code> upon import, do everything programmatically</li>\n<li>Major refactoring - every function is now in its own file, java-style (we&#8217;ll see how that goes..)</li>\n</ul>"}
publish: 2013-12-023
tags: python, dataviz, prettyplotlib, pydata
title: Prettyplotlib update!
-->


Prettyplotlib update!
=====================

Check it out:
[http://nbviewer.ipython.org/github/olgabot/prettyplotlib/blob/master/ipython\_notebooks/Examples%20of%20everything%20pretty%20and%20plotted!.ipynb?create=1](http://nbviewer.ipython.org/github/olgabot/prettyplotlib/blob/master/ipython_notebooks/Examples%20of%20everything%20pretty%20and%20plotted!.ipynb?create=1)

Major changes:

-   Don’t have to supply `ax` object to everything
-   All functions return an `ax` object (let me know if this is not
    true!)
-   Added `fill_between` and `fill_betweenx`
-   `pcolormesh` accepts `center_value` keyword argument (‘kwarg’) to
    re-center diverging colormaps
-   Don’t change `rcParams` upon import, do everything programmatically
-   Major refactoring - every function is now in its own file,
    java-style (we’ll see how that goes..)



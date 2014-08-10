<!--
id: 67676121450
link: http://blog.olgabotvinnik.com/post/67676121450/succinct-loop-testing-in-python
slug: succinct-loop-testing-in-python
date: Thu Nov 21 2013 11:00:54 GMT-0800 (PST)
raw: {"blog_name":"sciencemeetproductivity","id":67676121450,"post_url":"http://blog.olgabotvinnik.com/post/67676121450/succinct-loop-testing-in-python","slug":"succinct-loop-testing-in-python","type":"text","date":"2013-11-21 19:00:54 GMT","timestamp":1385060454,"state":"published","format":"markdown","reblog_key":"qK0hPrBu","tags":["python","for loops","testing","data science"],"short_url":"http://tmblr.co/ZStENu-1pwbg","highlighted":[],"note_count":1,"title":"Succinct loop testing in Python","body":"<p>Since I&#8217;m a data scientist and all, my datasets can be too big to deal with when I&#8217;m initially testing an idea. So to test a for loop in Python with just a few examples, I used to do this kind of stuff:</p>\n\n<pre><code>n = 0\nfor thing in things:\n    print thing\n    n += 1\n    if n &gt; 10:\n        break\n</code></pre>\n\n<p>But python&#8217;s <code>zip</code> is smart in that it stops when the shortest item in the zip stops!</p>\n\n<p>So you can do:</p>\n\n<pre><code>for i, thing in zip(range(5), things):\n    print thing\n</code></pre>\n\n<p>And it will only show you the first 5 things! And it&#8217;s so succinct! This is especially pertinent when your <code>things</code> is a generator and you can&#8217;t necessarily get the <code>len</code> of it.</p>\n\n<p>Hope that helps you out! It saves me some headache, KeyboardInterrupts, and keystrokes.</p>"}
publish: 2013-11-021
tags: python, for loops, testing, data science
title: Succinct loop testing in Python
-->


Succinct loop testing in Python
===============================

Since I’m a data scientist and all, my datasets can be too big to deal
with when I’m initially testing an idea. So to test a for loop in Python
with just a few examples, I used to do this kind of stuff:

    n = 0
    for thing in things:
        print thing
        n += 1
        if n > 10:
            break

But python’s `zip` is smart in that it stops when the shortest item in
the zip stops!

So you can do:

    for i, thing in zip(range(5), things):
        print thing

And it will only show you the first 5 things! And it’s so succinct! This
is especially pertinent when your `things` is a generator and you can’t
necessarily get the `len` of it.

Hope that helps you out! It saves me some headache, KeyboardInterrupts,
and keystrokes.


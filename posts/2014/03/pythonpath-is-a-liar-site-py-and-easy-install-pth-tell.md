<!--
id: 78514449277
link: http://blog.olgabotvinnik.com/post/78514449277/pythonpath-is-a-liar-site-py-and-easy-install-pth-tell
slug: pythonpath-is-a-liar-site-py-and-easy-install-pth-tell
date: Mon Mar 03 2014 20:15:41 GMT-0800 (PST)
raw: {"blog_name":"sciencemeetproductivity","id":78514449277,"post_url":"http://blog.olgabotvinnik.com/post/78514449277/pythonpath-is-a-liar-site-py-and-easy-install-pth-tell","slug":"pythonpath-is-a-liar-site-py-and-easy-install-pth-tell","type":"text","date":"2014-03-04 04:15:41 GMT","timestamp":1393906541,"state":"published","format":"markdown","reblog_key":"zPpRd6u0","tags":["pythonpath","python"],"short_url":"http://tmblr.co/ZStENu197qsjz","highlighted":[],"note_count":1,"title":"PYTHONPATH is a liar. site.py and easy-install.pth tell the truth","body":"<p>Lately I&#8217;ve been working in <a href=\"http://www.virtualenv.org/en/latest/\" target=\"_blank\">virtualenvs</a> which as been <strong>great</strong> for developing but not so great for installing. I&#8217;ve run into numerous issues where I prepend my <code>PYTHONPATH</code> with the directory I want to get imported first, but to no avail. You&#8217;ve run into this: you <code>export PYTHONPATH</code> exactly as you want, only to see that <code>import sys; sys.path</code> includes all kinds of other junk before it!</p>\n\n<p>So, after watching a <a href=\"http://blip.tv/pycon-us-videos-2009-2010-2011/pycon-2011-reverse-engineering-ian-bicking-s-brain-inside-pip-and-virtualenv-4899496\" target=\"_blank\">talk</a> about reverse-engineering <code>virtualenv</code>. I realized I actually don&#8217;t understand anything about what python does at startup. So after a lot of searching, I found out that to load packages, Python reads the <code>site.py</code> file. I read about <a href=\"http://docs.python.org/2/library/site.html\" target=\"_blank\"><code>site.py</code></a>. And I found out&#8230;..</p>\n\n<p><strong>TURNS OUT PYTHON SECRETLY LOADS PACKAGES BEFORE LOOKING AT PYTHONPATH.</strong></p>\n\n<p><img src=\"http://media0.giphy.com/media/aCpmM0W4tfG48/giphy.gif\" alt=\"Shocking!\"/></p>\n\n<p>And where it looks is the <code>*.pth</code> files found in <code>path/to/python/lib/site-packages/*.pth</code>. The biggest culprit is usually <code>easy_-nstall.pth</code>. Mine had all kinds of absolute paths to the bigger Python install that I had to remove.</p>\n\n<p>On my computer, <code>easy-install.pth</code> looks like this:</p>\n\n<pre><code>import sys; sys.__plen = len(sys.path)\n./pytz-2013.8-py2.7.egg\n./brewer2mpl-1.3.2-py2.7.egg\n./pybedtools-0.6.2-py2.7-macosx-10.9-x86_64.egg\n/Users/olga/workspace-git/pandas\n/Users/olga/workspace-git/prettyplotlib\n/Users/olga/workspace-git/statsmodels\n/Users/olga/workspace-git/seaborn\n./husl-2.1.0-py2.7.egg\n/Users/olga/workspace-git/gffutils\n./simplejson-3.3.1-py2.7-macosx-10.9-x86_64.egg\n./argcomplete-0.6.5-py2.7.egg\n./argh-0.23.3-py2.7.egg\n./moss-0.2.0-py2.7.egg\n/Users/olga/workspace-git/scikit-learn\n/Users/olga/workspace-git/matplotlib/lib\n./Theano-0.6.0-py2.7.egg\n./pysam-0.7.7-py2.7-macosx-10.9-x86_64.egg\n/Users/olga/workspace-git/YeoLab/gscripts\nimport sys; new=sys.path[sys.__plen:]; del sys.path[sys.__plen:]; p=getattr(sys,'__egginsert',0); sys.path[p:p]=new; sys.__egginsert = p+len(new)\n</code></pre>\n\n<p>So let&#8217;s say I never wanted to use my development versions of something, then I&#8217;d remove that line from the file. Although eventually this got so much of a problem in my <code>virtualenv</code> at work that at I added the <code>site-packages</code> directory of that python distribution indirectly with <code>./</code>:</p>\n\n<pre><code>import sys; sys.__plen = len(sys.path)\n./\n./distribute-0.6.14-py2.7.egg\n/home/obotvinnik/workspace-git/gscripts\n/home/obotvinnik/workspace-git/rnaseqlib\nimport sys; new=sys.path[sys.__plen:]; del sys.path[sys.__plen:]; p=getattr(sys,'__egginsert',0); sys.path[p:p]=new; sys.__egginsert = p+len(new)\n</code></pre>\n\n<p>Because I <em>always</em> wanted to import from the <code>virtualenv</code> FIRST and never look at any other packages if I could help it.</p>\n\n<p>I honestly don&#8217;t know how kosher this is but it worked for me. Hope it helps you!</p>"}
publish: 2014-03-03
tags: pythonpath, python
title: PYTHONPATH is a liar. site.py and easy-install.pth tell the truth
-->


PYTHONPATH is a liar. site.py and easy-install.pth tell the truth
=================================================================

Lately I’ve been working in
[virtualenvs](http://www.virtualenv.org/en/latest/) which as been
**great** for developing but not so great for installing. I’ve run into
numerous issues where I prepend my `PYTHONPATH` with the directory I
want to get imported first, but to no avail. You’ve run into this: you
`export PYTHONPATH` exactly as you want, only to see that
`import sys; sys.path` includes all kinds of other junk before it!

So, after watching a
[talk](http://blip.tv/pycon-us-videos-2009-2010-2011/pycon-2011-reverse-engineering-ian-bicking-s-brain-inside-pip-and-virtualenv-4899496)
about reverse-engineering `virtualenv`. I realized I actually don’t
understand anything about what python does at startup. So after a lot of
searching, I found out that to load packages, Python reads the `site.py`
file. I read about
[`site.py`](http://docs.python.org/2/library/site.html). And I found
out…..

**TURNS OUT PYTHON SECRETLY LOADS PACKAGES BEFORE LOOKING AT
PYTHONPATH.**

![Shocking!](http://media0.giphy.com/media/aCpmM0W4tfG48/giphy.gif)

And where it looks is the `*.pth` files found in
`path/to/python/lib/site-packages/*.pth`. The biggest culprit is usually
`easy_-nstall.pth`. Mine had all kinds of absolute paths to the bigger
Python install that I had to remove.

On my computer, `easy-install.pth` looks like this:

    import sys; sys.__plen = len(sys.path)
    ./pytz-2013.8-py2.7.egg
    ./brewer2mpl-1.3.2-py2.7.egg
    ./pybedtools-0.6.2-py2.7-macosx-10.9-x86_64.egg
    /Users/olga/workspace-git/pandas
    /Users/olga/workspace-git/prettyplotlib
    /Users/olga/workspace-git/statsmodels
    /Users/olga/workspace-git/seaborn
    ./husl-2.1.0-py2.7.egg
    /Users/olga/workspace-git/gffutils
    ./simplejson-3.3.1-py2.7-macosx-10.9-x86_64.egg
    ./argcomplete-0.6.5-py2.7.egg
    ./argh-0.23.3-py2.7.egg
    ./moss-0.2.0-py2.7.egg
    /Users/olga/workspace-git/scikit-learn
    /Users/olga/workspace-git/matplotlib/lib
    ./Theano-0.6.0-py2.7.egg
    ./pysam-0.7.7-py2.7-macosx-10.9-x86_64.egg
    /Users/olga/workspace-git/YeoLab/gscripts
    import sys; new=sys.path[sys.__plen:]; del sys.path[sys.__plen:]; p=getattr(sys,'__egginsert',0); sys.path[p:p]=new; sys.__egginsert = p+len(new)

So let’s say I never wanted to use my development versions of something,
then I’d remove that line from the file. Although eventually this got so
much of a problem in my `virtualenv` at work that at I added the
`site-packages` directory of that python distribution indirectly with
`./`:

    import sys; sys.__plen = len(sys.path)
    ./
    ./distribute-0.6.14-py2.7.egg
    /home/obotvinnik/workspace-git/gscripts
    /home/obotvinnik/workspace-git/rnaseqlib
    import sys; new=sys.path[sys.__plen:]; del sys.path[sys.__plen:]; p=getattr(sys,'__egginsert',0); sys.path[p:p]=new; sys.__egginsert = p+len(new)

Because I *always* wanted to import from the `virtualenv` FIRST and
never look at any other packages if I could help it.

I honestly don’t know how kosher this is but it worked for me. Hope it
helps you!


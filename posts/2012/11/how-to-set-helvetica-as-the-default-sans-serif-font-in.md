<!--
id: 35807476900
link: http://blog.olgabotvinnik.com/post/35807476900/how-to-set-helvetica-as-the-default-sans-serif-font-in
slug: how-to-set-helvetica-as-the-default-sans-serif-font-in
date: Thu Nov 15 2012 16:39:00 GMT-0800 (PST)
raw: {"blog_name":"sciencemeetproductivity","id":35807476900,"post_url":"http://blog.olgabotvinnik.com/post/35807476900/how-to-set-helvetica-as-the-default-sans-serif-font-in","slug":"how-to-set-helvetica-as-the-default-sans-serif-font-in","type":"text","date":"2012-11-16 00:39:00 GMT","timestamp":1353026340,"state":"published","format":"markdown","reblog_key":"6p55l94W","tags":["matplotlib","helvetica","tutorial"],"short_url":"http://tmblr.co/ZStENuXMIhoa","highlighted":[],"note_count":3,"title":"How to set Helvetica as the default sans-serif font in Matplotlib","body":"<p>If you&#8217;re a typography junkie like me, you&#8217;re probably sick of seeing Bitstream Vera Sans as the typesetting font for <a href=\"http://python.org/\" target=\"_blank\">Python</a>'s plotting library, <a href=\"http://matplotlib.org/\" target=\"_blank\">Matplotlib</a>.</p>\n\n<p>Just look at it:</p>\n\n<p><img src=\"http://media.tumblr.com/tumblr_mdjt6nmDvg1rw6gvj.png\" alt=\"\"/></p>\n\n<p>So ugly, I know.</p>\n\n<p>While Helvetica is a <a href=\"http://www.helveticafilm.com/\" target=\"_blank\">controversial</a> font, it is  simple, clean, and undisputedly easy to read. In my scientific figures, I&#8217;m not going for originality in the typography, I just want it clean and readable. And, as I use Helvetica in my own documents, I want my plot text and my document text to match, without having to go into Adobe Illustrator and change it all.</p>\n\n<h2>If you&#8217;ve tried to add Helvetica by editing your <code>.matplotlibrc</code> file</h2>\n\n<p>You&#8217;ve probably seen this error:</p>\n\n<pre><code>/Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/font_manager.py:1216: UserWarning: findfont: Font family ['sans-serif'] not found. Falling back to Bitstream Vera Sans\n</code></pre>\n\n<h2>Adding Helvetica to the default font list</h2>\n\n<p>Helvetica is stored in OS X as a <code>.dfont</code> file which is inaccessible to Matplotlib, so we need to make it accessible. We will do this in six (6) steps.</p>\n\n<h3>1. Download and install <em>Fondu</em> to convert Mac-Helvetica to ttf-Helvetica</h3>\n\n<p>EDIT: changed source <code>.tgz</code> install to homebrew install.</p>\n\n<p>To install <em>Fondu</em>, use <a href=\"http://mxcl.github.io/homebrew/\" target=\"_blank\"><code>homebrew</code></a>, the &#8220;missing package manager for OSX.&#8221; It&#8217;s really fantastic, downloading and installing dependencies automagically for each package.</p>\n\n<p>After you install <code>homebrew</code>, the command  to install <em>Fondu</em> is,</p>\n\n<pre><code>brew install fondu\n</code></pre>\n\n<p>You may need to <code>brew update</code> if you are getting an error.</p>\n\n<h3>2. Find Helvetica on your system</h3>\n\n<p>Can use &#8216;FontBook.app&#8217; to find where Helvetica is on your system, but in all likelihood it&#8217;s here:</p>\n\n<pre><code>/System/Library/Fonts/Helvetica.dfont\n</code></pre>\n\n<p>Need to convert this font file to <code>.ttf</code> via <em>Fondu</em>.</p>\n\n<h3>3. Find where matplotlib stores its data</h3>\n\n<p>To find out where matplotlib stores its data, we use this command. Note that these outputs are specific to my machine and Python installation.</p>\n\n<pre><code>$ python\n&amp;gt;&amp;gt;&amp;gt; import matplotlib ; matplotlib.matplotlib_fname()\n/Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/mpl-data/matplotlibrc\n</code></pre>\n\n<p>We need to put the <code>.ttf</code> in: <code>matplotlib/mpl-data/fonts/ttf</code></p>\n\n<pre><code>$ cd /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/mpl-data/fonts/ttf/\n$ sudo fondu -show /System/Library/Fonts/Helvetica.dfont  # need sudo access to get into the .dfont directory\n</code></pre>\n\n<p>If this is not your own machine, copy the Helvetica file to somewhere you can edit, and then run fondu:</p>\n\n<pre><code>$ mkdir ~/font_copies ;cp /System/Library/Fonts/Helvetica.dfont ~/font_copies\n$ cd /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/mpl-data/fonts/ttf/\n$ sudo fondu -show ~/font_copies/Helvetica.dfont \n</code></pre>\n\n<p>Because we specified <code>-show</code>, which in this program is the equivalent of <code>--verbose</code>, we see all the files that are created. I don&#8217;t know why there&#8217;s a bunch of &#8220;Untitled&#8221; files but we&#8217;ll just leave them there. Now you should have a bunch of <code>Helvetica*.ttf</code> files in this directory. Let&#8217;s double-check:</p>\n\n<pre><code>$ ls -1 Helvetica*   # The '-1' argument forces the output to be a single column\nHelvetica.ttf\nHelveticaBold.ttf\nHelveticaBoldOblique.ttf\nHelveticaLight.ttf\nHelveticaLightOblique.ttf\nHelveticaOblique.ttf\n</code></pre>\n\n<p>BUT WAIT. Before you can go on happily using Helvetica, you need to set Helvetica as the default font in your <code>.matplotlibrc</code> file. Remember we found the location of your <code>matplotlibrc</code> file using <code>matplotlib.matplotlib_fname()</code>&#160;?</p>\n\n<h3>4. Edit your <code>.matplotlibrc</code> file</h3>\n\n<p>The <a href=\"http://matplotlib.org/users/customizing.html\" target=\"_blank\"><code>matplotlibrc</code></a> file gets read in every time you import matplotlib, and this is where you can set customizations such as default fonts and colors. Since we&#8217;re good computer scientists, we&#8217;re going to copy the original <code>.matplotlibrc</code> file into a personal directory so it doesn&#8217;t get over written when we update <code>matplotlib</code>.</p>\n\n<pre><code>$ cp /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/mpl-data/matplotlibrc ~/.matplotlib/matplotlibrc\n</code></pre>\n\n<p>Now use your terminal-based editor of choice (mine is <a href=\"http://www.gnu.org/s/emacs/\" target=\"_blank\">Emacs</a>) to edit the file:</p>\n\n<pre><code>$ emacs -q ~/.matplotlib/matplotlibrc  # the '-q' doesn't load my .emacs file which has settings that aren't compatible with my terminal\n</code></pre>\n\n<p>Find the line:</p>\n\n<pre><code>#font.sans-serif     : Bitstream Vera Sans, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif\n</code></pre>\n\n<p>This list of fonts is ordered in decreasing priority, meaning that Bitstream Vera Sans is used first, but if it&#8217;s not there, use Lucida Grande, if that&#8217;s not there, use Verdana, and so on. We want to uncomment it (remove the <code>#</code>), and make Helvetica the first priority:</p>\n\n<pre><code>font.sans-serif     : Helvetica, Bitstream Vera Sans, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif\n</code></pre>\n\n<p>You can remove Helvetica from the end of the list, but it shouldn&#8217;t matter because the program will stop searching once it finds a font it knows.</p>\n\n<h3>5. Force matplotlib to re-scan the font lists and add Helvetica</h3>\n\n<p>Now we need to force matplotlib to re-create the font lists by removing the files.</p>\n\n<pre><code>$ rm ~/.matplotlib/fontList.cache ~/.matplotlib/fontManager.cache ~/.matplotlib/ttffont.cache\n$ python -v  # watch all the imports fly by\n&amp;gt;&amp;gt;&amp;gt; import matplotlib.pyplot as plt  # this will import and create the *.cache files we just removed\n</code></pre>\n\n<p>I was stuck at this:</p>\n\n<pre><code>dlopen(\"/Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/ft2font.so\", 2);\nimport matplotlib.ft2font # dynamically loaded from /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/ft2font.so\n</code></pre>\n\n<p>for quite some time, but be patient. It <em>will</em> finish :)</p>\n\n<h3>6. Plot your figures with Helvetica!</h3>\n\n<p>Helvetica!</p>\n\n<p><img src=\"http://media.tumblr.com/tumblr_mdjt64BRCu1rw6gvj.png\" alt=\"\"/></p>\n\n<h2>What if you want to use something <em>other</em> than Helvetica?</h2>\n\n<h3>If you have a <code>.ttf</code> file for the font</h3>\n\n<p>My favorite fixed-width font is <a href=\"http://nodnod.net/2009/feb/12/adding-straight-single-and-double-quotes-inconsola/\" target=\"_blank\">Inconsolata-dz</a> (this fixes the weird quotations of Inconsolata), which I use in all my terminal and code editing programs. If you installed the font before installing <code>matplotlib</code>, you can probably just use it. Let&#8217;s check the fontList file just to make sure:</p>\n\n<pre><code>$ grep Inconsolata ~/.matplotlib/fontList.cache\nS'Inconsolata'\nS'/Users/olgabotvinnik/Library/Fonts/Inconsolata.otf'\nS'Inconsolata-dz'\nS'/Users/olgabotvinnik/Library/Fonts/Inconsolata-dz.otf'\naS'/Users/olgabotvinnik/Library/Fonts/Inconsolata.otf'\naS'/Users/olgabotvinnik/Library/Fonts/Inconsolata-dz.otf'\n</code></pre>\n\n<p>Looks like Inconsolata&#8217;s there, so let&#8217;s try it out! To change your default font on the fly, add this to your code:</p>\n\n<pre><code>import matplotlib as mpl\nmpl.rcParams['font.fixed-width'] = 'Inconsolata-dz'\nmpl.rcParams['font.family'] = 'fixed-width'\n</code></pre>\n\n<h3>If you do not have a <code>.ttf</code> file for the font</h3>\n\n<p>Say you&#8217;re a huge fan of the sans-serif font <a href=\"http://www.typography.com/fonts/font_overview.php?productLineID=100008\" target=\"_blank\">Gotham</a> and want to use it for your plots. It&#8217;s a proprietary font and will probably not appear as a <code>.ttf</code> file. Follow the same steps 1-6, but find the location of your font using FontBook.app. Search for your font, and then either hit Command-R or click File &#8212;&gt; Show in Finder. Then drag that file into Terminal and you&#8217;ll get the full path name. I use this Finder-Terminal drag and drop trick all the time!</p>\n\n<h3>To edit your final <code>.pdf</code> files in another program such as Illustrator</h3>\n\n<p>After producing your matplotlib figures, you may still want to tweak the axis or typography. To be able to do that, edit your <code>~/.matplotlibrc</code> file from:</p>\n\n<pre><code>pdf.fonttype = 3\n</code></pre>\n\n<p>To:</p>\n\n<pre><code>pdf.fonttype = 42\n</code></pre>\n\n<p>And that&#8217;ll do it! Hat tip to Benjamin Reedlunn!</p>\n\n<h2>Appendix</h2>\n\n<p>The code and data used to create these files can be found <a href=\"https://github.com/olgabot/coding-cucumber/tree/master/helvetica_disordered_protein\" target=\"_blank\">here</a>. To create the file, run the command:</p>\n\n<pre><code>python pondrfit_plots.py -f *.pondrfit -t 'Oct4 isoform 2' -c '#0080FF' -s\n</code></pre>\n\n<p>One of the hardest parts of this code was figuring out how to extract the start and end of the disordered regions (disorder score&gt;0.5) when all I had was a sequential list of indices for each region. Turns out the trick is using both <code>itertools</code> and <code>operator</code>:</p>\n\n<pre><code>ranges = []\nfor k, g in groupby(enumerate(df_disordered.index), lambda (i,x):i-x):\n    inds = map(itemgetter(1), g)\n    ranges.append([inds[0], inds[-1]])\n</code></pre>\n\n<p>Where <code>df_disordered.index</code> is the indices of all the rows whose disordered score is greater than 0.5. This code is quite magical to me, as I&#8217;m still figuring out <code>groupby</code> and <code>map</code> but I <em>will</em> understand it!</p>\n\n<h3>OSX 10.7 (and older) Lion users</h3>\n\n<p>Apparently &#8216;fondu&#8217; does not install on OSX 10.7 (I&#8217;m running 10.8) due to the XCode update in 10.8. One reader suggested <a href=\"http://fontforge.org/\" target=\"_blank\">FontForge</a> as an alternative to convert between <code>.dfont</code> and <code>.ttf</code> formats. Based on Apple&#8217;s &#8220;Leave old OS&#8217;s behind&#8221; strategy, I&#8217;m guessing users of anything older than 10.7 will also need to use FontForge.</p>\n\n<p><strong>UPDATE:</strong> This same user informed me that FontForge does not perform the full conversion necessary for <code>matplotlib</code> in iPython (below). If you are running into a similar problem, I suggest finding someone with OSX 10.8 and getting them to do it. I cannot post the files myself because Apple technically prohibits tampering with system fonts.</p>\n\n<h3>iPython</h3>\n\n<p>If you use <a href=\"http://ipython.org\" target=\"_blank\">iPython</a> notebooks, you may need to take an extra step for your rendered figures in <code>ipython notebook --pylab inline</code> to appear with Helvetica. I did the following:</p>\n\n<ol><li>Shutdown and re-open my notebook. This purges all the imports. This can also be accomplished by interrupting and restarting the kernel from within the notebook, but I wanted a full reboot.</li>\n<li>Edit my both my <code>~/.bashrc</code> and <code>./bash_profile</code> (since I&#8217;m not sure which one matplotlib uses) to include the text: <code>export MPLCONFIGDIR=$HOME/.matplotlib</code>, which should tell matplotlib to look in that directory for the configuration files, such as your <code>.matplotlibrc</code> file.</li>\n</ol><p>Hope that helps!</p>\n\n<h2>EDIT: Potential issues with other fonts.</h2>\n\n<p>Thanks to Benjamin Reedlun about the tips for specifying other fonts - it may not be as straightforward as you hoped.</p>\n\n<blockquote>\n  <p>Olga, here are two other potential issues your readers might run into. I had success using your method with two different TrueType fonts, but then I tried to install TeXGyreHeros, a free Helvetica Clone with additional characters.</p>\n  \n  <p>1) I found TeXGyreHeros as OpenType files (.otf extension), so I opened them up using FontForge. When I went to save TeXGyreHeros as .ttf files, FontForge pointed out that, &#8220;The convention is that TrueType fonts should have an Em-size which is a power of 2.&#8221; Each of the .otf font files have sizes of 1000. (In FontForge, Element &gt; Font Info &gt; General) Comparing the character sizes in TeXGyreHeros to those in a Helvetica.ttf, I decided to increase the size to 2^11 = 2048. After doing this, the sizes in TeXGyreHeros and Helvetica matched up reasonably well, so I saved TeXGyreHeros as .ttf files.</p>\n  \n  <p>2) I placed the TeXGyreHeros.ttf files in my matplotlib/mpl-data/fonts/ttf directory, rebuilt my font lists, and specified &#8220;TeXGyreHeros&#8221; as my default font in Matplotlib. However, Matplotlib could not find TeXGyreHeros. After much playing around, I found that the TeXGyreHeros.ttf files had an attribute called &#8220;Preferred Family&#8221;. (In FontForge, go to Element &gt; Font Info &gt; TTF Names.) The &#8220;Preferred Family&#8221; was &#8220;TeX Gyre Heros&#8221; (with spaces between each word). I supplied Matplotlib the font filename, &#8220;TeXGyreHeros&#8221;. Once I made the two names match, or deleted the &#8220;Preferred Family&#8221; attribute, then everything worked correctly. Looking back at the two previous TrueType fonts I installed, neither of them had the &#8220;Preferred Family&#8221; attribute, so that is why I did not run into this issue before.</p>\n</blockquote>"}
publish: 2012-11-015
tags: matplotlib, helvetica, tutorial
title: How to set Helvetica as the default sans-serif font in Matplotlib
-->


How to set Helvetica as the default sans-serif font in Matplotlib
=================================================================

If you’re a typography junkie like me, you’re probably sick of seeing
Bitstream Vera Sans as the typesetting font for
[Python](http://python.org/)'s plotting library,
[Matplotlib](http://matplotlib.org/).

Just look at it:

![](http://media.tumblr.com/tumblr_mdjt6nmDvg1rw6gvj.png)

So ugly, I know.

While Helvetica is a [controversial](http://www.helveticafilm.com/)
font, it is simple, clean, and undisputedly easy to read. In my
scientific figures, I’m not going for originality in the typography, I
just want it clean and readable. And, as I use Helvetica in my own
documents, I want my plot text and my document text to match, without
having to go into Adobe Illustrator and change it all.

If you’ve tried to add Helvetica by editing your `.matplotlibrc` file
---------------------------------------------------------------------

You’ve probably seen this error:

    /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/font_manager.py:1216: UserWarning: findfont: Font family ['sans-serif'] not found. Falling back to Bitstream Vera Sans

Adding Helvetica to the default font list
-----------------------------------------

Helvetica is stored in OS X as a `.dfont` file which is inaccessible to
Matplotlib, so we need to make it accessible. We will do this in six (6)
steps.

### 1. Download and install *Fondu* to convert Mac-Helvetica to ttf-Helvetica

EDIT: changed source `.tgz` install to homebrew install.

To install *Fondu*, use [`homebrew`](http://mxcl.github.io/homebrew/),
the “missing package manager for OSX.” It’s really fantastic,
downloading and installing dependencies automagically for each package.

After you install `homebrew`, the command to install *Fondu* is,

    brew install fondu

You may need to `brew update` if you are getting an error.

### 2. Find Helvetica on your system

Can use ‘FontBook.app’ to find where Helvetica is on your system, but in
all likelihood it’s here:

    /System/Library/Fonts/Helvetica.dfont

Need to convert this font file to `.ttf` via *Fondu*.

### 3. Find where matplotlib stores its data

To find out where matplotlib stores its data, we use this command. Note
that these outputs are specific to my machine and Python installation.

    $ python
    &gt;&gt;&gt; import matplotlib ; matplotlib.matplotlib_fname()
    /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/mpl-data/matplotlibrc

We need to put the `.ttf` in: `matplotlib/mpl-data/fonts/ttf`

    $ cd /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/mpl-data/fonts/ttf/
    $ sudo fondu -show /System/Library/Fonts/Helvetica.dfont  # need sudo access to get into the .dfont directory

If this is not your own machine, copy the Helvetica file to somewhere
you can edit, and then run fondu:

    $ mkdir ~/font_copies ;cp /System/Library/Fonts/Helvetica.dfont ~/font_copies
    $ cd /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/mpl-data/fonts/ttf/
    $ sudo fondu -show ~/font_copies/Helvetica.dfont 

Because we specified `-show`, which in this program is the equivalent of
`--verbose`, we see all the files that are created. I don’t know why
there’s a bunch of “Untitled” files but we’ll just leave them there. Now
you should have a bunch of `Helvetica*.ttf` files in this directory.
Let’s double-check:

    $ ls -1 Helvetica*   # The '-1' argument forces the output to be a single column
    Helvetica.ttf
    HelveticaBold.ttf
    HelveticaBoldOblique.ttf
    HelveticaLight.ttf
    HelveticaLightOblique.ttf
    HelveticaOblique.ttf

BUT WAIT. Before you can go on happily using Helvetica, you need to set
Helvetica as the default font in your `.matplotlibrc` file. Remember we
found the location of your `matplotlibrc` file using
`matplotlib.matplotlib_fname()` ?

### 4. Edit your `.matplotlibrc` file

The [`matplotlibrc`](http://matplotlib.org/users/customizing.html) file
gets read in every time you import matplotlib, and this is where you can
set customizations such as default fonts and colors. Since we’re good
computer scientists, we’re going to copy the original `.matplotlibrc`
file into a personal directory so it doesn’t get over written when we
update `matplotlib`.

    $ cp /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/mpl-data/matplotlibrc ~/.matplotlib/matplotlibrc

Now use your terminal-based editor of choice (mine is
[Emacs](http://www.gnu.org/s/emacs/)) to edit the file:

    $ emacs -q ~/.matplotlib/matplotlibrc  # the '-q' doesn't load my .emacs file which has settings that aren't compatible with my terminal

Find the line:

    #font.sans-serif     : Bitstream Vera Sans, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif

This list of fonts is ordered in decreasing priority, meaning that
Bitstream Vera Sans is used first, but if it’s not there, use Lucida
Grande, if that’s not there, use Verdana, and so on. We want to
uncomment it (remove the `#`), and make Helvetica the first priority:

    font.sans-serif     : Helvetica, Bitstream Vera Sans, Lucida Grande, Verdana, Geneva, Lucid, Arial, Helvetica, Avant Garde, sans-serif

You can remove Helvetica from the end of the list, but it shouldn’t
matter because the program will stop searching once it finds a font it
knows.

### 5. Force matplotlib to re-scan the font lists and add Helvetica

Now we need to force matplotlib to re-create the font lists by removing
the files.

    $ rm ~/.matplotlib/fontList.cache ~/.matplotlib/fontManager.cache ~/.matplotlib/ttffont.cache
    $ python -v  # watch all the imports fly by
    &gt;&gt;&gt; import matplotlib.pyplot as plt  # this will import and create the *.cache files we just removed

I was stuck at this:

    dlopen("/Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/ft2font.so", 2);
    import matplotlib.ft2font # dynamically loaded from /Library/Frameworks/EPD64.framework/Versions/7.3/lib/python2.7/site-packages/matplotlib/ft2font.so

for quite some time, but be patient. It *will* finish :)

### 6. Plot your figures with Helvetica!

Helvetica!

![](http://media.tumblr.com/tumblr_mdjt64BRCu1rw6gvj.png)

What if you want to use something *other* than Helvetica?
---------------------------------------------------------

### If you have a `.ttf` file for the font

My favorite fixed-width font is
[Inconsolata-dz](http://nodnod.net/2009/feb/12/adding-straight-single-and-double-quotes-inconsola/)
(this fixes the weird quotations of Inconsolata), which I use in all my
terminal and code editing programs. If you installed the font before
installing `matplotlib`, you can probably just use it. Let’s check the
fontList file just to make sure:

    $ grep Inconsolata ~/.matplotlib/fontList.cache
    S'Inconsolata'
    S'/Users/olgabotvinnik/Library/Fonts/Inconsolata.otf'
    S'Inconsolata-dz'
    S'/Users/olgabotvinnik/Library/Fonts/Inconsolata-dz.otf'
    aS'/Users/olgabotvinnik/Library/Fonts/Inconsolata.otf'
    aS'/Users/olgabotvinnik/Library/Fonts/Inconsolata-dz.otf'

Looks like Inconsolata’s there, so let’s try it out! To change your
default font on the fly, add this to your code:

    import matplotlib as mpl
    mpl.rcParams['font.fixed-width'] = 'Inconsolata-dz'
    mpl.rcParams['font.family'] = 'fixed-width'

### If you do not have a `.ttf` file for the font

Say you’re a huge fan of the sans-serif font
[Gotham](http://www.typography.com/fonts/font_overview.php?productLineID=100008)
and want to use it for your plots. It’s a proprietary font and will
probably not appear as a `.ttf` file. Follow the same steps 1-6, but
find the location of your font using FontBook.app. Search for your font,
and then either hit Command-R or click File —\> Show in Finder. Then
drag that file into Terminal and you’ll get the full path name. I use
this Finder-Terminal drag and drop trick all the time!

### To edit your final `.pdf` files in another program such as Illustrator

After producing your matplotlib figures, you may still want to tweak the
axis or typography. To be able to do that, edit your `~/.matplotlibrc`
file from:

    pdf.fonttype = 3

To:

    pdf.fonttype = 42

And that’ll do it! Hat tip to Benjamin Reedlunn!

Appendix
--------

The code and data used to create these files can be found
[here](https://github.com/olgabot/coding-cucumber/tree/master/helvetica_disordered_protein).
To create the file, run the command:

    python pondrfit_plots.py -f *.pondrfit -t 'Oct4 isoform 2' -c '#0080FF' -s

One of the hardest parts of this code was figuring out how to extract
the start and end of the disordered regions (disorder score\>0.5) when
all I had was a sequential list of indices for each region. Turns out
the trick is using both `itertools` and `operator`:

    ranges = []
    for k, g in groupby(enumerate(df_disordered.index), lambda (i,x):i-x):
        inds = map(itemgetter(1), g)
        ranges.append([inds[0], inds[-1]])

Where `df_disordered.index` is the indices of all the rows whose
disordered score is greater than 0.5. This code is quite magical to me,
as I’m still figuring out `groupby` and `map` but I *will* understand
it!

### OSX 10.7 (and older) Lion users

Apparently ‘fondu’ does not install on OSX 10.7 (I’m running 10.8) due
to the XCode update in 10.8. One reader suggested
[FontForge](http://fontforge.org/) as an alternative to convert between
`.dfont` and `.ttf` formats. Based on Apple’s “Leave old OS’s behind”
strategy, I’m guessing users of anything older than 10.7 will also need
to use FontForge.

**UPDATE:** This same user informed me that FontForge does not perform
the full conversion necessary for `matplotlib` in iPython (below). If
you are running into a similar problem, I suggest finding someone with
OSX 10.8 and getting them to do it. I cannot post the files myself
because Apple technically prohibits tampering with system fonts.

### iPython

If you use [iPython](http://ipython.org) notebooks, you may need to take
an extra step for your rendered figures in
`ipython notebook --pylab inline` to appear with Helvetica. I did the
following:

1.  Shutdown and re-open my notebook. This purges all the imports. This
    can also be accomplished by interrupting and restarting the kernel
    from within the notebook, but I wanted a full reboot.
2.  Edit my both my `~/.bashrc` and `./bash_profile` (since I’m not sure
    which one matplotlib uses) to include the text:
    `export MPLCONFIGDIR=$HOME/.matplotlib`, which should tell
    matplotlib to look in that directory for the configuration files,
    such as your `.matplotlibrc` file.

Hope that helps!

EDIT: Potential issues with other fonts.
----------------------------------------

Thanks to Benjamin Reedlun about the tips for specifying other fonts -
it may not be as straightforward as you hoped.

> Olga, here are two other potential issues your readers might run into.
> I had success using your method with two different TrueType fonts, but
> then I tried to install TeXGyreHeros, a free Helvetica Clone with
> additional characters.
>
> ​1) I found TeXGyreHeros as OpenType files (.otf extension), so I
> opened them up using FontForge. When I went to save TeXGyreHeros as
> .ttf files, FontForge pointed out that, “The convention is that
> TrueType fonts should have an Em-size which is a power of 2.” Each of
> the .otf font files have sizes of 1000. (In FontForge, Element \> Font
> Info \> General) Comparing the character sizes in TeXGyreHeros to
> those in a Helvetica.ttf, I decided to increase the size to 2\^11 =
> 2048. After doing this, the sizes in TeXGyreHeros and Helvetica
> matched up reasonably well, so I saved TeXGyreHeros as .ttf files.
>
> ​2) I placed the TeXGyreHeros.ttf files in my
> matplotlib/mpl-data/fonts/ttf directory, rebuilt my font lists, and
> specified “TeXGyreHeros” as my default font in Matplotlib. However,
> Matplotlib could not find TeXGyreHeros. After much playing around, I
> found that the TeXGyreHeros.ttf files had an attribute called
> “Preferred Family”. (In FontForge, go to Element \> Font Info \> TTF
> Names.) The “Preferred Family” was “TeX Gyre Heros” (with spaces
> between each word). I supplied Matplotlib the font filename,
> “TeXGyreHeros”. Once I made the two names match, or deleted the
> “Preferred Family” attribute, then everything worked correctly.
> Looking back at the two previous TrueType fonts I installed, neither
> of them had the “Preferred Family” attribute, so that is why I did not
> run into this issue before.


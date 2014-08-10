<!--
id: 58941062205
link: http://blog.olgabotvinnik.com/post/58941062205/prettyplotlib-painlessly-create-beautiful-matplotlib
slug: prettyplotlib-painlessly-create-beautiful-matplotlib
date: Wed Aug 21 2013 13:20:00 GMT-0700 (PDT)
raw: {"blog_name":"sciencemeetproductivity","id":58941062205,"post_url":"http://blog.olgabotvinnik.com/post/58941062205/prettyplotlib-painlessly-create-beautiful-matplotlib","slug":"prettyplotlib-painlessly-create-beautiful-matplotlib","type":"text","date":"2013-08-21 20:20:00 GMT","timestamp":1377116400,"state":"published","format":"markdown","reblog_key":"AkKdTeaK","tags":["python","matplotlib","prettyplotlib","programming","data visualization","dataviz"],"short_url":"http://tmblr.co/ZStENusvAJmz","highlighted":[],"note_count":13,"title":"prettyplotlib: Painlessly create beautiful matplotlib plots","body":"<p>A while back I wrote a few tutorials about how to work with Python&#8217;s plotting library, <code>matplotlib</code>, so that it behaves nicely and produces beautiful plots. Well, I got tired of tweaking every single figure individually so I wrote this library, <a href=\"http://olgabot.github.io/prettyplotlib\" target=\"_blank\"><code>prettyplotlib</code></a> to have pretty default plots in Python&#8217;s <code>matplotlib</code>.</p>\n\n<p>I truly believe that poor visualizations obstruct scientific progress, and this is my contribution.</p>\n\n<p>A couple motivating examples are below, but if you just want an overview, check out the <a href=\"https://github.com/olgabot/prettyplotlib/wiki/Comparison-to-matplotlib\" target=\"_blank\">Comparison to matplotlib defaults</a>, <a href=\"https://github.com/olgabot/prettyplotlib/wiki/Gallery\" target=\"_blank\">Examples Gallery</a>,  and <a href=\"https://github.com/olgabot/prettyplotlib/wiki/Examples-with-code\" target=\"_blank\">Examples with code</a> on Github.</p>\n\n<p>To install, do the usual <code>pip install</code> stuff:</p>\n\n<pre><code>pip install prettyplotlib\n</code></pre>\n\n<p>I truly hope you enjoy using this library! If you have any comments or suggestions, let me know!</p>\n\n<h1><code>prettyplotlib.scatter</code></h1>\n\n<p>The default <code>matplotlib</code> color cycle is not pretty to look at. What&#8217;s even worse is\nthat if you just do a <code>scatter</code> plot, then it doesn&#8217;t cycle at all through any values</p>\n\n<pre><code>import matplotlib.pyplot as plt\n# Set the random seed for consistency\nnp.random.seed(12)\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    ax.scatter(x, y, label=str(i))\nax.legend()\n\nax.set_title('prettyplotlib `scatter` example\\nshowing default matplotlib `scatter`')\nfig.savefig('scatter_matplotlib_default.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_default.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<h2>Before <code>prettyplotlib</code>: how to make nice plots</h2>\n\n<p>Now I&#8217;m going to take you through ALL the steps I used to take to make nice\nlooking plots.</p>\n\n<p>First, change the colors with <code>brewer2mpl</code>:</p>\n\n<pre><code># Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n...\ncolor = set2[i]\nax.scatter(x, y, label=str(i), facecolor=color)\n</code></pre>\n\n<p>The full code is,</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), color=color)\n\nfig.savefig('scatter_matplotlib_improved_01_changed_colors.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_01_changed_colors.png\" alt=\"Matplotlib scatter improved 01: changed colors\"/></p>\n\n<p>This looks nice, almost like an impressionist painting, but it&#8217;s still hard to\nsee overlaps here. So let&#8217;s fill the symbols with <code>0.5</code> opacity using\n<code>alpha=0.5</code>.</p>\n\n<pre><code>ax.scatter(x, y, label=str(i), color=color, alpha=0.5)\n</code></pre>\n\n<p>The full code is,</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), color=color, alpha=0.5)\n\nfig.savefig('scatter_matplotlib_improved_02_added_alpha.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_02_added_alpha.png\" alt=\"Matplotlib scatter improved 02: added alpha\"/></p>\n\n<p>This is still pretty lovely and impressionist-y but I still didn&#8217;t like that it\nwas hard to see when the dots overlapped. So let&#8217;s add a black outline, and\nspecify that <code>color</code> is just the <code>facecolor</code>:</p>\n\n<pre><code>ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black',\nfacecolor=color)\n</code></pre>\n\n<p>The full code is,</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color)\n\nfig.savefig('scatter_matplotlib_improved_03_added_outline.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_03_added_outline.png\" alt=\"Matplotlib scatter improved 03: added black outline\"/></p>\n\n<p>Ack, but those lines are too thick &#8230; let&#8217;s think them down to <code>linewidth=0.15</code></p>\n\n<pre><code>ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black',\nfacecolor=color, linewidth=0.15)\n</code></pre>\n\n<p>The full code is,</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color, linewidth=0.15)\nfig.savefig('scatter_matplotlib_improved_04_thinned_outline.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_04_thinned_outline.png\" alt=\"Matplotlib scatter improved 04: thinned out black outline\"/></p>\n\n<p><em>Now</em> we&#8217;re getting somewhere. This looks very lovely. Don&#8217;t you want to just\ncuddle up with that cute plot?</p>\n\n<p>What are those top and right axes lines really doing for us? They&#8217;re boxing the\ndata in, but we can do that with our eyes from the other axis lines. So let&#8217;s\nremove the top and right axis lines using <code>ax.spines</code>:</p>\n\n<pre><code># Remove top and right axes lines (\"spines\")\nspines_to_remove = ['top', 'right']\nfor spine in spines_to_remove:\nax.spines[spine].set_visible(False)\n</code></pre>\n\n<p>The full code is,</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color, linewidth=0.15)\n\n# Remove top and right axes lines (\"spines\")\nspines_to_remove = ['top', 'right']\nfor spine in spines_to_remove:\n    ax.spines[spine].set_visible(False)\nfig.savefig('scatter_matplotlib_improved_05_removed_top_right_spines.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_05_removed_top_right_spines.png\" alt=\"Matplotlib scatter improved 05: removed top and right axis lines\"/></p>\n\n<p>Oops, but we still have the ticks on the top and right axes. We&#8217;ll need to get\nrid of them. Actually, why don&#8217;t we just get rid of all ticks altogether? We can\ntell by the position of the number where it indicates, so we don&#8217;t need an\nadditional tick.</p>\n\n<pre><code># Get rid of ticks. The position of the numbers is informative enough of\n# the position of the value.\nax.xaxis.set_ticks_position('none')\nax.yaxis.set_ticks_position('none')\n</code></pre>\n\n<p>Here&#8217;s the full code:</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color, linewidth=0.15)\n\n# Remove top and right axes lines (\"spines\")\nspines_to_remove = ['top', 'right']\nfor spine in spines_to_remove:\n    ax.spines[spine].set_visible(False)\n\n# Get rid of ticks. The position of the numbers is informative enough of\n# the position of the value.\nax.xaxis.set_ticks_position('none')\nax.yaxis.set_ticks_position('none')\nfig.savefig('scatter_matplotlib_improved_06_removed_ticks.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_06_removed_ticks.png\" alt=\"Matplotlib scatter improved 06: removed tick marks\"/></p>\n\n<p>Ahh, much better. But we won&#8217;t stop there. Now we&#8217;ll tweak the remaining pieces\nof the figure. For the rest of the spines, let&#8217;s thin the line down to <code>0.5</code>\npoints instead of the default <code>1.0</code> points. Also, we&#8217;ll change it from pure\nblack to a slightly lighter dark grey. Here they are side by side:</p>\n\n<pre><code>fig, axes = plt.subplots(2)\naxes[0].set_axis_bgcolor('black')\naxes[0].text(0.5, 0.5, 'black', color='white', fontsize=24, va='center', ha='center')\naxes[1].set_axis_bgcolor('#262626')\naxes[1].text(0.5, 0.5, 'almost black', fontsize=24, color='white', va='center', ha='center')\nfig.savefig('black_vs_almost_black.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/black_vs_almost_black.png\" alt=\"Matplotlib scatter improved 06: removed tick marks\"/></p>\n\n<p>So not a <em>huge</em> difference, and the dark grey still looks pretty black, but it&#8217;s\n<a href=\"http://ianstormtaylor.com/design-tip-never-use-black/\" target=\"_blank\">a little more pleasant on the eyes</a> to use a dark grey instead of black. There&#8217;s very few things in\nnature that are truly black. Just look at shadows! They&#8217;re just dark grey, or\nblue, or red or purple. But I digress. Back to plotting libraries&#8230;</p>\n\n<p>To change the x-axis and y-axis line colors, and the outlines of the scatter\nsymbols from black to dark grey, we&#8217;ll do:</p>\n\n<pre><code># For remaining spines, thin out their line and change the black to a slightly off-black dark grey\nalmost_black = '#262626'\n...\nax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color, linewidth=0.15)`\n...\nspines_to_keep = ['bottom', 'left']\nfor spine in spines_to_keep:\n    ax.spines[spine].set_linewidth(0.5)\n    ax.spines[spine].set_color(almost_black)\n</code></pre>\n\n<p>The full code is,</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\n# Save a nice dark grey as a variable\nalmost_black = '#262626'\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor=almost_black, facecolor=color, linewidth=0.15)\n\n# Remove top and right axes lines (\"spines\")\nspines_to_remove = ['top', 'right']\nfor spine in spines_to_remove:\n    ax.spines[spine].set_visible(False)\n\n# Get rid of ticks. The position of the numbers is informative enough of\n# the position of the value.\nax.xaxis.set_ticks_position('none')\nax.yaxis.set_ticks_position('none')\n\n# For remaining spines, thin out their line and change the black to a slightly off-black dark grey\nspines_to_keep = ['bottom', 'left']\nfor spine in spines_to_keep:\n    ax.spines[spine].set_linewidth(0.5)\n    ax.spines[spine].set_color(almost_black)\nfig.savefig('scatter_matplotlib_improved_07_axis_black_to_almost_black.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_07_axis_black_to_almost_black.png\" alt=\"Matplotlib scatter improved 07: changed axis lines from black to almost black\"/></p>\n\n<p>This is nice. But if you look closely, the tick labels are still black :(  We\nhave to change them separately, using</p>\n\n<pre><code># Change the labels to the off-black\nax.xaxis.label.set_color(almost_black)\nax.yaxis.label.set_color(almost_black)\n</code></pre>\n\n<p>And while we&#8217;re at it, let&#8217;s add a title and make it dark grey too.</p>\n\n<pre><code># Change the axis title to off-black\nax.title.set_color(almost_black)\n\nax.set_title('prettyplotlib `scatter` example\\nshowing improved matplotlib `scatter`')\n</code></pre>\n\n<p>The full code is,</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\n# Save a nice dark grey as a variable\nalmost_black = '#262626'\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor=almost_black, facecolor=color, linewidth=0.15)\n\n# Remove top and right axes lines (\"spines\")\nspines_to_remove = ['top', 'right']\nfor spine in spines_to_remove:\n    ax.spines[spine].set_visible(False)\n\n# Get rid of ticks. The position of the numbers is informative enough of\n# the position of the value.\nax.xaxis.set_ticks_position('none')\nax.yaxis.set_ticks_position('none')\n\n# For remaining spines, thin out their line and change the black to a slightly off-black dark grey\nspines_to_keep = ['bottom', 'left']\nfor spine in spines_to_keep:\n    ax.spines[spine].set_linewidth(0.5)\n    ax.spines[spine].set_color(almost_black)\n\n# Change the labels to the off-black\nax.xaxis.label.set_color(almost_black)\nax.yaxis.label.set_color(almost_black)\n\n# Change the axis title to off-black\nax.title.set_color(almost_black)\n\nax.set_title('prettyplotlib `scatter` example\\nshowing improved matplotlib `scatter`')\nfig.savefig('scatter_matplotlib_improved_08_labels_black_to_almost_black.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_08_labels_black_to_almost_black.png\" alt=\"Matplotlib scatter improved 08: changed labels to almost black\"/></p>\n\n<p>If you remember in the original example, we also had an axis legend, using</p>\n\n<pre><code>ax.legend()\n</code></pre>\n\n<p>So let&#8217;s add it to this code, too.</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\n# Save a nice dark grey as a variable\nalmost_black = '#262626'\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor=almost_black, facecolor=color, linewidth=0.15)\n\n# Remove top and right axes lines (\"spines\")\nspines_to_remove = ['top', 'right']\nfor spine in spines_to_remove:\n    ax.spines[spine].set_visible(False)\n\n# Get rid of ticks. The position of the numbers is informative enough of\n# the position of the value.\nax.xaxis.set_ticks_position('none')\nax.yaxis.set_ticks_position('none')\n\n# For remaining spines, thin out their line and change the black to a slightly off-black dark grey\nalmost_black = '#262626'\nspines_to_keep = ['bottom', 'left']\nfor spine in spines_to_keep:\n    ax.spines[spine].set_linewidth(0.5)\n    ax.spines[spine].set_color(almost_black)\n\n# Change the labels to the off-black\nax.xaxis.label.set_color(almost_black)\nax.yaxis.label.set_color(almost_black)\n\n# Change the axis title to off-black\nax.title.set_color(almost_black)\n\nax.legend()\n\nax.set_title('prettyplotlib `scatter` example\\nshowing improved matplotlib `scatter`')\nfig.savefig('scatter_matplotlib_improved_09_ugly_legend.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_09_ugly_legend.png\" alt=\"Matplotlib scatter improved 09: added ugly legend\"/></p>\n\n<p>There are many things I don&#8217;t like about this legend.</p>\n\n<ol><li>First of all, why does it have such a thick border line? What does that\nreally add to our interpretation of the legend? The black line is so thick that\nit distracts from what we&#8217;re trying to portray - which label goes with which\ncolor.</li>\n<li>Why does it show three points? Does this legend think I&#8217;m dumb and can&#8217;t\nfigure out which symbol goes with which label after one iteration, so it does it\nthree times?</li>\n<li>Finally, the legend labels are pure black. Maybe you notice it too, after\ncomparing to x-axis and y-axis lines and labels.</li>\n</ol><p>We&#8217;ll accomplish these three things using this code:</p>\n\n<pre><code># Remove the line around the legend box, and instead fill it with a light grey\n# Also only use one point for the scatterplot legend because the user will\n# get the idea after just one, they don't need three.\nlight_grey = np.array([float(248)/float(255)]*3)\nlegend = ax.legend(frameon=True, scatterpoints=1, fontcolor=almost_black)\nrect = legend.get_frame()\nrect.set_facecolor(light_grey)\nrect.set_linewidth(0.0)\n\n# Change the legend label colors to almost black, too\ntexts = legend.texts\nfor t in texts:\n    t.set_color(almost_black)\n</code></pre>\n\n<p>Now our code is pretty huge &#8230;</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport brewer2mpl\n\n# Get \"Set2\" colors from ColorBrewer (all colorbrewer scales: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">http://bl.ocks.org/mbostock/5577023</a>)\nset2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\n# Save a nice dark grey as a variable\nalmost_black = '#262626'\n\nfig, ax = plt.subplots(1)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    color = set2[i]\n    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor=almost_black, facecolor=color, linewidth=0.15)\n\n# Remove top and right axes lines (\"spines\")\nspines_to_remove = ['top', 'right']\nfor spine in spines_to_remove:\n    ax.spines[spine].set_visible(False)\n\n# Get rid of ticks. The position of the numbers is informative enough of\n# the position of the value.\nax.xaxis.set_ticks_position('none')\nax.yaxis.set_ticks_position('none')\n\n# For remaining spines, thin out their line and change the black to a slightly off-black dark grey\nalmost_black = '#262626'\nspines_to_keep = ['bottom', 'left']\nfor spine in spines_to_keep:\n    ax.spines[spine].set_linewidth(0.5)\n    ax.spines[spine].set_color(almost_black)\n\n# Change the labels to the off-black\nax.xaxis.label.set_color(almost_black)\nax.yaxis.label.set_color(almost_black)\n\n# Change the axis title to off-black\nax.title.set_color(almost_black)\n\n# Remove the line around the legend box, and instead fill it with a light grey\n# Also only use one point for the scatterplot legend because the user will \n# get the idea after just one, they don't need three.\nlight_grey = np.array([float(248)/float(255)]*3)\nlegend = ax.legend(frameon=True, scatterpoints=1)\nrect = legend.get_frame()\nrect.set_facecolor(light_grey)\nrect.set_linewidth(0.0)\n\n# Change the legend label colors to almost black, too\ntexts = legend.texts\nfor t in texts:\n    t.set_color(almost_black)\n\n\nax.set_title('prettyplotlib `scatter` example\\nshowing improved matplotlib `scatter`')\nfig.savefig('scatter_matplotlib_improved_10_pretty_legend.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_10_pretty_legend.png\" alt=\"Matplotlib scatter improved 10: beautiful legend\"/></p>\n\n<p>Aaaaaaaaaaand I got tired of doing all those steps, EVERY time I wanted to make a simple scatterplot. So I wrote\n<a href=\"http://github.com/olgabot/prettyplotlib\" target=\"_blank\"><code>prettyplotlib</code></a>. Here&#8217;s an\nillustrative example of how awesome <code>prettyplotlib</code> is, and how it will save\nall the time you spent agonizing over making your <code>matplotlib</code> plots beautiful.</p>\n\n<pre><code>import prettyplotlib as ppl\nimport numpy as np\n\nfig, ax = ppl.subplots()\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    ppl.scatter(ax, x, y, label=str(i))\n\nppl.legend()\n\nax.set_title('prettyplotlib `scatter` example\\nshowing default color cycle and scatter params')\nfig.savefig('scatter_prettyplotlib_default.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_prettyplotlib_default.png\" alt=\"Matplotlib scatter improved 10: beautiful legend\"/></p>\n\n<p>The only commands that were different from the very first example with\nmatplotlib are:</p>\n\n<pre><code>ppl.scatter(ax, x, y, label=str(i), facecolor='none')\n</code></pre>\n\n<p>instead of:</p>\n\n<pre><code>ax.scatter(x, y, label=str(i))\n</code></pre>\n\n<p>And a different legend command:</p>\n\n<pre><code>ppl.legend(ax)\n</code></pre>\n\n<p>instead of:</p>\n\n<pre><code>ax.legend()\n</code></pre>\n\n<p>If you <strong><em>really</em></strong> want to get the original matplotlib style back in\nprettyplotlib, you can do:</p>\n\n<pre><code>import prettyplotlib as ppl\n\n# Set the random seed for consistency\nnp.random.seed(12)\n\nfig, ax = plt.subplots(1)\n\n#mpl.rcParams['axis.color_cycle'] = ['blue']\n\n# Show the whole color range\nfor i in range(8):\n    x = np.random.normal(loc=i, size=1000)\n    y = np.random.normal(loc=i, size=1000)\n    ax.scatter(x, y, label=str(i), facecolor='blue', edgecolor='black', linewidth=1)\n\n# Get back the top and right axes lines (\"spines\")\nspines_to_remove = ['top', 'right']\nfor spine in spines_to_remove:\n    ax.spines[spine].set_visible(True)\n\n# Get back the ticks. The position of the numbers is informative enough of\n# the position of the value.\nax.xaxis.set_ticks_position('both')\nax.yaxis.set_ticks_position('both')\n\n# For all the spines, make their line thicker and return them to be black\nall_spines = ['top', 'left', 'bottom', 'right']\nfor spine in all_spines:\n    ax.spines[spine].set_linewidth(1.0)\n    ax.spines[spine].set_color('black')\n\n# Change the labels back to black\nax.xaxis.label.set_color('black')\nax.yaxis.label.set_color('black')\n\n# Change the axis title also back to black\nax.title.set_color('black')\n\n# Remove the line around the legend box, and instead fill it with a light grey\n# Also only use one point for the scatterplot legend because the user will \n# get the idea after just one, they don't need three.\nax.legend()\n\nax.set_title('prettyplotlib `scatter` example\\nrevert everything back to default matplotlib parameters')\nfig.savefig('scatter_prettyplotlib_back_to_matplotlib_default.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_prettyplotlib_back_to_matplotlib_default.png\" alt=\"Prettyplotlib back to matplotlib\"/></p>\n\n<p>Notice that the default calls of <code>ax.scatter</code> and <code>ax.legend</code> do the usual\nthing. This is important, because for <code>prettyplotlib</code> to work, you&#8217;ll need to\nuse a syntax that&#8217;s different from the usual <code>matplotlib</code> one:  <code>ppl.scatter(ax,\nx, y...)</code> instead of <code>ax.scatter(x, y, ...)</code></p>\n\n<h2><code>prettyplotlib.pcolormesh</code>: Improving heatmaps in <code>matplotlib</code></h2>\n\n<h3>Both positive and negative values</h3>\n\n<p>The default <code>matplotlib</code> <code>pcolormesh</code> heatmaps use a rainbow colormap, which has\nbeen known to mislead data visualization. Specifically, [<em>\"the rainbow color map\nis universally inferior to all other color maps\"</em>](<a href=\"http://www.jwave.vt.edu/~rkri\" target=\"_blank\">http://www.jwave.vt.edu/~rkri</a>\nz/Projects/create_color_table/color_07.pdf). Unfortunately, <code>matplotlib</code> took\nits default colors from MATLAB, and there the default is also rainbow.</p>\n\n<pre><code>import matplotlib.pyplot as plt\nimport numpy as np\n\nfig, ax = plt.subplots(1)\n\nnp.random.seed(10)\n\n#ax.pcolor(np.random.randn((10,10)))\n#ax.pcolor(np.random.randn(10), np.random.randn(10))\np = ax.pcolormesh(np.random.randn(10,10))\nfig.colorbar(p)\nfig.savefig('pcolormesh_matplotlib_default.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_matplotlib_default.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<p>Using the same zero-centered randomly distributed gaussian distribution, we can\nplot it using <code>prettyplotlib</code> with a few modifications in syntax:</p>\n\n<pre><code>ppl.pcolormesh(fig, ax, np.random.randn(10,10))\n</code></pre>\n\n<p>You&#8217;ll notice that the &#8220;hot&#8221; (large, positive) color is still red, and the\n&#8220;cold&#8221; (small, negative) color is still blue, but the in between colors are\ngradations of red and blue, so it&#8217;s easier to tell the difference between\nvalues.</p>\n\n<pre><code>import prettyplotlib as ppl\nimport numpy as np\n\nfig, ax = ppl.subplots(1)\n\nnp.random.seed(10)\n\nppl.pcolormesh(fig, ax, np.random.randn(10,10))\nfig.savefig('pcolormesh_prettyplotlib_default.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_default.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<p>You may have also noticed similar changes as were made in <code>prettyplotlib.scatter</code>,\nwhere axis lines were removed, and blacks were changed to almost black.</p>\n\n<h3>Only positive (or negative) values</h3>\n\n<p>If your data is only positive (or negative), <code>matplotlib</code> does nothing to change\nthe color scale. It&#8217;s still a rainbow, but look at the colorbar, the range is\ndifferent (0 to 1 instead of -2 to +2)</p>\n\n<pre><code>import prettyplotlib as ppl\nimport numpy as np\n\nfig, ax = ppl.subplots(1)\n\nnp.random.seed(10)\n\np = ax.pcolormesh(np.random.uniform(size=(10,10)))\nfig.colorbar(p)\nfig.savefig('pcolormesh_matplotlib_positive_default.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_matplotlib_positive_default.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<p>If your data is only positive or negative, then <code>prettyplotlib</code> will auto-detect\nthis and use a single-color colormap. The default for positive data is the\n<code>reds</code> colormap.</p>\n\n<pre><code>import prettyplotlib as ppl\nimport numpy as np\n\nfig, ax = ppl.subplots(1)\n\nnp.random.seed(10)\n\nppl.pcolormesh(fig, ax, np.abs(np.random.randn(10,10)))\nfig.savefig('pcolormesh_prettyplotlib_positive.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_positive.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<p>And the default for negative data is the <code>blues</code> colormap.</p>\n\n<pre><code>import prettyplotlib as ppl\nimport numpy as np\n\nfig, ax = ppl.subplots(1)\n\nnp.random.seed(10)\n\nppl.pcolormesh(fig, ax, -np.abs(np.random.randn(10,10)))\nfig.savefig('pcolormesh_prettyplotlib_negative.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_negative.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<h2>Add x or y tick labels</h2>\n\n<p>Plus you can add x- and y-ticklabels directly!</p>\n\n<p>Normally, when you add x- and y-ticklabels on <code>pcolormesh</code> in <code>matplotlib</code>,\nthey&#8217;re not centered on the blocks, and you have to do a lot of annoying work\njust getting a label on each box. You have to specify the xticks explicitly,\nsince you want to label each box.</p>\n\n<pre><code>xticks = range(10)\nyticks = range(10)\n\nxticklabels=string.uppercase[:10]\nyticklabels=string.lowercase[-10:]\n\nax.set_xticks(xticks)\nax.set_xticklabels(xticklabels)\n\nax.set_yticks(yticks)\nax.set_yticklabels(yticklabels)\n</code></pre>\n\n<p>The full, <code>matplotlib</code> code is:</p>\n\n<pre><code>import prettyplotlib as ppl\nfrom prettyplotlib import plt\nimport numpy as np\nimport string\n\nfig, ax = plt.subplots(1)\n\nnp.random.seed(10)\n\np = ax.pcolormesh(np.abs(np.random.randn(10,10)))\nfig.colorbar(p)\n\nxticks = range(10)\nyticks = range(10)\n\nxticklabels=string.uppercase[:10]\nyticklabels=string.lowercase[-10:]\n\nax.set_xticks(xticks)\nax.set_xticklabels(xticklabels)\n\nax.set_yticks(yticks)\nax.set_yticklabels(yticklabels)\n\n\nfig.savefig('pcolormesh_matplotlib_positive_labels.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_matplotlib_positive_labels.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<p>But <code>prettyplotlib.pcolormesh</code> assumes that you want the <code>xticklabels</code> and\n<code>yticklabels</code> on each block, and makes it easy to specify.</p>\n\n<pre><code>ppl.pcolormesh(fig, ax, np.random.uniform(size=(10,10)),\n               xticklabels=string.uppercase[:10],\n               yticklabels=string.lowercase[-10:])\n</code></pre>\n\n<p>The full <code>prettyplotlib</code> code is,</p>\n\n<pre><code>import prettyplotlib as ppl\nimport numpy as np\nimport string\n\nfig, ax = ppl.subplots(1)\n\nnp.random.seed(10)\n\nppl.pcolormesh(fig, ax, np.random.randn(10,10), \n               xticklabels=string.uppercase[:10], \n               yticklabels=string.lowercase[-10:])\nfig.savefig('pcolormesh_prettyplotlib_labels.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_labels.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<h3>Custom colormaps</h3>\n\n<p>Or pick your own colormap! The diverging colormap <code>PRGn</code> or Purple and Green is\npretty nice. I usually use this website to look up the colormaps: <a href=\"http://bl.ocks.org/mbostock/5577023\" target=\"_blank\">Every\nColorbrewer Scale</a> (hover over the colors to get the name of the colormap)</p>\n\n<pre><code>import prettyplotlib as ppl\nimport brewer2mpl\nimport numpy as np\nimport string\n\ngreen_purple = brewer2mpl.get_map('PRGn', 'diverging', 11).mpl_colormap\n\nfig, ax = ppl.subplots(1)\n\nnp.random.seed(10)\n\nppl.pcolormesh(fig, ax, np.random.randn(10,10), \n               xticklabels=string.uppercase[:10], \n               yticklabels=string.lowercase[-10:],\n               cmap=green_purple)\nfig.savefig('pcolormesh_prettyplotlib_labels_other_cmap_diverging.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_labels_other_cmap_diverging.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<p>Or if you want your own colormap for positive-only data:</p>\n\n<pre><code>import prettyplotlib as ppl\nimport brewer2mpl\nimport numpy as np\nimport string\n\nred_purple = brewer2mpl.get_map('RdPu', 'Sequential', 9).mpl_colormap\n\nfig, ax = ppl.subplots(1)\n\nnp.random.seed(10)\n\nppl.pcolormesh(fig, ax, np.abs(np.random.randn(10,10)),\n               xticklabels=string.uppercase[:10], \n               yticklabels=string.lowercase[-10:],\n               cmap=red_purple)\nfig.savefig('pcolormesh_prettyplotlib_labels_other_cmap_sequential.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_labels_other_cmap_sequential.png\" alt=\"Matplotlib default scatterplot\"/></p>\n\n<h3>Log normalization or other parameters</h3>\n\n<p>Plus, this will take the usual parameters of <code>pcolormesh</code> like if you want to\nrescale your data to log-scale:</p>\n\n<pre><code>from matplotlib.colors import LogNorm\n...\nppl.pcolormesh(..., norm=LogNorm(vmin=x.min().min(), vmax=x.max().max()))\n</code></pre>\n\n<p>The full <code>prettyplotlib</code> code is,</p>\n\n<pre><code>import prettyplotlib as ppl\nimport brewer2mpl\nimport numpy as np\nimport string\nfrom matplotlib.colors import LogNorm\n\nred_purple = brewer2mpl.get_map('RdPu', 'Sequential', 9).mpl_colormap\n\nfig, ax = ppl.subplots(1)\n\nnp.random.seed(10)\n\nx = np.abs(np.random.randn(10,10))\nppl.pcolormesh(fig, ax, x,\n               xticklabels=string.uppercase[:10], \n               yticklabels=string.lowercase[-10:],\n               cmap=red_purple, \n               norm=LogNorm(vmin=x.min().min(), vmax=x.max().max()))\nfig.savefig('pcolormesh_prettyplotlib_labels_lognorm.png')\n</code></pre>\n\n<p><img src=\"https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_labels_lognorm.png\" alt=\"Log-normalized heatmap\"/></p>\n\n<p>And now you can easily make beautiful heatmaps!</p>\n\n<h2>That&#8217;s all, folks!</h2>\n\n<p>That&#8217;s my introduction to <code>prettyplotlib</code> and why you need it. There are similar\nexamples for the other functions, but these ones for <code>ppl.scatter</code> and <code>ppl.pcolormesh</code> are the most\nextensive.</p>"}
publish: 2013-08-021
tags: python, matplotlib, prettyplotlib, programming, data visualization, dataviz
title: prettyplotlib: Painlessly create beautiful matplotlib plots
-->


prettyplotlib: Painlessly create beautiful matplotlib plots
===========================================================

A while back I wrote a few tutorials about how to work with Python’s
plotting library, `matplotlib`, so that it behaves nicely and produces
beautiful plots. Well, I got tired of tweaking every single figure
individually so I wrote this library,
[`prettyplotlib`](http://olgabot.github.io/prettyplotlib) to have pretty
default plots in Python’s `matplotlib`.

I truly believe that poor visualizations obstruct scientific progress,
and this is my contribution.

A couple motivating examples are below, but if you just want an
overview, check out the [Comparison to matplotlib
defaults](https://github.com/olgabot/prettyplotlib/wiki/Comparison-to-matplotlib),
[Examples
Gallery](https://github.com/olgabot/prettyplotlib/wiki/Gallery), and
[Examples with
code](https://github.com/olgabot/prettyplotlib/wiki/Examples-with-code)
on Github.

To install, do the usual `pip install` stuff:

    pip install prettyplotlib

I truly hope you enjoy using this library! If you have any comments or
suggestions, let me know!

`prettyplotlib.scatter`
=======================

The default `matplotlib` color cycle is not pretty to look at. What’s
even worse is that if you just do a `scatter` plot, then it doesn’t
cycle at all through any values

    import matplotlib.pyplot as plt
    # Set the random seed for consistency
    np.random.seed(12)

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        ax.scatter(x, y, label=str(i))
    ax.legend()

    ax.set_title('prettyplotlib `scatter` example\nshowing default matplotlib `scatter`')
    fig.savefig('scatter_matplotlib_default.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_default.png)

Before `prettyplotlib`: how to make nice plots
----------------------------------------------

Now I’m going to take you through ALL the steps I used to take to make
nice looking plots.

First, change the colors with `brewer2mpl`:

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors
    ...
    color = set2[i]
    ax.scatter(x, y, label=str(i), facecolor=color)

The full code is,

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), color=color)

    fig.savefig('scatter_matplotlib_improved_01_changed_colors.png')

![Matplotlib scatter improved 01: changed
colors](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_01_changed_colors.png)

This looks nice, almost like an impressionist painting, but it’s still
hard to see overlaps here. So let’s fill the symbols with `0.5` opacity
using `alpha=0.5`.

    ax.scatter(x, y, label=str(i), color=color, alpha=0.5)

The full code is,

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), color=color, alpha=0.5)

    fig.savefig('scatter_matplotlib_improved_02_added_alpha.png')

![Matplotlib scatter improved 02: added
alpha](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_02_added_alpha.png)

This is still pretty lovely and impressionist-y but I still didn’t like
that it was hard to see when the dots overlapped. So let’s add a black
outline, and specify that `color` is just the `facecolor`:

    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black',
    facecolor=color)

The full code is,

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color)

    fig.savefig('scatter_matplotlib_improved_03_added_outline.png')

![Matplotlib scatter improved 03: added black
outline](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_03_added_outline.png)

Ack, but those lines are too thick … let’s think them down to
`linewidth=0.15`

    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black',
    facecolor=color, linewidth=0.15)

The full code is,

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color, linewidth=0.15)
    fig.savefig('scatter_matplotlib_improved_04_thinned_outline.png')

![Matplotlib scatter improved 04: thinned out black
outline](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_04_thinned_outline.png)

*Now* we’re getting somewhere. This looks very lovely. Don’t you want to
just cuddle up with that cute plot?

What are those top and right axes lines really doing for us? They’re
boxing the data in, but we can do that with our eyes from the other axis
lines. So let’s remove the top and right axis lines using `ax.spines`:

    # Remove top and right axes lines ("spines")
    spines_to_remove = ['top', 'right']
    for spine in spines_to_remove:
    ax.spines[spine].set_visible(False)

The full code is,

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color, linewidth=0.15)

    # Remove top and right axes lines ("spines")
    spines_to_remove = ['top', 'right']
    for spine in spines_to_remove:
        ax.spines[spine].set_visible(False)
    fig.savefig('scatter_matplotlib_improved_05_removed_top_right_spines.png')

![Matplotlib scatter improved 05: removed top and right axis
lines](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_05_removed_top_right_spines.png)

Oops, but we still have the ticks on the top and right axes. We’ll need
to get rid of them. Actually, why don’t we just get rid of all ticks
altogether? We can tell by the position of the number where it
indicates, so we don’t need an additional tick.

    # Get rid of ticks. The position of the numbers is informative enough of
    # the position of the value.
    ax.xaxis.set_ticks_position('none')
    ax.yaxis.set_ticks_position('none')

Here’s the full code:

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color, linewidth=0.15)

    # Remove top and right axes lines ("spines")
    spines_to_remove = ['top', 'right']
    for spine in spines_to_remove:
        ax.spines[spine].set_visible(False)

    # Get rid of ticks. The position of the numbers is informative enough of
    # the position of the value.
    ax.xaxis.set_ticks_position('none')
    ax.yaxis.set_ticks_position('none')
    fig.savefig('scatter_matplotlib_improved_06_removed_ticks.png')

![Matplotlib scatter improved 06: removed tick
marks](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_06_removed_ticks.png)

Ahh, much better. But we won’t stop there. Now we’ll tweak the remaining
pieces of the figure. For the rest of the spines, let’s thin the line
down to `0.5` points instead of the default `1.0` points. Also, we’ll
change it from pure black to a slightly lighter dark grey. Here they are
side by side:

    fig, axes = plt.subplots(2)
    axes[0].set_axis_bgcolor('black')
    axes[0].text(0.5, 0.5, 'black', color='white', fontsize=24, va='center', ha='center')
    axes[1].set_axis_bgcolor('#262626')
    axes[1].text(0.5, 0.5, 'almost black', fontsize=24, color='white', va='center', ha='center')
    fig.savefig('black_vs_almost_black.png')

![Matplotlib scatter improved 06: removed tick
marks](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/black_vs_almost_black.png)

So not a *huge* difference, and the dark grey still looks pretty black,
but it’s [a little more pleasant on the
eyes](http://ianstormtaylor.com/design-tip-never-use-black/) to use a
dark grey instead of black. There’s very few things in nature that are
truly black. Just look at shadows! They’re just dark grey, or blue, or
red or purple. But I digress. Back to plotting libraries…

To change the x-axis and y-axis line colors, and the outlines of the
scatter symbols from black to dark grey, we’ll do:

    # For remaining spines, thin out their line and change the black to a slightly off-black dark grey
    almost_black = '#262626'
    ...
    ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor='black', facecolor=color, linewidth=0.15)`
    ...
    spines_to_keep = ['bottom', 'left']
    for spine in spines_to_keep:
        ax.spines[spine].set_linewidth(0.5)
        ax.spines[spine].set_color(almost_black)

The full code is,

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    # Save a nice dark grey as a variable
    almost_black = '#262626'

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor=almost_black, facecolor=color, linewidth=0.15)

    # Remove top and right axes lines ("spines")
    spines_to_remove = ['top', 'right']
    for spine in spines_to_remove:
        ax.spines[spine].set_visible(False)

    # Get rid of ticks. The position of the numbers is informative enough of
    # the position of the value.
    ax.xaxis.set_ticks_position('none')
    ax.yaxis.set_ticks_position('none')

    # For remaining spines, thin out their line and change the black to a slightly off-black dark grey
    spines_to_keep = ['bottom', 'left']
    for spine in spines_to_keep:
        ax.spines[spine].set_linewidth(0.5)
        ax.spines[spine].set_color(almost_black)
    fig.savefig('scatter_matplotlib_improved_07_axis_black_to_almost_black.png')

![Matplotlib scatter improved 07: changed axis lines from black to
almost
black](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_07_axis_black_to_almost_black.png)

This is nice. But if you look closely, the tick labels are still black
:( We have to change them separately, using

    # Change the labels to the off-black
    ax.xaxis.label.set_color(almost_black)
    ax.yaxis.label.set_color(almost_black)

And while we’re at it, let’s add a title and make it dark grey too.

    # Change the axis title to off-black
    ax.title.set_color(almost_black)

    ax.set_title('prettyplotlib `scatter` example\nshowing improved matplotlib `scatter`')

The full code is,

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    # Save a nice dark grey as a variable
    almost_black = '#262626'

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor=almost_black, facecolor=color, linewidth=0.15)

    # Remove top and right axes lines ("spines")
    spines_to_remove = ['top', 'right']
    for spine in spines_to_remove:
        ax.spines[spine].set_visible(False)

    # Get rid of ticks. The position of the numbers is informative enough of
    # the position of the value.
    ax.xaxis.set_ticks_position('none')
    ax.yaxis.set_ticks_position('none')

    # For remaining spines, thin out their line and change the black to a slightly off-black dark grey
    spines_to_keep = ['bottom', 'left']
    for spine in spines_to_keep:
        ax.spines[spine].set_linewidth(0.5)
        ax.spines[spine].set_color(almost_black)

    # Change the labels to the off-black
    ax.xaxis.label.set_color(almost_black)
    ax.yaxis.label.set_color(almost_black)

    # Change the axis title to off-black
    ax.title.set_color(almost_black)

    ax.set_title('prettyplotlib `scatter` example\nshowing improved matplotlib `scatter`')
    fig.savefig('scatter_matplotlib_improved_08_labels_black_to_almost_black.png')

![Matplotlib scatter improved 08: changed labels to almost
black](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_08_labels_black_to_almost_black.png)

If you remember in the original example, we also had an axis legend,
using

    ax.legend()

So let’s add it to this code, too.

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    # Save a nice dark grey as a variable
    almost_black = '#262626'

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor=almost_black, facecolor=color, linewidth=0.15)

    # Remove top and right axes lines ("spines")
    spines_to_remove = ['top', 'right']
    for spine in spines_to_remove:
        ax.spines[spine].set_visible(False)

    # Get rid of ticks. The position of the numbers is informative enough of
    # the position of the value.
    ax.xaxis.set_ticks_position('none')
    ax.yaxis.set_ticks_position('none')

    # For remaining spines, thin out their line and change the black to a slightly off-black dark grey
    almost_black = '#262626'
    spines_to_keep = ['bottom', 'left']
    for spine in spines_to_keep:
        ax.spines[spine].set_linewidth(0.5)
        ax.spines[spine].set_color(almost_black)

    # Change the labels to the off-black
    ax.xaxis.label.set_color(almost_black)
    ax.yaxis.label.set_color(almost_black)

    # Change the axis title to off-black
    ax.title.set_color(almost_black)

    ax.legend()

    ax.set_title('prettyplotlib `scatter` example\nshowing improved matplotlib `scatter`')
    fig.savefig('scatter_matplotlib_improved_09_ugly_legend.png')

![Matplotlib scatter improved 09: added ugly
legend](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_09_ugly_legend.png)

There are many things I don’t like about this legend.

1.  First of all, why does it have such a thick border line? What does
    that really add to our interpretation of the legend? The black line
    is so thick that it distracts from what we’re trying to portray -
    which label goes with which color.
2.  Why does it show three points? Does this legend think I’m dumb and
    can’t figure out which symbol goes with which label after one
    iteration, so it does it three times?
3.  Finally, the legend labels are pure black. Maybe you notice it too,
    after comparing to x-axis and y-axis lines and labels.

We’ll accomplish these three things using this code:

    # Remove the line around the legend box, and instead fill it with a light grey
    # Also only use one point for the scatterplot legend because the user will
    # get the idea after just one, they don't need three.
    light_grey = np.array([float(248)/float(255)]*3)
    legend = ax.legend(frameon=True, scatterpoints=1, fontcolor=almost_black)
    rect = legend.get_frame()
    rect.set_facecolor(light_grey)
    rect.set_linewidth(0.0)

    # Change the legend label colors to almost black, too
    texts = legend.texts
    for t in texts:
        t.set_color(almost_black)

Now our code is pretty huge …

    import matplotlib.pyplot as plt
    import brewer2mpl

    # Get "Set2" colors from ColorBrewer (all colorbrewer scales: http://bl.ocks.org/mbostock/5577023)
    set2 = brewer2mpl.get_map('Set2', 'qualitative', 8).mpl_colors

    # Set the random seed for consistency
    np.random.seed(12)

    # Save a nice dark grey as a variable
    almost_black = '#262626'

    fig, ax = plt.subplots(1)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        color = set2[i]
        ax.scatter(x, y, label=str(i), alpha=0.5, edgecolor=almost_black, facecolor=color, linewidth=0.15)

    # Remove top and right axes lines ("spines")
    spines_to_remove = ['top', 'right']
    for spine in spines_to_remove:
        ax.spines[spine].set_visible(False)

    # Get rid of ticks. The position of the numbers is informative enough of
    # the position of the value.
    ax.xaxis.set_ticks_position('none')
    ax.yaxis.set_ticks_position('none')

    # For remaining spines, thin out their line and change the black to a slightly off-black dark grey
    almost_black = '#262626'
    spines_to_keep = ['bottom', 'left']
    for spine in spines_to_keep:
        ax.spines[spine].set_linewidth(0.5)
        ax.spines[spine].set_color(almost_black)

    # Change the labels to the off-black
    ax.xaxis.label.set_color(almost_black)
    ax.yaxis.label.set_color(almost_black)

    # Change the axis title to off-black
    ax.title.set_color(almost_black)

    # Remove the line around the legend box, and instead fill it with a light grey
    # Also only use one point for the scatterplot legend because the user will 
    # get the idea after just one, they don't need three.
    light_grey = np.array([float(248)/float(255)]*3)
    legend = ax.legend(frameon=True, scatterpoints=1)
    rect = legend.get_frame()
    rect.set_facecolor(light_grey)
    rect.set_linewidth(0.0)

    # Change the legend label colors to almost black, too
    texts = legend.texts
    for t in texts:
        t.set_color(almost_black)


    ax.set_title('prettyplotlib `scatter` example\nshowing improved matplotlib `scatter`')
    fig.savefig('scatter_matplotlib_improved_10_pretty_legend.png')

![Matplotlib scatter improved 10: beautiful
legend](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_matplotlib_improved_10_pretty_legend.png)

Aaaaaaaaaaand I got tired of doing all those steps, EVERY time I wanted
to make a simple scatterplot. So I wrote
[`prettyplotlib`](http://github.com/olgabot/prettyplotlib). Here’s an
illustrative example of how awesome `prettyplotlib` is, and how it will
save all the time you spent agonizing over making your `matplotlib`
plots beautiful.

    import prettyplotlib as ppl
    import numpy as np

    fig, ax = ppl.subplots()

    # Set the random seed for consistency
    np.random.seed(12)

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        ppl.scatter(ax, x, y, label=str(i))

    ppl.legend()

    ax.set_title('prettyplotlib `scatter` example\nshowing default color cycle and scatter params')
    fig.savefig('scatter_prettyplotlib_default.png')

![Matplotlib scatter improved 10: beautiful
legend](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_prettyplotlib_default.png)

The only commands that were different from the very first example with
matplotlib are:

    ppl.scatter(ax, x, y, label=str(i), facecolor='none')

instead of:

    ax.scatter(x, y, label=str(i))

And a different legend command:

    ppl.legend(ax)

instead of:

    ax.legend()

If you ***really*** want to get the original matplotlib style back in
prettyplotlib, you can do:

    import prettyplotlib as ppl

    # Set the random seed for consistency
    np.random.seed(12)

    fig, ax = plt.subplots(1)

    #mpl.rcParams['axis.color_cycle'] = ['blue']

    # Show the whole color range
    for i in range(8):
        x = np.random.normal(loc=i, size=1000)
        y = np.random.normal(loc=i, size=1000)
        ax.scatter(x, y, label=str(i), facecolor='blue', edgecolor='black', linewidth=1)

    # Get back the top and right axes lines ("spines")
    spines_to_remove = ['top', 'right']
    for spine in spines_to_remove:
        ax.spines[spine].set_visible(True)

    # Get back the ticks. The position of the numbers is informative enough of
    # the position of the value.
    ax.xaxis.set_ticks_position('both')
    ax.yaxis.set_ticks_position('both')

    # For all the spines, make their line thicker and return them to be black
    all_spines = ['top', 'left', 'bottom', 'right']
    for spine in all_spines:
        ax.spines[spine].set_linewidth(1.0)
        ax.spines[spine].set_color('black')

    # Change the labels back to black
    ax.xaxis.label.set_color('black')
    ax.yaxis.label.set_color('black')

    # Change the axis title also back to black
    ax.title.set_color('black')

    # Remove the line around the legend box, and instead fill it with a light grey
    # Also only use one point for the scatterplot legend because the user will 
    # get the idea after just one, they don't need three.
    ax.legend()

    ax.set_title('prettyplotlib `scatter` example\nrevert everything back to default matplotlib parameters')
    fig.savefig('scatter_prettyplotlib_back_to_matplotlib_default.png')

![Prettyplotlib back to
matplotlib](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/scatter_prettyplotlib_back_to_matplotlib_default.png)

Notice that the default calls of `ax.scatter` and `ax.legend` do the
usual thing. This is important, because for `prettyplotlib` to work,
you’ll need to use a syntax that’s different from the usual `matplotlib`
one: `ppl.scatter(ax, x, y...)` instead of `ax.scatter(x, y, ...)`

`prettyplotlib.pcolormesh`: Improving heatmaps in `matplotlib`
--------------------------------------------------------------

### Both positive and negative values

The default `matplotlib` `pcolormesh` heatmaps use a rainbow colormap,
which has been known to mislead data visualization. Specifically, [*"the
rainbow color map is universally inferior to all other color
maps"*]([http://www.jwave.vt.edu/\~rkri](http://www.jwave.vt.edu/~rkri)
z/Projects/create\_color\_table/color\_07.pdf). Unfortunately,
`matplotlib` took its default colors from MATLAB, and there the default
is also rainbow.

    import matplotlib.pyplot as plt
    import numpy as np

    fig, ax = plt.subplots(1)

    np.random.seed(10)

    #ax.pcolor(np.random.randn((10,10)))
    #ax.pcolor(np.random.randn(10), np.random.randn(10))
    p = ax.pcolormesh(np.random.randn(10,10))
    fig.colorbar(p)
    fig.savefig('pcolormesh_matplotlib_default.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_matplotlib_default.png)

Using the same zero-centered randomly distributed gaussian distribution,
we can plot it using `prettyplotlib` with a few modifications in syntax:

    ppl.pcolormesh(fig, ax, np.random.randn(10,10))

You’ll notice that the “hot” (large, positive) color is still red, and
the “cold” (small, negative) color is still blue, but the in between
colors are gradations of red and blue, so it’s easier to tell the
difference between values.

    import prettyplotlib as ppl
    import numpy as np

    fig, ax = ppl.subplots(1)

    np.random.seed(10)

    ppl.pcolormesh(fig, ax, np.random.randn(10,10))
    fig.savefig('pcolormesh_prettyplotlib_default.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_default.png)

You may have also noticed similar changes as were made in
`prettyplotlib.scatter`, where axis lines were removed, and blacks were
changed to almost black.

### Only positive (or negative) values

If your data is only positive (or negative), `matplotlib` does nothing
to change the color scale. It’s still a rainbow, but look at the
colorbar, the range is different (0 to 1 instead of -2 to +2)

    import prettyplotlib as ppl
    import numpy as np

    fig, ax = ppl.subplots(1)

    np.random.seed(10)

    p = ax.pcolormesh(np.random.uniform(size=(10,10)))
    fig.colorbar(p)
    fig.savefig('pcolormesh_matplotlib_positive_default.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_matplotlib_positive_default.png)

If your data is only positive or negative, then `prettyplotlib` will
auto-detect this and use a single-color colormap. The default for
positive data is the `reds` colormap.

    import prettyplotlib as ppl
    import numpy as np

    fig, ax = ppl.subplots(1)

    np.random.seed(10)

    ppl.pcolormesh(fig, ax, np.abs(np.random.randn(10,10)))
    fig.savefig('pcolormesh_prettyplotlib_positive.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_positive.png)

And the default for negative data is the `blues` colormap.

    import prettyplotlib as ppl
    import numpy as np

    fig, ax = ppl.subplots(1)

    np.random.seed(10)

    ppl.pcolormesh(fig, ax, -np.abs(np.random.randn(10,10)))
    fig.savefig('pcolormesh_prettyplotlib_negative.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_negative.png)

Add x or y tick labels
----------------------

Plus you can add x- and y-ticklabels directly!

Normally, when you add x- and y-ticklabels on `pcolormesh` in
`matplotlib`, they’re not centered on the blocks, and you have to do a
lot of annoying work just getting a label on each box. You have to
specify the xticks explicitly, since you want to label each box.

    xticks = range(10)
    yticks = range(10)

    xticklabels=string.uppercase[:10]
    yticklabels=string.lowercase[-10:]

    ax.set_xticks(xticks)
    ax.set_xticklabels(xticklabels)

    ax.set_yticks(yticks)
    ax.set_yticklabels(yticklabels)

The full, `matplotlib` code is:

    import prettyplotlib as ppl
    from prettyplotlib import plt
    import numpy as np
    import string

    fig, ax = plt.subplots(1)

    np.random.seed(10)

    p = ax.pcolormesh(np.abs(np.random.randn(10,10)))
    fig.colorbar(p)

    xticks = range(10)
    yticks = range(10)

    xticklabels=string.uppercase[:10]
    yticklabels=string.lowercase[-10:]

    ax.set_xticks(xticks)
    ax.set_xticklabels(xticklabels)

    ax.set_yticks(yticks)
    ax.set_yticklabels(yticklabels)


    fig.savefig('pcolormesh_matplotlib_positive_labels.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_matplotlib_positive_labels.png)

But `prettyplotlib.pcolormesh` assumes that you want the `xticklabels`
and `yticklabels` on each block, and makes it easy to specify.

    ppl.pcolormesh(fig, ax, np.random.uniform(size=(10,10)),
                   xticklabels=string.uppercase[:10],
                   yticklabels=string.lowercase[-10:])

The full `prettyplotlib` code is,

    import prettyplotlib as ppl
    import numpy as np
    import string

    fig, ax = ppl.subplots(1)

    np.random.seed(10)

    ppl.pcolormesh(fig, ax, np.random.randn(10,10), 
                   xticklabels=string.uppercase[:10], 
                   yticklabels=string.lowercase[-10:])
    fig.savefig('pcolormesh_prettyplotlib_labels.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_labels.png)

### Custom colormaps

Or pick your own colormap! The diverging colormap `PRGn` or Purple and
Green is pretty nice. I usually use this website to look up the
colormaps: [Every Colorbrewer
Scale](http://bl.ocks.org/mbostock/5577023) (hover over the colors to
get the name of the colormap)

    import prettyplotlib as ppl
    import brewer2mpl
    import numpy as np
    import string

    green_purple = brewer2mpl.get_map('PRGn', 'diverging', 11).mpl_colormap

    fig, ax = ppl.subplots(1)

    np.random.seed(10)

    ppl.pcolormesh(fig, ax, np.random.randn(10,10), 
                   xticklabels=string.uppercase[:10], 
                   yticklabels=string.lowercase[-10:],
                   cmap=green_purple)
    fig.savefig('pcolormesh_prettyplotlib_labels_other_cmap_diverging.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_labels_other_cmap_diverging.png)

Or if you want your own colormap for positive-only data:

    import prettyplotlib as ppl
    import brewer2mpl
    import numpy as np
    import string

    red_purple = brewer2mpl.get_map('RdPu', 'Sequential', 9).mpl_colormap

    fig, ax = ppl.subplots(1)

    np.random.seed(10)

    ppl.pcolormesh(fig, ax, np.abs(np.random.randn(10,10)),
                   xticklabels=string.uppercase[:10], 
                   yticklabels=string.lowercase[-10:],
                   cmap=red_purple)
    fig.savefig('pcolormesh_prettyplotlib_labels_other_cmap_sequential.png')

![Matplotlib default
scatterplot](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_labels_other_cmap_sequential.png)

### Log normalization or other parameters

Plus, this will take the usual parameters of `pcolormesh` like if you
want to rescale your data to log-scale:

    from matplotlib.colors import LogNorm
    ...
    ppl.pcolormesh(..., norm=LogNorm(vmin=x.min().min(), vmax=x.max().max()))

The full `prettyplotlib` code is,

    import prettyplotlib as ppl
    import brewer2mpl
    import numpy as np
    import string
    from matplotlib.colors import LogNorm

    red_purple = brewer2mpl.get_map('RdPu', 'Sequential', 9).mpl_colormap

    fig, ax = ppl.subplots(1)

    np.random.seed(10)

    x = np.abs(np.random.randn(10,10))
    ppl.pcolormesh(fig, ax, x,
                   xticklabels=string.uppercase[:10], 
                   yticklabels=string.lowercase[-10:],
                   cmap=red_purple, 
                   norm=LogNorm(vmin=x.min().min(), vmax=x.max().max()))
    fig.savefig('pcolormesh_prettyplotlib_labels_lognorm.png')

![Log-normalized
heatmap](https://raw.github.com/olgabot/prettyplotlib/master/ipython_notebooks/pcolormesh_prettyplotlib_labels_lognorm.png)

And now you can easily make beautiful heatmaps!

That’s all, folks!
------------------

That’s my introduction to `prettyplotlib` and why you need it. There are
similar examples for the other functions, but these ones for
`ppl.scatter` and `ppl.pcolormesh` are the most extensive.


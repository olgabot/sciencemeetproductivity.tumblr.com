<!--
id: 84438142546
link: http://blog.olgabotvinnik.com/post/84438142546/implementing-annotation-free-quantification-of-splice
slug: implementing-annotation-free-quantification-of-splice
date: Thu May 01 2014 10:52:00 GMT-0700 (PDT)
raw: {"blog_name":"sciencemeetproductivity","id":84438142546,"post_url":"http://blog.olgabotvinnik.com/post/84438142546/implementing-annotation-free-quantification-of-splice","slug":"implementing-annotation-free-quantification-of-splice","type":"text","date":"2014-05-01 17:52:00 GMT","timestamp":1398966720,"state":"published","format":"markdown","reblog_key":"IiGyzji5","tags":["rna splicing","alternative splicing","python","pandas","rna","science"],"short_url":"http://tmblr.co/ZStENu1EevyvI","highlighted":[],"note_count":1,"title":"Implementing annotation-free quantification of splice junction usage via RNA-STAR and Python+Pandas magic","body":"<p>If you&#8217;ve been reading my blog for general python/productivity stuff, you are in for a TREAT! Here is some REAL SCIENCE! I&#8217;ll give a quick intro but please let me know if anything is unclear. I spend most of my time talking to a few people who study this stuff in painful detail, so I&#8217;d love to know where my explanation becomes cloudy.</p>\n\n<p><strong><em>tl;dr: I wrote a small tool for quantifying alternative junction usage from RNA-STAR alignment output: <a href=\"https://github.com/olgabot/sj2psi\" target=\"_blank\">https://github.com/olgabot/sj2psi</a> And it&#8217;s on PyPI so you can <code>pip install sj2psi</code> as well.</em></strong></p>\n\n<p>Alternative splicing is a method of creating different versions of a gene using the same DNA. It is awesome because it increases the possible number of genes in say the human genome from just 30,000 to millions. An extreme example is the <em>Drosoephila</em> (fruit fly) gene <em>Dscam</em>, which has over 60,000 potential versions, more than the genes in the <em>Drosophila</em> genome! Alternative splicing happens by combining different &#8220;<em>exons</em>\" together (always in linear order) to create different proteins (or other gene products like <a href=\"http://en.wikipedia.org/wiki/Non-coding_RNA\" target=\"_blank\">ncRNAs</a> but we won&#8217;t go into that)</p>\n\n<p><img src=\"http://upload.wikimedia.org/wikipedia/commons/0/0a/DNA_alternative_splicing.gif\" alt=\"\"/></p>\n\n<p>These different versions of the same gene are called &#8220;isoforms.&#8221; What we care about is looking at individual events because we study <a href=\"http://en.wikipedia.org/wiki/RNA-binding_protein\" target=\"_blank\">RNA binding proteins</a> (RBPs), some of which have an effect on alternative splicing when they bind RNA. So maybe we know that an RBP binds some gene&#8217;s RNA, but it (most likely) only has an effect on the exon(s) immediately before or after. For example, the RBP FOX2 is one of <a href=\"http://yeolab.ucsd.edu/yeolab/Home.html\" target=\"_blank\">our lab&#8217;s</a> favorites, partly because our boss found that if FOX2 binds upstream (&#8220;before&#8221; in DNA terms) of an exon, it suppresses inclusion of it. HOWEVER! FOX2 binding downstream (&#8220;after&#8221;) an exon promotes inclusion! Check out this figure from <a href=\"http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2735254/\" target=\"_blank\">Yeo et al, <em>Nat Struct Mol Bio</em> (2009)</a>:</p>\n\n<p><img src=\"https://31.media.tumblr.com/8749e1c26cb36237e1d8d2ea874327f1/tumblr_inline_n4wpkodHzT1rw6gvj.png\" alt=\"\"/></p>\n\n<p>Quantifying alternative exon usage and changes is always a challenge, and this is especially difficult for unannotated splicing events. Which is why I was excited when I read <a href=\"http://genome.cshlp.org/content/early/2013/12/03/gr.161034.113?top=1\" target=\"_blank\">a single-cell paper</a> learned of <a href=\"https://github.com/pervouchine/bam2ssj\" target=\"_blank\"><code>bam2ssj</code></a> (Paper: <a href=\"http://www.ncbi.nlm.nih.gov/pubmed/23172860\" target=\"_blank\">Pervouchine et al, <em>Bioinformatics</em> (2013)</a> and its successor, <a href=\"https://github.com/pervouchine/sjcount\" target=\"_blank\"><code>sjcount</code></a>. The promise is that instead of quantifying whole splicing events which have to fall into some specific category (SE/MXE/RI/A5SS/A3SS/AFE/ALE/TandemUTR, see this figure from <a href=\"http://www.nature.com/nature/journal/v456/n7221/abs/nature07509.html\" target=\"_blank\">Wang ET et al, <em>Nature</em> (2008)</a>. Each &#8220;box&#8221; is an exon and the lines in between exons are stretches of DNA that are removed (called &#8220;introns&#8221; because they &#8220;interrupt&#8221; the exons)</p>\n\n<p><img src=\"http://www.nature.com/nature/journal/v456/n7221/images/nature07509-f2.2.jpg\" alt=\"\"/></p>\n\n<p>Plus these events have to be annotated for use with my splicing quantifier of choice (<a href=\"http://miso.readthedocs.org/en/fastmiso/\" target=\"_blank\">MISO</a>), or you could try to pull them out from the aligned file. However, I work with single-cell data and since splicing is mostly binary in single cells (i.e. you have mostly isoform A and not both isoform A and B, citation: <a href=\"http://www.ncbi.nlm.nih.gov/pubmed/23685454\" target=\"_blank\">Shalek et al, <em>Nature</em> (2013)</a>), it becomes a pain to make these annotations, because you have to merge ALL the files together to create like a terrabyte-sized alignment file (usually a <a href=\"http://genome.sph.umich.edu/wiki/BAM\" target=\"_blank\"><code>*.bam</code> file</a>) and then you are introducing all kinds of craziness because what could have been noise in a few cells gets added up together and becomes signal.</p>\n\n<p>Sigh.</p>\n\n<p>So I&#8217;d much rather use something that quantifies splicing of all junctions, arbitrarily. But when I tried using <code>sjcount</code> I couldn&#8217;t figure out what one of the outputs was (like what&#8217;s the difference between a &#8220;splice junction&#8221; and a &#8220;splice boundary?&#8221;). Plus I realized that the &#8220;splice junction counts&#8221; (<code>*.ssj</code>) file was really similar to the &#8220;<code>SJ.out.tab</code>\" file created by the <a href=\"https://code.google.com/p/rna-star/\" target=\"_blank\">RNA-STAR</a> aligner.</p>\n\n<p><code>.ssj</code> files look like (I added the column names):</p>\n\n<pre><code>chrom   start   stop    strand  overhang    count\nchr1    146509  155767  .       20      1\nchr1    146509  155767  .       34      1\nchr1    146509  155767  .       49      3\nchr1    168165  169049  .       10      2\nchr1    168165  169049  .       49      3\nchr1    694503  700103  .       29      1\nchr1    694503  700103  .       41      1\nchr1    694503  700103  .       49      1\nchr1    894461  894595  .       7       2\nchr1    894461  894595  .       9       5\n</code></pre>\n\n<p>\"overhang\" is the number of base pairs of the sequencing read that overlaps with the upstream exon of the splicing event. The rest of the read is on the downstream exon. In our case, our reads are 100 base pairs (\"100bp\") so if there&#8217;s 20bp overhanging on one exon, the other 80bp are on the other exon.</p>\n\n<p>While <code>SJ.out.tab</code> files are (again I added column names):</p>\n\n<table border=\"1\" class=\"dataframe\">\\n  <thead>\\n    <tr style=\"text-align: right;\">\\n      <th></th>\\n      <th>chrom</th>\\n      <th>first_bp_intron</th>\\n      <th>last_bp_intron</th>\\n      <th>strand</th>\\n      <th>intron_motif</th>\\n      <th>annotated</th>\\n      <th>unique_junction_reads</th>\\n      <th>multimap_junction_reads</th>\\n      <th>max_overhang</th>\\n    </tr>\\n  </thead>\\n  <tbody>\\n    <tr>\\n      <th>0</th>\\n      <td> chr1</td>\\n      <td> 135332</td>\\n      <td> 138297</td>\\n      <td> 1</td>\\n      <td> GC/AG</td>\\n      <td> False</td>\\n      <td> 0</td>\\n      <td> 1</td>\\n      <td> 38</td>\\n    </tr>\\n    <tr>\\n      <th>1</th>\\n      <td> chr1</td>\\n      <td> 146510</td>\\n      <td> 155766</td>\\n      <td> 2</td>\\n      <td> CT/AC</td>\\n      <td>  True</td>\\n      <td> 5</td>\\n      <td> 1</td>\\n      <td> 34</td>\\n    </tr>\\n    <tr>\\n      <th>2</th>\\n      <td> chr1</td>\\n      <td> 156895</td>\\n      <td> 158300</td>\\n      <td> 1</td>\\n      <td> GC/AG</td>\\n      <td> False</td>\\n      <td> 0</td>\\n      <td> 2</td>\\n      <td> 17</td>\\n    </tr>\\n    <tr>\\n      <th>3</th>\\n      <td> chr1</td>\\n      <td> 168166</td>\\n      <td> 169048</td>\\n      <td> 2</td>\\n      <td> CT/AC</td>\\n      <td>  True</td>\\n      <td> 5</td>\\n      <td> 3</td>\\n      <td> 46</td>\\n    </tr>\\n    <tr>\\n      <th>4</th>\\n      <td> chr1</td>\\n      <td> 169265</td>\\n      <td> 172556</td>\\n      <td> 2</td>\\n      <td> CT/AC</td>\\n      <td>  True</td>\\n      <td> 0</td>\\n      <td> 2</td>\\n      <td> 13</td>\\n    </tr>\\n  </tbody>\\n</table><p>The start/stop are off by one here because <code>.ssj</code>'s index is from the exon end/exon start, and this is from the intron start/intron end. So the SJ.out.tab coordinates are (start from <code>.ssj</code>+1) and (end from <code>.ssj</code>-1).</p>\n\n<p>FYI, the <code>.ssc</code> files looked like:</p>\n\n<pre><code>chr1    894595  894595  .       8       1\nchr1    1228468 1228468 .       1       1\nchr1    1228468 1228468 .       3       3\nchr1    1228468 1228468 .       5       1\nchr1    1228468 1228468 .       6       1\nchr1    1228468 1228468 .       7       2\nchr1    1228468 1228468 .       12      1\nchr1    1228468 1228468 .       13      2\nchr1    1228468 1228468 .       16      2\nchr1    1228468 1228468 .       27      1\n</code></pre>\n\n<p>This claims to have the same columns as <code>.ssj</code> files except instead of start/stop it&#8217;s position/position. I was very confused by how you can have a count at a position. Why does <code>chr1:894595</code> have one line while <code>chr1:1228468</code> have a bunch? If you search for <code>894595</code> in the <code>.ssj</code> file, you get:</p>\n\n<pre><code>chr1    894461  894595  .       7       2\nchr1    894461  894595  .       9       5\nchr1    894461  894595  .       10      2\nchr1    894461  894595  .       11      1\nchr1    894461  894595  .       12      1\nchr1    894461  894595  .       14      1\nchr1    894461  894595  .       16      2\nchr1    894461  894595  .       19      1\nchr1    894461  894595  .       20      1\nchr1    894461  894595  .       21      13\nchr1    894461  894595  .       22      1\nchr1    894461  894595  .       42      2\nchr1    894461  894595  .       49      14\n</code></pre>\n\n<p>How did all these counts in the last column (which sum to 46 by my calculation) become <strong>1</strong> in the <code>.ssc</code> file? I was pretty lost.</p>\n\n<p>ANYWAYS all of this is a buildup to say that I used Pervouchine et al&#8217;s method of quantifying alternative junctions as asking, when this upstream exon (splicing <em>donor</em>, <em>D</em> in figure below) is used with this downstream exon (splicing <em>acceptor</em>, <em>A</em> in figure below), how often does that happen relative to all other donors and acceptors? In the figure below, the splicing event of interest is a thick, bold line, joined by a solid arc representing the spliced read. The dotted lines represent that this donor could be spliced to a bunch of different acceptor sites.</p>\n\n<p><img src=\"http://bioinformatics.oxfordjournals.org/content/29/2/273/F2.medium.gif\" alt=\"Donor with alternative acceptors on left, Acceptor with alternative donors on right\"/></p>\n\n<p>This is quantified with a $\\Psi_5$ (&#8220;Percent spliced-in&#8221;, or &#8220;Psi&#8221; of the donor, which is at the 5&#8217; end of the RNA) and $\\Psi_3$ (&#8220;Psi&#8221; of the acceptor, located at the 3&#8217; end of the RNA) scores:</p>\n\n<p><img src=\"http://bioinformatics.oxfordjournals.org/content/29/2/273/embed/graphic-3.gif\" alt=\"\"/></p>\n\n<p>Where the summation is over all other possible acceptors (in the case of <em>A&#8217;</em>) or all other possible donors (in the case of <em>D&#8217;</em>).</p>\n\n<p>So using the SJ.out.tab files from RNA-STAR and the magic of <a href=\"http://pandas.pydata.org/\" target=\"_blank\"><code>pandas</code></a>, and a <a href=\"https://github.com/olgabot/sj2psi/blob/master/sj2psi/__init__.py#L64\" target=\"_blank\">bit of filtering</a>, you can calculate these scores very easily:</p>\n\n<pre><code>sj['psi5'] = sj.groupby(['chrom', \n    'first_bp_intron'])['total_filtered_reads'].transform(lambda x: x/x.sum())\n</code></pre>\n\n<p>While this does $\\Psi$ scores from raw counts which is frequentist and I prefer to be Bayesian, this is a reasonable start.</p>\n\n<p>Check out the package I wrote for this: <a href=\"https://github.com/olgabot/sj2psi\" target=\"_blank\">https://github.com/olgabot/sj2psi</a> I&#8217;d love your feedback and contribution. It&#8217;s also <a href=\"https://pypi.python.org/pypi/sj2psi/0.0.1\" target=\"_blank\">available on PyPI</a>, so you can <code>pip install sj2psi</code> and start using it today! It&#8217;s under the MIT license so just attribute me :)</p>"}
publish: 2014-05-01
tags: rna splicing, alternative splicing, python, pandas, rna, science
title: Implementing annotation-free quantification of splice junction usage via RNA-STAR and Python+Pandas magic
-->


Implementing annotation-free quantification of splice junction usage via RNA-STAR and Python+Pandas magic
=========================================================================================================

If you’ve been reading my blog for general python/productivity stuff,
you are in for a TREAT! Here is some REAL SCIENCE! I’ll give a quick
intro but please let me know if anything is unclear. I spend most of my
time talking to a few people who study this stuff in painful detail, so
I’d love to know where my explanation becomes cloudy.

***tl;dr: I wrote a small tool for quantifying alternative junction
usage from RNA-STAR alignment output:
[https://github.com/olgabot/sj2psi](https://github.com/olgabot/sj2psi)
And it’s on PyPI so you can `pip install sj2psi` as well.***

Alternative splicing is a method of creating different versions of a
gene using the same DNA. It is awesome because it increases the possible
number of genes in say the human genome from just 30,000 to millions. An
extreme example is the *Drosoephila* (fruit fly) gene *Dscam*, which has
over 60,000 potential versions, more than the genes in the *Drosophila*
genome! Alternative splicing happens by combining different “*exons*"
together (always in linear order) to create different proteins (or other
gene products like [ncRNAs](http://en.wikipedia.org/wiki/Non-coding_RNA)
but we won’t go into that)

![](http://upload.wikimedia.org/wikipedia/commons/0/0a/DNA_alternative_splicing.gif)

These different versions of the same gene are called “isoforms.” What we
care about is looking at individual events because we study [RNA binding
proteins](http://en.wikipedia.org/wiki/RNA-binding_protein) (RBPs), some
of which have an effect on alternative splicing when they bind RNA. So
maybe we know that an RBP binds some gene’s RNA, but it (most likely)
only has an effect on the exon(s) immediately before or after. For
example, the RBP FOX2 is one of [our
lab’s](http://yeolab.ucsd.edu/yeolab/Home.html) favorites, partly
because our boss found that if FOX2 binds upstream (“before” in DNA
terms) of an exon, it suppresses inclusion of it. HOWEVER! FOX2 binding
downstream (“after”) an exon promotes inclusion! Check out this figure
from [Yeo et al, *Nat Struct Mol Bio*
(2009)](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2735254/):

![](https://31.media.tumblr.com/8749e1c26cb36237e1d8d2ea874327f1/tumblr_inline_n4wpkodHzT1rw6gvj.png)

Quantifying alternative exon usage and changes is always a challenge,
and this is especially difficult for unannotated splicing events. Which
is why I was excited when I read [a single-cell
paper](http://genome.cshlp.org/content/early/2013/12/03/gr.161034.113?top=1)
learned of [`bam2ssj`](https://github.com/pervouchine/bam2ssj) (Paper:
[Pervouchine et al, *Bioinformatics*
(2013)](http://www.ncbi.nlm.nih.gov/pubmed/23172860) and its successor,
[`sjcount`](https://github.com/pervouchine/sjcount). The promise is that
instead of quantifying whole splicing events which have to fall into
some specific category (SE/MXE/RI/A5SS/A3SS/AFE/ALE/TandemUTR, see this
figure from [Wang ET et al, *Nature*
(2008)](http://www.nature.com/nature/journal/v456/n7221/abs/nature07509.html).
Each “box” is an exon and the lines in between exons are stretches of
DNA that are removed (called “introns” because they “interrupt” the
exons)

![](http://www.nature.com/nature/journal/v456/n7221/images/nature07509-f2.2.jpg)

Plus these events have to be annotated for use with my splicing
quantifier of choice ([MISO](http://miso.readthedocs.org/en/fastmiso/)),
or you could try to pull them out from the aligned file. However, I work
with single-cell data and since splicing is mostly binary in single
cells (i.e. you have mostly isoform A and not both isoform A and B,
citation: [Shalek et al, *Nature*
(2013)](http://www.ncbi.nlm.nih.gov/pubmed/23685454)), it becomes a pain
to make these annotations, because you have to merge ALL the files
together to create like a terrabyte-sized alignment file (usually a
[`*.bam` file](http://genome.sph.umich.edu/wiki/BAM)) and then you are
introducing all kinds of craziness because what could have been noise in
a few cells gets added up together and becomes signal.

Sigh.

So I’d much rather use something that quantifies splicing of all
junctions, arbitrarily. But when I tried using `sjcount` I couldn’t
figure out what one of the outputs was (like what’s the difference
between a “splice junction” and a “splice boundary?”). Plus I realized
that the “splice junction counts” (`*.ssj`) file was really similar to
the “`SJ.out.tab`" file created by the
[RNA-STAR](https://code.google.com/p/rna-star/) aligner.

`.ssj` files look like (I added the column names):

    chrom   start   stop    strand  overhang    count
    chr1    146509  155767  .       20      1
    chr1    146509  155767  .       34      1
    chr1    146509  155767  .       49      3
    chr1    168165  169049  .       10      2
    chr1    168165  169049  .       49      3
    chr1    694503  700103  .       29      1
    chr1    694503  700103  .       41      1
    chr1    694503  700103  .       49      1
    chr1    894461  894595  .       7       2
    chr1    894461  894595  .       9       5

"overhang" is the number of base pairs of the sequencing read that
overlaps with the upstream exon of the splicing event. The rest of the
read is on the downstream exon. In our case, our reads are 100 base
pairs ("100bp") so if there’s 20bp overhanging on one exon, the other
80bp are on the other exon.

While `SJ.out.tab` files are (again I added column names):

\\n

\\n

\\n

\\n

chrom

\\n

first\_bp\_intron

\\n

last\_bp\_intron

\\n

strand

\\n

intron\_motif

\\n

annotated

\\n

unique\_junction\_reads

\\n

multimap\_junction\_reads

\\n

max\_overhang

\\n

\\n

\\n

\\n

\\n

0

\\n

chr1

\\n

135332

\\n

138297

\\n

1

\\n

GC/AG

\\n

False

\\n

0

\\n

1

\\n

38

\\n

\\n

\\n

1

\\n

chr1

\\n

146510

\\n

155766

\\n

2

\\n

CT/AC

\\n

True

\\n

5

\\n

1

\\n

34

\\n

\\n

\\n

2

\\n

chr1

\\n

156895

\\n

158300

\\n

1

\\n

GC/AG

\\n

False

\\n

0

\\n

2

\\n

17

\\n

\\n

\\n

3

\\n

chr1

\\n

168166

\\n

169048

\\n

2

\\n

CT/AC

\\n

True

\\n

5

\\n

3

\\n

46

\\n

\\n

\\n

4

\\n

chr1

\\n

169265

\\n

172556

\\n

2

\\n

CT/AC

\\n

True

\\n

0

\\n

2

\\n

13

\\n

\\n

\\n

The start/stop are off by one here because `.ssj`'s index is from the
exon end/exon start, and this is from the intron start/intron end. So
the SJ.out.tab coordinates are (start from `.ssj`+1) and (end from
`.ssj`-1).

FYI, the `.ssc` files looked like:

    chr1    894595  894595  .       8       1
    chr1    1228468 1228468 .       1       1
    chr1    1228468 1228468 .       3       3
    chr1    1228468 1228468 .       5       1
    chr1    1228468 1228468 .       6       1
    chr1    1228468 1228468 .       7       2
    chr1    1228468 1228468 .       12      1
    chr1    1228468 1228468 .       13      2
    chr1    1228468 1228468 .       16      2
    chr1    1228468 1228468 .       27      1

This claims to have the same columns as `.ssj` files except instead of
start/stop it’s position/position. I was very confused by how you can
have a count at a position. Why does `chr1:894595` have one line while
`chr1:1228468` have a bunch? If you search for `894595` in the `.ssj`
file, you get:

    chr1    894461  894595  .       7       2
    chr1    894461  894595  .       9       5
    chr1    894461  894595  .       10      2
    chr1    894461  894595  .       11      1
    chr1    894461  894595  .       12      1
    chr1    894461  894595  .       14      1
    chr1    894461  894595  .       16      2
    chr1    894461  894595  .       19      1
    chr1    894461  894595  .       20      1
    chr1    894461  894595  .       21      13
    chr1    894461  894595  .       22      1
    chr1    894461  894595  .       42      2
    chr1    894461  894595  .       49      14

How did all these counts in the last column (which sum to 46 by my
calculation) become **1** in the `.ssc` file? I was pretty lost.

ANYWAYS all of this is a buildup to say that I used Pervouchine et al’s
method of quantifying alternative junctions as asking, when this
upstream exon (splicing *donor*, *D* in figure below) is used with this
downstream exon (splicing *acceptor*, *A* in figure below), how often
does that happen relative to all other donors and acceptors? In the
figure below, the splicing event of interest is a thick, bold line,
joined by a solid arc representing the spliced read. The dotted lines
represent that this donor could be spliced to a bunch of different
acceptor sites.

![Donor with alternative acceptors on left, Acceptor with alternative
donors on
right](http://bioinformatics.oxfordjournals.org/content/29/2/273/F2.medium.gif)

This is quantified with a \$\\Psi\_5\$ (“Percent spliced-in”, or “Psi”
of the donor, which is at the 5’ end of the RNA) and \$\\Psi\_3\$ (“Psi”
of the acceptor, located at the 3’ end of the RNA) scores:

![](http://bioinformatics.oxfordjournals.org/content/29/2/273/embed/graphic-3.gif)

Where the summation is over all other possible acceptors (in the case of
*A’*) or all other possible donors (in the case of *D’*).

So using the SJ.out.tab files from RNA-STAR and the magic of
[`pandas`](http://pandas.pydata.org/), and a [bit of
filtering](https://github.com/olgabot/sj2psi/blob/master/sj2psi/__init__.py#L64),
you can calculate these scores very easily:

    sj['psi5'] = sj.groupby(['chrom', 
        'first_bp_intron'])['total_filtered_reads'].transform(lambda x: x/x.sum())

While this does \$\\Psi\$ scores from raw counts which is frequentist
and I prefer to be Bayesian, this is a reasonable start.

Check out the package I wrote for this:
[https://github.com/olgabot/sj2psi](https://github.com/olgabot/sj2psi)
I’d love your feedback and contribution. It’s also [available on
PyPI](https://pypi.python.org/pypi/sj2psi/0.0.1), so you can
`pip install sj2psi` and start using it today! It’s under the MIT
license so just attribute me :)


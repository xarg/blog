Title: Make your own Wikipedia dictionaries for Kindle
Date: 2011-08-04 18:20
Category: coding
Tags: wikipedia, kindle, python, mobi, dictionaries 
Slug: wikipedia-dictionaries-on-kindle
Author: Alex Plugaru
Summary: An introduction about this blog

One day I was siting on my balcony, reading an article I downloaded on my kindle using <a href="http://www.instapaper.com/">instapaper</a> and stumbled upon this term: <a href="http://en.wikipedia.org/wiki/Stone_Wales_defect">Stone-Wales defect</a>. I had no idea what this defect was since I'm don't know much about carbon nanotubes. Then I realized that I needed to leave my tea and the conform I got used to and get up and go to my computer to lookup what this term was all about.

I'm lazy. I didn't want to get up and search the term on my desktop so I opened up that terrible kindle browser and tried to search that term on Wikipedia which was really slow and uncomfortable and the wireless was down - you got my point. So it struck me that I could probably create some python script to generate a kindle dictionary. So I did.

This article will be about how I accomplished this. Also hopefully it will give you an insight on how to tune this tool to make your own kindle dictionaries.

<strong>Note:</strong> If you're not interested in the implementation details and just want to generate a dictionary for kindle you can just get the source: <a href="https://github.com/humanfromearth/wikimobi">https://github.com/humanfromearth/wikimobi</a> and use it. (usage details in README).
<h3>Wikipedia is big</h3>
It's so big that all the abstracts of all the articles from wikipedia would be a <strong>3GB</strong> mobi file. 3GB is also the storage size on a Kindle 3. That's no good. So I needed a way to filter out  the articles I don't need. I needed physics mostly. I wanted to create a physics dictionary! Eureka! So all I needed to do was to query the physics related articles and get their articles.

At first I tried to use the sql dumps from <a href="http://en.wikipedia.org/wiki/Wikipedia:Database_download" target="_blank">http://en.wikipedia.org/wiki/Wikipedia:Database_download</a> but that failed because sql was really slow to load into a mysql database. Fortunately I found an alternative: <a href="http://downloads.dbpedia.org/">http://downloads.dbpedia.org</a>

These guys had archives of wikipedia in a <a href="http://en.wikipedia.org/wiki/N-Triples">N-Triples</a> format which is more convening to use and a lot faster that sql (at least in my case).

Cool. So I needed a few archives.

This one contains english abstracts:
<pre class="code">http://downloads.dbpedia.org/3.6/en/short_abstracts_en.nt.bz2 (300mb)</pre>
<span>This one contains the article -&gt; categories relations (so we can find the articles from a specific category):</span>
<pre class="code">http://downloads.dbpedia.org/3.6/en/article_categories_en.nt.bz2 (115mb)</pre>
<span>Contains relations between categories and subcategories:</span>
<pre class="code">http://downloads.dbpedia.org/3.6/en/skos_categories_en.nt.bz2 (18mb)</pre>
Great! We have the data. It's time to filter out that data and generate a .mobi file.

Again.. Problems.. Kindle really sucks at this because it uses the proprietary .mobi format which doesn't have good open converters which I could use. So I used the mobigen.exe provided by <a href="http://www.mobipocket.com/dev/">http://www.mobipocket.com/dev/</a> that allows you to create mobi dictionaries from an .opf file. Ok so now I need and opf file? That sucks. Fortunately this guy: Klokan Petr Přidal (www.klokan.cz) created this script: <a href="https://github.com/humanfromearth/wikimobi/blob/master/tab2opf.py">tab2opf.py</a>. Which converts tab separated files into opf dictionaries.

Ok so in the end I needed this:
<ol>
    <li>Create a tab separted file: [term]\t[definition] -&gt; out.tab</li>
    <li>Convert out.tab -&gt; out.opf</li>
    <li>Convert out.opf -&gt; dicitionary.mobi</li>
</ol>
Easy right? So after writing the stuff I described above we use the tool (wikimobi.py):
<pre class="code">python wikimobi.py -nt path_to_nt_files_dir/ -o output_file -c Category -l levels</pre>
For physics it would something like:
<pre class="code">python wikimobi.py -nt path_to_nt_files_dir/ -o physics -c Physics -l 3</pre>
The level options goes down 3 levels in the category hierarchy so we don't end up in <a href="http://xkcd.com/903/">Philosophy</a>:

<img title="Wikipedia trivia: if you take any article, click on the first link in the article text not in parentheses or italics, and then repeat, you will eventually end up at &quot;Philosophy&quot;." src="http://imgs.xkcd.com/comics/extended_mind.png" alt="Extended Mind" />

If you have an idea on how to perform the filtering of articles based on their categories so we don't get a lot of unrelated stuff I'd be interested to hear it.

Have fun!
<h3 id="update-download">Update:</h3>
<h4><span class="Apple-style-span" style="font-weight: normal;">On <strong>stehk's </strong>request I've generated a few dictionaries:</span></h4>
<ol>
    <li>Physics: <a href="http://blog.hiresasha.net/wp-content/uploads/dictionaries/physics.mobi">http://blog.hiresasha.net/wp-content/uploads/dictionaries/physics.mobi</a></li>
    <li>Chemistry: <a href="http://blog.hiresasha.net/wp-content/uploads/dictionaries/chemistry.mobi">http://blog.hiresasha.net/wp-content/uploads/dictionaries/chemistry.mobi</a></li>
    <li>Mathematics: <a href="http://blog.hiresasha.net/wp-content/uploads/dictionaries/math.mobi">http://blog.hiresasha.net/wp-content/uploads/dictionaries/math.mobi</a></li>
    <li>Biology: <a href="http://blog.hiresasha.net/wp-content/uploads/dictionaries/biology.mobi">http://blog.hiresasha.net/wp-content/uploads/dictionaries/biology.mobi</a></li>
    <li>Philosophy: <a href="http://blog.hiresasha.net/wp-content/uploads/dictionaries/philosophy.mobi">http://blog.hiresasha.net/wp-content/uploads/dictionaries/philosophy.mobi</a></li>
</ol>
<div><strong>Note: </strong>The dictionaries are usable but there are still some unicode and metadata problems which I wasn't able to solve. Hopefully someone can help me to solve those.</div>

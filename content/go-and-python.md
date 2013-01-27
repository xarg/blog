Title: Go and Python
Date: 2011-08-04 18:14
Category: coding
Tags: python, golang, coding
Slug: go-and-python
Author: Alex Plugaru
Summary: A personal comparison between Python and Golang

I see a lot of talk nowadays about <a href="http://golang.org/">Go</a><a href="http://golang.org/">lang</a>. It's fresh, it's shiny, not too many features, documented, statically typed, garbage collected, parallel, python zen inspired sweet candy.</p>
<p style="margin-bottom: 0in;">I'm a python addict, I admit it. Looking at poor people from 3rd world of computer languages with their cluttered syntaxes and semicolons and other useless unintelligible syntactic voodoo makes me feel really good. Too good actually. When I need to do something in some other language I feel sick sometimes. I'm serious. I have a familiar feeling with the process called: eating spinach. I can eat it, don't get me wrong here, but I don't enjoy it. Just moving my maxilla to get the job done. My brain got comfortable with python. My synapses are now irreversibly fused towards python and python ways of doing stuff.</p>
<p style="margin-bottom: 0in;">So what about Go? Well.. it turns out Go is remarkably similar to python in many ways. In my mind it's like a python for the system-programming/statically-typed/compiled/crazy-fast world. For me this language is good news. Because sometimes python is too slow and I don't want to get dirty with C. I want a language that actually has a usable standard library similar to python's standard library that allows me to get low level enought without sacrificing my time and patience. I have a specific example I played with recently which I intend to bring forward for comparison of these two amazing languages.</p>
<p style="margin-bottom: 0in;">I needed a <a href="http://en.wikipedia.org/wiki/Boyer%E2%80%93Moore%E2%80%93Horspool_algorithm">Boyer-Moore-Horspool</a> search algorithm which I stole from this <a href="http://code.activestate.com/recipes/117223-boyer-moore-horspool-string-searching/">recipe</a> here. My example is a little bit modified so that it does not stop on the first search result, but continues until is done counting the number of occurences of a 'word' in a string:</p>
<p style="margin-bottom: 0in;"><strong>Attention! </strong>I don't want to compare the speed of the two languages. It would be stupid. Go is faster. What I want to do instead is to compare the code complexity/readability for both. <em>I believe that clean-slow code is bettter than ugly-fast code</em>. I want to see if my code is still readable in Go and if it's close to the python example. Maybe even better for this particular example. Who knows?</p>
<p style="margin-bottom: 0in;">[gist id=1049183]</p>
The above example is not equivalent to python's: str.count because it does more checking. As you can see there are a few optimisations here and there, but overall the code looks pretty clean. This code has a big dissadvantage. It's called speed. Compared to the find function in cpython (which is really fast - I think it uses the same algorithm: bmh) it's really slow. I tested this in <a href="http://pypy.org/">pypy</a> as well, it is very slow. Like 10 times slower then CPython for this example. Is this normal? Am I wrong?

Anyway.. here is the same written in Go:

[gist id=1050644]

Now I don't know about you, but this looks good to me. I mean just visually compare the python example with this one. I looks similar right?

This example is bad. It may even say: "Go is better".. It's not true. This example surelly does not demostrate all the benefits that you get from Go or Python.. it's a simple example.. one of many.. and should be treated as such.

Now here are a few rants about Go:
<ul>
    <li>Api stability. Go guys please don't change the API's so often, it makes people nervous. It makes serious comercial development problematic. I know it's a new language and a lot of things are changing which is normal, but keep in mind that people like me who need to get their shit done fast will think twice before building a real world product without a api 'stability'.</li>
    <li>Debugger - I hear there is one in development right now. No language is worth much without a usable debugger. I'm sorry.. I'm just too lazy to look at my code to figure out what's the problem. I want a usable debugger right now.</li>
    <li>Too little zen of python in some third party libraries. import this. Please...</li>
</ul>
I will say that Go certainly sparked my interest. It's a cool language. It's a keeper. I will try to find a project to actually do something practical with it.

Until then gooo!

A more complete example of Go (with tests): <a href="https://github.com/humanfromearth/goooo">https://github.com/humanfromearth/goooo</a>


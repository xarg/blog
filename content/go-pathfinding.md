Title: Go pathfinding
Date: 2011-08-05 18:09
Category: coding
Tags: golang, pathfinding, a-star
Slug: go-pathfinding
Author: Alex Plugaru
Summary: An a-star implementation in golang

<h3>Remember:</h3>
I found out why it's a good idea to implement common algorithms in a language you wish to learn.

First because you don't use advanced language features. <strong>Simple constructs should be sufficient to implement an efficient common algorithm</strong>. You don't have to memorize a lot of new concepts which can be confusing and can throw you off your path. Sometimes even quit learning that language! You become more accustomed with low level stuff before getting into complicated standard library features.

Second advantage is that you immediately start doing something practical. I mean who doesn't need A* or Dijkstra? Everybody?!
<h3>Practical:</h3>
So I needed a good path-finding algorithm such as <a href="http://en.wikipedia.org/wiki/A*_search_algorithm">A*</a>. I wanted it to be implemented in golang. I wasn't  able to find any standalone implementation of pathfinding algorithms in golang. Naturally I implemented one. So here it is:

<a href="https://github.com/humanfromearth/gopathfinding">https://github.com/humanfromearth/gopathfinding</a>
<h3>Lessons learned:</h3>
<ul>
    <li>I found that if you can use slices instead of maps you should always use slices.</li>
    <li>I found that you don't need to use make all the time. You can use append which does the allocation for you.</li>
    <li>I found that pointers can be really cool.</li>
    <li>I found that again golang is a robust language and gives great hints when hitting an error.</li>
    <li>Did I say I love the test framework?</li>
</ul>
<h3>Update:</h3>
<div>This is the answer to <strong>Patrick's </strong>comment:</div>
<div><em>Why should I use slices instead of maps when possible and why append is sometimes better than make?</em></div>
<div>First of all you can't use the built-in append function on maps - just slices. So adding new values to maps or extending maps are not as easy as for slices. 'append' is very powerful. Here is why:</div>
<div>[gist id=42cf9365cb8f06266fb9]</div>
<div>Maybe the above it's not the best example, but you can see that we need to specify a length to the make function to allocate and initialize s11.  Comparing that to append: you don't need to know the exact size of your slice to append stuff. This is better for a because  you don't have to deal with indexes and can make your code a little bit cleaner. Also append does the allocation on runtime.  Doing lots of allocation calls is not very efficient - keep that in mind.</div>
<div>Because slices have append to support them it makes them more powerful than maps in some scenarios. My conclusion again: use slices instead of maps if you don't really need maps - it's easier.</div>

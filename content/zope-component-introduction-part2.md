Title: A zope.component introduction - Part 2
Date: 2011-11-24 18:09
Category: coding
Tags: python, zope, components, components architecture
Slug: zope-component-introduction-part2
Author: Alex Plugaru
Summary: An introduction to zope.component package. Examples and advices on how to use it.

 <address<address>Note: If you are already familiar with the adapter concept and you just want to learn about zope.component you can skip to <a href="http://blog.hiresasha.net/zope-component-introduction-part-2/">part 2</a>.</address>
nent</h2>
<a href="http://pypi.python.org/pypi/zope.component">zope.component</a> is a reusable python package that usually comes with zope.interface and zope.event. We can split this package into two categories:
<ol>
    <li>Components: adapters, utilities, named adapters, multi-adapters and others</li>
    <li>Registries: global component registry, local component registry.</li>
</ol>
What we need to remember from all this is that there are registries that store adapters. The idea is to use these registries through out our application. Think of it as a global dictionary with values: adapters and keys: interfaces - because it's exactly what it is. Without further ado we'll jump directly to a simple example, but instead of adapters will use an utility (an adapter that adapts nothing).
<h2>A small utility</h2>
[gist id=1391210]

Let's see what we did here:
<ol>
    <li>We declared an interface and an utility class <em>Hello</em>.</li>
    <li>We got the global registry and registered our utility for the <em>ISpeach</em> interface.</li>
    <li>In the end we used our utility first by querying the register and then just used it as a <em>Hello</em> object.</li>
</ol>
<h2>The benefits</h2>
The nice thing about registering a utility like this is that you can change its location in the project, you can refactor it and give it a different name, you can override it with another utility at will - and most importantly is that the <span style="text-decoration: underline;">last 2 lines never change</span>! Here is why:
<div>
<ul>
    <li>All we really care about is the interface with which we are making the query - the implementation can change in the future.</li>
    <li>The code is decoupled (no direct imports to the implementation) - just the interface is known to us.</li>
</ul>
</div>
A direct result to this is:  more control for the developer which can replace the implementation at will without worrying that he/she needs to refactor a lot of code. Looks like a lot of code just to register one simple utility isn't it? I couldn't agree more, but remember that we are talking here about big applications with tons of utilities and adapters and events and event handlers. In the end everything is a structured chaos. It's structured because as long as we have a registry that has all the components we have an overview of our application and all its components.
<blockquote>But my application is already far in it's 3rd year of development. I don't have everything in one big component registry!</blockquote>
The good news is that you don't need to have everything as components - I strongly advice against it actually. You can use this just for the parts you feel that your application will benefit the most. You can gradually use it where it makes sense.

Now let's return to our apples and oranges example, but make it use adapters.
<h2>Oranges and Apples - again</h2>
[gist id= 1391374]

It's really not that different from our previous example with apples and oranges except the whole interfaces and registration stuff. The benefit however is that this is a lot more usable in the long run for the reasons I explained above. And the difference from the utility example is that it actually adapts something - <em>IApple</em> in our case. The key thing to remember is that all the registry queries are done using interfaces.

In the next part I will try to draw the big picture, what else can components do besides adapters and utilities and what I personally don't like about their implementation - discussing possible alternatives.
<h2>The problem</h2>
Recently I gave a <a title="Zope Components - an introduction" href="http://hiresasha.net/slides/zca/introduction/">talk</a> about <a href="http://www.muthukadan.net/docs/zca.html">zope component architecture</a> at a company in Paris where I started consulting on zope, plone, django and python in general. In these posts, aided by code examples, I will try to show what  zope components are capable of, what are the common use-cases and some of my personal views about the subject.

First lets <span style="text-decoration: underline;">identify the problem</span> that we are trying to solve using a component architecture:
<blockquote>Applications tend to get big. Especially those applications that have a lot of users and have years of development. Users want different features/changes in different stages of the development of the product. Hence the key to a successful and long lasting application is to comply with the user needs as fast as possible with as little technical dept as possible. Finding a balance between the two is not easy.</blockquote>
Speed of development is important especially in the beginning of a project. Your clients want that software today and you can't waste time on thinking a lot about long term benefits if your bank account is empty. Those benefits are innexistent if you don't finish in time so the shortest path usually gives you the best chances of success. This is the correct way to think about it. However when the project is getting bigger you may want to start thinking in different terms. Questions like these are usually good onces to ask:
<ul>
    <li>How much time does it take to create a feature?</li>
    <li>Refactoring is hard, but can it be made faster?</li>
    <li>Is it easy to extend existing features?</li>
    <li>How different parts of your application talk to each other?</li>
    <li>How about debugging?</li>
</ul>
These questions matter both to managers and coders. Even if the managers and developer are very good at what they do usually what happens is that layer after layer gets added on top of the existing product until it becomes a big pile of crap - new developers are screwed because they can't figure out what is happening in the system and the time to train them is very expensive. New features take enormous amounts of time to develop and your technical dept gets bigger and bigger with every new modification.

Very little can be done about this once you're big. There are no easy solutions and old applications sometimes just whither and die. Tough. But maybe components can help.
<h2>Prelude</h2>
Python is a great language. It is easy to learn and there are many different frameworks that can get you started really fast. What I found very important about python is that it's a good prototyping language - some complain that it's too good. It's no accident that it's the language of choice for many successful startups (Dropbox, Disqus, Quora, etc.).

So once you start delivering and get some sort of income you try to improve the product to make your users happy. You provide missing features, improve stuff. More code, more complexity, more tests, more features, more developers and so on and we get back to our problem.

Zope components architecture promises to alleviate some of the problems of big applications by using adapters as a way to organize code. If this is true for your case you will have to decide for yourself.

What's an adapter?
<blockquote><a href="http://en.wikipedia.org/wiki/Adapter_pattern">Wikipedia</a>:

In <a title="Computer programming" href="http://en.wikipedia.org/wiki/Computer_programming">computer programming</a>, the <strong>adapter pattern</strong> (often referred to as the <strong>wrapper pattern</strong> or simply a <strong>wrapper</strong>) is a <a title="Design pattern (computer science)" href="http://en.wikipedia.org/wiki/Design_pattern_(computer_science)">design pattern</a> that translates one <a title="Interface (computer science)" href="http://en.wikipedia.org/wiki/Interface_(computer_science)">interface</a> for a <a title="Class (computer science)" href="http://en.wikipedia.org/wiki/Class_(computer_science)">class</a> into a compatible interface. An <em>adapter</em> allows classes to work together that normally could not because of incompatible interfaces, by providing its interface to clients while using the original interface.</blockquote>
In other words, an adapter (usually a function or a class) allows two interfaces to talk to each other. Consider this example:
<blockquote>Say we have interfaces iA and iB, receptively  A and B their implementations. At the beginning they don't know about each other. But then you decide that you want to make them talk to each other. Let's say A wants to list all Bs. At the same time you don't want to change A or B because they are central to your application and a lot of stuff uses both A and B and changing them can be both ugly and difficult. This sucks!</blockquote>
The good news is that in some cases this can be avoided by using a third party that acts as a intermediary (the adapter) between iA and iB and provides functionality that did not exist before. Not only that, but it allows you to keep your A and B <span style="text-decoration: underline;">unchanged</span>.

Let's forget about interfaces and adapters for a while and let's consider this simple scenario of apples and oranges in a basket:

[gist id=1388813]

It's pretty clear that <strong><em>Apple</em></strong> doesn't have slices and the last line will fail with an <em><strong>AttributeError. </strong></em>

So how do we make apple have slices? There are many possible solutions. But let's impose these 2 restrictions (I will explain why later):
<ol>
    <li>We are not allowed to <span style="text-decoration: underline;">modify</span> the <em>Apple</em> class</li>
    <li>We are not allowed to <span style="text-decoration: underline;">subclass</span> the <em>Apple</em> class.</li>
</ol>
A possible solution is to create a wrapper class around the Apple class that will give us the functionality we need.

[gist id=1388877]

So let's see what we did here.
<ol>
    <li>We had an <em>Apple</em> class which didn't have any slices</li>
    <li>We created an <em>AppleWrapper</em> which takes an <em>Apple</em> object and provides a <em>slices</em> function which can be used later with that apple object.</li>
    <li>As far as <em>get_slices</em> is concerned it just needs to call a function called <em>slices</em> on basket's items.</li>
    <li>We also didn't subclass the <em>Apple</em> class and most importantly <em>Apple</em> class remains untouched.</li>
    <li><em>AppleWrapper</em> is what we call an adapter and the apple object is the <strong>adaptee</strong>.</li>
</ol>
<em>Why it is so important not to subclass?</em>

For a longer explanation of why subclassing is considered harmful search for <a href="http://google.com?q=inheritance is evil">"inheritance is evil"</a>. The short explanation is that we don't really need to inherit all Apple's properties and methods - we just need to make it have slices and that's it.

<em>Why just not modify the Apple class to give it slices?</em>

Because we don't really need the Apple to have slices all the time. Just the time when it's in a basket. This way the <em>Apple</em> class will stay a clean and simple - and as a result easily extendable in the future.

Ok. Now that we understood how an adapter works we can switch to a more complicated example and this time we are going to use zope components.
<h4>Go to <a href="http://blog.hiresasha.net/zope-component-introduction-part-2/">part 2</a> in which I will explain how to use zope.component.</h4> >Note: If you are already familiar with the adapter concept and you just want to learn about zope.component you can skip to <a href="http://blog.hiresasha.net/zope-component-introduction-part-2/">part 2</a>.</address>
<h2>The problem</h2>
Recently I gave a <a title="Zope Components - an introduction" href="http://hiresasha.net/slides/zca/introduction/">talk</a> about <a href="http://www.muthukadan.net/docs/zca.html">zope component architecture</a> at a company in Paris where I started consulting on zope, plone, django and python in general. In these posts, aided by code examples, I will try to show what  zope components are capable of, what are the common use-cases and some of my personal views about the subject.

First lets <span style="text-decoration: underline;">identify the problem</span> that we are trying to solve using a component architecture:
<blockquote>Applications tend to get big. Especially those applications that have a lot of users and have years of development. Users want different features/changes in different stages of the development of the product. Hence the key to a successful and long lasting application is to comply with the user needs as fast as possible with as little technical dept as possible. Finding a balance between the two is not easy.</blockquote>
Speed of development is important especially in the beginning of a project. Your clients want that software today and you can't waste time on thinking a lot about long term benefits if your bank account is empty. Those benefits are innexistent if you don't finish in time so the shortest path usually gives you the best chances of success. This is the correct way to think about it. However when the project is getting bigger you may want to start thinking in different terms. Questions like these are usually good onces to ask:
<ul>
    <li>How much time does it take to create a feature?</li>
    <li>Refactoring is hard, but can it be made faster?</li>
    <li>Is it easy to extend existing features?</li>
    <li>How different parts of your application talk to each other?</li>
    <li>How about debugging?</li>
</ul>
These questions matter both to managers and coders. Even if the managers and developer are very good at what they do usually what happens is that layer after layer gets added on top of the existing product until it becomes a big pile of crap - new developers are screwed because they can't figure out what is happening in the system and the time to train them is very expensive. New features take enormous amounts of time to develop and your technical dept gets bigger and bigger with every new modification.

Very little can be done about this once you're big. There are no easy solutions and old applications sometimes just whither and die. Tough. But maybe components can help.
<h2>Prelude</h2>
Python is a great language. It is easy to learn and there are many different frameworks that can get you started really fast. What I found very important about python is that it's a good prototyping language - some complain that it's too good. It's no accident that it's the language of choice for many successful startups (Dropbox, Disqus, Quora, etc.).

So once you start delivering and get some sort of income you try to improve the product to make your users happy. You provide missing features, improve stuff. More code, more complexity, more tests, more features, more developers and so on and we get back to our problem.

Zope components architecture promises to alleviate some of the problems of big applications by using adapters as a way to organize code. If this is true for your case you will have to decide for yourself.

What's an adapter?
<blockquote><a href="http://en.wikipedia.org/wiki/Adapter_pattern">Wikipedia</a>:

In <a title="Computer programming" href="http://en.wikipedia.org/wiki/Computer_programming">computer programming</a>, the <strong>adapter pattern</strong> (often referred to as the <strong>wrapper pattern</strong> or simply a <strong>wrapper</strong>) is a <a title="Design pattern (computer science)" href="http://en.wikipedia.org/wiki/Design_pattern_(computer_science)">design pattern</a> that translates one <a title="Interface (computer science)" href="http://en.wikipedia.org/wiki/Interface_(computer_science)">interface</a> for a <a title="Class (computer science)" href="http://en.wikipedia.org/wiki/Class_(computer_science)">class</a> into a compatible interface. An <em>adapter</em> allows classes to work together that normally could not because of incompatible interfaces, by providing its interface to clients while using the original interface.</blockquote>
In other words, an adapter (usually a function or a class) allows two interfaces to talk to each other. Consider this example:
<blockquote>Say we have interfaces iA and iB, receptively  A and B their implementations. At the beginning they don't know about each other. But then you decide that you want to make them talk to each other. Let's say A wants to list all Bs. At the same time you don't want to change A or B because they are central to your application and a lot of stuff uses both A and B and changing them can be both ugly and difficult. This sucks!</blockquote>
The good news is that in some cases this can be avoided by using a third party that acts as a intermediary (the adapter) between iA and iB and provides functionality that did not exist before. Not only that, but it allows you to keep your A and B <span style="text-decoration: underline;">unchanged</span>.

Let's forget about interfaces and adapters for a while and let's consider this simple scenario of apples and oranges in a basket:

[gist id=1388813]

It's pretty clear that <strong><em>Apple</em></strong> doesn't have slices and the last line will fail with an <em><strong>AttributeError. </strong></em>

So how do we make apple have slices? There are many possible solutions. But let's impose these 2 restrictions (I will explain why later):
<ol>
    <li>We are not allowed to <span style="text-decoration: underline;">modify</span> the <em>Apple</em> class</li>
    <li>We are not allowed to <span style="text-decoration: underline;">subclass</span> the <em>Apple</em> class.</li>
</ol>
A possible solution is to create a wrapper class around the Apple class that will give us the functionality we need.

[gist id=1388877]

So let's see what we did here.
<ol>
    <li>We had an <em>Apple</em> class which didn't have any slices</li>
    <li>We created an <em>AppleWrapper</em> which takes an <em>Apple</em> object and provides a <em>slices</em> function which can be used later with that apple object.</li>
    <li>As far as <em>get_slices</em> is concerned it just needs to call a function called <em>slices</em> on basket's items.</li>
    <li>We also didn't subclass the <em>Apple</em> class and most importantly <em>Apple</em> class remains untouched.</li>
    <li><em>AppleWrapper</em> is what we call an adapter and the apple object is the <strong>adaptee</strong>.</li>
</ol>
<em>Why it is so important not to subclass?</em>

For a longer explanation of why subclassing is considered harmful search for <a href="http://google.com?q=inheritance is evil">"inheritance is evil"</a>. The short explanation is that we don't really need to inherit all Apple's properties and methods - we just need to make it have slices and that's it.

<em>Why just not modify the Apple class to give it slices?</em>

Because we don't really need the Apple to have slices all the time. Just the time when it's in a basket. This way the <em>Apple</em> class will stay a clean and simple - and as a result easily extendable in the future.

Ok. Now that we understood how an adapter works we can switch to a more complicated example and this time we are going to use zope components.
<h4>Go to <a href="http://blog.hiresasha.net/zope-component-introduction-part-2/">part 2</a> in which I will explain how to use zope.component.</h4> 

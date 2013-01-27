Title: A zope.component introduction - Part 1
Date: 2011-11-23 18:09
Category: coding
Tags: python, zope, components, components architecture
Slug: zope-component-introduction-part1
Author: Alex Plugaru
Summary: An introduction to zope.component package. Examples and advices on how to use it.

<address>Note: If you are already familiar with the adapter concept and you just want to learn about zope.component you can skip to <a href="http://blog.hiresasha.net/zope-component-introduction-part-2/">part 2</a>.</address>
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

---
ID: 422
post_title: >
  Comparision of Swift Programming
  Language on the support for Generics
author: kandelvijaya
post_date: 2016-06-24 20:25:46
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2016/06/24/comparision-of-swift-programming-language-on-the-support-for-generics/
published: true
---
<blockquote>
<p style="text-align: justify;">Programming can be great if we can replace a family of similar algorithms of 100 or 1000 lines into 10.</p>
<p style="text-align: justify;">Programming can be great if we can use those 10 lines for ages to come and criteria we didn't anticipated.</p>
<p style="text-align: justify;">Programming can only be great if those 10 lines can be equally performant and efficient as those raw concrete implementation.</p>
</blockquote>
<p style="text-align: justify;">This is Generics. And these are some benefits we can reap from Generics programming.</p>
<p style="text-align: justify;">Apart from my daily job at Zalando iOS team, i stumbled across a problem which then lead to Generics programming. I was trying to <strong>wrap UITableViewDelegate into a protocol Extension</strong> inspired by the Protocol Oriented Programming. I soon stumbled across another piece of puzzle. <strong>Static Dispatch vs Dynamic Dispatch</strong>. This is a topic i will write extensively sometime later. Then I was scanning the internet and reading research papers. I was reading this book <a href="https://www.amazon.com/Advanced-Swift-Chris-Eidhof/dp/1523831715" target="_blank">Advanced Swift</a> and inside it somewhere was a link to this research paper titled "<a href="http://www.crest.iu.edu/publications/prints/2003/comparing_generic_programming03.pdf">Comparison of Programming Languages on the basis of support for Generics </a>(Gracia 2003)". The paper was <a href="http://www.osl.iu.edu/publications/prints/2005/garcia05:_extended_comparing05.pdf">extended in 2007</a>.  The gist is, it compared various programming languages like Java, C++ and Haskell among others. Then I asked can I extend this research paper to include Swift?</p>
<p style="text-align: justify;">It turned out, I along with my colleague's support wrote the paper which shall be public sometime soon. And i made a talk for these concepts and presented to the entire mobile team on iOS guild.</p>
<p style="text-align: justify;">I believe people who are curious enough to dig deeper and people like you can benefit from these talk and hence I'm posting the presentation slides on this page rather than typing all that i said.</p>
&nbsp;

.

&nbsp;
<p style="text-align: justify;">Just to spoil the part without explanation heres how Swift compares to the rest.</p>
<p style="text-align: justify;"><img class="alignnone size-full wp-image-448" src="http://139.59.142.118/wp-content/uploads/2016/06/Generic-supprt-Comparision-for-Swift-kandelvijaya.com_.png" alt="Generic supprt Comparision for Swift - kandelvijaya.com" width="2326" height="1240" /></p>
<p style="text-align: justify;">I will go onto the details of each attribute with Swift code sample and prove how each is possible. Not only that my aim is also to show how you can see some errors and compiler warnings and understand what actually is going on. On the extended paper i will also get into where Swift Generics kind of falls short and some edge cases and programming language design challenges.</p>
<p style="text-align: justify;">I will update this page when i am able to have other materials out. Until then 🗡🍉</p>
<p style="text-align: justify;">Hi there again, here is the actual paper linked. For comments and/or arguments please leave a comment and i will try to get back.</p>
<p style="text-align: justify;"><a title="ComparisonofProgramminglanguagesonthebasisofsupportforGenericsprogrammingincludingSwift-3" href="http://139.59.142.118/wp-content/uploads/2016/08/ComparisonofProgramminglanguagesonthebasisofsupportforGenericsprogrammingincludingSwift-3.pdf">ComparisonofProgramminglanguagesonthebasisofsupportforGenericsprogrammingincludingSwift-3</a></p>
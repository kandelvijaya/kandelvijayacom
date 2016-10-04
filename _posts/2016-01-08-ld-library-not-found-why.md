---
ID: 152
post_title: '&quot;ld: library not found&#8230;.&quot; WHY?'
author: kandelvijaya
post_date: 2016-01-08 07:37:06
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2016/01/08/ld-library-not-found-why/
published: true
---
<div id="titlearea">
<h2>Debugging the most common "ld:" error!</h2>
</div>
<div id="contentarea">
<div class="cell text-cell"><span style="font-family: Menlo; font-size: 11px; text-indent: -12px; color: #ff0000;">ld: library not found for -lPods-Bolts</span></div>
<div class="cell text-cell"><img class="alignnone size-full wp-image-167" src="http://139.59.142.118/wp-content/uploads/2016/01/Screen-Shot-2016-01-08-at-4.00.43-PM-1.png" alt="Screen Shot 2016-01-08 at 4.00.43 PM.png" width="832" height="728" /></div>
<div class="cell text-cell">This error is extremely probable during app development if you are using Parse SDK. Unfortunately there is no proper error message as to why there is such error. The other annyoing thing is it produce a huge command log which is neither readable or encouraging to solve, looking at the message.</div>
<div class="cell text-cell">I can go on and runt on the error for it caused me many troubles, set backs and trauma.</div>
<div class="cell text-cell"></div>
<div class="cell text-cell">Today, in my best of mood, i will try to dig in and find whats this error really has to say and how we solve it. This entire post is not written afterwards but progresses with my research. We will together find the solution but im sorry if i make inaccurate judgement at the front. The material gets concrete and fool proof as we move ahead. Let go.</div>
<div class="cell text-cell"></div>
<div class="cell text-cell">

The major sightings of this error can be under these circumstances:
<div>
<ul>
	<li>If the Bolts is actually not installed (which might happen when you do things manually but its also probable to find in cocoapods managed workspaces)</li>
	<li>2 or more libraires have the same name, which is rare but what if Parse and ParseUI uses bolts internally (<em>this is just a example: they dont!</em>)</li>
	<li>you defined a library named the same (<em>which you wont for sure</em>)</li>
	<li>if you dont import bolts in the bridging header if you are still using Objective-C (<em>atleast for some good reason other than the learning curve for Swift</em>)</li>
	<li>if you have modules/libraries that are some swift version and some objective-c version. They might badly interact. (<em>This is just what i think from a really plain user of cocoapods. I dont know what cocoapods does inside the hodd nor have i looked into. Which i might just to solve this one.</em>)</li>
	<li>If you changed the various options available through <b>build settings</b> which might includes
<ul>
	<li>enable bit code</li>
	<li>enable modules</li>
	<li>change bridging header location</li>
	<li>improper linker flags (<em>misspelled, not available ones</em>)</li>
	<li>improper framework search path</li>
	<li>imporper names or mispelled libraires in the search path</li>
	<li>OR even fiddling with Cocoapods defaults when you have no idea whats going inside</li>
</ul>
</li>
	<li>The erro also occurs when there is a error in your code. For instance, you have 60 errors and Xcode will mostly than rare case, show you 40 of those. You fix them and who knows theres another set of erros. So you fix them. The end error is <strong>ld</strong> error. But the linker(<strong>ld</strong>) message is there because you might have interacted with the library specified in a wrong way and the compiler doesnot compile it until later in the comilation process. Making the point extremely hard to find out where things went wrong.</li>
	<li>Funny thing is there are other things too which i have no idea. Let me know.</li>
</ul>
&nbsp;

</div>
</div>
<div class="cell text-cell">

Coming to the fun things:
<div>
<ul>
	<li>You wont have an idea why a working project until yesterday suddenly with a <b>Mach-O linker error.</b></li>
	<li><em>You might have had a bad night. Maybe its your hangover detection and stopping you to type more code.</em></li>
	<li>When you know you change a portion of your code and then <b>git commit,</b> then try to run it, now<b> </b>it wont work. Now obviously you rollback to the last commit and keep doing it for few more until you find a point where this error doesnot exist. You then bring back those changes, branch and then wish it work. Sometimes it works and then merge it back. But you just dont know what the heck is that for.</li>
	<li>Sometimes Apple updates the OSX with surprising security measures which will make certain things beyond even <b>sudo su. </b>(After upgrading to El Capitan, i seem to find Cocoapods "pod" command missing and "<strong>sudo gem update</strong>" broken with a permission to <strong>/usr/bin/  </strong>restricted.)</li>
	<li>Theres lot i can go on for this topic but i wanted to come to a constructive side and spend my day or two dealing with this error and see why is it that?</li>
	<li>I also want to find all the ways i can mess up my project and get this error.</li>
	<li>In doing so i hope the entire community of developers, at least the ones who are not writing the compiler or are language developers get to know the problem and rather than sparodically going to the stackoverflow and testing one thing at a time and then spending 2 days just to solve a mystery, which you wont even know  and which step of SO answer actually worked, to be able to understand this huge burden deeply.</li>
	<li>If you take into consideration that a average iOS developer will waste 6 hours in this error and multiply by 2 million devs. Thats 12 million hour.</li>
	<li>75 x 365 x 24 = 657,000hrs. We waste atleast 12 iOS engineers a year just trying to tinker on this problem.</li>
	<li>Im just taking the time in consideration, the motivation, productivity loss and energy loss are completely open to your own analysis.</li>
	<li>Lets begin.</li>
</ul>
</div>
</div>
<div class="cell text-cell"></div>
<div class="cell text-cell"></div>
<h3><strong>What is ld?</strong></h3>
<div>
<ul>
	<li><b> </b>I searched on the <b>SO </b>again. Where would i better?</li>
	<li>This guy was to some point int he good path: <a href="http://stackoverflow.com/questions/8928658/understand-xcode-linking-ld">http://stackoverflow.com/questions/8928658/understand-xcode-linking-ld</a></li>
</ul>
</div>
<div class="cell text-cell">

<strong>ld = something to do with Library</strong>

<strong>Library?</strong>

<strong>Analogy: </strong>As if in a program lots of function might require the draw() function to draw on screen without making duplicates, we can bundle this whole set of common functions and thats a <strong>library</strong>.

The most basic form of a library is a <strong><i>static library</i></strong>. The above paragraph mentioned that you could share code by just reusing object files (common functions bundled together); it turns out that static libraries really aren't much more sophisticated than that.

On UNIX systems, the command to produce a static library is normally <code><b>ar</b></code>, and the library file that it produces typically has a <code>.a</code> extension. These library files are normally also prefixed with "<code>lib</code>" and passed to the linker with a "-l" option followed by the name of the library, without prefix or extension (so "<code>-lfred</code>" will pick up "<code>libfred.a</code>”).
<ul>
	<li><span style="color: #000000; font-family: -webkit-standard;"><span style="white-space: normal;">Cocoapods generate a <b>.a </b>files </span></span></li>
	<li><span style="color: #000000; font-family: -webkit-standard;"><span style="white-space: normal;">the linker has a error saying “<b>library not found for -lpods-xxx</b>"</span></span></li>
</ul>
</div>
<div class="cell text-cell">
<h3>Might be useful:</h3>
<div>Another important detail to note is the <i>order</i> of events; the libraries are consulted only when then the normal linking is done, and they are processed <i> in order</i>, left to right. This means that if an object pulled in from a library late in the link line needs a symbol from a library earlier in the link line, the linker won't automatically find it.</div>
</div>
<div></div>
<div class="cell text-cell">
<h3>Static Library vs Shared Library</h3>
<div>
<ul>
	<li>Static library (<b>.a</b>)is linked and binded into the program. So if 2 programs are using the same static library they will have seperate copies. If one gets updated then other wont. It needs recompiling.</li>
	<li>Shared Libraries (<b>.so || .dylib on MacOSX</b>) can be loaded once seperately and then during the program execution linked (<b>JIT: </b>just in time) by a small linker. When 2 or more programs using the same library is loaded once, it is space efficient and change can be reflected in all easily. Linker is often <b>ld.so</b></li>
	<li><b>An executable file linked against a shared library contains only a small table of the functions it requires, instead of the complete machine code from the object files(library) for the external functions. Before the executable file starts running, the machine code for the external functions is copied into memory from the shared library file on disk by the operating system--a process referred to as <i>dynamic linking</i>.
</b></li>
</ul>
<div></div>
</div>
</div>
<div class="cell text-cell">
<h3>The basics of LD command</h3>
<div>
<pre style="font-family: 'Courier New', monospace; font-size: 12px; color: #000000;"><strong>$ld -o &lt;output&gt; /lib/crt0.o hello.o -lc</strong></pre>
<pre style="font-family: 'Courier New', monospace; font-size: 12px; color: #000000;"><em>//ld — linker dynamic</em></pre>
<pre style="font-family: 'Courier New', monospace; font-size: 12px; color: #000000;"><em>//-o  output</em></pre>
<pre style="font-family: 'Courier New', monospace; font-size: 12px; color: #000000;"><em>//link <span style="white-space: pre-wrap;">/lib/crt0.o hello.o</span></em></pre>
</div>
</div>
<div class="cell text-cell">

Some options for LD
<div>
<ul>
	<li><b>-L </b>library path search directory to search for
<ul>
	<li>= at the fornt replaced with sysroot</li>
	<li><b>SEARCH_DIR </b>can be provided via script.</li>
</ul>
</li>
	<li><b>-F </b>filter</li>
</ul>
</div>
</div>
<div class="cell text-cell">

The problem can be traced by carefully looking at the ld error which seems like a garbage of text although it happened to be a long command for <b>ld</b>
<div><b><img class="alignnone size-full wp-image-214" src="http://139.59.142.118/wp-content/uploads/2016/01/2639B2B7-EC1F-4DCC-9FFF-F6EBFB4FFDE0.png" alt="2639B2B7-EC1F-4DCC-9FFF-F6EBFB4FFDE0.png" width="2328" height="1148" />
</b></div>
</div>
<div class="cell text-cell">

I had to search where that -lPods-Bolts and other lstuffs were coming from and here it is.
<div><img class="alignnone size-full wp-image-216" src="http://139.59.142.118/wp-content/uploads/2016/01/2F3286F6-DAD5-4DB6-9BE8-207FDE0FE72A.png" alt="2F3286F6-DAD5-4DB6-9BE8-207FDE0FE72A.png" width="2012" height="644" /></div>
<div>Just double click and find the undefined symbol names and delete them. Recompile. Done!</div>
</div>
</div>
<div></div>
<div id="contentarea">
<div class="cell text-cell">

Now is this the only situation. I guess not. Lets produce this same situation again. Im guessing the Cocoapods to be the culprit.
<div>
<ul>
	<li>At the moment my pod file is like this</li>
	<li><img class="alignnone size-full wp-image-219" src="http://139.59.142.118/wp-content/uploads/2016/01/48ADC3F9-B893-49F4-AFCC-4D3C018061B8.png" alt="48ADC3F9-B893-49F4-AFCC-4D3C018061B8.png" width="568" height="418" /></li>
	<li>Now im going to revert back to not using <b>use_frameworks! </b>and see what happens then</li>
</ul>
</div>
</div>
<div class="cell text-cell">

The obvious need to simulate the situation is because i knew something has to fiddle around with the “<b>other linker flags</b>”. whos better candidate then cocoa pods?
<div></div>
</div>
<div class="cell text-cell"></div>
<div class="cell text-cell">
<h3><b>Investigation 1</b></h3>
<div><b> </b></div>
</div>
<div class="cell text-cell">

When the cocoa pods pod is executed this way:

<img class="alignnone size-full wp-image-218" src="http://139.59.142.118/wp-content/uploads/2016/01/0A941209-0753-4B22-A036-307D2B1BEF02.png" alt="0A941209-0753-4B22-A036-307D2B1BEF02" width="848" height="382" />
<div></div>
<div>notice there is no <b>use_frameworks!, </b>well find out what it exactly does a little bit later.</div>
<div></div>
<div>The result of compiling is this:</div>
<div><img class="alignnone size-full wp-image-223" src="http://139.59.142.118/wp-content/uploads/2016/01/10E97EA0-3D31-4A66-AA97-8B18EB1EC3A0.png" alt="10E97EA0-3D31-4A66-AA97-8B18EB1EC3A0.png" width="2314" height="108" /></div>
<div></div>
<div>Which basically says we dont have access to the libraries but it knows we have the libraires. The solution to this is to use a Bridging Header but why?</div>
<div></div>
</div>
<div class="cell text-cell">
<h3><b>What is in use_frameworks! ?</b></h3>
<div>
<ul>
	<li>When using a framework/library written in Objective-C, please specify “<b>Other Linker Flag</b>” to <b>-ObjC. </b>This tells the xcode project, we are linking with Objective-C code. <i>&lt;Note: cocoapods will insert one for you but still why dont you do that first right?&gt;</i></li>
	<li>And if you are using that Objective-C framework from Swift then you do need a <b>bridging header. </b></li>
	<li>In our case, if we try to comment out the <b>import Parse</b> we will get <b>undefined refrence </b>error everywhere as we try to access PFObject.</li>
	<li>This is one use case. Whats with <b>use_frameworks!</b></li>
</ul>
</div>
</div>
<div class="cell text-cell">

<img class="alignnone size-full wp-image-228" src="http://139.59.142.118/wp-content/uploads/2016/01/002CA167-04EC-4E3A-8B55-34B7A6BD78EB.png" alt="002CA167-04EC-4E3A-8B55-34B7A6BD78EB.png" width="1266" height="702" />
<div><em>Source: <a href="http://iosdevbits.blogspot.jp/2014/12/finally-cocoapods-with-swift.html">http://iosdevbits.blogspot.jp/2014/12/finally-cocoapods-with-swift.html</a></em></div>
</div>
<div class="cell text-cell">

&nbsp;

So we could conclude if we need to use Objective-C written Libraries then we can either choose to not use <b>use_frameworks! </b>and stick to using bridgning header which will import the libraries for us. This way all the swift files get access to the libraries without explicit <strong>import </strong>on all swift files.
<div></div>
<div><strong>OR,</strong> we put <b>use_frameworks!</b> and then not use bridging header. Just import Libraries in files that require them explicitly.</div>
</div>
<div></div>
<div class="cell text-cell">
<div>But notice, the above image it distinguishes <b>libraries instead of framework. </b>So maybe we need to understand that too?</div>
</div>
<div class="cell text-cell">

&nbsp;
<h3><b>Libraries vs Frameworks in Xcode/Cocoapods</b></h3>
<div>
<ul>
	<li>One answer from SO reads (source: <a href="http://stackoverflow.com/a/6389802/2419589">http://stackoverflow.com/a/6389802/2419589</a>)</li>
	<li><img class="alignnone size-full wp-image-237" src="http://139.59.142.118/wp-content/uploads/2016/01/16F55D46-B5AF-4F13-8216-3DD27347B080-1.png" alt="16F55D46-B5AF-4F13-8216-3DD27347B080" width="1574" height="584" /></li>
	<li>We kind of understand they are just 2 different naming jargons. It might boil down to static library and shared(dynamic) library we talked at first.</li>
	<li>It matches in a sense that: Static Library need to be copied and refrenced inside a program explicitly ahead of time. Thats where Objective-C bridge comes to make the connection explicitly.</li>
	<li>However, Framework (Shared Library) can be used when required but it is also loaded into memory. Remember <b>it is not required for entire shared library to be bundled inside the code segment of the program that uses it. It just knows its out there and ready to be used when needed. </b>The imports we are making on the fly might be that.</li>
	<li>This concept of Library vs Framework might not hold true or exact when you look from the eye of <b>Inversion of Control. </b></li>
	<li>Another usefiul comparision might be (source:<a href="http://stackoverflow.com/a/15331319/2419589">http://stackoverflow.com/a/15331319/2419589</a>):<img class="alignnone size-full wp-image-242" src="http://139.59.142.118/wp-content/uploads/2016/01/67214E7D-ACA8-4B40-BAF6-82AA33A796D5.png" alt="67214E7D-ACA8-4B40-BAF6-82AA33A796D5.png" width="1588" height="718" /></li>
	<li>But from the iOS8 onwards, apple allowed third party developers to create the so called frameworks which are Shared Libraries.</li>
</ul>
</div>
<h3>Conclusion:</h3>
Is this all? No. But, I figured out what was wrong in my project and then did understand why it happened at first place. With our little research i hope you found something to take away and apply.

I believe Knowing the why things work the way it does is important to become a good Engineer.

If there are any amendments to make and things i left out or misused, then dont hesitate to comment below.

</div>
&nbsp;
<div class="cell text-cell"></div>
</div>
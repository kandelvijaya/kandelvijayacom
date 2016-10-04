---
ID: 461
post_title: 'XcodeFormatter: ZStyle!'
author: kandelvijaya
post_date: 2016-10-01 14:23:03
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2016/10/01/xcode-code-zstyle-code-formatter/
published: true
---
Yet again somebody missed to insert a empty line before the end of file, I missed to provide a empty space after dictionary Key <code>[AnyHashable:Any]</code> and you might miss to leave any of these kinds of code:
<pre><code>//.......
return data
}
func compute(a:Int,b:Int)-&gt;Int{
//.......
</code></pre>
Which should have been:
<pre><code>//.......
return data
}

func compute(a: Int, b: Int) -&gt; Int {
//.......
</code></pre>
So you see where I'm heading with this.
<h2>Bigger Picture</h2>
To just briefly explain the scenario, we are 12 iOS Engineers working harder than before (pun intended), cramming hundreds of line of code daily.
We have bunch of features to roll out, release deadline to meet and a generation next to come and read the code we wrote with great thought.
We couldn't possibly achieve if, each of us, with unique style <code>(tabs vs spaces)</code> worked on. Thus, we have a Zalando iOS coding guidelines.
Which keeps us in the same fashion. But like again, there is no way to actually force the project to have a strict guidelines and we humans are error prone.
<h2>Problem</h2>
Some empty line or no empty spaces sneaks in the code base. We need a tool to correct out the our style to Zalando iOS style.
After all we care, because mainly we are a fashionable technology company. All right!
<h2>Solutions Available</h2>
<ol>
 	<li>Swift Lint <a href="https://github.com/realm/SwiftLint">Swift Lint on Github</a></li>
</ol>
* Its a good library but we have some serious issue with it.
* We have a large build time and this adds some more.
* We already have hundreds of TODO and FIXME that are signaling warnings. This adds even more errors/warnings.
* The tool has limited possibility of correction. Our solution is not to give things to the devs to correct. They are already busy. It is to work for them.
<h2>Our Attempt</h2>
<ol>
 	<li>Before WWDC 2016</li>
</ol>
* Integrate SwiftLint and use regex to auto correct. But still the disadvantage was bigger than advantage.
* Abandoned!
2. After WWDC 2016
* Source Kit Extension API announced.
* I started secretly using this extension to auto correct only things we cared.
* The birth of XcodeFormatter!
<h2>XcodeFormatter</h2>
<ol>
 	<li>Its a file based auto formatter. It wont warn or error. It just silently formats to the ZStyle. Pretty silently.</li>
 	<li>Reach out to the Editor -&gt; XFormatter -&gt; ...(options)</li>
 	<li>When you absolutely don't want to, then don't.</li>
 	<li>If you want to, which you should, then provide a shortcut or if you are mouse ninja then reach in the Editor -&gt;</li>
 	<li>Done!</li>
</ol>
<h2>Technical Side</h2>
I prefer to run down the tech side by data flow or message flow.
<ol>
 	<li>When user presses, Editor -&gt; XcodeFormatter -&gt; Correct All: Xcode sends the file content to our App Extension.</li>
 	<li>The app extension, choses a certain formatting method based on the command user clicked.</li>
 	<li>Inside the app extension, there are <code>RegexMatch</code> and <code>CodeBlockAnalyzer</code> which stays at the heart of matching the wrong style.</li>
 	<li>Inside the app extension, there are <code>MatchCorrection</code> and <code>EmptyLineCorrection</code> which stays at the heart of correcting the matched code.</li>
 	<li><code>MatchCorrectionInfo</code> is used to correct the match in case of <code>MatchCorrection</code>. Idea is to replace the Capture Group. <code>MatchCorrectionInfo</code> provides a <code>[Int: String]</code> with int for index of Capture Group and String for what to replace the found match with.</li>
 	<li><code>EmptyLineCorrection</code> does not need a correction rule as it trivially inserts/removes empty line above/below each <code>CodePosition</code> passed into correct.</li>
 	<li>All the matches and analyzation happens if the current character in code is not inside a <code>//Comment</code> or <code>"String quote"</code></li>
 	<li>When everything is done, the <code>XCSourceEditorExtension</code> gets the corrected data, uses it to put the changes back into the <code>NSMutableArray</code> of lines Xcode provided us initially.</li>
 	<li>This change is then reflected in Xcode once the completion handler is called.</li>
 	<li>Boom! Done!</li>
</ol>
<h3>A more deeper level working will be covered when this article is updated.</h3>
For details, check the readme file on github. <a href="https://github.com/kandelvijaya/XcodeFormatter">XcodeFormatter On Github</a>
<h2>Cheers!</h2>
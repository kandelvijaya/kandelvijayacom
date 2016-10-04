---
ID: 110
post_title: iOS custom View and Nib loading
author: kandelvijaya
post_date: 2015-07-12 14:00:05
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2015/07/12/ios-custom-view-and-nib-loading/
published: true
audiourl:
  - ""
videourl:
  - ""
---
<p><!-- common.css --></p><p><!-- exported.css --></p><p><!-- User CSS --></p><p><script src="http://cdnjs.cloudflare.com/ajax/libs/highlight.js/8.2/highlight.min.js"></script><script type="text/javascript">// <![CDATA[
window.MathJax = {       tex2jax: {         inlineMath: [ ['$','$'], ["\\(","\\)"] ],         processEscapes: true,         processClass: 'latex-cell'       },       "HTML-CSS": {         preferredFont: "STIX"       }     };
// ]]></script><br /><script src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML" type="text/javascript"></script><script>// <![CDATA[
hljs.initHighlightingOnLoad();
// ]]></script></p><div id="contentarea"><div class="cell text-cell"><p>When the nib file is loaded from the View Controller in these way:</p><p>&nbsp;</p><div><div><p style="margin: 0px; font-size: 14px; line-height: normal; font-family: Menlo; color: #4f8187;">sentencePicker<span style="font-variant-ligatures: no-common-ligatures; color: #000000;"> = </span><span style="font-variant-ligatures: no-common-ligatures; color: #703daa;">NSBundle</span><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">.</span><span style="font-variant-ligatures: no-common-ligatures; color: #3d1d81;">mainBundle</span><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">().</span><span style="font-variant-ligatures: no-common-ligatures; color: #3d1d81;">loadNibNamed</span><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">(</span><span style="font-variant-ligatures: no-common-ligatures; color: #d12f1b;">"SentencePartsPicker"</span><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">, owner: </span><span style="font-variant-ligatures: no-common-ligatures; color: #bb2ca2;">self</span><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">, options: </span><span style="font-variant-ligatures: no-common-ligatures; color: #bb2ca2;">nil</span><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">).</span><span style="font-variant-ligatures: no-common-ligatures; color: #703daa;">last</span> <span style="font-variant-ligatures: no-common-ligatures; color: #bb2ca2;">as</span><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">? </span>SentencePartsPickerView</p><p style="margin: 0px; font-size: 14px; line-height: normal; font-family: Menlo; color: #4f8187;"><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">        </span><span style="font-variant-ligatures: no-common-ligatures; color: #703daa;">view</span><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">.</span><span style="font-variant-ligatures: no-common-ligatures; color: #3d1d81;">addSubview</span><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">(</span>sentencePicker<span style="font-variant-ligatures: no-common-ligatures; color: #000000;">!)</span></p><p style="margin: 0px; font-size: 14px; line-height: normal; font-family: Menlo;">        <span style="font-variant-ligatures: no-common-ligatures; color: #4f8187;">sentencePicker</span>?.<span style="font-variant-ligatures: no-common-ligatures; color: #703daa;">center</span> = sender.<span style="font-variant-ligatures: no-common-ligatures; color: #703daa;">center</span></p><p style="margin: 0px; font-size: 14px; line-height: normal; font-family: Menlo; color: #4f8187;"><span style="font-variant-ligatures: no-common-ligatures; color: #000000;">        </span>sentencePicker<span style="font-variant-ligatures: no-common-ligatures; color: #000000;">?.</span>delegate<span style="font-variant-ligatures: no-common-ligatures; color: #000000;"> = </span><span style="font-variant-ligatures: no-common-ligatures; color: #bb2ca2;">self</span></p></div></div><div> </div></div><div class="cell text-cell"><p><!--more--></p><p>The view controller gets a custom object or subclass of UIView associated with the nib to either house the outlets or actions or delegations. Meaning: nib loading loads the class associted with it by calling this code:</p><div><p style="margin: 0px; font-size: 14px; line-height: normal; font-family: Menlo;"><span style="font-variant-ligatures: no-common-ligatures; color: #bb2ca2;">required</span> <span style="font-variant-ligatures: no-common-ligatures; color: #bb2ca2;">init</span>(coder aDecoder: <span style="font-variant-ligatures: no-common-ligatures; color: #703daa;">NSCoder</span>) {</p><p style="margin: 0px; font-size: 14px; line-height: normal; font-family: Menlo;">        <span style="font-variant-ligatures: no-common-ligatures; color: #bb2ca2;">super</span>.<span style="font-variant-ligatures: no-common-ligatures; color: #bb2ca2;">init</span>(coder: aDecoder)</p><p style="margin: 0px; font-size: 14px; line-height: normal; font-family: Menlo;">        <span style="font-variant-ligatures: no-common-ligatures; color: #008400;">//setup</span></p><p style="margin: 0px; font-size: 14px; line-height: normal; font-family: Menlo;">        <span style="font-variant-ligatures: no-common-ligatures; color: #31595d;">setup</span>()</p><p style="margin: 0px; font-size: 14px; line-height: normal; font-family: Menlo;">    }</p></div></div><div class="cell text-cell"><p>The setup() method here is a custom point where we can do something as sson as the views are loaded and the outlets are configured.</p><div> </div></div><div class="cell text-cell"><p><b>But what if you dont want the view controller to know the nifty gritty details of loading the nib file</b>. Instead why not get the same thing done from the UIView subclass. Yes fun but a little tricky.</p><div> </div></div><pre class="cell code-cell"><code>
import UIKit

protocol OverlayMultipersonViewDelegate:class{
    func overlayMutlipersonViewNeedsToClose(sender:OverlayMutlipersonView)
}

class OverlayMutlipersonView: UIView {
    var delegate:OverlayMultipersonViewDelegate?{
        didSet{
            (self.subviews.first as? OverlayMutlipersonView)?.delegate = self.delegate
        }
    }
    required init(coder aDecoder: NSCoder) {
        super.init(coder: aDecoder)
    }

    private var wantedFrame:CGRect?

    //MARK:- view loading
    override init(frame: CGRect) {
        super.init(frame: frame)
        wantedFrame = frame
        setup()
    }

    func setup(){
        let nib = NSBundle.mainBundle().loadNibNamed("OverlayMultipersonView", owner: self, options: nil)
        let nibbedFrame = nib?.last as! UIView
        nibbedFrame.frame = wantedFrame!
        self.addSubview(nibbedFrame)
    }


    //MARK:- properties
    @IBOutlet var translatedLabel:UILabel!
    @IBOutlet var originalLabel:UILabel!

    @IBOutlet var translatedActionButton:UIButton!

    @IBOutlet var originalActionButton:UIButton!

    @IBAction func exitMultiPersonView(sender: UIButton) {

       delegate?.overlayMutlipersonViewNeedsToClose(self)
    }
    //MARK:- other methods
}</code></pre><div class="cell text-cell">Pay attention to the nib loading setup() method. The loaded nib come with the same UIView so we put it on the stack as a subview of the same kind. I know its confusing to understand at first but believe me it gives a lot of problems debugging why the delegates you set from the View controllers wont work or the outlets dont update the content. These is how you call the above calss from the View c</div><pre class="cell code-cell"><code>        //MARK:-CONFIGURING OVERLAY VIEW
        let newFrame = CGRect(x: 0, y: navigationController!.navigationBar.frame.origin.y , width: tableView.bounds.width, height: tableView.bounds.height)

        //add the subview on top
        overlayView = OverlayMutlipersonView(newFrame)
        overlayView?.backgroundColor = UIColor.clearColor()
        overlayView?.delegate = self</code></pre><div class="cell text-cell"><p>Lets break the process down:</p><div><ol><li>The view controller makes a object of Custom UIView by calling <span style="font-family: Menlo; font-size: 14px; line-height: normal; color: #000000;">        </span><span style="color: #4f8187; font-family: Menlo; font-size: 14px; line-height: normal;">overlayView</span><span style="font-family: Menlo; font-size: 14px; line-height: normal; color: #000000;"> = </span><span style="color: #4f8187; font-family: Menlo; font-size: 14px; line-height: normal;">OverlayMutlipersonView</span><span style="font-family: Menlo; font-size: 14px; line-height: normal; color: #000000;">()</span></li><li>The object of UIView is created but lets take how the UIView is actually created<ol><li>The initWithFrame is called that calls setup</li><li>setup makes loads the nib now and puts the loaded view which esentially is that same view as the current view on top of itself</li><li>Meaning we have 2 exact UIViews on stack whereas the latest has the nib resources, the former is just an object without resource</li><li>Lets look at the view in Xcode6 view debugger</li><li><a href="http://www.kandelvijaya.com/wp-content/uploads/2015/07/CF018965-A728-496C-AA1F-50CA1662BADB.png"><img class="aligncenter size-large wp-image-111" src="http://www.kandelvijaya.com/wp-content/uploads/2015/07/CF018965-A728-496C-AA1F-50CA1662BADB-652x1024.png" alt="kandelvijayaIOS" width="652" height="1024" /></a></li></ol></li></ol></div></div><div class="cell text-cell"> </div><div class="cell text-cell">The above view is the view that View Controller has direct refrence to. See it doesnot have any thing to display on itself. Now see below that this view has a subview which is the same object but with resources loaded from the nib files.</div><div class="cell text-cell"><a href="http://www.kandelvijaya.com/wp-content/uploads/2015/07/F570D93F-85F4-4010-A3F2-7B38668C1AB8.png"><img class="aligncenter size-large wp-image-112" src="http://www.kandelvijaya.com/wp-content/uploads/2015/07/F570D93F-85F4-4010-A3F2-7B38668C1AB8-1024x1011.png" alt="kandelvijaya iOS" width="960" height="947" /></a></div><div class="cell text-cell"><p>Now lets get back to the Custom UIView class that loads our nib file and makes subview. In our interface of nib file we have a button when clicked it needs to delegate back to the view controller to remove the whole overlay view.</p><div><ol><li>We set a delegate on the Custom UIView class and set that via the controller.</li><li>The user presses the button but immediate superview of the button loaded from nib file doesnot have the delegate set to delegate back. The program neither compains nor guides us in the right path. (Thats why i went in detail to explain the way nibs are loaded)</li><li>It is beacuse the button from nib is on a sepearate UIView, not on the first UIView which our controller thinks and we think it should be.</li></ol></div><div> </div></div><div class="cell text-cell"><p>How do we solve that?</p><div>Knowing what is going on is the first and the only step to solve it. I spent an hour to figure what went wrong. I was confused because i thought i was doing good, compiler never compained, program never crashed but the debugger keeps saying i have nil delegate.</div><div> </div><div>The solution as i implemented above is to forward delegations and frames to the actual view and this requires knowledge whats going on. This is a bad solution i know but it pointed me in the right field.</div><div> </div><div>2 other possible solutions:</div><div><ol><li>replace the self with the loaded nib resource and then set the delegates</li><li>Load the nib in the View Controller after all its not so scary as it sounds. (I prefer this way uless there is a good reason)</li></ol></div></div></div><p><!--more--></p><p><!--more--></p><p><!--more--></p><p><!--more--></p><p><!--more--></p><p><!--more--></p><p><!--more--></p><p><!--more--></p><p><!--more--></p><p>&nbsp;</p>
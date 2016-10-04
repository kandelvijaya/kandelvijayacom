---
ID: 96
post_title: Show UIRefreshControl programmatically
author: kandelvijaya
post_date: 2015-05-08 07:56:23
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2015/05/08/show-uirefreshcontrol-programmatically/
published: true
---
UIRefreshControl comes with a UITableViewController. You just set it to be enabled from the storyboard inspector and you are good to pull the tableview to display the refreshing controller which animates a spinner. Just like this.

<a href="http://www.kandelvijaya.com/wp-content/uploads/2015/05/Screen-Shot-2015-05-08-at-4.37.04-PM.png"><img class="aligncenter  wp-image-97" src="http://www.kandelvijaya.com/wp-content/uploads/2015/05/Screen-Shot-2015-05-08-at-4.37.04-PM.png" alt="UIRefresh control" width="461" height="444" /></a>

&nbsp;

&nbsp;

Now the catch is you might want to animate it down the screen whenever the user types in some search key and you go fetch it from a web server asynchronously which definitely will take some time, if not more than offline search. It would be extremely if apple were to provide a simple way.

Now, most people confuse it works simply with this.
<pre class="lang:swift decode:true">self.refreshControl?.beginRefreshing()</pre>
But it does not show up the refresh control. It just starts spinning somewhere above the screen. The visual layout is not done by the framework in any case beside a real pull on the table view.

&nbsp;

Now it is our responsibility to layout the views, bringing the refresh control view down to the appropriate place. Seems like a huge task to reshuffle views around but theres a neat way.

We must consider the fact that user might have a navigation bar and top status bar. The tableview is tucked under them. This is done via the UITableViewController API using the tableView.contentOffset which is set dynamically.

We might grab this property on viewWillLayoutSubviews but if we were to it changes the property in every single movement. Which isn't good at all.

The solution,
<pre class="lang:swift decode:true ">//in respect to the navbar + top bar //the top bar goes away in landscape mode
                let navigationBarHeight = self.navigationController!.navigationBar.frame.height
                let navBarY = self.navigationController!.navigationBar.frame.origin.y
                self.currentTableOffestNormal = CGPointMake(0, (-navBarY - navigationBarHeight))
</pre>
But what if the search bar is active. Check if it is and add this.
<pre class="lang:swift decode:true">var heightOfSearchBar = self.searchController.searchBar.frame.height
                self.currentTableOffestNormal = CGPointMake(0, heightOfSearchBar)</pre>
So to pull them together hers a look at it:
<pre class="lang:swift decode:true">var currentTableOffestNormal:CGPoint?

//property observer
var defaultTableOffset:CGPoint?{
        get{
            if self.searchController.active{
                //we take the offset in respect to search bars height only
                var heightOfSearchBar = self.searchController.searchBar.frame.height
                self.currentTableOffestNormal = CGPointMake(0, heightOfSearchBar)
            }else{
                //in respect to the navbar + top bar //the top bar goes away in landscape mode
                let navigationBarHeight = self.navigationController!.navigationBar.frame.height
                let navBarY = self.navigationController!.navigationBar.frame.origin.y
                self.currentTableOffestNormal = CGPointMake(0, (-navBarY - navigationBarHeight))
            }

            return self.currentTableOffestNormal
        }

        set{
            self.currentTableOffestNormal = newValue
        }
    }</pre>
That will get us or help us set/get the value when anything changes like view animating from portrait to landscape or view appearing.

So we got the offset property which handles lots of different situation quite smartly. Lets use it to animate the refresh control.
<pre class="lang:swift decode:true ">//set this property to start or end refreshing
var refreshControlStatus = false{
        willSet{
            "Refresh Value set"
            //if we already have refresh control ending then leave it
            //if we already have refresh control starting then leave it
            if self.refreshControl!.refreshing != newValue{
                if newValue{
                    //TRUE WE START
                    //our tableview can have some internal offset set due to the presence of navigation bar
                    //it is our responsibility to adjust the offset of tableview by proper amount while displaying refresh bar from code
                    var refreshHeight = self.refreshControl!.frame.size.height
                    self.tableView.setContentOffset(CGPointMake(self.tableView.contentOffset.x, defaultTableOffset!.y - refreshHeight), animated: false)
                    self.refreshControl?.beginRefreshing()
                }else{
                    //we stop
                    //we set to default :: reversing the above process
                    self.tableView.setContentOffset(self.defaultTableOffset!, animated: true)
                    self.refreshControl?.endRefreshing()
                }
            }
        }
    }</pre>
The above code also checks to not call the beginRefreshing() if the spinner is already refreshing. Sometimes calling twice cause the spinner to disappear.

&nbsp;

So there you go, all with cool swift property observers you get a full smart refresh control that shows up programmatically even when device orientation changes. Hope you find it useful. Comment if you have a better idea.

&nbsp;

Make sure you don't get confused that when you call refreshControl to start programmatically, you don't fire the action method which you attached with the UIControlEven of pulling. Its not the same as simulating real pull.
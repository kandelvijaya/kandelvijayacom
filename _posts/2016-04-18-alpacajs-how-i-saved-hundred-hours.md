---
ID: 366
post_title: 'AlpacaJS: How i saved hundred hours on Web Form?'
author: kandelvijaya
post_date: 2016-04-18 18:35:18
post_excerpt: ""
layout: post
permalink: >
  http://kandelvijaya.com/index.php/2016/04/18/alpacajs-how-i-saved-hundred-hours/
published: true
---
On my recent project, we needed to collect data. This was a medical app and we dont have a web interface. We are truly mobile centric because we need to support the offline capability. The app works mostly on the content. Specifically, content include Hospital Profiles and Doctor Profiles. We wanted to give the responsibility to the doctors to enter/edit/update their profile. The best way we know is via Web. But we are not a big fan of creating web apps. I personally hate this idea of writing HTML, CSS (which is confusing) and Javascript. Then comes the database connection. Bootstrap. Responsive Design. Date&amp;Time support. DropDowns. Validations. Confirmation. There is lot going on. But we had to make a CRUD (Create, Read, Update Delete(We dont support delete. Funny!) ) app. I could have done it manually like most do but then when the specs changes then i have to configure it manually everywhere. Support Browsers and the mobile. This is too much. Luckily there is just a tool that helped me out.

AlpacaJS. This is a open source library for Form creation. There are lots of crappy tools that say they will build forms. Most of them will produce PHP code which kind of is ugly in the sense that you need to refresh the page to revel next set of items. So PHP was out of scope. We needed a JS solution. AlpacaJS is written in Javascript and is a form renderer.

Provide a JSON object for what the form items are and what types of values it accept to the AlapacaJS engine and boom...  It renders the form using JQueryUI and bootstrap. It will validate with the type JSON provides and gives you all the buttons to navigate to NEXT/ PREVIOUS page and Creates a JSON data structure when you hit SUBMIT. Which was perfect because we were using a no-sql database. So a direct map for us. I dont think its hard to map to a relation database either. This is what it takes to make a simple form.

1.  JSON schema to describe the form

<img class="alignnone size-full wp-image-386" src="http://139.59.142.118/wp-content/uploads/2016/04/Screen-Shot-2016-04-18-at-8.19.31-PM.png" alt="Screen Shot 2016-04-18 at 8.19.31 PM" width="1442" height="1004" />

2. JSON options to validate the schema

<img class="alignnone size-full wp-image-388" src="http://139.59.142.118/wp-content/uploads/2016/04/Screen-Shot-2016-04-18-at-8.18.39-PM.png" alt="Screen Shot 2016-04-18 at 8.18.39 PM" width="1406" height="1176" />

3. give the JSON to AlpacaJS to render and BOOM!

<img class="alignnone size-full wp-image-387" src="http://139.59.142.118/wp-content/uploads/2016/04/Screen-Shot-2016-04-18-at-8.19.13-PM.png" alt="Screen Shot 2016-04-18 at 8.19.13 PM" width="1054" height="180" />

4. Here you go!!!!!

<img class="alignnone size-full wp-image-393" src="http://139.59.142.118/wp-content/uploads/2016/04/Screen-Shot-2016-04-18-at-8.22.27-PM.png" alt="Screen Shot 2016-04-18 at 8.22.27 PM" width="1776" height="1150" />

&nbsp;

I couldnot provide the full code samples because of the fact that they are used in production. But if you like to check out how incredible it is then head over to the <a href="http://www.alpacajs.org" target="_blank">www.alpacajs.org</a>

<strong>Learning Curve:</strong> It took me about a week to fully get the concept and complete a demo prototype of the form. That was it. It took one week because despite the fact this project is super cool there are few contributors (25 or so ) and is not widely famous to get answers for some edge cases which im sure everyone will have. So you need to dig deep and/or experiment on what works with lots of trial.

<strong>My Rating: </strong>4.8

<strong>Rationale: </strong>The library is super cool becuase you just give it schema and it renders. If you give it the data in JSON then it will also render the data into the form. Which effectively makes it possible to edit and then update the database. In fact, this is what we do in our app. You dont care about tiny HTML, CSS, cross browser details but work on the business data layer in a sweet language Javascript to add some more functionality if needed. This is what i really look for.

<strong>ROI (Return On Investment): </strong>Saved me atlest 30 hours within 4 months. If i were to implement it with my web skills i would be still working on that hitting my desk why do i have to do this crap!! Maintainance is a breeze. This morning, before heading to my office, our data team called me to remove a <em>required </em>form field to <em>optional. </em>They wanted to add 2 extra properties we need for the next release. I just opened the Schema, added removed required field and added 2 Key value pairs to the JSON. Boom its done. 5 minutes. Our data team thinks im super web developer.

If you want to know more about the details and how we use No-Sql and/or why? Leave us a comment and ill try to answer when possible.

&nbsp;
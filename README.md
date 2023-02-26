Today, we will look at how you can export data from your PowerApps applications to Excel.

Basically, the scenario of today's post, is to dump a collection from PowerApps to a .csv file.
<h1>Requirements:</h1>
We need the following:
<h2>PowerApps Canvas Application</h2>
This can also be a model-driven app, you will need to create a simple app (or have your existing app) with the following:
<ul>
 	<li>A Gallery to display your data</li>
 	<li>The Export Button, which will call the Power Automate and pass the collection to be exported.</li>
</ul>
Here is an example of my basic application:
<ul>
 	<li>1st, I am initialising a collection on the App.OnStart event, you might be using your existing collection<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-1.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100339 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-1-1024x470.png" alt="" width="640" height="294" /></a></li>
 	<li>Next, I will put a gallery in my PowerApps Screen - This Step is optional, just to show me the data before I export it.</li>
</ul>
<ul>
 	<li>Last, we need an Export Button, which will be used to call our Power Automate later. We will come back to the Button's action after we create the Power Automate.</li>
</ul>
<h2 data-wp-editing="1"><a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-2.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100340 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-2-1024x540.png" alt="" width="640" height="338" /></a>PowerAutomate</h2>
This is most important component of the solution.

We will need an "Instant Cloud Flow"

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-3.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100342 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-3-1024x616.png" alt="" width="640" height="385" /></a>

Please, make sure to use "PowerApps" Trigger for the new flow, provide a name and let's get started !

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-4.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100343 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-4-1024x648.png" alt="" width="640" height="405" /></a>

&nbsp;

Add a new action, "<strong>Compose</strong>", please rename the action to something user friendly, and for the value, chose, "<strong>Ask in PowerApps</strong>", this will create an input parameter for your Power Automate.

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-5.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100345 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-5-1024x478.png" alt="" width="640" height="299" /></a>

&nbsp;

Next, you will need to parse the content, using the JSON function.
We will be passing our collection from PowerApps in a JSON format, so we will need to de-serialise the input to get the structured data.

You can add the action "<strong>Create a CSV Table</strong>"

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-6.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100346 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-6-1024x416.png" alt="" width="640" height="260" /></a>

For the from value of this action, use the expression:
<pre class="EnlighterJSRAW" data-enlighter-language="generic">json(outputs('Content'))</pre>
<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-7.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100347 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-7-1024x293.png" alt="" width="640" height="183" /></a>

&nbsp;

Now, we need to create a new file with the output of the "Create CSV table",

To do so, we will use OneDrive for Business to create the new file.

Add the action "<strong>Create file</strong>"

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-8.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100348 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-8-1024x452.png" alt="" width="640" height="283" /></a>

Provide the file's location, name and content. The content should be the output of "<strong>Create a CSV Table</strong>" action:

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-9.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100349 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-9-1024x369.png" alt="" width="640" height="231" /></a>

Now, we can create a share link to return from PowerAutomate to PowerApps, the link will be used to open or download the newly create file (the export file).

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-10.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100351 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-10-1024x647.png" alt="" width="640" height="404" /></a>

This action takes couple of parameters,
<ul>
 	<li>ID of the file (this needs to be the ID from the previous action "<strong>Create file</strong>"),</li>
 	<li>Type of link, this can be edit or view link.</li>
 	<li>Last, the scope of your link, anonymous (public) or organisation (internal link).</li>
</ul>
<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-11.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100350 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-11-1024x339.png" alt="" width="640" height="212" /></a>

&nbsp;

Finally, we will return the link to PowerApps, to do so, we will need to use the "<strong>Respond to PowerApps</strong>" action.

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-12.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100353 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-12-1024x395.png" alt="" width="640" height="247" /></a>

I have added an output of type Text, and used the Web Url from the previous action as value of the output parameter.

Now, back to PowerApps, we will need to attach our PowerAutmate to the PowerApps Application.

To do so,

1- Click the PowerAutomates tab.

2- Select the Add Flow button.

3- Add your new flow, if you can't see it yet, try to refresh the list (...)

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-13.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100354 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-13-1024x756.png" alt="" width="640" height="473" /></a>

All good, now we can call the PowerAutomate from our Button.

To do so, select your Export Button and use the following action in the OnSelect Event.

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-14.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100355 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-14-1024x152.png" alt="" width="640" height="95" /></a>

This would have been enough to trigger the flow, but, as we are expecting an output value from the flow, I will assign the result of the flow call to a variable, which I can use later to navigate to the newly created file as follow:

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-15.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100357 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-15-1024x242.png" alt="" width="640" height="151" /></a>

Now, when you run your application and click on the Export Button, the call will be made and after couple of seconds the link to your new file will open in a new tab, and Voila !

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-16.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100356 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PAPPS-Export-16-1024x802.png" alt="" width="640" height="501" /></a>

&nbsp;

Full code (PowerApps and PowerAutomate) available from Github --&gt; <a href="https://github.com/samirlogisam/PowerApps-Export-To-Excel">https://github.com/samirlogisam/PowerApps-Export-To-Excel</a>

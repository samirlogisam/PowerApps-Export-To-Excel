In Today's Article, I will explain how we can easily save a signature from your PowerApps Application to SharePoint document library as an image file.

Requirements:
<h1>PowerApps App</h1>
For this demo, I will create a new Canvas App (Phone layout).

All we need in this app are:
<ul>
 	<li><strong>Pen Input</strong> Control (which will be used to capture the Signature)</li>
 	<li><strong>A button</strong>, which will trigger the Power Automate flow, and pass the signature image to be saved in SharePoint.</li>
</ul>
Your app should look like this:
<h1><a href="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-1.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100371 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-1-1024x532.png" alt="" width="640" height="333" /></a></h1>
&nbsp;
<h1>Power Automate</h1>
The Power Automate Flow is triggered from the PowerApp and receives two parameters:
<ul>
 	<li><strong>File Content</strong>: of type File, this will be the actual image</li>
 	<li><strong>Filename</strong>: of type Text, this is optional, if you want to have a custom filename passed from the PowerApp, you can set the value of this parameter.</li>
</ul>
The trigger of the PowerAutomate flow looks like this:

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-2.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100373 size-full" src="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-2.png" alt="" width="650" height="293" /></a>

Next, we will need to use the SharePoint/ Create File action, to save the image to a SharePoint Document library

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-3.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100376 size-full" src="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-3.png" alt="" width="682" height="276" /></a>

Here, I define:
<ul>
 	<li><strong>Site Address</strong>: The SharePoint site.</li>
 	<li><strong>Folder Path</strong>: The document library which will be used to save the image</li>
 	<li><strong>File Name</strong>: I am using the Parameter "<em>Filename</em><strong>"</strong>.</li>
 	<li>File Content: This is the image content, passed through the parameter "<em>File Content</em>".</li>
</ul>
Your entire Power Automate would look like this:

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-4.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100377 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-4-1024x409.png" alt="" width="640" height="256" /></a>

&nbsp;

Back to Power Apps, we will need to include the flow in our PowerApps.

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-5.0.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100379 size-full" src="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-5.0.png" alt="" width="642" height="552" /></a>
Finally, let's have a look at the button's action:

<a href="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-5.png" target="_blank" rel="noopener"><img class="aligncenter wp-image-100378 size-large" src="https://daoudisamir.com/wp-content/uploads/2023/02/PowerApp-Signature-SharePoint-5-1024x413.png" alt="" width="640" height="258" /></a>

What I am doing here, is just to call the PowerAutomate Flow, and pass both parameters, File Content and Filename.
<pre class="EnlighterJSRAW" data-enlighter-language="generic" data-enlighter-theme="monokai">SignatureToSp.Run(
    {
        contentBytes: PenInput1.Image,
        name: Text(
            Today(),
            "ddmmyyy"
        ) &amp; "_signature.jpg"
    },
    Text(
        Today(),
        "ddmmyyy"
    ) &amp; "_signature.jpg"
)</pre>
That's it, enjoy !

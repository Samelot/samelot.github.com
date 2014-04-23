---
title: Setup OSC communication for Photoshop
layout: blogpost
category: blog
custom_sub: osc+pd
---
<!--
+ the or our : the/our : OUR/WE < ok?
+ USE ARGUMENT>> NOT VALUE !!
+ swap vs swaps. Having now changed order of main.js code/code description!
+ plugin menu-state vs plugin's menu-state
+ internal/built-in... it's ok to use internal once or twice, but not enough to overpower our understanding.
+ don't get too into osculator.
+ extendscript is .JSX
+ capitalization
-->

{% include injectbullet.html %}

{% include injectblognav.html %}

## Overview and Requirements

In this post, we'll setup Photoshop to use OSC (Open Sound Control). If your curious about what this looks like, and you want to see it before you try it, [check out this video](https://vimeo.com/92411743).

<!-- If your curious about what this looks like, and you want to see it with your eyes before you try it with your thighs, [click here](https://vimeo.com/92411743). -->

<!-- If you're curious about why we'd want to do that, please refer to the previous post, [magic bridge](). -->

We'll use a Generator plugin called [Openbuttkiss](https://github.com/Samelot/openbuttkiss), which gives OSC messages access to Photoshop's API. More specifically, when an OSC message with an argument of 1 is recieved, the foreground and background colors are swapped. Utilized by this plugin is a Nodejs module that provides the protocols to communicate via OSC. I made the plugin to demonstrate this sort of integration at its most fundamental level; it is left simple and concise. Expansion will come later.

I used some code from Tom Krcha's [getting-started-plugin](https://github.com/adobe-photoshop/generator-getting-started "getting-started-plugin") - this provided a good foundation for my plugin and the features I would add. Big thanks to him and the others out there that made great tutorials employing Photoshop-Generator and Nodejs:

+ **Andy Hall:** [Node.js + Photoshop: How Comments Feed Design and Generator](http://aphall.com/2013/10/generator%E3%81%AE%E8%A8%AD%E8%A8%88%E3%81%A8%E4%BD%BF%E3%81%84%E6%96%B9/), [JSConf.Asia 2013](http://www.youtube.com/watch?v=wqmMqB91zdI)

+ **Tom Krcha:** [Script Your First Adobe Generator Plugin For Photoshop](http://tomkrcha.com/?p=3896)

+ **Lee Brimelow:** [Introduction to Photoshop Generator](http://www.youtube.com/watch?v=lmSKgiY0ZPM)

The purpose of writing this post is to explain and understand the OSC implementation that is happening in my plugin. I'd like to clear up any areas of the code that are prone to confusion, and that may have caused myself some frustration.

To follow along, you'll need:

Photoshop CC, [Generator-Core](https://github.com/adobe-photoshop/generator-core), [Node.js](http://nodejs.org/), [Osculator](http://www.osculator.net/), [Openbuttkiss](https://github.com/Samelot/openbuttkiss)

## Installation

1. Download [openbuttkiss](https://github.com/Samelot/openbuttkiss).

2. In terminal, cd to the plugin folder (openbuttkiss) and run `npm install` to download any dependencies.

3. Follow one of the methods below to set up the plugin with a Photoshop Generator.

**Built-in Generator**

> Place the plugin inside the Photoshop Plug-ins folder:
>
> `Applications/Adobe Photoshop CC/Plug-ins/Generator`
>
> ![Built-in Generator plugin location](/images/internal-plugin-folder.png "Plugins to be run by Built-in Generator")
>
> Open Photoshop.
> Go to `Photoshop > Preferences > Plug-ins`
> and check `Enable Generator`. You may need to restart Photoshop for this to take effect.

<!-- Enable Remote Connections -->

**External Generator**

> Run the plugin from a locally cloned [generator-core](https://github.com/adobe-photoshop/generator-core) library (think of this as our external generator, which exists outside of the Photoshop application). Don't forget to `npm install`, to grab all the generator-core dependencies. My workflow when using this library consists of having all my plugins inside the generator-core plugins folder:
>
> ![External Generator plugin location](/images/external-plugin-folder.png "Plugins to be run by External Generator")
>
> In terminal, cd to `generator-core` and run:
>
> `node app.js -f test/plugins/openbuttkiss`
>
> This app.js contains all the magic needed to run our plugin. The plugin location is specified at the end of the command.

<!-- generator-core plugins folder? test/plugins or plugins -->

Photoshop can use Built-in and External Generators to process a plugin; each generator provides a slightly different workflow. Regardless of which generator is being used, the plugin should produce the same results. Be aware that conflicts may arise when using both internal and external generators. In my experience, I had to disable the built-in generator to properly run openbuttkiss with the external generator.

[Read more on Photoshop Generators and the Generator Architecture](https://github.com/adobe-photoshop/generator-core/wiki/Generator-Architecture).

## Running

After completing the above steps, openbuttkiss should now be available for use. In Photoshop, select the plugin from the drop down menu:

`File > Generate > openbuttkiss`

**Plugin being run by the Built-in Generator**

![built-in generator running our plugin](/images/internal-generators.png "Run a plugin with Built-in Generator")

**Plugin being run by an External Generator**

![external generator running our plugin](/images/external-generators.png "Run a plugin with External Generator")

When selected, the plugin name in the Generate dropdown menu should be checked, telling us and Photoshop that it's enabled. To disable the plugin, select it again.

Now that we're running openbuttkiss, we are ready to send it some OSC action. We'll need to use a program that can handle sending/recieving OSC messages. Here, I'll be using [Osculator](http://www.osculator.net/ "Osculator website"), which is free and fully functional to download, minus the 20 second timeouts.

**Configuring Osculator**

> 1. Open up Osculator.
>
1. Go to `View > Parameters`, or click on the `Parameters` icon. This page stores any OSC addresses we plan on communicating to, with each address, being stored as a target. Under the `OSC URL or choice from gear menu` column, double click on the empty address slot of target #1. Type in:
>
>    `osc.udp://localhost:3333`
>
>    ![Osculator address](/images/osculator-address.png "Create a target to send our OSC messages to")
>
> 1. Click the `+` symbol located in the bottom left corner of the Parameters page. Close the Parameters page.
>
> 1. Go to `Routing > Manually Create Message...` Name it whatever you want and hit ok. A new message appears in the application window.
>
> 1. Click the empty message parameter under the `Event Type` column, select `OSC Routing` from the dropdown menu.
>
> 1. Click the empty message parameter under the `Value` column, select `Đ` from the dropdown menu.

<!-- If you have any issues with doing this, see the video I made. In it, I set up Osculator to connect to our osc server. REWORD IF INCLUDED -->

Done!

Our new OSC message should now be targeting the address we specified earlier. We should now be able to send it to Photoshop. Do this by clicking the small grey box on the left hand side of the message we just created, it should flash green.

If everything went over smoothly, when we trigger the message in Osculator, we should see the foreground and background colors swap in Photoshop. Remember that openbuttkiss must be enabled in Photoshop.

## Plugin Code

**package.json**

~~~
{
    "name": "openbuttkiss",
    "description": "osc-ps plugin",
    "version": "0.0.0",
    "main": "main.js",
    "generator-core-version": ">=1.0.0",
    "dependencies": {
        "node-osc": "0.2.1"
    },
    "devDependencies": {}
}
~~~

+ The `"main"` field tells us that main.js will be the primary entry point of the plugin.

+ The `"generator-core-version"` field tells us what version of generator we are using. Inaccurately specified version may render the plugin incompatible.

+ The `"dependencies"` field lists any dependencies that our plugin requires to run. Node-osc is the module our plugin uses to process OSC messages. As a matter of fact, if you were to look at node-osc's package.json, you'd find that node-osc requires another nodejs module, osc-min. This module was used in the original [buttkiss](https://github.com/maxenglander/buttkisshttps://github.com/maxenglander/buttkiss "our first osc-ps implementation"). Check out some other popular [nodejs OSC modules](https://nodejsmodules.org/tags/osc) - maybe you'll find one you really like!

**main.js**

~~~
var oscR = require('node-osc'),
~~~

<!-- Plural?? Retrieves. We aren't giving directions to a person. We are describing the program's direction. -->

Retrieve the logic for receiving OSC messages. The logic comes from the node-osc dependency we installed earlier, which exists in the node_modules folder of our plugin.

~~~
oscServer = new oscR.Server(3333, '127.0.0.1'),
~~~

Create an osc server and assign it to the variable oscServer. Server initialization happens when we run the plugin from terminal, or, if using the internal/built-in Generator, whenever we start up Photoshop. Once we set up a listerner, our server will be able to recieve incoming osc messages at 127.0.0.1 (localhost), port 3333.

**Don't get the oscServer mixed up with our server listener, they are two different things.**

~~~
toggler = 0,
~~~

Toggler gets assigned either a 0 or 1 based on the plugin's menu-state - whether is is checked or unchecked.

+ **0 = disabled / unchecked**
+ **1 = enabled / checked**

~~~
psToFront = "app.bringToFront();";
~~~

Switch to Photoshop application. Bring it to the front of any other applications we have open. Must be passed to _generator.evaluateJSXString().

<!-- {:.break-me} -->

~~~
var colorFlip = "var fColor = app.foregroundColor;
    app.foregroundColor = app.backgroundColor;
    app.backgroundColor = fColor;";
~~~

Swap foreground and background colors. Both colorFlip and psToFront are extendscript code (.jsx) and must be handled differently than other javascript in main.js. They are passed through _generator.evaluateJSXString(). I got stuck on this when I first tried to use extendscript in my Generator plugins - [Check out my issue at Github](https://github.com/adobe-photoshop/generator-core/issues/138 "thx Joel").

~~~
oscServer.on("message", function (msg, rinfo) {
    if(toggler==1 && msg[1]==1){
        actions();
    }
});
~~~

oscServer.on is the server's listener - it listens for incoming osc messages. The plugin must be enabled at least once for the listener to initialize. After that, the server will always be listening, choosing whether or not to execute actions() based on the conditions of the if statement. If these conditions are not met, the message will still be recieved, but nothing will be done.

There are probebly other ways in which our plugin menu-state can influence our OSC server and server listener. To enable/disable these components, in accordance to our our menu-state, might be a better alternative to our current setup.

~~~
function actions(){
    _generator.evaluateJSXString(colorFlip);
    _generator.evaluateJSXString(psToFront);
}
~~~

Called from our oscServer.on, this function executes two things, colorFlip followed by our psToFront. Both of these are passed to _generator.evaluateJSXString().

~~~
if(checked==true) {
    toggler = 1;
    console.log("OSC enabled!");
} else {
    toggler = 0;
    console.log("OSC disabled!");
}
~~~

Inside the onMenuClicked() function. Each time the plugin's menu-state changes, an if / else statement is executed, setting toggler to 0 or 1.

## Understanding OSC Messages

Node-osc gives us access to different parts of our osc message.

+ msg[0] retrieves the address of the message
+ msg[1] retrieves the argument (value) of the message

The message bubbahjubbah, sent from Osculator to openbuttkiss, could be broken down like this:

_bubbahjubbah transmits two arguments each time it is triggered_

{: .table}
|msg|msg[0]|msg[1]|
|:-:|:-:|:-:|
|[ 'bubbahjubbah', 1 ]|bubbahjubbah|1|
|[ 'bubbahjubbah', 0 ]|bubbahjubbah|0|

I'm sure node-osc and other Nodejs OSC modules allow for a more elaborate parsing of messages. I haven't fully explored Open Sound Control, and my involvment with it has not surpassed a most basic usage. The benefits of osc seem to be in its ability to develop a rich organization of flexible parameter names. Openbuttkiss keeps clear of all that. Its only interest is a message, of any address, with an argument of 1.

<!-- of any address, of any messages? -->

**For more info on Open Sound Control**

[Open Sound Control 1.0 Specification](http://opensoundcontrol.org/spec-1_0 ), [Features and Future of Open Sound Control 1.1](http://cnmat.berkeley.edu/system/files/attachments/Nime09OSCfinal.pdf ), [Control of VST Plug-ins Using OSC](http://opensoundcontrol.org/files/zbyszynski_ICMC3.pdf )

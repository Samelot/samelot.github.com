---
title: Photoshop Index Color Script
layout: blogpost
category: blog
custom_sub: ps-scripts
---

{% include injectblognav.html %}

## Intro

The Magic Wand is the key tool for extracting elements from an image. Clicking on a designated area, or, designated pixel, incorporates similarly colored pixels into a new selection. <!-- With this selection being ( something that is driven? ) derived/determined from/by our image information, we can manipulate that information to affect/influence our subsequent selections. -->

<!-- WORDING Whenever I am unhappy with my magic wand selections - usually I am work sensibly, and they  - ... -->

Whenever I find myself restricted<!-- /limited --> by the selections outputted by the Magic Wand, or if I just want to get some spontaneity in my selections, I’ll alter an image’s color information to determine subsequent selections that I make.

<!-- It is in complex detail - diverse overlapping ecosystems, or the fur of a striped tiger - where interference is likey to occur (with the Magic Wand). Sifting through such tightly woven/integrated color information, while maintaining an accurate selection, is a challenge. These probelmatic features/elements, restrict the path and locations to which a selection encompasses (may inhabit) can be altered to influences subsequent selections. Changing attributes at the image level, ie/aka the pixels themselves, provides for more drastic changes to our selections. -->  

<!-- ( It helps to know/(think about) that Photoshop is a pixel oriented/based program. As Photoshop is a pixel based program, knowing that each pixel must make up some color helps facilitate our use of the magic wand. ) -->

Sometimes I'll do this is by converting to indexed color mode, experiment with the forfeiting of color information. <!-- When going from RGB to indexed color, the dialog popup lets you specify the target number of colors to convert to; for documents containing more colors than the number of colors target in conversion, these colors will be truncated, forced to be represented as a color in the schema/template. -->Trying out dither options like diffusion, pattern, and noise can be affect what we see in our selections too. Observing the relationship between color information and magic wand selections, might help us drive/stylize the pixels of our image a certain way to influence selections we make later. I illustrate this workflow in this Photoshop script. 

<!--
boundries of this selection can be fine-tuned by/through Magic Wand's options (tolerance, anti-alias, etc), or by the Color Range and Refine Selection dialogs (screens), they do so with "some" (restricted) influence over the boundries of the selection, so in this post, I'll describe a different/another technique.

As I use this Magic Wand quite often, I no longer feel baffled by its output. 

I'd like to share some thoughts on Magic Wand use and how I've learned to tame, and even, direct the selections that I make with it.

As I use this Magic Wand quite often, I no longer feel baffled by its output; some of my selections have reached the point(?) of being save-worthy.

**altering an image's color information to determine subsequent selections that are made.**

Sifting through such tightly woven/integrated color information, targeting their respective pixels, all while maintaing a proper selection is a challenge.

While these options give "some" (restricted) influence over (the) selections output(ed) by the magic wand, changing attributes at the image level, ie/aka the pixels themselves, provides for more drastic changes to our selections.  

How we restrict colors first in our images, then use the magic wand..

... And what about the script I made to automate the process.

my thoughts and ideas:

by altering the color information of an image, we gain new further access to previous unselectable territories.

and how their boundries (which offer some submission to malleability) but only do so along some a rigid, fixed contour. This contour seems to be mostly determined by the image's color.

captured creatures creaturesque?

There are of course many great ways in which this can be done. I'd like to share one of my ways of doing this, along with a script I made to automate the process.

The images i'm interested in, for which I'd like to apply this procedure, often contain natural shapes and forms. 

These things make up our world and share all its its intricacies, its physics, are difficult to (manually) replicate; cutting out, sampling these elements is not.

use parts of that image utilize in my own work is what i want to do.

Selecting elements from images is an 
Creating and maintaing proper selections with the magic wand tool

Maintaining control over selections in Photoshop 

Maintaing selections made with the Magic Wand tool

Selections made by the Magic Wand tool can seem unpredicatble unmaintainable, untameable

can seem unresponsive and chaotic to its supplied

Listed below are the sequence of actions executed by the script. They're described with some respect/accordance to the Photoshop's menu system and UI).

Described below - with some accordance to Photoshop's menu system and UI - is the sequence of actions taken by the script.
 -->


## Overview of Photoshop Script

Described below is the sequence of actions taken by the script.

> 1. Set image mode to indexed ( `Image > Mode > Indexed Color...` ) 
>
> 1. Go into the color table ( `Image > Mode > Color Table...` ) and replace the first 3 colors with the colors: red, green, and blue.
>
> 1. Set mode back to RGB ( `Image > Mode > Indexed Color...` ).
>
> 1. For each color (red, green, blue), make a selection of that color. If a selection exists, create a new Photoshop layer and fill it with a shade of grey. Using the current selection, add a mask to each layer. 
>
> _If a selection can't be made from one of our colors (added into our color table), a layer will not be made. The script then moves onto the next color._


![RicePatty1 normal](/images/ps-indexcolor-blogpost/RicePatty1_1norm.png "original image")
![RicePatty1 rgb](/images/ps-indexcolor-blogpost/RicePatty1_2rgb.png "rgb values added to color table")
![RicePatty1 grey](/images/ps-indexcolor-blogpost/RicePatty1_3grey.png "new layers with masks")

![RicePatty2 normal](/images/ps-indexcolor-blogpost/RicePatty2_1norm.png "original image")
![RicePatty2 rgb](/images/ps-indexcolor-blogpost/RicePatty2_2rgb.png "rgb values added to color table")
![RicePatty2 grey](/images/ps-indexcolor-blogpost/RicePatty2_3grey.png "new layers with masks")

<!-- The new colors are chosen from the array defined at the top of the script. As of this time, I have only created three possible colors within the array. If you'd like to increase the number of colors (colors to select), you will have to (please add) add new colors to the array. -->


<!-- NOTE TO SELF: It seems as though tolerance and changing the image information (through indexed color) can yield very similar changes in selection output. -->

<!-- FROM ONLINE: select pixels in an image based on shapes or by detecting object edges, the Magic Wand selects pixels based on tone and color. -->

## Code

~~~
#target photoshop
var doc = app.activeDocument;
var fuzz = 200;
var shadeNumber = 3;
var shadeValue = 110;

var colorRange = new Array();
colorRange[0] = {
    red: 255,
    green: 0,
    blue: 0
};
colorRange[1] = {
    red: 0,
    green: 255,
    blue: 0
};
colorRange[2] = {
    red: 0,
    green: 0,
    blue: 255
};

function convertToDuotone() {
    // write options for indexed color mode here 
    myObject = new IndexedConversionOptions();
    myObject.palette = Palette.LOCALADAPTIVE;
    // how many colors to split/filter the image into
    myObject.colors = shadeNumber;
    doc.changeMode(ChangeMode.INDEXEDCOLOR, myObject);
};

function convertToRGB() {
    doc.changeMode(ChangeMode.RGB);
}

function setColorTable() {
    var idsetd = charIDToTypeID("setd");
    var desc20 = new ActionDescriptor();
    var idnull = charIDToTypeID("null");
    var ref6 = new ActionReference();
    var idClr = charIDToTypeID("Clr ");
    var idClrT = charIDToTypeID("ClrT");
    ref6.putProperty(idClr, idClrT);
    desc20.putReference(idnull, ref6);
    var idT = charIDToTypeID("T   ");
    var list1 = new ActionList();
    for (var i = 0; i < shadeNumber; i++) {
        var tableColor = new ActionDescriptor();
        var idRd = charIDToTypeID("Rd  ");
        tableColor.putDouble(idRd, colorRange[i]['red']);
        var idGrn = charIDToTypeID("Grn ");
        tableColor.putDouble(idGrn, colorRange[i]['green']);
        var idBl = charIDToTypeID("Bl  ");
        tableColor.putDouble(idBl, colorRange[i]['blue']);
        var idRGBC = charIDToTypeID("RGBC");
        list1.putObject(idRGBC, tableColor);
    }
    desc20.putList(idT, list1);
    var idTrnI = charIDToTypeID("TrnI");
    desc20.putInteger(idTrnI, shadeNumber);
    executeAction(idsetd, desc20, DialogModes.NO);
}


convertToDuotone();

setColorTable();

convertToRGB();

doc.selection.deselect();


for (var i = 0; i < 3; i++) {

    var selectColour = new SolidColor();
    selectColour.rgb.red = colorRange[i]['red'];
    selectColour.rgb.green = colorRange[i]['green'];
    selectColour.rgb.blue = colorRange[i]['blue'];

    var scaleA = (selectColour.lab.a + 128.5) / 256;
    var desc11 = new ActionDescriptor();
    desc11.putInteger(charIDToTypeID("Fzns"), fuzz);
    var desc12 = new ActionDescriptor();
    desc12.putDouble(charIDToTypeID("Lmnc"), selectColour.lab.l);
    desc12.putDouble(charIDToTypeID("A   "), selectColour.lab.a + scaleA);
    desc12.putDouble(charIDToTypeID("B   "), selectColour.lab.b + scaleA);
    desc11.putObject(charIDToTypeID("Mnm "), charIDToTypeID("LbCl"), desc12);
    desc11.putObject(charIDToTypeID("Mxm "), charIDToTypeID("LbCl"), desc12);
    executeAction(charIDToTypeID("ClrR"), desc11, DialogModes.NO);
    if (hasSelection() == true) {
        var layerRef = doc.artLayers.add();

        var desc140 = new ActionDescriptor();
        desc140.putClass(charIDToTypeID('Nw  '), charIDToTypeID('Chnl'));
        var ref51 = new ActionReference();
        ref51.putEnumerated(charIDToTypeID('Chnl'), charIDToTypeID('Chnl'), charIDToTypeID('Msk '));
        desc140.putReference(charIDToTypeID('At  '), ref51);
        desc140.putEnumerated(charIDToTypeID('Usng'), charIDToTypeID('UsrM'), charIDToTypeID('RvlS'));
        executeAction(charIDToTypeID('Mk  '), desc140, DialogModes.NO);
        doc.activeChannels = [doc.channels[0], doc.channels[1], doc.channels[2]]

        var myColor = new SolidColor();
        myColor.rgb.red = myColor.rgb.green = myColor.rgb.blue = shadeValue;
        app.activeDocument.selection.selectAll();

        app.activeDocument.selection.fill(myColor, undefined, undefined, true);
        app.activeDocument.selection.fill(myColor);
        
        if ((shadeValue+36) > 255)
        {
            shadeValue = 255;
        } else {
            shadeValue += 36;
        };
    };
};

function hasSelection() {
    var ref = new ActionReference();
    ref.putEnumerated(charIDToTypeID("Dcmn"), charIDToTypeID("Ordn"), charIDToTypeID("Trgt"));
    var docDesc = executeActionGet(ref);
    var hasSelection = docDesc.hasKey(stringIDToTypeID("selection"));
    return hasSelection;
};

doc.selection.deselect();
~~~
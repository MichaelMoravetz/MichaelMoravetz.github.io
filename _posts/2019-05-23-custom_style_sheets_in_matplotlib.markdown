---
layout: post
title:      "Custom Style Sheets In Matplotlib"
date:       2019-05-23 14:15:20 -0400
permalink:  custom_style_sheets_in_matplotlib
---

*Pointers for creating attractive plot styles that can be easily reused.*

***

There are a lot of great styles already provided between matplotlib and seaborn. But, sometimes the right style just isn't provided for your needs at hand.  In that situation you have a couple options.  First, you can manually pass in every parameter for each customization you want to employ.  For more complicated stylings, this can get time consuming and take quite a bit of code in a notebook that makes presenting to a non technical audience unnecesarily complicated. If you are considering using a style more than once, I recomend getting familar with Matplotlib's custom style sheets.  Once familiar with the layout, using a custom style sheet can be an easy way to save a lot of time and use unique stylings without having to remember every parameter.  Therefore this post will focus on making a custom style sheet and not on creating temporary stylings, because a style sheet can easily be used to do that as well as be used with other style sheets to cerate a more complex style.

Matpltlib allows you to control the defaults of almost every property in matplotlib.  This includes basic properties such as color maps and line properties as well as animation and keymap properties for interacting with the plot using event keys.  With a little patience,  most desired styles can be achieved and more importantly saved for easily using again.

### Getting Started

To start, I recomend getting acquainted with the documentation on this subject which can be found at this link: 
>[https://matplotlib.org/tutorials/introductory/customizing.html].

**Things to remember** :

Color is a fun and important property that allows to make plots both nicer to look at and easier to interpret.  Matplotlib support the use of xkgb colors from the color survey and will accept those names as well as standard rgb, and hex codes.  When starting out, it was easy to get carried away, having a lot of fun with different color selections.  It is important to keep a few things in mind before deciding on the final selection for color maps, and all other plot color properties.  

**When Selecting Color Properties** :

- Keep in mind the purpose of this style sheet.  Will this style be used for plots in a notebook, a presentation or will it be printed?  While certain colors may look good in comparison in your local notebook environment or jupyter theme in a notebook, in other circumstances it could become hard to read and a bit of an eye sore.  Choosing color that either work on a dark or light colored background will make them more versatile. Otherwise, the style will be best used for particular circumstances. ie. a dark themed plot can be great for a presentation slide show, but not at all ideal fro printing. Similarly a yellow font color will look great in the dark theme of your local environment, but when pushed to github, the them will be defaulted rendering the yellow font illegible on a light background.
- It is always best to use colors that are visible for people who are colorblind.  Using colors that are not colorblind safe, can lead to people interpretting data differently than those who are not afflicted.  I recomend taking a look at a reference that can render colors as somebody with colorblindness would see them. An interactive tool for this is located here: 
>https://davidmathlogic.com/colorblind/#%23D81B60-%231E88E5-%23FFC107-%23004D40
- Choose a safe defalt color map.  The cycler function in matplotlib cycles through colors while plotting to show different colors for different features.  Having a unique color map can really make your plots stand out, but it is important to default to a general purpose categorical palette.   While a color palette can be easily changed when plotting, generally, having distinctly different colors that work well together will fill many plotting needs.  However, if what you are styling is a heat map, it can be useful to default to a diverging or sequential color map.  Remember that humans interpret different color values and saturation sequentially and this has a large effect on interpretation.  There are many options for custom palettes and seaborn has a tool for manipulating palettes. If you are ultra ambitious, you can create youre own custom palettes for whatever needs you have. A good starting place for selecting a default color map is with matplot libs provided color maps, check out the documentaion here:
>https://matplotlib.org/users/colormaps.html


Remember that style sheets are designed to be composed together.  The style sheet last called will over write previously styled configurations.  Being aware of what each sheet does can eliminate a lot of confusion.  Using different sheets for configuring different elements is a good way to build complimentary styles yet maintain unique differences and maintain control of configurations.  For example, a sheet that only configures color, and a sheet that only configures  line elements could easily be used together. Later on, the same color sheet could be used with a different set of line configurations and this would eliminate the need to rewrite the same code in every file used.  This can create an attractive general style theme while implementing minor subtle changes for individual plotting needs.

Often, less is more. Building off of default configurations can be a good start. As well as slighty modifying other style sheets for your needs.   Keeping things simple can make a wide range of plotting easy and good for interpretation.  Dont forget the point of a plot is to visualize data, feel free to show your creative side but keep the goal of your plotting in mind and have fun! Custom plotting styles are a nice way to show a little personality while making a coherent set of plots that go well with your presention theme, school colors or your businesses brand.



There are three main steps to creating and implementing your own style sheet:
1.  Defining your style
2.  Creating a file or url for that style sheet to live in.
3.  Calling the style to be used for either changing the current rc settings or for temporarily using the style sheet.

## Defining You First Style Sheet

Take some time to  get familiar with matplot libs rc prameters and default stylings.  This can be done by throoughly reading through documentation or going directly to a style sheet and viewing its contents in its directory.  I would recommend finding a style with some of the settings you like ande comparing them to the default settings as a way o get comfortable with what each parameter is actually doing.

A conveneniant tool for viewing the currect rc params in a notebook setting is by calling the following function.  It will display a list of all the current parameters that are being used in your notebook.

```
# import the plotting library
import matplotlib.pyplot as plt

# View all current config settings
plt.rcParams

>>>This will display a dictionary with the keys as the parameters and the values as the current configurations set.  This >>>is a useful tool for exploring the current settings are.

# view all styles available with the following function
print(plt.style.available)

>>> prints a list with all style names

```


The ideal way to create a stule sheet is to get ahold of a style sheet template from the source at the link above. Or, you can find a copy of it on your system in 'site-packages/matplotlib/mpl-data/matplotlibrc'.  Using  this template in a text editor is an easy way of writing  a proper py file that you can store in your mpl style library.  The template provided has all of the paramters you could configure with some descriptions.  The lines are all hashed out which makes it easy to implement changes, un-comment them, and run them to see the effects.  Building a new style similar to a previously defined style is as easy as copying the file, uncommenting or commenting out specific lines and changing certain paramters to your needs.   Playing around with a file like this is another good way to get familiar with the effects of individual parameters.

A portion of the style sheet template provided by matplotlib looks like this:

```
#### LINES
## See http://matplotlib.org/api/artist_api.html#module-matplotlib.lines for more
## information on line properties.
#lines.linewidth   : 1.5     ## line width in points
#lines.linestyle   : -       ## solid line
#lines.color       : C0      ## has no affect on plot(); see axes.prop_cycle
#lines.marker      : None    ## the default marker
#lines.markerfacecolor  : auto    ## the default markerfacecolor
#lines.markeredgecolor  : auto    ## the default markeredgecolor
#lines.markeredgewidth  : 1.0     ## the line width around the marker symbol
#lines.markersize  : 6            ## markersize, in points
#lines.dash_joinstyle : round        ## miter|round|bevel
#lines.dash_capstyle : butt          ## butt|round|projecting
#lines.solid_joinstyle : round       ## miter|round|bevel
#lines.solid_capstyle : projecting   ## butt|round|projecting
#lines.antialiased : True         ## render lines in antialiased (no jaggies)

## The three standard dash patterns.  These are scaled by the linewidth.
#lines.dashed_pattern : 3.7, 1.6
#lines.dashdot_pattern : 6.4, 1.6, 1, 1.6
#lines.dotted_pattern : 1, 1.65
#lines.scale_dashes : True

#markers.fillstyle: full ## full|left|right|bottom|top|none

```

## Create File and Directory For Storing Your Style Sheet

Once a style sheet has been defined, it needs to be properly stored so it can be accessed from within a jupyter notebook or python file.   This blog will focus on the first option, but there are alternative approaches.  The first is store the style_sheet.py file in the approprate directory in matplotlib.  A second option is to use a url. You can then call the style by using either the file path or the url.  However, a more ideal way of dealing with this is to give your style a name like the provided styles provided with matplotlib.  Here are the steps for storing a style sheet in the appropriate directory and giving it a name so it can be easily called when needed.

1. Locate the configuration directory and stylelib like so:


```
import matplotlib as mpl

# Locate the file where matplotlib is installed
mpl.__file__

>>> prints path of directory


# use matplotlibs function for locating where the directory is on you computer
mpl.get_configdir()

# Different for Macs and Windows, locate the cache directory with this function
mpl.get_cachedir()

```

The output for  ```mpl.get_configdir()``` will display something like this by default: ```~/.config/matplotlib```.
In some situations, you may have to create a ```mpl_configdir/stylelib``` directory.  Inside of that file put the style sheet with the name of the file followed by ```.mplstyle```. Boom, you now have a custom style sheet which can be called whenever you desire.

the file path will look something like this:

```
~/.config/matplotlib/mpl_configdir/stylelib.your_custom_style_sheet.mplstyle
```


## Using Your Style Sheet

At this point the hard part has been done.  To use in a jupyter notebook for example, simply  fire up a new notebook and run the following code:

```
import matplotlib.pyplot as plt

# Set default style for the whole notebook
plt.style.use('your_custom_style_sheet')

# Set temporary styling using your new style
with plt.style.context('your_custom_style_sheet'):
         plt.plot(x, y)
plt.show()

```

Using complimentary style sheets can be done in the same manor, with the difference of passing a list of styles to the same function as above.

```
import matplotlib.pyplot as plt

# Use your new style with another style, note that styles farther to the right on the list will overwrite valuies already defined to the left

plt.style.use( ['dark_background', 'your_custom_style_sheet', 'your_custom_font_style_sheet'])
plt.plot(X, y)
plt.show()
```

I hope this was useful.  Although not covered in great detail here, much of the work will be getting a solid understanding of the different configurations.  For me that was the fun part of the project.  For whatever reason figuring out out to save a custom style was a little daunting, but once you have done that with these steps, many great custom styles can easily be made on the fly whenever necesary.  As time goes by, you'll find that you can easily streamline mixing styles and creating a unified theme amongst your plots.  Have fun and Good Luck!

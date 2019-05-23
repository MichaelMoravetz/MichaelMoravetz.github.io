---
layout: post
title:      "Custom Style Sheets In Matplotlib"
date:       2019-05-23 18:15:19 +0000
permalink:  custom_style_sheets_in_matplotlib
---


There are a lot of great styles already provided between matplotlib and seaborn. But, sometimes the right style just isn't provided for your needs at hand.  In that situation you have a couple options.  First, you can manually pass in every parameter for each customization you want to employ.  For more complicated stylings, this can get time consuming and take quite a bit of code in a notebook that makes presenting to a non technical audience unnecesarily complicated. If you are considering using a style more than once, I recomend getting familar with Matplotlib's custom style sheets.  Once familiar with the layout, using a custom style sheet can be an easy way to save a lot of time and use unique stylings without having to remember every parameter.  Therefore this post will focus on making a custom style sheet and not on creating temporary stylings, because a style sheet can easily be used to do that as well as be used with other style sheets to cerate a more complex style.

To start, I recomend getting acquainted with the documentation on this subject which can be found at this link: 
https://matplotlib.org/tutorials/introductory/customizing.html.

There are three main steps to creating and implementing your own style sheet:
1.  Defining your style
2.  Creating a file or url for that style sheet to live in.
3.  Calling the style to be used for either changing the current rc settings or for temporarily using the style sheet.

# Defining You First Style Sheet
Take some time to  get familiar with matplot libs rc prameters and default stylings.  This can be done by throoughly reading through documentation or going directly to a style sheet and viewing its contents in its directory.  I would recommend finding a style with some of the settings you like ande comparing them to the default settings as a way o get comfortable with what each parameter is actually doing.

A conveneniant tool for viewing the currect rc params in a notebook setting is by calling the following function.  It will display a list of all the current parameters that are being used in your notebook.

```
# import the plotting library
import matplotlib.pyplot as plt

plt.rcParams

```
This will display a dictionary with the keys as the parameters and the values as the current configurations set.  This is a useful tool for exploring what the current settings are.

To be continued...

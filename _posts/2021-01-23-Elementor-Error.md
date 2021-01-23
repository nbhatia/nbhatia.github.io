---
layout: post
title: "How to get rid of the ‘Preview Could Not Be Loaded’ Error in Elementor"
author: "Naveen Bhatia"
comments: true
tags: [Elementor, Troubleshooting]
---

The other day when I signed into my Wordpress website's admin console, and tried to edit a page in Elementor, it just refused to open and I got this message:


![Preview-Error](assets/imgs/preview-error.png)


Seems like a very familiar problem many of us have faced. The first time I got it, I had no idea why I was getting this error or what could be causing it. Initial course of action is to always try to refresh the window a few of times, or try to open the editor in an Incognito window of the browser (to rule out any browser caching issues) but these steps usually don't help and the error persists.   I suspected it to be an issue with the latest version of Elementor that I had updated to recently. I downgraded Elementor, but that also did not help. 

## Safe Mode

So, how do we fix this error? 

This error usually comes when you have installed either a plugin (most likely) or a theme that conflicts with the normal working of Elementor. As the dialog on the bottom right of this screen suggests, as a first step towards getting the Elementor editor back is to 'Enable Safe Mode'. 

When loading in Safe Mode, Elementor disables all the plugins and the active theme (as per Elementor documentation) and tries to load the Editor on a clean slate. 

When the editor has loaded successfully in the Safe Mode, the plugins get enabled back again. (I say this because when I faced this issue and I loaded Elementor in Safe mode, the editor loaded successfully, but when I checked the plugins, all my plugins were still enabled.). So if you're able to load your Elementor editor in Safe mode, you can bet it's one of the plugins causing this issue - for me that's been the case most of the times. 

Once your Elementor editor has loaded in Safe mode, you should now see your normal editor screen. And In the bottom right corner of your screen, you should now also see this message:


![Safe-Mode](assets/imgs/safe-mode.png)


This tells you that the editor is running in Safe mode and you should try to fix the issue that is preventing the editor to load in normal mode, before editing your page.

## Isolating The Culprit

Follow this **7 step process** to identify the culprit plugin and get your Elementor working normally again:

1. Open your site's dashboard in a new browser window/tab and go to plugins and **disable all plugins** except Elementor. 
   
2. Now go back to your Elementor editor page and click on the '**Disable Safe Mode**' button - it would disable the safe mode and try to load the Editor again. The editor should load successfully this time because all the plugins have been manually disabled. 
   
3. Now you should try to **enable plugins one by one**. I would suggest enabling plugins in the 'most recently installed' order. Just select a plugin and enable it.
   
4. Now go back to the editor screen and **refresh** the browser using CMD/CTR + R.
   
5. If you get the 'Preview could not be loaded' message again - this is the plugin that's been messing with your Elementor. You should go ahead and **de-activate** (and delete!) it now.
   
6. Else if the editor loads successfully, it means that the plugin you just enabled is not the one standing in Elementor's way. You should now enable one more plugin and **repeat** the process from steps 3 through 6, till you've enabled all the plugins one by one.
   
7. By the time you enable the last plugin, you would have already identified the culprit - in my case it was the 'WP Editor.md" plugin. You should try to replace it with another plugin offering similar functionality.

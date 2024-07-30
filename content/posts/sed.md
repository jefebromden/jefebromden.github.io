---
title: "Sed"
author: "JefeBromden"
date: 2024-07-30T10:02:47-03:00
draft: true
---

## Sed: Making small changes to the configuration
The following task can be easily be accomplished with any UI Text Editor. However, it will be convenient if we can make atomic changes to the configuration, that means, to some parameter values, without the need to manually edit the file.

All this setup and configuration can be easily be made via a script, so if we need to make more than one site, but with different looks or value names, we just run the script giving those values.

What is *Sed*? On Linux, you can read the *Man Page* by running `man sed`. This is what it says:
> Sed is a stream editor. A stream editor is used to perform basic text transformations on an input stream [...]

That means that you can edit a text file by running a command, without the need of a *UI*. But, without further introduction, let's put our hands on.

He have now, this configuration file (`hugo.yml`):
```bash
baseURL: http://example.org/
languageCode: en-us
title: My New Hugo Site
theme: LoveIt
```
Which contains four keys and its respective values separated by a colon. If you want to change the title, you just have to run this command:
```bash
sed -i '/^title/ s/My*.*Site/Cookbooks/' hugo.yml
```
The *title* key is gonna set the browser's tab title of the site. I don't want to shift the focus from this guide, if you want a different title, just change *Cookbooks* with whatever title you want. I'll being adding a Sed article soon since it is an important tool. Also I'll be adding *RegEx*, which is a concept needed be able to use it.


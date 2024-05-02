---
title: "Firefox Addon Basics"
author: "Rosendo Ayala"
date: 2024-05-01T10:38:02-03:00
draft: true
---

# Firefox Addon Basics
The following knowledge is assumed:
- Bash interpreter. Althought is just used to create directories and plain text files to simplified this cookbook. You can use a file explorer and any text editor
- The syntax of the following formats:
  - `json`
  - `javascript`
  - `HTML`
  - `CSS`

## Install
### Create an Addon
If you have an extension already, you can skip to the next subseccion. This addon doesn't do anything, it's just a proof of concept
```bash
mkdir ~/link-extractor && pushd $_
cat > manifest.json << EOF
{
    "manifest_version": 2,
    "name": "Link Extractor",
    "version": "1.0",
    "description": "Simple link extractor for whatever.com",
}
```

This is the `Addon`, everything you can to add to it goes between those two brakets

### Load the Addon
1. On the browser, go to `about:debugging`
2. On the left, go to `This Firefox`
3. Click on `Load Temporary Add-on...`
4. On the `Open box`, select the `manifest.json` file you created

Now that the addon is created, when you make a change on the extension, you have to come back to this page and click on `Reload`

## Cookbook
Every keyword on the `manifest.json` file are called `shotcut`
Before adding a shortcut, don't forget to add a colon after the last line on the `json` file

### Adding Functionality
There are four ways to add functionality to an addon. Some has its own shortcuts:
- `background_script`: Is a single long-running process. You can use them, for example, to collect some statistics on the sites you visit

- `Content Scripts`: Executed on a specific pages. You can use the DOM to read details and make changes on the page, like styling. You can inser this kind of script in two ways:
  - Injecting a script from a `Background Script`[1]
  - Using the `content_script` shortcut

- Using a `browser_action` popup. It gets terminated when it is closed[2]. With this option, you can call the script by:
  - Adding a `listener` on the background script
  - Using a `script tag` on the popup itself

#### Using the `content_script` shortcut
- Point to the `javascript` script you want to execute on the page
- Tell the extension in which pages it should execute them
  - `"<all_urls>"`: Matches any page
  - `"*://*.example.com/*"`: Matches `example.com`

```json
"content_scripts": [
    {
        "matches": ["<all_urls>"],
        "js": ["main.js"],
    }
]
```

Then, create the script, if you haven't already:
```javascript
// main.js
// This oneliner draws a red border around the page body
document.body.style.border = "5px solid red";
```

Finally, come back to `about:debbugin`, reload the extension and check the results. In this case every page `body` should have a red border

[1]: [Injecting a script from a background script](https://radu-dita.medium.com/chrome-extension-when-to-use-content-scripts-and-injected-scripts-238563dce8af)
[2]: [A WebExtension Guide](https://dev.to/christiankaindl/a-webextension-guide-36ag)
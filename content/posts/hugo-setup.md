---
title: "Hugo Cookbook"
author: "JefeBromden"
date: 2024-03-03T13:07:49-03:00
draft: false
---

# How to Create a Basic Site With Hugo
## Creating a New Site
To simplify this guide, I will use the command to install on my current platform, *Fedora Linux*. You can check the Web for the corresponding command on your platform. Hugo is multi-platform, so the rest of the commands should work.
```bash
# Install (Fedora)
dnf install hugo

# Check version
hugo version

# Create new site
hugo new site cookbooks
```

This will create the basic directory structure of the site. On *Linux*, you can check it out with the `tree` command:
```bash
tree cookbooks
.
├── archetypes
│   └── default.md
├── assets
├── hugo.toml
├── content
├── data
├── layouts
├── public
├── static
└── themes
```

## Choosing Theme
By default, *Hugo* comes with no *HTML* pages, not even a basic one. If you try to run the site now, you'll get a *Page Not Found* page.

To render content, *Hugo* uses *Themes*, which contains the necessary *HTML* code, *CSS* style and other assets like images and *Javascript* to show your content.

You have two ways to add a *Theme*. You can write your own from scratch, or you can use one that somebody else write for you. To simplify this guide, we'll be using an already made theme.

There are plenty out there. The problem with most of them is that they use `Node`. I  don't have any problems with `Node`, but when you're trying to host a simple *Static Site*, it feels really bloated.

To find a *minimalistic* theme, you can search the Web and try one by one (what I did), or you can you use GitHub Search with `NOT` operator: `hugo theme NOT node`. Try to sponsor the project you choose whenever you can.

I choose [LoveIt](https://github.com/dillonzq/LoveIt) for various reasons:
- It focus on writing.
- It *has* good documentation. You can check it out [here](https://www.hugoloveit.com)
- Simple.
- Easy to use.
- Visually attractive.
- It has support for multi-language.

## Creating Git Repository
With most themes, you have two ways to use them, with a *Hugo Module*, or a *Git Submodule*. To use it as a *Hugo Module* you may have to eventually learn basic *Go* syntax, and that will shift the focus from this guide.

Since I'll be using *Git* to save the project, we'll use a submodule.
```bash
# Create repository
cd cookbooks
git init
```
You don't have to know to use *Git* to make you're site, you just need two commands to add the theme. You do need it to deploy it. Make sure you *bookmark* this page, I'll be adding a basic introduction to *Git Workflow* to run you own blog. 

## Adding Theme
To use the theme on your site, you have to run a command to add it as a *Git* submodule and add a single line to the configuration file to tell *Hugo* to use it.
```bash
# Add submodule. Change URL and destination directory accordingly to your theme
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt

# Set the theme on Hugo's configuration.
# On versions prior to Hugo 0.109.0, configuration file
# is `config.toml`, change name accordingly
echo "theme = 'LoveIt'" >> hugo.toml
```

## Starting Server
To show the site on the browser, *Hugo* uses a *Live Server*, which will be watching changes made on your site's directory structure and files. If a change is made, it will be reflected on the browser.
```bash
# Start webserver
hugo server
```
Now you can go to `http://localhost:1313` on the browser and see the default look of *LoveIt* theme.

As the output of the *Live Server* says, you have to press `Ctrl+C` to stop it.

You won't be able to use the current terminal tab until you stop the server. To run commands from now on, you'll have to open a new one. You can use the mouse, or, if you're a keyboard freak like me, you can use the keyboard.

The *Shortcut* may change depending on you're platform. On *Gnome Terminal*, the one I'm using, and in most terminals on *Linux* is `Ctrl+Shift+T`.

## Convert TOML Configuration to YAML
The default format for configuration in Hugo is *TOML*, which looks for example, like this:
```toml
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = 'LoveIt'

[markup]
  [markup.goldmark]
    [markup.goldmark.parser]
      wrapStandAloneImageWithinParagraph = false
```

The same configuration in *YAML*, on the other hand, looks like this:
```yaml
baseURL: http://example.org/
languageCode: en-us
title: My New Hugo Site
theme: LoveIt

markup:
  goldmark:
    parser:
      wrapStandAloneImageWithinParagraph: false
```
Which is more clearer. For a more detailed explanation on why *YAML* is a better format, [read this article](https://www.brycewray.com/posts/2023/05/organizing-hugo-configuration/)

To convert to *YAML*, on *Linux*, you can use [Yq](https://github.com/mikefarah/yq).

Open a new tab on your terminal emulator:
```bash
# Install (Fedora)
dnf install yq

# Check version
yq --version

# Convert
# Since Hugo 0.109.0, configuration changed from `config.toml` to `hugo.toml`
# `-oy` option specifies output format (*YAML*)
yq -oy config.toml > hugo.yml

# Rename old configuration file so Hugo won't read it
mv config.toml _config.toml

# Make a small change for testing purposes
# This is gonna set browser's tab title
sed -i '/^title/ s/My*.*Site/Cookbooks/' hugo.yml
```
After creating a new configuration, you're gonna have to restart server.

Come back to the tab where it is running, press `Ctrl+C` to stop it, and then run `hugo server` to start it

## Use Directory Structure for Configuration
Configuration files tend to get messy when customization grows. You can divide 
If the configuration is working correctly, you can delete old configuration file

## Why You Need Leaf Bundles
Open a new tab on your terminal emulator
```bash
# Create About Page
hugo new about.md

# Add content
echo 'About me' >> content/about.md

# If the content gets too big, you can
# move a page to a Leaf Bundle,
# so you can divide content in pages
mkdir content/about
mv content/about.md content/about/index.md
```
For a more in depth explanation of Leaf Bundles go to [GoHugo.io](https://gohugo.io/content-management/page-bundles/#leaf-bundles)

## Menu Entries For Navigation
To be able to access a new section, in this case, a new Bundle for About page, you have to add a button to the interface.

Menu entries can be specified in various ways, you can read about it [here](https://gohugo.io/content-management/multilingual/#menus). We're gonna use a *configuration directory* because is the most simple and comfortable way on the long run, it allow us to centralize your preferences and divide it on different files if necessary
```bash
# Create directory structure
mkdir -pv config/_default

# Move configuration to new location
mv config.toml config/_default/

# Create menu entry
cat > config/_default/menus.yml << EOF
- name: About
  pageRef: /about
  weight: 1
EOF
```
`name` and `pageRef` are self explanatory. `weight` field refers to priority in menu hierarchy. A lowest number will appear to the left on [LTR](https://en.wikipedia.org/wiki/Writing_system#Directionality) languages

## Multilanguage
```bash
# If you write content in more than one language,
# use language codes on file names for i18 compatibility
hugo new about/index.es.md
pushd content/about
mv index.md index.en.md
popd
```
Read [Hugo Documentation](https://gohugo.io/content-management/multilingual/#configure-languages) for more info on multilanguage

## Deploy to GitHub
```bash
# Add a .gitignore file to exclude unnecessary directories and files when building 
cat > .gitignore << EOF
/public/
/resources/_gen/
.hugo_build.lock
EOF
```

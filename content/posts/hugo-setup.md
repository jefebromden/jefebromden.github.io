---
title: "Hugo Cookbook"
author: "JefeBromden"
date: 2024-03-03T13:07:49-03:00
draft: false
---

# Hugo Cookbook
## Quickstart
The minimal commands to build the site are highlighted
{{< highlight bash "hl_lines=2 8-9 12-14 24" >}}
# Install (Fedora)
dnf install hugo

# Check version
hugo version

# Create new site
hugo new site cookbooks
pushd cookbooks

# Add theme
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
echo "theme = 'LoveIt'" >> config.toml

# Add a .gitignore file to exclude unnecessary directories and files when building 
cat > .gitignore << EOF
/public/
/resources/_gen/
.hugo_build.lock
EOF

# Start webserver
hugo server         # Web Server is available at http://localhost:1313
hugo server -D      # Build drafts
hugo server -p 1314 # Change default port
{{< / highlight >}}

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

# Use Directory Structure for Configuration
If the configuration is working correctly, 

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

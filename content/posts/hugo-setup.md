---
title: "Hugo Cookbook"
author: "JefeBromden"
date: 2024-03-03T13:07:49-03:00
draft: false
---

# Hugo Cookbook
## Quickstart
The minimal commands to build the site are highlighted
{{< highlight bash "hl_lines=2 8-9 12-13 15 26" >}}
# Install (Fedora)
dnf install hugo

# Check version
hugo version

# Create new site
hugo new site renaissance
pushd renaissance

# Add theme
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
# Since Hugo 0.109.0, configuration changed from `config.toml` to `hugo.toml`
echo "theme = 'LoveIt'" >> hugo.toml

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
`name` and `pageRef` are self explanatory. `weight` field refers to priority in menu hierarchy. A lowest number will appear to the left on LTR (Left To Right) languages

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

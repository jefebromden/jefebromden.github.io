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

# Create About Page
Open a new tab on your terminal emulator
```bash
# Create About Page
hugo new about.md

# Add content
echo 'About me' >> content/about.md

# Move About Page to a Leaf Bundle
mkdir content/about
mv content/about.md content/about/index.md
```

---
title: "Hugo Cookbook"
author: "JefeBromden"
date: 2024-03-03T13:07:49-03:00
draft: false
---

# How to Create a Basic Site With Hugo
## Themes: Choosing How Your Site Will Look
By default, Hugo comes with no *HTML* pages, not even a basic one. If you try to run the site using defaults, you'll get a *Page Not Found* page.

To render content, Hugo uses *Themes*, which contains the necessary HTML code, *CSS* style and other assets like *images* and *Javascript* to show your content.

You have two ways to add a theme. You can write your own from scratch, or you can use one that somebody else write for you. To simplify this guide, we'll be using an already made theme.

There are plenty out there. The problem with most of them is that they use *Node*. I  don't have any problems with Node, but when you're trying to host a simple *Static Site*, it feels really bloated.

To find a *minimalistic* theme, you can search the Web and try one by one (what I did), or you can you use GitHub Search with `NOT` operator: `hugo theme NOT node`. Try to sponsor the project you choose whenever you can.

<!-- TODO: Add a more detailed explanation on how to use GH Search -->

I choosed [LoveIt](https://github.com/dillonzq/LoveIt) for various reasons:
- It focus on writing.
- It has good documentation. You can check it out [here](https://www.hugoloveit.com)
- Simple.
- Easy to use.
- Visually attractive.
- It has support for multi-language.

Take note of the repository *URL*, we will need it later.

## Installing Hugo
To simplify this guide, I will use the command to install on my current platform, *Fedora Linux*. You can check the Web for the corresponding command for you. *Hugo* is multi-platform, so the rest of the commands should work.
```bash
# Install (Fedora)
dnf install hugo

# Check version
hugo version
```

## Creating a New Site
```bash
# Create new site
hugo new site cookbooks

# Enter site directory
cd $_
```
> On *Bash* shell, `$_` variable holds the last argument of the last command. In this case, *cookbooks*.

The `new site` command will create the basic directory structure of the site. On *Linux*, you can check it out with the `tree` command:
```bash
tree
.
├── archetypes
│   └── default.md
├── assets
├── config.toml
├── content
├── data
├── layouts
├── public
├── static
└── themes
```

## Cat: Giving a Quick Look to the Configuration File
When you are on the terminal, sometimes you need to take a look at the content of a file. You can, of course, use a *Terminal Editor*, like *Nano*, or a *UI Text Editor*.

The truth is, *Context Switching* is not good to focus our attention, because, we humans, [are not good at Multitasking](https://www.youtu.be/iM4u-7Z5URk). It's better to separate tasks we can do on a terminal, from those we should be doing on a *UI*.

On Linux, we have a tool for that, the `Cat` command. To show the content of Hugo's configuration, run the following command:
```bash
cat config.toml
```
This is what you will see:
```toml
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
```
The basic configuration consists of three keys and their respective values, separated by an equal sign:
- *baseURL*: *URL* in which your site will be shown.
- *LanguageCode: [RSS Language Code](https://www.rssboard.org/rss-language-codes) used as default language for your site.
- *title*: It sets the browser's tab title of the site.

Of course, ass configuration file gets bigger, `cat` command isn't the better choice, but that's something we'll cover on other section

## Convert TOML Configuration to Yaml
The default format for configuration in Hugo is *TOML*, which looks for example, like this:
```toml
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'

[markup]
  [markup.goldmark]
    [markup.goldmark.parser]
      wrapStandAloneImageWithinParagraph = false
```

The same configuration in *Yaml*, on the other hand, looks like this:
```yaml
baseURL: http://example.org/
languageCode: en-us
title: My New Hugo Site

markup:
  goldmark:
    parser:
      wrapStandAloneImageWithinParagraph: false
```
Which is more compact and clearer. For a more detailed explanation on why *Yaml* is a better format, [read this article](https://www.brycewray.com/posts/2023/05/organizing-hugo-configuration/)

To convert to *Yaml*, on *Linux*, you can use [Yq](https://github.com/mikefarah/yq). Run the following commands to install it:
```bash
# Install (Fedora)
dnf install yq

# Check version
yq --version
```

Since Hugo 0.109.0, configuration file name changed from `config.toml` to `hugo.toml`. If you're on a version prior to 0.109.0, use `config.toml` as file name on the rest of commands instead of `hugo.toml`.

`yq` doesn't have an option to list the available formats for conversion. Run the following command to list them:
```bash
yq --help | grep format
```

<!-- TODO: Add detailed explanation of Grep command -->

The important options are:
- *-p*: specifies the input format. *--input-format fmt_string* can be used also.
- *-o*: specifies the output format. *--output-format fmt_string* can be used also.

The input/output format passed to both options, can be a single letter (*y*) or the format name (*yaml*).

`yq` can detect the input based on the file extension, so you can skip that option.

The help says that you can skip the output format too, and that it defaults to *Yaml*, but if you try, you get an error:
```bash
Error: only scalars (e.g. strings, numbers, booleans) are supported for TOML output at the moment. Please use yaml output format (-oy) until the encoder has been fully implemented
```

That feature is not available yet, so you gonna have to use the output format option.

Run the following command to convert to *Yaml*
```bash
yq -oy config.toml > hugo.yml
```

Check the new file was created correctly by running `cat hugo.yml`. It should look like this:
```yaml
baseURL: http://example.org/
languageCode: en-us
title: My New Hugo Site
```

Now that we have the new configuration file, you can delete the old one.
```bash
rm config.toml
```

## Creating a Git Repository
With most themes, you have two ways to use them, with a *Hugo Module*, or a *Git Submodule*. To use it as a Hugo module you may have to eventually learn basic *Go* syntax. I don't want to unnecessary shift the focus from this guide.

Since we'll be using *Git* to save the project on the cloud and deploy it, we'll use a submodule.
```bash
# Create repository
git init
```
You don't have to know to use Git to make a site, you just need two commands to add the theme. You do need it to deploy it. Make sure you *bookmark* this page, I'll be adding a basic introduction to *Git Workflow* to run your own blog. 

## Adding a Theme
Themes have to be placed under `themes` directory, on its own directory, and then be referenced on the configuration file.

To use the theme on your site, you have to run two commands
- *git*: to add your theme as a submodule
- *echo*: to add a single line to the configuration file
```bash
# Add submodule. Change URL and destination directory accordingly to your theme
# The format is simple: `git submodule add repository_url theme_directory`
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt

# Set the theme on Hugo's configuration.
# Since version 0.109.0, the configuration file
# can be named `hugo.toml`, we will change it later
echo "theme: LoveIt" >> hugo.yml
```
> `>` is the *redirection operator* on Bash. It is used to dump content to a file. A single operator (`>`) creates a new file or overwrites it. A double operator (`>>`) add content.

## Starting the Server
To show the site on browser, Hugo uses a *Live Server*, which will be watching changes made on your site's directory structure and files. If a change is made, it will be reflected on the browser.
```bash
# Start webserver
hugo server
```

You will see the output of the *Live Server*:
```bash
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

As you see, you can go to `http://localhost:1313` on the browser and see the default look of *LoveIt* theme.

<!-- TODO: Add Web Browser screenshot -->

You won't be able to use the current terminal tab until you stop the server. To run commands from now on, you'll have to open a new one. You can use the mouse, or, if you're a keyboard freak like me, you can use a *Shortcut*.

This may change depending on your platform. On *Gnome Terminal*, the one I'm using, and in most terminals on *Linux*, is `Ctrl+Shift+T`.

## Adding a New Post
There is a lot of workflows you can use with Hugo, and *theme developers* use appropriate for their goals.

As I said on the first section, I choose Love It because it focus on writing. That means, once you got Hugo setup, you can create a new article and immediately see it on your site. Use `new` command to create a new *post*:
```bash
hugo new posts/hugo.md
```
A common struggle on newcomers to Hugo is dealing with Hugo's web server. You won't see this new post on your site, but that is because all new posts are *drafts* by default. That means they won't be rendered on site, unless you tell it otherwise.

To tell web server to render drafts, you have to use `-D` option. Come back to tab where web server is running, press `Ctrl+C` to stop it, and start it again with that option:
```bash
hugo server -D
```
Now, refresh your site on web browser, and you'll see the new post.
<!-- TODO: Add Web Browser screenshoot with new post -->

You just created a new entry on your *blog*. You can edit it on text editor of your choice. To list available drafts, use *list* command:
```bash
hugo list drafts
```
As you see on last command output, by default, all new content are created under `content` directory. `post` slag on post file name (`post/hugo.md`) is called *Content Type*. Is a way to organized your content, and is default content type in hugo when listing entries on *Home Page*.

*Home Page* is first page you see when you open your site. It is a *List Page* by  default, but you can convert it to *Single Page*. Home Page is a special page, we will cover it on later sections.

List Page is a page listing items, like posts you created.

Single Page is used to show content, the like post you just created. You can click on that post to see how your post is rendered as a Single Page.

Previews of available posts are listed on Home Page. You have a title, publish date, and when you add more content, you'll see first part of text as *Summary*.

You may notice *Author* item. You can set that field and customize summary by editing *Front Matter* on next section.

## What is Front Matter
Front Matter are properties of documents (*metadata*). You can check defaults fields for your version of Hugo by taking a look at the document you just created. Run the following command:
```bash
cat content/posts/hugo.md
```

You'll see the content of your document:
```yaml
---
title: "Hugo"
date: 2024-07-31T07:31:31-03:00
draft: true
---
```

If the output format is different than what you see above, we will cover that on the next section.

Depending on your version of Hugo, you may see different fields. On older ones, there was an *author* field, which we will add shortly. *Title* field is self explanatory. I will explain *date* and *draft* on separate sections.

## Interlude: Front Matter Format
In newer versions of Hugo, default front matter format has changed from Yaml to Toml, which is fine, because now is consistent with default configuration file (Toml).

But, as we discuss on a previous section, Yaml is more compact, clearer and easier to write, especially for newcomers. So, in case your front matter is using Toml, you can use `convert` command to convert all your documents at once.

I will assume you didn't create important documents. In that case, you will have to make a backup, but that will not be covered here.

To do the conversion, run the following command:
```bash
hugo convert toYAML --unsafe content/
```

That will convert all documents on `content` directory of your site. To cover all bases, do a conversion on `archetypes` directory too: 
```bash
hugo convert toYAML --unsafe archetypes/
```

`archetypes` directory contains *templates* for your documents (in *markdown*). We will cover that on other section.

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
 ls
 

---
title: "Setup Hugo Site Locally"
date: 2023-03-29T16:27:03+08:00
description:
tags: [Hugo]
draft: false
---

Hello there. This is Audrey and this is the real "First Post" of this blog.

Today I want to take a note of how to set up a Hugo site on my GitHub.
I worked with a Mac M1 machine with a x86 iTrem.

<!--more-->

1. Type `brew install hugo` to install the latest version of Hugo on my machine.
   If you want to know about the version installed, can use the command `hugo version`.
2. Type `hugo new site your-new-project-name` then move to the current project position `cd your-new-project-name`.
3. Then initialize a git repo inside the folder and install a theme. Here is the sample provided on the official tutorial doc.
   ```
   git init
   git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke themes/ananke
   echo "theme = 'ananke'" >> config.toml
   ```
4. Run `hugo server` to see if it functions normally.

After checking the local server, you can start to write your first post.
Type `hugo new posts/your-post-name.md` and edit the .md file. Remember to set the bool value of the "draft" option to true, or it won't be visible on your blog.

---
Ref

Official doc: https://gohugo.io/getting-started/quick-start/  
Theme creator: https://github.com/mavidser/hugo-rocinante  

+++ 
draft = false
date = 2021-08-19T16:55:03+03:00
title = "Setting user access token on GitHub.com"
description = ""
slug = ""
authors = []
tags = ["Coding", "Reminder"]
categories = []
externalLink = ""
series = []
+++

_Reminder to myself_

GitHub.com removed its support for password authentication on August 13, 2021. Now I need to use a personal access token instead on VS Code. These are the steps to setup GitHub auth. with personal access token:

1. Create personal access token on Github page: Settings / Developer Settings / Personal Access Tokens 
2. Set the current directory on command line to your Project root:
{{< highlight go "linenos=table,hl_lines=2,linenostart=1" >}}
> cd C:\Users\MyProject
{{< / highlight >}} 
3. Run this command: 
{{< highlight go "linenos=table,hl_lines=2,linenostart=1" >}}
> git remote set-url youroriginname 
https://yourusername:pastetokenhere@github.com/yourusername/yourrepository.git

{{< / highlight >}} 
+++ 
draft = false
date = 2020-03-26T22:00:12+03:00
title = "I'm learning R programming language"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
+++

I'm trying to learn R programming language through an excellent online course on [datacamp.com](https://www.datacamp.com) and it's a lot of fun. This is my second attempt of writing code after the classic "Hello World". I know that I'm in the very beginning of this journey but I want to leave this code here as a milestone :grimacing:

 
{{< highlight go "linenos=table,hl_lines=2,linenostart=1" >}} 
# Count how many times a specific word was repeated in a text
rquote <- "Add text here "
words <- strsplit(rquote, " ") [[1]]
rcount <- 0
for(word in words) {
  if(word == "add word here") {
    rcount <- rcount + 1
  }
}
rcount
{{< / highlight >}}
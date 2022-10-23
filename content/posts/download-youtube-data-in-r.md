+++ 
draft = false
date = 2020-04-23T00:06:40+03:00
title = "Getting YouTube Data with R using Tuber: Download YouTube Comments"
description = ""
slug = "" 
tags = ["Coding", "Education"]
categories = []
externalLink = ""
series = []
+++
## Enable APIs

The first step is to enable APIs on [Google API Console](https://console.developers.google.com/apis/dashboard). You need a Google account to do this. 
* Click on +ENABLE APIS AND SERVICES
* Type 'Youtube' on the search bar. You will see four options (Youtube Data API v3, Youtube Analytics API, Youtube Ads Reach API and Youtube Reporting API). Enable all of them. 
* Type 'Freebase API' on the search bar and enable it, too.

## Now all the APIs are enabled. It's time to create your credentials. 

* Click on 'Credentials' link on the left menu.
* Now click on +CREATE CREDENTIALS and choose 'OAuth Client ID' option. Type in some name in the box and choose 'Other' from 'Application Type' list and click the'Create' button. 
> At this stage, you might get a warning message about 'OAuth consent screen'. If that is the case, click on 'OAuth consent screen' link on the left menu and fill in the information (application name, support email) and save. 
* When you create 'OAuth Client ID' you will see a message stating that OAuth is limited to 100 sensitive scope  logins... Just ignore that message, it is nothing to worry about.
* You can now reach your client_id and client_secret when you click on the client ID name under OAuth 2.0 Client IDs list. 
* One last thing, we will choose localhost to upload the data. On the 'OAuth 2.0 Client IDs list', you will see an edit button (small pen shape icon), click on it. You will see an 'Authorized redirect URLs' option at the end. Write http://localhost:1410/ in the box and save.

## Install Tuber

Tuber package enables you to "Get comments posted on YouTube videos, information on how many times a video has been liked, search for videos with particular content, andvmuch more. You can also scrape captions from a few videos

Install the latest version from CRAN
{{< highlight go "linenos=table,linenostart=1" >}}
install.packages("tuber")
{{< / highlight >}}

and load the package
{{< highlight go "linenos=table,linenostart=1" >}}
library(tuber)
{{< / highlight >}}

## Example: Using Tuber to download YouTube comments of a video.

* Paste your client_id and client_list inside the code below.
* Paste the video_id of the YouTube video ([How to find YouTube video ID](https://youtu.be/liJVSwOiiwg))

{{< highlight go "linenos=table,hl_lines=1-2,linenostart=1" >}}
app_id <- "PASTE HERE"
app_secret <- "PASTE HERE"
yt_oauth(app_id, app_secret, token = '')
get_all_comments(video_id = "PASTE HERE")
comments1 <- get_all_comments(video_id = "PASTE HERE")
write.csv(comments1, file = “youtubecommments.csv”)
{{< / highlight >}}


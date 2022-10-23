+++ 
draft = false
date = 2021-02-25T22:05:14+03:00
title = "Adding Netlify CMS to an existing Hugo Site"
description = ""
slug = ""
authors = []
tags = ["Coding", "Education"]
categories = []
externalLink = ""
series = []
+++

I built this web site with Hugo and host it on Netlify. I use Visual Studio Code to make all the editing on my local repository and to push to/pull from my remote repository on GitHub. I'm a beginner so this process helped me alot to learn the basic of Markdown and Git. It's fun to update the website content following these steps but sometimes it can be frustrating. At those times, I really miss the user interface of content management systems such as Wordpress. 

I recently learned that it is possible to integrate [frontend interfaces with a Hugo Website](https://gohugo.io/tools/frontends/). I tried to integrate Netlify CMS with my website since I already use Netlify services. 

There is a detailed explanation about [how to set up Netlify CMS on an existing Hugo website](https://www.netlifycms.org/docs/add-to-your-site/) or [starting with a template](https://www.netlifycms.org/docs/start-with-a-template/) on Netlif website. I went with the "start with a template" option first and it worked smoothly, but it creates the website with a built in theme and I couldn't manage to change the theme after building the web site. Besides, I already have a static web site built with Hugo and I want to integrate a CMS with it. It took some time for me to understand the whole process from the guidelines on the website, that's why I decided to note each step here in case if I forget it again :smile:

**Step 1**: Create a folder called `admin` inside the `static` folder in your local repository.

**Step 2**: Inside the `admin` folder, create two files: `index.html` and `config.yml` . The first file `admin/index.html` is an entry point to Netlify CMS. You will access the CMS interface from `yourwebsite.com/admin` (in my case it is `naimcinar.com/admin/`) when everyhing is ready.

**Step 3**: Paste this code in `index.html` and save. This HTML starter page will load the Netlify CMS Javascript file:
{{< highlight go "linenos=table,linenostart=1" >}}

<!doctype html>

<html>
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Content Manager</title>
</head>
<body>
  <!-- Include the script that builds the page and powers Netlify CMS -->
  <script src="https://unpkg.com/netlify-cms@^2.0.0/dist/netlify-cms.js"></script>
</body>
</html>
{{< / highlight >}}

**Step 4**: Now let's move to the configuration section. This process is diffent for every site. I followed the guidelines on the web site and made minor adjustments. It worked on my web site. This is how I configured my `config.yml` file:

{{< highlight go "linenos=table,linenostart=1" >}}
backend:
  name: git-gateway
  branch: master # Branch to update (optional; defaults to master)

publish_mode: editorial_workflow
media_folder: "static/images/uploads" # Media files will be stored in the repo under static/images/uploads
public_folder: "/images/uploads" # The src attribute for uploaded media will begin with /images/uploads

collections:

name: "posts" # Used in routes, e.g., /admin/collections/blog
      label: "posts" # Used in the UI
      folder: "content/posts" # The path to the folder where the documents are stored
      create: true # Allow users to create new documents in this collection
      slug: "{{year}}-{{month}}-{{day}}-{{slug}}" # Filename template, e.g., YYYY-MM-DD-title.md
      fields: # The fields for each document, usually in front matter
        - {label: "Layout", name: "layout", widget: "hidden", default: "blog"}
        - {label: "Title", name: "title", widget: "string"}
        - {label: "Publish Date", name: "date", widget: "datetime"}
        - {label: "Featured Image", name: "thumbnail", widget: "image", required: FALSE}
        - {label: "Body", name: "body", widget: "markdown"}
        - {label: "Tags", name: "tags", widget: "string"}
  {{< / highlight >}}

Based on the structure of your web site you may need to change the `folder:` source in collections. You can also decide add or a remove label based on your needs when submitting a post. I succesfully managed to add posts from the CMS interface after this setup but can't reach the other folders on my web site such as `teaching`. I'll update this step if I will learn how to make all folders visible in the Netlify CMS user interface. For now, this configuration enables me to reach and edit my blog posts and post new ones from the CMS.

**Step 5**: Our `index.html` and `config.yml` files are now ready. Let's move to the Authentication step. Let's first enable Netlify Identity and Git Gateway:

* On your Netlify account, go to **Settings > Identity** and select **Enable Identity Service**
* Under **Registration preferences**, select **Invite Only**.
* If you want to enable login from external providers such as Google and GitHub, check the boxes you want to use,  under **External Providers** section. I find it practical to log in with GitHub, so I added it. 
* Go to **Services > Git Gateway**, and click **Enable Git Gateway**. This authenticates with your Git Host and generates and APi token.
* Evertything is almost set up. What we need now is a frontend interface to connect. We will add the following script in two places, to include the **Netlify Identity Widget** on our web site:

{{< highlight go "linenos=table,linenostart=1" >}}

<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>

{{< / highlight >}}

Add the script to the `<head>` of your CMS index page (`/admin/index.html`

Add the script `<head>`of your site's main index page, too.

I added the script on these locations manually but somehow it did not work correctly. I'm sure I did a very simple mistake which I couldn't yet figure out what, but if you are an amateur like me you can include the script on your web site using Netlify's [Script Injection](https://docs.netlify.com/site-deploys/post-processing/snippet-injection/) feature.  

To use snippet injection, go to **Site Settings > Build & Deploy > Post processing**. Find the **Snippet Injection** section and select **Add Snippet**. Write a name for the script in the first box (e.g. Netlify Identity Widget). Paste the script inside the second box. Choose `Insert before </body>` option and save. 

Lastly, we will add the following script before the closing `<body>` tag of our website's main index page. This script allows user to redirect back to the `/admin/` path after comopleting the login with the Netlify Identity Widget:

{{< highlight go "linenos=table,linenostart=1" >}}

<script>
  if (window.netlifyIdentity) {
    window.netlifyIdentity.on("init", user => {
      if (!user) {
        window.netlifyIdentity.on("login", () => {
          document.location.href = "/admin/";
        });
      }
    });
  }
</script>

{{< / highlight >}}

**Step 5**: Now we're all set. Sinca we set our registration preferences to "Invite Only", we need to invite ourself as a site user. To do this, select the **Identity** tab from Netlify site dashboard and select the **Invite Users** button. Add you email adress and send an invitation. You will recevive an email. In your email, click "accept the invitation". You will be directed to your website admin sign up panel. Choose a password. Now you are ready to log in to Netlify CMS.
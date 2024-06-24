---
title: "How to: Adding Google Analytics 4 to your Hugo Site"
date: 2024-06-18T00:00:00.000Z
author: Analytics Toyin
image: /images/blog/ga4_logo.png
type: blog
---
Hello and welcome to another one of my How-to articles. In this post I'll be taking you through the steps of adding google analytics to your Hugo site. More importantly I'll point out things that personally caused me issues and had me scratching my head for a while. Whilst I was doing my own research, I found other articles to be a little out of date so this is an up to how-to article for. your pleasure.

This article assumes you have a working knowledge of Hugo and are using Netlify for web hosting.

### Set up your google Analytics account

Navigate to this link and create an analytics account: https://analytics.google.com/analytics/web

OR

Search for 'Google Analytics' on Google. Don't look for 'Google Analytics 4' because the first few results from that search can be confusing and don't quite make sense. I have found this to be the easiest way.

![GA4 search result screen](/images/blog/image.png "Google Analytics Search result")

Sign in with your google account or create one if you don't already have one.

Then you should find yourself on the home page where you can click on the 'Start Measuring' which will allow you to set up an account for analytics. A single account can hold many 'properties' which are essentially separate websites you can measure data for.

* On the first page, follow the instructions create your account name and add data sharing preferences.

Now you can add your first "Property" - give it a name and point the url to the site you want to start measuring things for.

Follow the remaining steps and then select "Web" when prompted to create a Data stream.

### If you already have an analytics account

I found it to be a bit confusing to figure out how to add another property since I've already got an analytics account. So follow the steps below to do this if you're in the same scenario.

Click into the admin tab to find your account details

![GA4 Home Dashboard](/images/blog/image1.png "GA4 Home Dashboard")

Now you can simply create a new property and follow the remaining steps as outlined

![GA4 Property Dashboard](/images/blog/image2.png "GA4 Property Dashboard")

### Configure Hugo site

After creating your web data stream you should be presented with a Measurement ID.

![GA4 Measurement ID](/images/blog/image3.png "GA4 Measurement ID")

Hugo has a built in template for google analytics so we can simply use this. In your header file, which contains your <head> tag (and hopefully sits in a partial so it can easily be added to each web page) you need to add the following to use Google analytics code. Without this in your header, GA4 won't function.

```
  <!--Google Analytics-->
  {{ template "_internal/google_analytics.html" . }}
```

In your config.toml, just add your GA4 measurement ID. This needs to be added at the top level

```
   googleAnalytics = "G-MSRD6H7JCM"
```

* Ensure this is added at the **top-level** this means it should not sit under params or anything else. Easiest is to just add it underneath the baseUrl
* Ensure you spell the *googleAnalytics* correctly - otherwise it won't function as expected

Now you can rebuild your site and everything should work as expected.

**Note:** You will be tracking all urls tagged with the measurement ID. This includes your local development environment when you're running it. To only track real production data, you can run hugo with multiple config files and only add the GA4 measurement ID to the production config file. Follow this tutorial [here](https://gohugo.io/getting-started/configuration/).

Peace and Blessings 

Toyin

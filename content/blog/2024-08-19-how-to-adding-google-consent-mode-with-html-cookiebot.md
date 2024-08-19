---
title: "How To: Adding Google consent mode with HTML & Cookiebot"
date: 2024-08-19T00:00:00.000Z
author: Consent Mode Toyin
image: /images/blog/google-consent-mode.webp
type: blog
---
If you have had the absolute displeasure of setting up google mandated consent mode, then you will know that it can be an absolute nightmare to set up. Not least because google likes to make things unnecessarily complicated and long winded but also because there are very few articles and blogs posts out there that walk you through the process of adding a cookie consent banner AND then implementing consent mode V2. 

I personally found that I had to get scraps of information from different sources and I ended up with little confidence that I had set things up correctly. However, I am here to fill the gap. Stick with me and in this article I’ll describe how to very simply implement a cookie consent banner and enable google consent mode for your website.

Before getting into it, A few disclaimers so you can determine whether this article/guide will be of use to you. This article assumes you have full control over your website’s raw html files rather than having used a website builder such as wordpress. Also, you’re looking to implement the consent banner and google consent mode by manually adding the script tags to the raw html files. Lastly, you are using gtag.js (or a similar manual script implementation) rather than using google tag manager or some kind of plugin to collect analytics data. So let’s get into it. 

3 very simple steps. Don’t be a hero and try to write your own html consent banner code. That’s a waste of time. Instead sign up to Cookiebot or a similar CMP service and follow these steps to manually the consent banner.

<https://support.cookiebot.com/hc/en-us/articles/10714664673564-Manually-implementing-Cookiebot-CMP>

Just to paraphrase the steps described above, there are 3 general steps to follow to add a cookie consent banner to your website:

1. Create a Cookiebot CMP account and add your domain (a scan of your website will kick of automatically and can take up to 24hrs)
2. Configure the cookie consent banner through cookiebot (only need cookie consent banner)
3. Manually insert the consent banner script on your website to make the cookie consent banner appear - insert it as the very **first** script on **every** html page’s HEAD-tag

After completing these steps, the Cookiebot consent banner will now be visible on your website. Now you just have to implement google consent mode. The steps are well defined [here](https://support.cookiebot.com/hc/en-us/articles/360016047000-Implementing-Google-consent-mode#h_01HBWS079DZRM28JBSFD5W3S12), just skip down to the inline script implementation. Very simply, just add the scripts that are mentioned and boom you’re done. See below a snippet of what should be inside your HEAD-tag when adding cookiebot banner and consent mode scripts:

```
<script id="Cookiebot" src="https://consent.cookiebot.com/uc.js" data-cbid="3357358d-0d8a-4c03-97ac-94726e214a32" data-blockingmode="auto" type="text/javascript"></script>

<script>
    window.dataLayer = window.dataLayer || [];
    function gtag() {dataLayer.push(arguments);}
    gtag("consent", "default", {
        ad_personalization: "denied",
        ad_storage: "denied",
        ad_user_data: "denied",
        analytics_storage: "denied",
        functionality_storage: "denied",
        personalization_storage: "denied",
        security_storage: "granted",
        wait_for_update: 500,
    });
    gtag("set", "ads_data_redaction", true);
    gtag("set", "url_passthrough", true);
</script>

<!-- Google tag (gtag.js) --> 
<script async src="https://www.googletagmanager.com/gtag/js?id=AW-<<ID>>"></script> 

<script> 
    window.dataLayer = window.dataLayer || []; 
    function gtag(){dataLayer.push(arguments);} gtag('js', new Date()); gtag('config', 'AW-<<ID>>'); 
</script>
```

The final steps, after adding all of these scripts to your website is just to wait for the cookiebot scan to run which should tell you which of your cookies or trackers are missing some information. Ensure that you use Cookiebot to fill in any missing information for these cookies/trackers such as missing descriptions or categorisations. After you’ve done this and made the recommended changes, kick of another scan and you’re done. 

It may look like a lot, but in reality, you just need to add the cookiebot consent banner script, default consent mode state scripts and then the gtag.js scripts. That’s all. 

I hope this has been helpful in collating what needs to be done to add a consent banner and enable google consent mode for your website by adding scripts to your raw html files. 

Peace and blessings

Toyin

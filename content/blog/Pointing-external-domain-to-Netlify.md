---
title: "How to: Pointing External Domain to your Netlify site"
date: 2024-05-24
image: images/blog/blog-post-2.png
author: Domain Toyin
type: blog
---

Hello and welcome back to my blog. In this article, I'll be taking you through how to point your external domain to Netlify. If you aren't using Netlify for your web hosting needs, then get to know. Great platform but that's not what we're here to talk about.

If you've already purchased your domain through a registrar, it is simple (when you know what you're doing) to point that domain to your newly created Netlify site and link them. I struggled a little bit because there doesn't seem to be clear guides/articles readily available so here I am creating one. In this guide, I've used Wix as the example external domain provider, but the same steps can be applied if you've registered the domain through another provider such as SquareSpace, Webflow or even Porkbun and goDaddy. This article assumes you have a working knowledge of how to deploy your site to Netlify and have done so. 

Below are the steps I followed to get this done in under 20 mins:
1. On Netlify, navigate to your site ➝ Domain management
    - Here you should see the Netlify subdomain that was auto-created for you - change it if you like
2. Add a Domain ➝ Enter the domain you own from Wix
3. You should get a message indicating it's already registered (good) ➝ Add Domain
4. The Domain management tab should now have 2 extra domains (www subdomain and root domain) with the below warning

![blog-details-image-01](/images/blog/post-02/1.png)

5. Click on this warning and see the below window:

![blog-details-image-02](/images/blog/post-02/2.png)

6. Now log into your Wix.com account where you purchased the domain. Go on profile (top right) ➝ Domains ➝ Domain actions (3 little dots) ➝ Manage DNS records
    - Follow similar steps to get into managing DNS records at your registrar of choice

![blog-details-image-03](/images/blog/post-02/3.png)

7. Under A records, Edit the first record and change the IP address to the one provided by Netlify window in step 5 (75.2.60.5). Ignore Wix warnings. Also delete any existing A records so you only remain with 1 record.
    - If no A record exists in your DNS, then create it

8. Do the same for CNAME (Aliases) - Edit the existing (or create new)
    - Host Name: www
    - Value: apex-loadbalancer.netlify.com
You should end up with this:

![blog-details-image-04](/images/blog/post-02/4.png)

9. Now you can navigate back to Netlify where after some time, the Awaiting External DNS warning should disappear - this can take a little while
10. After that, scroll to the bottom and request a SSL/TLS certificate
11. The last step, scroll back up to Production domains and for the www domain ➝ options ➝ Set as primary domain 

The final step ensures that encryption is properly configured and connections are only allowed over HTTPS. 

That should be it. It can take some time because DNS propagation can take time, but in most cases you should be able to visit your site now on the domain that you own. 
If there's any questions/issues get in touch and I'd be happy to help. 

You can find official Netlify docs on how to do this [here](https://docs.netlify.com/domains-https/custom-domains/configure-external-dns/)

Peace and Blessings

Toyin
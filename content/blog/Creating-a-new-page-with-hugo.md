---
title: Creating a new page with Hugo
date: 2024-05-31
author: Hugo Toyin
image: /images/blog/creating-a-hugo-theme-from-scratch.jpg
type: blog
---
Hello and welcome back to my blog. This blog post is about creating a new page using Hugo. Hugo is one of the most popular open-source static site generators (straight from their website) and I’ve been having great fun learning to use it. If you’re a fan of Hugo or looking to get into it, then you’re in the right place. Hugo has great documentation in general (find it [here](https://gohugo.io/)) but I found that the way Hugo works, being based on hierarchy, can be confusing at times. So here is a very simple explanation of how page content and layouts work and how to create your own single page with a custom layout. Of course I'm assuming you have a basic working knowledge of Hugo. If anything is confusing please feel free to get in touch and maybe I can explain things a bit better.

Your directory structure could look something like this:

```
content
├── _index.md
└── contact
        ├── _index.md
        ├── page-one.md
        └── page-two.md
└── about.md
```

Firstly, just to talk about what the the relative paths for these files. In Hugo, a `.md` file is treated as a web page and is accessible via a URL. So, every markdown file you have in your /content directory, will be their own page. The below codeblock shows the directory structure with the URL paths you can expect as a result.

```
content
  ├── _index.md             // <- https://example.com (optional)
  └── contact
          ├── _index.md     // <- https://example.com/contact/
          ├── page-one.md   // <- https://example.com/contact/page-one/
          └── page-two.md   // <- https://example.com/contact/page-two/
  └── about.md              // <- https://example.com/about/
```

* `_index.md` is the default markdown content used for the root - this is optional
* There are 2 ways to create a new standalone page as above. Either create it directly in inside`/content` or create a subfolder where you can use `_index.md` as the relative root and so on.

Now that you have your web pages, you will want to use a template to render the content. Everything in Hugo is a hierarchy of specificity. This is no different. When Hugo renders content, it looks for templates in the following order of specificity (always inside the `/layouts` directory):

1. **Specific content type templates**: Hugo looks for templates named after the content type first. For example, if your content type is `contact`, it will look for a template named `contact.html`.
2. **Section templates**: If Hugo doesn't find a specific content type template, it looks for a template named after the section in which the content resides. For example, if your content is in a section named `contact`, it will look for a template named `section/contact.html`.
3. **`_default` templates**: If Hugo doesn't find a specific content type or section template, it falls back to the `_default` templates. These templates apply to all content types unless overridden by more specific templates. Important here is `single.html` which is the template used for single pages as a default.

So if you were creating a standalone contact page, you can create the HTML file in a few ways and see which is most comfortable and intuitive for you.

```
layouts
  └── _default
          ├── single.html
          └── contact.html
  └── contact
          └── single.html
  └── contact.html
```

1. Place `contact.html` inside `/layouts`
2. Place `single.html` inside `/layouts/contact/`
3. Place `contact.html` inside`/layouts/_default/`

If none of these templates are added, Hugo will default and use `/_default/single.html` and apply it.

Now go forth and create some pages with custom template renders.

**N.B: I have personally found that placing html files directly in the layouts folder does not apply them in which case use 2 or 3 instead.**

**Note:** When creating a new page by using method 2, you may find a file call `list.html` which is used as the template file for any subpages of the new page. E.g., Your new blog page at `/blog` could have many blog posts sitting in subpages and they would use all use the html template at `list.html`.

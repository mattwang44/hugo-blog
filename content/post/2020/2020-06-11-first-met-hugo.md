---
title: "Hugo Tips"
subtitle: ""
description: " "
excerpt: " "
date: 2020-06-11
author: "Matt Wang"
image: "https://s3.wp.wsu.edu/uploads/sites/1417/2017/04/ms_plant_health_banner_990.jpg"
published: true
tags:
  - misc
URL: "/2020/06/11/hugo-tips/"
categories: ["Tech"]
---

## Shortcode Collection

- [Hugo native shortcodes](https://gohugo.io/content-management/shortcodes/)
- [Embed Google Presentation](https://ashish.one/gist/add-responsive-google-slides-on-hugo/)
- [`figure` & `gallery`](https://github.com/liwenyip/hugo-easy-gallery/)
- [Insert HTML in Markdown](https://anaulin.org/blog/hugo-raw-html-shortcode/)

## Enable MathJax

Add following lines into `theme/<theme>/layout/partials/math.html`

```html
<script
  src="//cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript"
></script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({ tex2jax: {inlineMath: [['$','$']]}  });
</script>
```

and put following lines in any template that the target page used:

```
{{ if or .Params.math .Site.Params.math }}
{{ partial "math.html" . }}
{{ end }}
```

and then put the parameter `math: true` in the header of the article page to enable MathJax.

## Simple SEO

- [configure sitemap](https://gohugo.io/templates/sitemap-template/#configure-sitemapxml) and provide the URL of sitemap (e.g. `<host>/sitemap/`) to [Google Search Console](https://search.google.com/search-console).
- Already done by the theme I used:
  - Integrate with Google Analytics.
  - Configure Open Graph tags.

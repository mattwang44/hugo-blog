---
title: "Use Hugo for the first time"
subtitle: ""
description: " "
excerpt: " "
date: 2020-06-11
author: "Matt Wang"
image: "https://s3.wp.wsu.edu/uploads/sites/1417/2017/04/ms_plant_health_banner_990.jpg"
published: true
tags:
  - misc
URL: "/2020/06/11/first-met-hugo/"
categories: ["Tech"]
---

# Shortcodes

- [Hugo native shortcodes](https://gohugo.io/content-management/shortcodes/)
- [Embed Google Presentation](https://ashish.one/gist/add-responsive-google-slides-on-hugo/)
- [`figure` & `gallery`](https://github.com/liwenyip/hugo-easy-gallery/)

# Enable MathJax

Add following lines into `theme/<theme>/layout/partials/head.html` or any template that the target page used:

```html
<script
  src="//cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript"
></script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({ tex2jax: {inlineMath: [['$','$']]}  });
</script>
```

---
layout: post
author: Chris Luedtke
title: How to Embed Python Code from a Jupyter Notebook
subtitle: The super easy way
header_img: /assets/images/post-bg.jpg
---

## Let's not get too technical

Unless I minimize barriers to the sharing process, this blog will remain a barren wasteland. I want the simplest option amongst the many which span a large range of prerequisite TML and CSS fuss.

## The Simple Process

<ol>
    <li>In a Jupyter Notebook, choose File, Download As, HTML</li>
    <li>Place the HTML file in your GitHub repo or webpage environment in a new folder (typically your "static" folder)</li>
    <li>In the HTML of your blog entry, add the following iframe, and experiment to set an appropriate height:</li>
</ol>

<code>&lt;iframe src="/some_folder/blog_sample.html" width="100%" height="200" scrolling="no" frameBorder="0"&gt;&lt;&#47;iframe&gt;</code>

That's it! You should end up with something like this:

<iframe src="/assets/html/code/Blog_sample.html" width="100%" height="200" scrolling="no" frameBorder="0"></iframe>

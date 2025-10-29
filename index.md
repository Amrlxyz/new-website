---
title: "My Website"
layout: splash
permalink: /
# date: 2016-03-23T11:48:41-04:00
header:
  overlay_color: "#ffffff"
  overlay_filter: rgba(0, 0, 0, 0.7)
  overlay_image: /assets/images/soldering-timelapse.gif
  actions:
   - label: "Projects Portfolio"
     url: "./portfolio/"
     
excerpt: "The place where I share my projects and cool stuff I learn in my engineering journey"

intro: 
  - excerpt: 'All written by me - no AI used for content'

feature_row_projects:
  - image_path: /assets/home/buggy-race-day.jpg
    alt: "placeholder image 2"
    title: "Engineering Projects"
    excerpt: 'I like doing random projects that interests me at the time and whichever I did not forget *should* be here.'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"

feature_row_exp:
  - image_path: /assets/images/bms-testing.jpg
    alt: "placeholder image 2"
    title: "Experiments and Technical Articles"
    excerpt: 'The best way to learn is to try, fail, learn, repeat.'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"

feature_row_about:
  - image_path: /assets/home/my-pic.jpg
    alt: "my picture with my FS team's car"
    title: "About Me"
    excerpt: 'Im just a guy who likes electronics and trying to get better at it'
    url: "/about"
    btn_label: "Learn More"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}

<!-- {% include feature_row %} -->

{% include feature_row id="feature_row_projects" type="left" %}

{% include feature_row id="feature_row_exp" type="right" %}

{% include feature_row id="feature_row_about" type="center" %}
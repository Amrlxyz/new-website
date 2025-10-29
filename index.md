---
title: "My Website"
layout: splash
permalink: /
# date: 2016-03-23T11:48:41-04:00
header:
  overlay_color: "#ffffff"
  overlay_filter: rgba(0, 0, 0, 0.7)
  overlay_image: /assets/images/unsplash-image-1.jpg
  actions:
   - label: "Projects Portfolio"
     url: "./portfolio/"
  # caption: "pcb design i made"
excerpt: "(Work in Progress) The place where I share my projects and cool stuff I learn in my engineering journey"

intro: 
  - excerpt: 'Centered with `type="center"`'

feature_row:

  - title: "Embedded Sytems"
    image_path: assets/images/unsplash-gallery-image-1-th.jpg
    alt: "placeholder image 1"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--info btn--small"


  - title: "Power Electronics"
    image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--info btn--small"

  - title: "C/C++"
    image_path: /assets/images/unsplash-gallery-image-3-th.jpg
    excerpt: "This is some sample content that goes here with **Markdown** formatting."

  - title: "Python"
    image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--info btn--small"

  - title: "PCB Design"
    image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--info btn--small"

  - title: "3D Mechanical CAD"
    image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--info btn--small"

feature_row2:
  - image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Engineering Projects"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Left aligned with `type="left"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"

feature_row3:
  - image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Experiments and Technical Articles"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Right aligned with `type="right"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"

feature_row4:
  - image_path: /assets/images/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "About Me"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Centered with `type="center"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
---

<!-- {% include feature_row id="intro" type="center" %} -->

{% include feature_row %}

{% include feature_row id="feature_row2" type="left" %}

{% include feature_row id="feature_row3" type="right" %}

{% include feature_row id="feature_row4" type="center" %}
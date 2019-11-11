---
layout: splash
permalink: /
header:
  # overlay_color: "#555555"
  # overlay_color: "#000000"
  # overlay_filter: 0.5
  overlay_filter: rgba(255, 165, 0, 0.5)
  overlay_image: /assets/images/mm-home-page-feature-lab.jpg
  actions:
    - label: "<i class='fas fa-download'></i> Install now"
      url: "/docs/setup-guide"
    - label: "<i class='fas fa-vial'></i> Labs"
      url: "/labs/"
excerpt: >
  Open source labs for the community, by the community for the M5StickC on Amazon FreeRTOS.<br />
  <small><a href="https://github.com/teuteuguy/amazon-freertos-m5stickc-workshop/releases/tag/0.0.1">Latest release v0.0.1</a></small>
feature_row:
  - image_path: /assets/images/mm-customizable-feature.png
    alt: "customizable"
    title: "Open Source"
    excerpt: "Everything from the labs, documentation, comments, and more can be submitted by the community thanks to Github."
    url: "/contributing/"
    btn_class: "btn--primary"
    btn_label: "Learn more"
  - image_path: /assets/images/mm-responsive-feature.png
    alt: "labs"
    title: "Labs"
    excerpt: "Labs designed to run on the M5StickC, showcasing the device and Amazon FreeRTOS capabilities."
    url: "/labs/"
    btn_class: "btn--primary"
    btn_label: "Learn more"
  - image_path: /assets/images/mm-free-feature.png
    alt: "100% free"
    title: "100% free"
    excerpt: "Free to use however you want under the MIT License. Clone it, fork it, customize it... whatever!"
    url: "/license/"
    btn_class: "btn--primary"
    btn_label: "Learn more"      
---

{% include feature_row %}
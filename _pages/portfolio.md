---
title: "Portfolio"
layout: "splash"
permalink: /portfolio/

header:
  overlay_color: "#ffffffff"
  overlay_filter: rgba(0, 0, 0, 0.7)
  overlay_image: /assets/images/unsplash-image-1.jpg
excerpt: "Highlights of My Projects"

# # Main categories row
# feature_row:
#   - image_path: /assets/portfolio/esp-final-race-day.jpg
#     alt: "Autonomous Line Following Buggy"
#     title: "Embedded Systems & Robotics"
#     excerpt: "Developing control systems, firmware, and autonomous robots using C++, FreeRTOS, Mbed, and ROS. Projects include a Formula Student VCU, autonomous buggy, and hexapod."
#   - image_path: /assets/portfolio/posts/vcu-3d.png
#     alt: "VCU PCB"
#     title: "Electronics & PCB Design"
#     excerpt: "Designing safety-critical hardware for high-voltage systems. Projects include a 400V+ BMS, Vehicle Control Unit (VCU), and BSPD. Skilled in Altium Designer and Kicad."
#   - image_path: /assets/portfolio/posts/f1is-render.jpg
#     alt: "F1 in Schools Render"
#     title: "3D Mechanical CAD & Manufacturing"
#     excerpt: "Bringing designs to life from concept to reality. Experienced with SOLIDWORKS (FEA/CFD), 3D printing, and CNC milling from F1 in Schools and various university projects."


project_row_vcu:
  - image_path: /assets/portfolio/vcu-3d.png
    alt: "VCU PCB"
    title: "Formula Student Vehicle Control Unit (VCU)"
    excerpt: "The 'brain' of our EV Formula Student car implemented with FreeRTOS, critical for managing core functionalities such as throttle control, telemetry and fault detection/handling. PCB designed in Altium Designer."
    url: "https://github.com/ManchesterStingerMotorsports/f446-vcu"
    btn_label: "View on GitHub"
    btn_class: "btn--primary"

project_row_buggy:
  - image_path: /assets/portfolio/esp-final-race-day.jpg
    alt: "Autonomous Buggy Race Day"
    title: "Autonomous Line Following Buggy"
    excerpt: "Final Race Winner! Designed and built a custom autonomous buggy, programmed with Mbed in C++. Implemented cascaded PID control, state machine architecture, with multiple timer interrupts."
    url: "https://github.com/Amrlxyz/esp-lfr-buggy"
    btn_label: "View on GitHub"
    btn_class: "btn--primary"

project_row_bms:
  - image_path: /assets/portfolio/bms-boards.png
    alt: "BMS Boards"
    title: "Formula Student Battery Management System (BMS)"
    excerpt: "Microcontroller firmware that manages the safety of the car's 400V+ battery pack. It includes my own custom SPI-to-isoSPI hardware driver for communication with ADMBS ICs."
    url: "https://github.com/ManchesterStingerMotorsports/g474-bms"
    btn_label: "View on GitHub"
    btn_class: "btn--primary"

project_row_bspd:
  - image_path: /assets/portfolio/bspd-3d.png
    alt: "BSPD 3D View Kicad"
    title: "Brake System Plausibility Device (BSPD)"
    excerpt: "A safety-critical component of the car that performs plausibility check between brake pressure and throttle position only using non-programmable logic. Designed in Kicad."
    url: "https://github.com/ManchesterStingerMotorsports/BSPD"
    btn_label: "View on GitHub"
    btn_class: "btn--primary"

project_row_f1is:
  - image_path: /assets/portfolio/f1is-render.jpg
    alt: "F1 in Schools Car Render"
    title: "F1 in Schools (World Finals 2017)"
    excerpt: "Used SOLIDWORKS to design the car and run FEA and CFD simulations to optimise its performance. Utilised CNC Milling and 3D Printing for precise manufacturing. Two times 'Fastest Car Award' winner and represented Malaysia."

others_row: 
  - excerpt: '[View Other Projects](/){: .btn .btn--large .btn--info }
              [About Me](/){: .btn .btn--large .btn--info }'
---



{% include feature_row id="feature_row" type="center" %}

<!-- <h2 style="text-align: center; font-weight: 500; margin-top: 2em; margin-bottom: 1.5em;">Project Highlights</h2> -->

{% include feature_row id="project_row_vcu" type="left" %}

{% include feature_row id="project_row_buggy" type="left" %}

{% include feature_row id="project_row_bms" type="left" %}

{% include feature_row id="project_row_bspd" type="left" %}

{% include feature_row id="project_row_f1is" type="left" %}

{% include feature_row id="others_row" type="center" %}
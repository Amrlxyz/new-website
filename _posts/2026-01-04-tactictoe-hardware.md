---
title: "Tactic-toe (Hardware)"
# last_modified_at: 2017-03-09T16:20:02-05:00
excerpt: "tactic-toe PCB game name card design"
categories:
  - Projects
tags:
  - PCB Design
  - Embedded Systems

header:
  image: /assets/posts_assets/tactictoe_pcb-all-layers.png
  teaser: /assets/posts_assets/tactictoe_pcb-all-layers.png
---


Please first see the [overview article](../tactictoe-overview) to learn about tactic-toe
{: .notice}

# Inspirations

One of the projects that I have always wanted to do is to make my own PCB business card. 

I've been inspired by the designs online that people share. Typically each of them have its own gimmicks, and they come in varying quality. Some are really good and some are what people online (not me) call as "E-Waste".

## Design Examples

Theres just so many designs out there but here are few of my favourites:

- **Stylophone** _(by Mitxela)_

![mitxela pcb](/assets/posts_assets/tactictoe_inspo_mitxela.png)

This might be the earliest design I saw on Youtube by [mitxela](https://mitxela.com/projects/stylocard). Its a stylophone pcb which is pretty cool. Its essentially one of the revolutionary ones because it has a USB-A as part of the pcb itself to make it very thin.

- **Fluid Simulator** _(by Nick Johnson)_

![fluid sim pcb](https://github.com/Nicholas-L-Johnson/flip-card/blob/main/media/1000003136.gif?raw=true)

This is an example where a simple idea if executed properly can be awesome. I have seen many attempts at making a similar idea but this time its much higher resolution and such at a smooth fps which just tickles your brain right. Its one of the more recent designs I saw and definitely one of the most complex software-wise. Plus, it also has a female type-C connector as a part of the pcb.

- **Persistence of Vision** _(by Ben Eater)_

![ben eater pcb](/assets/posts_assets/tactictoe_inspo_benEater.png)

_Image taken from [instagram](https://www.instagram.com/p/C8TNpaUJmOJ/)_

This one is from my favourite ASMR electronics youtuber [Ben Eater](https://www.youtube.com/c/BenEater). Its so simple yet one of the coolest one in my opinion. It acts like a persistence of vision wand, when you to shake the card, the LED displays text of his name. It has an IMU and an MCU to control the LED timing when shaken. It also has a very creative battery holder which make use of the PCB's flexibility.

- **Other designs**

There are other designs that i'm not really a fan of which are quite common.

1. **NFC cards:** Its cool and complex to tune the the antenna but I think it doesnt fully make use of a PCB when you can get NFC cards cheaply pretty common nowadays.
2. **Not business card sized or too thick:** Typically either the buttons or Some of these are genuinely really cool but it just doesnt fit the theme of a business card.
3. **PCB without any components:** Some designs sometimes just have a ruler and printed graphics. Sure it looks nice but again it doesnt fully make use of the PCB medium.

How good the design is depends on what you are actually trying to do with it. At the end of the day having one is cooler than not having one. Plus, its a great learning opportunity.


# Requirements

Now that we have looked at some inspirations. I have a few requirements for my own design.

- **As thin as possible + credit card sized**

Its not really a business card if its too thick or in a different size. So for this as a minimum it has to fit inside a typical wallet. This requirement alone rules out a lot of typical components including through hole components and buttons. 

- **Cheap and Simple**

Its not really an engineering project if it doesnt involve any cost restriction. Since I'm planning to give it to people, I will be making quite a few of them and the cheaper they are the better. This inderictly means as little components as possible (within reason).

- **Runs the perfect Tactic-toe AI**

As part of the overall goal of the project, the microcontroller used must be capable of running the AI locally. This shouldnt be much of an issue since most MCU nowadays are fast enough, but as I later realise, ROM size might be an issue.


# My Design Idea

Roughly, this is what I came up with:

![initial idea](/assets/posts_assets/tactictoe_initialIdea.png)

On the other side of the PCB, it has the graphics for my contact-details and a QR code to my website.

Theres a few challenges with this design, how do player interface with the game and how does the game board display the board. 

First, player interface. buttons are a no go for me since its quite thick and makes the pcb slightly uneven. There are super thin buttons but they are rather ugly in my opinion. A much better alternative is touch sensor thats built in the PCB layer. This comes at a more complex trace routing design but its worth it for a cleaner look. 

Second, board display. The big issue with this is how would you display an X and O, or atleast differentiate them? You could use a single LED with a 

# Component Selection

For the cheapest generic components bought in bulk, LCSC is the the place to go. Aliexpress can offer better price but navigating the amount of scams and counterfiet components is just not worth it. So, all the components are surveyed on LCSC for this project. 

## LEDs



## Player Input


## Linear Regulator

Since the MCU requires 3.3V, a linear regulator is needed to step down the USB 5V to 3.3V. We dont need any fancy feature or the best noise performance, so the cheapest Linear Regulator from Ti was chosen which is 


# Name Card Graphics



# Version 1 prototype

led 1

# Version 2

led 2




# final







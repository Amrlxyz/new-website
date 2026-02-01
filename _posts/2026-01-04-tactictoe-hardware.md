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

gallery-sk6805:
- image_path: /assets/posts_assets/tactictoe_sk6805-top.png
  alt: "sk6805-top"
- image_path: /assets/posts_assets/tactictoe_sk6805-bottom.png
  alt: "sk6805-bottom"

gallery-2812:
- image_path: /assets/posts_assets/tactictoe_2812-top.png
  alt: "6812-top"
- image_path: /assets/posts_assets/tactictoe_2812-bottom.png
  alt: "6812-bottom"

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

Cost is a huge consideration. Since I'm planning to give it to people, I will be making quite a few of them and the cheaper they are the better. This inderictly means as little components as possible (within reason).

- **Runs the perfect Tactic-toe AI**

As part of the overall goal of the project, the microcontroller used must be capable of running the AI locally. This shouldnt be much of an issue since most MCU nowadays are fast enough, but as I later realise, ROM size might be an issue.


# My Design Idea

Roughly, this is what I came up with:

![initial idea](/assets/posts_assets/tactictoe_initialIdea.png)

On the other side of the PCB, it has the graphics for my contact-details and a QR code to my website.

Theres a few challenges with this design, how do player interface with the game and how does the game board display the board. 

First, player interface. buttons are a no go for me since its quite thick and makes the pcb slightly uneven. There are super thin buttons but they are rather ugly in my opinion. A much better alternative is touch sensor thats built in the PCB layer. This comes at a more complex trace routing design but its worth it for a cleaner look. 

Second, board display. Definitely will use LEDs for lower cost. But how would you indicate X or an O? You could use different colours or different shapes. 

A typical LED would require one GPIO from the MCU to control ON/OFF. Assuming the least amount of LEDs, i.e. 2 LEDs per square, one for each color player, you would need `9x2=18` gpio pins. Plus, with 9 touch inputs, then you would need an MCU with a larger pins package which typically comes at a higher cost. This is not even considering if the MCU can sink/source enough current for all LEDs.

Eventually, I settled with 5 RGB LEDs in X shape config:

![LED config](/assets/posts_assets/tactictoe_led-config.png)

Atleast that was the plan. After getting showing the version 1 to people and getting feedback, I ended up just using the X with different colour to differentiate X and O.

Using addressable RGBs has the advantage of much easier routing as each RGB led has its own built in controller. What that means is that each LED is controlled using a single asynchrounous data line (like UART), and it can also be daisy-chained, like. Plus, each LED can be powered directly from 5V without any current limiting resistor. Brightness and colour can easily be changed using the data signal.

To keep costs lower, and not make the PCB too thick, I decided not to have any battery onboard. Instead, the pcb has to be powered using 5V USB. Having a battery requires an extra DC-DC converter circuitry and potentially charging circuitry if rechargable battery is used. I think its worth the tradeoff especially since power banks are pretty cheap nowadays to make it portable if needed.

# Component Selection

For the cheapest generic components bought in bulk, LCSC is the the place to go. Aliexpress can offer better price but navigating the amount of scams and counterfiet components is just not worth it. So, all the components are surveyed on LCSC for this project. 

## RGB LEDs

This will be the most expensive part of this project due to the amount required. They are not as cheap as normal LEDs. So, for this, I sorted by price on LCSC and looked at its size, where smaller is better but not too small otherwise it will be impossible to solder by hand.

In the first revision I have opted for **OPSCO Optoelectronics SK6805-EC15**:

{% include gallery id="gallery-sk6805" %}

SK6805 is the internal RGB LED controller, and EC15 _(1.5mm x 1.5mm)_ is the package size. This was actually not the cheapest at $0.0605 each as I was not very familiar with RGB LEDs at the time and there were many confusing info online, so I just went with.

Its super tiny with the pins underneath which makes fixing soldering mistakes 100x much harder than it should, and so I in the next version I swapped it out with a much cheaper **XINGLIGHT XL-1615RGBC-2812B-S**:

{% include gallery id="gallery-2812" %}

Its the cheapest RGB LED at $0.0236 with a well known controller (2812B) and a small enough package. Its a different RGB controller with a slightly different data signal timings but its similar enough to sk6805 in which a simple change in software can make it compatible. A more common variant of this LED with the same controller is WS2812 which are more common with addressable LED strips with much higher brightness.

## Capacitive Touch Sensor

As discussed, I wanted a touch sensor. There are many ways to achieve this, but the most common way to implement directly on a PCB is capacitive touch sensor. To put simply, a capacitor is formed with two plates placed close together. The way the sensor works, the copper on the pcb forms one side of the capacitor and your finger becomes the other plate, and depending on the distance of your finger to the copper, it varies the capacitor value in the order of picoFarads (pF).

Then to measure the capacitance, there are many methods to do so. There are even plenty of dedicated touch controller ICs than can directly be used. However, in an attempt to make it as simple as possible, I opted to use STM32's hardware touch sensor peripheral. This means that I have to look for STM32 that has this peripheral as not all of them has it.

![TSC Compability](/assets/posts_assets/tactictoe_tsc-compatibility.png)

**TSC** or **Touch Sensing Controller** is STM32's implementation of hardware touch sensing peripheral. The way TSC works, it charges the sensor capacitor, and then uses it to charge a sampling capacitor. Since the sampling capcitor are typically much larger than the sensor capacitor, it will take multiple cycles to charge the sampling capacitor. Then, depending on how many cycles it took to charge the sampling capacitor, you can relatively measure the capacitance of the sensor capacitor, i.e. if a finger is close to the sensor. 

The equivalent circuit is shown below:

![TSC Charge Diagram](/assets/posts_assets/tactictoe_tsc-charge.png)

_Image taken from [STM32 wiki](https://wiki.st.com/stm32mcu/wiki/Introduction_to_touch_sensing_with_STM32)_

Different microcontrollers have their own implemention of measuring capacitance and I read somewhere that Microchip has a better implementation but I couldn't find where I read it. Its just worth mentioning because there are other methods of measuring capacitance and some might be better than the other depending on the application.

## Microcontroller

The second highest cost component is the MCU. To reduce the complexity of this project I've opted to use STM32 that I am already quite familiar with. So, I filtered STM32 series with hardware TSC and sort by price. The cheapest one I found is **STM32F042G6U6TR** for $0.8811 each.

It uses ARM Cortex-M0 with 48MHz clock speed. Plenty for this application. It also comes in UFQFN-28 package which is solderable with hot air or even hand solder. However, that also means we are quite limited in usable GPIO pins. Luckily, it has just enough pins to make this design possible. 

Whats unfortunate is the ROM size, which is only 32 KB Flash. This is plenty for most application but it does limit what I can do and became quite a limitation in firmware when implementing the AI. _Will be explained more in firmware article._

## Linear Regulator and Other Passives

Since the STM32 runs on 3.3V, a linear regulator is needed to step down the USB 5V to 3.3V. We dont need any fancy feature or the best noise performance, so the cheapest Linear Regulator from Ti was chosen (TLV74033PDBVR) in the standard SOT-23 package.

There are no requirements for the passives (resistor and capacitor), so the cheapest generic component with appropriate voltage rating are selected. 0603 package size are chosen for all since its small enough but also solderable by hand in case needed fixing. However, there is a recommendation by application note [AN4310](https://www.st.com/content/ccc/resource/technical/document/application_note/28/4b/40/7c/e6/68/44/9c/DM00087593.pdf/files/DM00087593.pdf/jcr:content/translations/en.DM00087593.pdf) to use specialised NPO or C0G for the best touch sensing sensitivity, but as I found, typical X7R MLCC are good enough for this application. 

# Version 1

![TTT PCB V1](/assets/posts_assets/tactictoe_v1-3d-front.png)

For this first version, I played it safe to get atleast something working since this is the first time I designed a touch sensor PCB. STM32 provides a lot of different [application notes](https://wiki.st.com/stm32mcu/wiki/Introduction_to_touch_sensing_with_STM32) to help with the design of the touch sensor. However, I realised that A lot of the application notes is for high sensitivity or off-board touch sensing application. 

I found the most useful application note for the design of touch sensor on a PCB is [AN4312](https://www.st.com/content/ccc/resource/technical/document/application_note/46/39/d6/92/4a/4d/40/9f/DM00087990.pdf/files/DM00087990.pdf/jcr:content/translations/en.DM00087990.pdf). It provides a lot of guidlines on how the sensor should be routed and some general dimensions to be followed. Many of the values and guidlines ties back to fundamentally how to maximise the capacitance on the touch pad while minimising capacitance of the traces to the MCU. 

## Stackup

Its practically impossible to route this board with only 2 layers knowing we need GND, 5V, Data IN/OUT on every corner of the LEDs. You might be able to but its not worth my time to optimise it to that level unless I have to. 4 Layers are the way to go, especially now the price are not that expensive compared to 2 layer boards with JLCPCB which is the service I will be using to manufacture the PCBs.

For the stackup, unlike your typical PCB design where you want signals to be as close to ground, the opposite is required for the touch sensing pads. This is because you don't want majority of the capacitance formed by the coupling between the ground plane and touch pad. If so, your finger will barely change the capacitance and it will be much difficult to detect the change in capacitance. 

However, having no plane at all can introduce noise suceptibility to the sensor. Thats why, it is recommended to have thinly **hatched** plane underneath the touch sensors to balance between noise and sensitivity of the sensor. The GND plane is shown below:

![TTT PCB GND Plane](/assets/posts_assets/tactictoe_v1-gnd-plane.png)

Therefore, in the first version, the layer stackup is:

1. Signal Layer (Touch Sensors)   
2. Signal Layer (LED data signal routing)
3. GND Plane
4. +5V Plane 

The touch sensors are distanced from the planes. The GND and 5V planes are closely placed to increase interplane capacitance and reduce impedance. 

Having 5V at the bottom with exposed copper QR code graphic was not a good idea and the 5V and GND was swapped in the second version.

## Touch sensor with RGB Routing

As you can imagine, the capacitive touch sensor pads which are typically in the range of pF, can be very sensetive to noise. This is a huge a problem when you have a fast digital signal for the RGB LEDs that are very close to the pads. To minimise the crosstalk between the capacitive sensor and the LED data signal, the smallest trace width was used and I optimised it to have minimal area overlapping between the two copper surfaces since im routing them in different layers:

![TTT PCB GND Plane](/assets/posts_assets/tactictoe_v1-sig2.png)

The way the sensor and LED is connected is by having three identical rows of sensors and routing. The three rows each has its own LED signal channel connected in daisy-chain configuration. So then it becomes this weird shape which i think is pretty cool looking. Though you can't really see it in the actual pcb because its in the second layer.

## Graphics

The back of the card has my contact details with a QR code to my website. Silkscreen are pretty plain, a much cooler looking is reflective copper, so you can get like 2 tone color for PCB graphics. To do this you need to create a custom shape on the copper Mask layer which are typically used to define exposed copper pads. You can do this very easily in Kicad using Image Converter tool and selecting F.Mask layer in the output. 

![TTT PCB Real Back](/assets/posts_assets/tactictoe_v1-pcb-back.jpg)

It doesnt look very different on camera since in the first version it uses lead-free HASL finish, so it looks silvery-ish. On the final version I used ENIG (Gold Plating) so that it looks much nicer and contrasts the normal white text.

Another big thing to notice is that the silkscreen around vias were removed as part of the manufaturing process because I chose the option of plugged via rather than the more expensive 'filled & capped' vias. The former is typically used for via-in-pad for a flat surface finish.

## Result

![TTT PCB Real Back](/assets/posts_assets/tactictoe_v1-pcb-game.gif){: .align-center}

The unspoken rule of any pcb design is never assume that the first version will work. Indeed, I made quite a few mistakes in the first design. Though luckily I did add quite a few test points and after some bodging I did manage to make it work.

![TTT PCB Real Front](/assets/posts_assets/tactictoe_v1-pcb-front.jpg)

The board was bit dusty when I took the picture. As you can probably see from the dark marks on the board, that shows how much the board has been 'cooked' in order for me properly solder it. The LEDs were a pain to solder even with a hotplate and hot air gun. I spent almost a whole day just to solder one board, and even that was pretty lucky because I failed to solder any other V1 boards completely.

In my design, I used via-in-pad for the RGB LEDs to make the routing much simpler with less footprint area. This comes at the downside that the vias can suck in solder paste when soldering the SMD pads. I wasnt expecting this to be a huge issue with manual soldering as I can fill in the vias first with solder, and then add the solder paste. But even with that extra step, some vias are not fully filled, so it sucked more solder resulting in uneven and unconnected solder joints.

To fix this, its simple, just use plugged & capped via if you are doing via in pad for SMD components. _Lesson learnt_


# Version 2

![TTT PCB V2 PCB](/assets/posts_assets/tactictoe_pcb-front-back-compressed.jpg)

Version 2 includes the changes mentioned previously with other minor changes:

In terms of the design:

- Replaced RGB LEDs
- Swapped GND and +5V Planes 
- Fixed wrong PWM pin fault in Version 1 
- Remade top PCB graphics
- Other minor fixes

![TTT PCB V2 All layers](/assets/posts_assets/tactictoe_pcb-all-layers.png)

Apart from that, the design are practically the same.

## Manufacturing Options

At the time Summer was almost ending, I decided I have to finish the project quickly. I dont have enough time to make another prototype version. So, I took a gamble and make the changes and fully commit to manufacture the final version (V2) without testing it first.

Considering that I took almost one whole day to solder just one board, I thought it might just be better that for this final version, I should try out JLCPCB Assembly service. Although, it will be a bit more expensive, it would save me so much time and the quality of solder will be much more consistent especially since I will be giving them away.

**Pro Tip:** To keep costs low, make sure to use basic components on JLCPCB assembly otherwise there will be extra setup costs. There are also some components considered difficult to assemble which will add even more fee on top. One example is the USB-C connector I used. So, I decided to solder them on my own. 

> Fun fact: LCSC and JLCPCB are kind of the same company so the components available on LCSC are likely to be also available for JLCPCB assembly service.

I also wanted to get the more expensive manufacturing options, ENIG finish, Epoxy Filled and Capped Vias, it will further add more to the cost. With all these in mind, I can't mess up this final version otherwise it will all go to waste.

After placing the order and waiting for 2 weeks anxiously, it arrived. I soldered on the USB-C connector and **it just works**. I was so happy that it works and how clean looking it turned out. The soldering job is far better than what I can do (obviously).

## Total Costs

Here comes one of the most frequently asked question when I showed the PCB to people: 

**How much does it cost to make one?**
{: .text-center}

Before I get into the cost, I have to address something:

At the start I mentioned that cost was one of the biggest factors but at the end, I chose the expensive way of manufacturing the boards. Sure, I could spend less money by optimising the board for the lowest cost possible and soldering each board myself, but that will take significantly more time. Plus, I recently got some extra money from my summer internship, so I call the extra costs as investment for myself.

Now that's settled,

The total cost is: `£213.09 + £56.89 (tax) = £270` for 50 boards, which is `£5.4` per board. Yeah, its not cheap. I made a lot to reduce per-board cost and 50 was the sweet spot. Also, I made plenty so that I dont have to make more in the future.

![PCB Costs](/assets/posts_assets/tactictoe_costs.png)

The assembly cost was suprisingly quite reasonable for high quantities.

I considered buying the components separately but even just considering the shipping costs, its almost the same cost as the setup cost for the assembly. If you are soldering it manually, you need a stencil and that adds costs too. So eventually, I think the assembly cost was not that bad. 

# Design Files

All the design files both V1 and V2 are open source and they are on my github repo. Feel free to check it out if you are interested.

Link to the github repo: [**github.com/Amrlxyz/tactictoe-pcb**](https://github.com/Amrlxyz/tactictoe-pcb) 
{: .notice}





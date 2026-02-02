---
title: "Tactic-toe (Firmware)"
# last_modified_at: 2017-03-09T16:20:02-05:00
excerpt: "Optimising tactic-toe for a microcontroller"
categories:
  - Projects
tags:
  - PCB Design
  - Python
  - C/C++
  - Embedded Systems

header:
  # image: /assets/posts_assets/tactictoe_pcb-gameplay-compressed.gif
  teaser: /assets/posts_assets/tactictoe_pcb-gameplay-compressed.gif
---

Please first see the [overview article](../tactictoe-overview) to learn about tactic-toe
{: .notice}

# Firmware

After using Python, the next step is to make it run on a microcontroller. Unlike running the algorithm on a PC, an MCU is much more limited in RAM/ROM resources. Not only that, the microcontroller also has to interface the RGB LEDs and touch sensors.

This article covers how the firmware for the PCB version of tactic-toe is designed. 

# Tactic-toe AI

I encourage you to first read the [software article](../tactictoe-software) to get the full context. 

In short, I solved tactic-toe by brute forcing exploring every possible game state and from the end game state, work backwards to figure out the best moves. I decided not to manually code a custom strategy algorithm becuase it doesnt guarantee that it will play perfectly. Plus, the brute force method generalizes better. 

Few key facts from the software section:

- There are **16030** unique game states (after symmetry optimisation)
- Each state has been assigned a score from 0 to ±16
  - 0 means its a draw
  - +16 means X will win 
  - -16 means O will win
  - Anything in between determines the amount of moves for the player to win
  - For example: +13 means X will win in 3 moves

To find the best move for a given state, just lookup the all next possible states (child states) and choose the child state with the most favourable score.

## Limitations

The chosen MCU is STM32G042G6U6 with only **6KB RAM** and **32KB Flash** (ROM).

To make the AI, your first thought might be to just store the best move for all the possible states into flash. So, for every state, you need to store the state and the best move itself. The AI algorithm then just have look for the state in memory and just grabs the best move.

This wouldn't be a problem if you have enough memory to store all of the precalculated states and moves. Given that there is 16030 states, and 32KB of flash, you need to store a single state-move pair in under 2 bytes to fit. Thats not even accounting for the rest of the program size which easily will take up a few KBs atleast.  

## Encoding states in the minimum amount of bytes

So how would you 



## Minimax


## Move Randomizer


# RGB LEDs

## PWM to send data

# Touch Sensor

## Touch Sensing Controller (TSC)


## interrupts with TSC



# Other stuff




## USB Support


## BOOT0 Programming bit

When I first programmed the board with an STlink, it works as expected. However, when I disconned the STlink and power it with usb, it just stopped working. I thought it might have been a usb-c connector issue but when I measure the voltage rails, the 5V and 3V3 rails are both fine. So, after some troubleshooting, I found out the issue.

Typically with most STM32 microcontrollers with higher pin counts, you will get BOOT0 and BOOT1 pin that determines from which memory it should boot from. It allows user to boot from user flash, system memory or even from embedded SRAM. 

One practical use of this is it allows for the MCU to boot from a bootloader and allows for reprogramming of flash. Then, after the new program is flashed, you can boot normally from flash memory. In fact, this is how its typically done with STM32 dev boards if you are trying to program the board without using a debugger.

This is typically not a problem, but since the F042 that im using had is in a small package, there are no BOOT pins. Instead it uses special bits to determine boot modes. Specifically for the F042 series I'm using, there 3 programmable bits to select boot option:

- nBOOT1
- BOOT_SEL
- nBOOT0

![F042 BootSel](/assets/posts_assets/tactictoe_bootsel.png)

_image taken from [AN4080](https://www.st.com/resource/en/application_note/an4080-getting-started-with-stm32f0x1x2x8-hardware-development-stmicroelectronics.pdf)_

Despite BOOT0 pin shown in the table, there was no BOOT0 pin on the actual IC. My guess that its not available for lower pin count packages.

Turns out, the F042 IC comes with all the BOOT bit set to 1. This resulted in the mcu always defaults to booting to system memory when powered on. System memory contains the embedded bootloader by STM32, whereas my program was flashed to the main flash memory.

The fix is simple, you just have to use STM32CubeProgrammer to set the BOOT_SEL bit to 0:

![F042 BootSel](/assets/posts_assets/tactictoe_cubeprogrammer.png)

Now, after being powered by USB, it boots to main Flash and it works as expected. It's only a one time thing and the next time its flashed using CubeIDE, the BOOT bit doesn't get reset.

# Source Code

All of the code is open source on my github.

If you are interested in adding new features, feel free to contact me or just make a PR.   




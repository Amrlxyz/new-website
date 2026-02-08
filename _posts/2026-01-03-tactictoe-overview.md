---
title: "Tactic-toe (Introduction and Overview)"
# last_modified_at: 2017-03-09T16:20:02-05:00
excerpt: "tic-tac-toe but with a 3-mark limit"
categories:
  - Projects
tags:
  - PCB Design
  - Python
  - C/C++
  - Embedded Systems

header:
  image: /assets/posts_assets/tactictoe_banner.jpg
  teaser: /assets/posts_assets/tactictoe_banner.jpg
---


---

# Introduction

What if Tic-tac-toe only had 3 maximum marks per player and after the fourth is placed, the first mark is removed? With this simple change, the game is now infinite where the same state can repeat, and the board will never be full.

Will it still be a solved game like the normal Tic-tac-toe where if both players played it perfectly, it will always be a draw?

> A solved game is a game whose outcome (win, lose or draw) can be correctly predicted from any position, assuming that both players play perfectly.

It was a question I tried to answer last summer as a side project. I call this version of infinite tic-tac-toe as: **"Tactic-Toe"**.

This was actually inspired by a game device I saw called "Tic-tac-toe Bolt" during my YT shorts doom-scrolling session.

{% include video id="6RaZqzoV2So" provider="youtube" %}

This video basically explains how the game works.

There are other similar videos on this device that had inspired this project. What make it more interesting to me is that, in each of its comment sections there are always comments arguing if the game is solved, whether:

1. First player always wins.
2. Second player always wins.
3. Always draw since second player can always defend.

As an engineer, rather than blindly trusting the comments, I spent an unreasonable amount of time just to scratch this itch in my brain to solve the game.

What do you think? Before I reveal my results, have a go with a web version i made, play it and take a guess. I made the AI play perfectly so good luck ;) >> _The answer is at [the end](#results) of the article._

**Play tactic-toe online:**

[tactic-toe.pages.dev](https://tactic-toe.pages.dev/){: .btn .btn--success .btn--large target="_blank"}
{: .text-center}

---

# The Project

Last summer, I decided that I wanted to do a fun side project that I can showcase to people and this game fits perfectly. The idea is simple enough that most people can understand.

I've also been inspired by PCB name cards people made I've seen online but haven't really got an idea for my own design as I don't want to just copy others' design as it meant to be personal. 

As I am graduating next Summer 2026, it'll be nice to have a cool name card that I can just give to people for networking as I need to find a job soon.

So, the goal of the project is to first solve the game. Then, make a tactic-toe game on a PCB name card with an AI that you can play against. Finally, make the play the game perfectly to destroy whoever dares to challenge it - HAHAHA.

![tactic-toe pcb](/assets/posts_assets/tactictoe_pcb-front-back-compressed.jpg){: .align-center}

Therefore, I will be separating the project into **3 different aspects** as each deserves their own article (Software, Hardware, Firmware). 

## Software

![tactic-toe online](/assets/posts_assets/tactictoe_web-game.png){: .align-center}

Having an unlimited game possibility makes the problem much harder. The typical minimax algorithm would fail and adding a search depth limit still has the problem of having how do you evaluate a game state? Unlike chess, there are no theory to say, for example, middle are better than corners, and you can't just make assumptions if you want it to play perfectly. (Un)surprisingly, **LLM AI tools kept confusing itself** with the assumptions of the normal tic-tac-toe. So, how would you make the AI, and how do you verify that it is playing perfectly?

Software article: **[Link](/projects/tactictoe-software/)** 
{: .notice}

## Hardware

![tactic-toe pcb design layers](/assets/posts_assets/tactictoe_pcb-all-layers.png){: .align-center}

The goal with the PCB to be as simple and as cheap as possible, so a single IC was used: **STM32F042G6** which has a touch sensing peripheral and almost all its GPIO pins are used, maximizing the utilization of a single MCU. Capacitive touch sensors are sensitive to noise as it's relying on parasitic capacitance of a copper trace. In this design, it's much more challenging when you also have a high frequency PWM signal for the RGB LEDs that are right on the sensor itself.

Hardware article: **[Link](/projects/tactictoe-hardware/)** 
{: .notice}

## Firmware

![tactic-toe pcb gameplay](/assets/posts_assets/tactictoe_pcb-gameplay-compressed.gif){: .align-center}

After creating the optimal AI in python, having to convert the program into C and to run it on a 6KB/32KB (RAM/ROM) microcontroller itself is a different challenge. Tactic-toe has **16030 unique game states** even after full optimization. Simply storing the best moves for every state in ROM is just impossible since you need at least 3 bytes to represent a unique state as I discovered. Rather than manually figuring out the best strategy, I managed to make the AI play perfectly without hard-coding specific strategies/patterns.

Firmware article: **[Link](/projects/tactictoe-firmware/)** 
{: .notice}

---

# Results

I found out that it is actually a solved game. The **first player will always win** if played perfectly, and it takes at least **13 moves** or _less_ from the start to win, shown below:

![tactictoe optimal moves](/assets/posts_assets/tactictoe_optimal-moves.gif){: .align-center}

> If you are curious, open the console on the [online tactic-toe][online-tactictoe] (Ctrl+Shift+I) and it will show you the state score after a move is placed. A positive value means X is winning, negative means O is winning, and 0 is a draw.

To learn more on how I got to the result and how I created the AI, read more in the [**software**](#software) section.

[online-tactictoe]: https://tactic-toe.pages.dev/ "Optional"
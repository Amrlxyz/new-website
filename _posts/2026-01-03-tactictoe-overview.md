---
title: "Tactic-toe (Overview)"
# last_modified_at: 2017-03-09T16:20:02-05:00
excerpt: "tic-tac-toe but with a 3-mark limit"
categories:
  - Blog
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

What if Tic-Tac-Toe only had 3 maximum marks per player and after the fourth is placed, the first mark is removed? With this simple change, the game is now infinite where the same state can repeat and the board will never be full.

Will it still be a solved game like the normal Tic-Tac-Toe where if both players played it perfectly, it will always be a draw?

It was a question I tried to answer last summer as a side project. I call this **"Tactic-Toe"** as it doesn’t really have an official name.

This was actually inspired by a game device I saw called "Tic-Tac-Toe Bolt" during my YT shorts doom scrolling session.

{% include video id="6RaZqzoV2So" provider="youtube" %}

This video basically explain how the game works.

There are other similar videos on this device that had inspired this project. What make it more interesting to me is that, in each of its comment sections there are always comments arguing if the game is solved, whether:

1. First player always win
2. Second player always win
3. Always draw since second player can always defend

As a an engineer rather than blindly trusting the comments, I spent an unreasonable amount of time just to scratch this itch in my brain to solve the game.

What do you think? Before I reveal my results, have a go with a web version i made, play it and take a guess. I made the AI play perfectly so good luck ;) >> _The answer is at [the end](#results) of the article._

**Play tactic-toe online:**

[tactic-toe.pages.dev](https://tactic-toe.pages.dev/){: .btn .btn--success .btn--large target="_blank"}
{: .text-center}

---

# The Project

Last summer, I decided that I wanted to do a fun side project that I can showcase to people and this project fits perfectly. The main idea is simple enough that most people can understand.

I've also been inspired by PCB name cards people made I've seen online but havent really got an idea for my own design as I dont want to just copy others' design as it meant to be personal. 

As I am graduating next Summer 2026, it'll be nice to have a cool name card that I can just give to people for networking as I need to find a job soon.

This game is my best idea to be made as my PCB name card, with the perfect AI running on it, so it can be played as a single player.

![tactic-toe pcb](/assets/posts_assets/tactictoe_pcb-front-back-compressed.jpg){: .align-center}

I will be separating the project into **3 different aspects** as each deserves their own article.

## Software

![tactic-toe online](/assets/posts_assets/tactictoe_web-game.png){: .align-center}

Having an unlimited game possibility makes the problem much harder. The typical minimax algorithm would fail and adding a search depth limit still has the problem of having how do you evaluate a game state? Unlike chess, there are no theory to say, for example, middle are better than corners and you cant just make assumptions if you want it to play perfectly. (Un)surprisingly, **LLM AI tools kept confusing itself** with the assumptions of the normal tictactoe. So, how do I make the AI and how do I verify that its playing perfectly? 

Software article: **link** 
{: .notice}

## Hardware

![tactic-toe pcb design layers](/assets/posts_assets/tactictoe_pcb-all-layers.png){: .align-center}

The goal with the PCB to be as simple and as cheap as possible, so a single IC was used: **STM32F042G6** which has a touch sensing peripheral and almost all its gpio pins are used, maximising the utilisation of a single MCU. Capacitive touch sensors are senstive to noise as its relying on parasitic capacitance of a copper trace. In this design, its much more challeging when you also have a high frequency PWM signal for the RGB LEDs that are right on the sensor itself.

Hardware article: **link** 
{: .notice}

## Firmware

![tactic-toe pcb gameplay](/assets/posts_assets/tactictoe_pcb-gameplay-compressed.gif){: .align-center}

After creating the optimal AI in python, having to convert the program into C and to run it on a 6KB/32KB RAM/ROM microcontroller itself is a different challenge. Tactic-toe has **16030 unique game states** even after full optimisation. Simply storing the best moves for every state in ROM is just impossible since you need atleast 3 bytes to represent a unique state as I discovered. Rather than manually figuring out the best strategy, I managed to make the AI play perfectly without hard-coding specific strategies/patterns.

Firmware article: **link** 
{: .notice}

---

# Results

Its actually a solved game. The **first player will always win** if played perfectly and it takes atleast **13 moves** or less from the start to win, shown below:

![tactictoe optimal moves](/assets/posts_assets/tactictoe_optimal-moves.gif){: .align-center}

If you are curious, open the console on the [online tactic-toe][online-tactictoe] (Ctrl+Shift+I) and it will show you the state score after a move is placed. A positive value means X is winning, negative means O is winning, and 0 is a draw.

To learn more on how I got to the result and how I created the AI, read more in the [**software**](#software) section.



[online-tactictoe]: https://tactic-toe.pages.dev/ "Optional"
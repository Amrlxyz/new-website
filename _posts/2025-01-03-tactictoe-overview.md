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
  image: /assets/posts_assets/tactictoe_web-game.png
  teaser: /assets/posts_assets/tactictoe_web-game.png
---

# Introduction

What if Tic-Tac-Toe only had 3 maximum marks per player and after the fourth is placed, the first mark is removed? With this simple change, the game is now infinite where the board will never be full.

Will it still be a solved game like the normal Tic-Tac-Toe where if both players played it perfectly, it will always be a draw?

It was a question I tried to answer last summer as a side project. I call this **"Tactic-Toe"** as it doesn’t really have an official name.

This was actually inspired by a game device I saw called tictactoe bolt during my doom scrolling session of youtube shorts.

{% include video id="6RaZqzoV2So" provider="youtube" %}

Its always fun looking at comments people arguing online lol.

Theres comments arguing the optimal outcome whether:

1. First player always win
2. Second player always win
3. Always draw since second player can always defend

What do you think? Before I reveal my results, have a go with a web version i made, play it and take a guess. I made the AI play perfectly so good luck ;)

Play tactic-toe online: [tactic-toe.pages.dev](https://tactic-toe.pages.dev/){:target="_blank"}

_The answer is at [the end](#results) of the article._

# The Project

As a an engineer rather than blindly trusting the comments, I spent an unreasonable amount of time just to scratch this itch in my brain to solve the game.

Then after that, I also made a PCB version of the game which also doubles as my name card, so I can give it to people to play it in real life, with the perfect AI running on it.



As I am graduating next Summer 2026, it'll be nice to have a cool name card that I can just give to people for networking as I need to find a job soon.

So, last summer, I decided that I wanted to do a fun side project that I can easily showcase to people and this project fits perfectly. Its a simple enough project that most people can understand as compared to my previous more technical projects that requires technical context.

So I will be separating the project into 3 different aspects as each deserves their own article.

## Software

How do I develop the AI and how do I verify its playing perfectly? Its not as easy as it seems: the typical minimax algorithm quickly hits a wall and **AI tools kept confusing itself** with the assumptions of the normal tictactoe. I had to actually come up with my own solution. 

Software article link

## Hardware

The goal with the PCB to be as simple and as cheap as possible, so a single IC was used: **STM32F042G6** which has a touch sensing peripheral and almost all its gpio pins are used, maximising the utilisation of a single MCU. Designing capacitive touch sensors are challeging where you are using parasitic property of traces to your advantange.

Hardware article link

## Firmware

Although after creating the optimal AI, having to convert the program into C and to run it on a 6KB/32KB RAM/ROM microcontroller itself is a challenge. This version of tictactoe has **16030 unique game states** after full optimisation. Simply storing the best moves for every state in ROM is just impossible since you need atleast 3 bytes to indentify represent a unique state.

Firmware article link

# Results

It is a solved game with the **first player will always win** if played perfectly.

It takes atleast 13 moves or less from the start for the first player to win, shown below:

![tactictoe optimal moves](/assets/posts_assets/tactictoe_optimal-moves.gif){: .align-center}

If you are curious, open the console on the [online tactic-toe][online-tactictoe] (Ctrl+Shift+I) and it will show you the state score after a move is placed. A positive value means X is winning, negative means O is winning, and 0 is a draw.

To learn more on how I got to the result and how I created the AI, read more In the [**software**](#software) section.



[online-tactictoe]: https://tactic-toe.pages.dev/ "Optional"
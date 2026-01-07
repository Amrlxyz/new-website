---
title: "Tactic-toe (Software)"
# last_modified_at: 2017-03-09T16:20:02-05:00
excerpt: "How tactic-toe was solved"
categories:
  - Projects
tags:
  - Python
  - AI

header:
  image: /assets/posts_assets/tactictoe_web-game.png
  teaser: /assets/posts_assets/tactictoe_web-game.png
---


---

# Intro

Please first see the [overview article](../tactictoe-overview) to learn about tactic-toe
{: .notice}


I will first go over the typical algorithm used for making an AI for a normal tic-tac-toe. 


# Minimax

The most common way of calculating the best move in a turn based game is with the **minimax** algorithm.

To put it simply, the algorithm works by having one player trying to maximise the score, while the opponent tries to minimise the score _(thus "minimax")_. It goes through to each possible moves, look ahead and evaluates the next possible moves by the opponent. The algorithm assumes both player chooses the most favourable move in a situation and so the resulting end state is propagated back up to inform if the move results in a good position for the current player.

I tried my best to explain it in my own words but I think this video below explains it well with a chess engine example.

{% include video id="l-hh51ncgDI" provider="youtube" %}

> Its crazy how much optimisation that goes into making chess engines, and yet chess has still not yet been solved even with super fast computers.


# First Attempt

If you've ever taken a course on an AI, chances are you might have used **minimax** to make an AI for tictactoe. Thats indeed what I did when I took CS50ai an online course by Harvard. I'd recommend everyone reading this to take on that course. It covers all kinds of AI types not just LLMs.

I started it off with the same code from the CS50ai course. So, I have the python code for a tictactoe game with a basic minimax AI algorithm.

The code here - recursive 

-- insert code here --

Then, I modified the game logic for it to become tactic-toe (3 maximum marks), and ran the same minimax algorithm and crashes due to too much recursion calls.

## Issue 1 - Infinite possible games

Since now the game can be repetetive and never ends, 





# Optimisation







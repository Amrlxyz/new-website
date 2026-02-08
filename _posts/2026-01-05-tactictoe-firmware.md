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

## Encoding game state in the minimum amount of bytes

To see if its even possible to store all the precalculated moves in under 32KB, we have to first figure out how many bytes do we need to store a game state.

### Attempt 1

Tactic-toe board has 9 squares and each of the square can either be X or O. However, each of the symbols can only stay on the board for only 3 moves. So, lets assign a number to the amount of "life" left for a symbol, and use (+ve) for X and (-ve) for O. 

Therefore, a square can either be ±3, ±2, ±1, or 0 (empty).

The way I think about this representation is when a move is played, all of the current player's symbols on board will all be reduced by 1 and if it goes to 0 it becomes an empty square. This is in fact how i programmed the game logic. 

As a result, there are 9 squares, each with 7 possible states. The 7 different states require atleast 3 bits (2^3 > 7). Then, Multiplied by 9 squares, 27 bits minimum, or **4 bytes** (since memory are in bytes rather than in bits).

Not to forget, you also need to store who's turn it is to move, but that can be fit in one of the extra empty bit.

With 4 bytes per state, you can only fit **8000 states** in a 32KB storage. Surely, we can do better than this.

### Attempt 2

In the previous attempt, I stored each of the squares content where at least 3 of them will be empty which is a waste of memory space. So, how about instead, we store the position of each symbol (with its corresponding "life") since there will only be one of each.

There are only 6 symbols (3 life for each X and O) positions to be stored. Each position can only be in either 9 squares. You would need only 4 bits to store 9 unique positions, so 1 byte can store positions for 2 symbols.

Therefore, the entire board can be stored with only 3 bytes.

For example, given a board below:

``` c
// -3 |   | -1 
// ---+---+---
//    | 3 | 1  
// ---+---+---
//    | 2 | -2  

uint8_t positionArr[9];

positionArr[0] = 0; // -3 
positionArr[1] = 8; // -2
positionArr[2] = 2; // -1
positionArr[3] = 5; // 1
positionArr[4] = 7; // 2
positionArr[5] = 4; // 3
```

The game state can then be encoded as shown below: 

``` c
uint8_t encodedBytes[3];

encodedBytes[0] = (positionArr[0] & 0x0F) << 4 | positionArr[1];
encodedBytes[1] = (positionArr[2] & 0x0F) << 4 | positionArr[3];
encodedBytes[2] = (positionArr[4] & 0x0F) << 4 | positionArr[5];
```

This approach saves 1 byte from the previous method. That doesnt sound a lot (from 4 bytes to 3 bytes) but for the same memory space we can +33% more states can be stored.

One caveat with this, is that the current player's turn is not encoded. So there is no way to differentiate who's turn it is given the encoded bytes. You could add 1 more byte just for it but instead I used a better way which will be explained in the minimax section. 

### Theoretical Value

With 16030 unique states, in theory, you only need 14 bits (2^14 = 16384) to represent each unique state. However, there is no easy way to encode/decode a given state to an single unique index value. 

There might be a way to do so but that might just involve super complex compression algorithm or something idk but im just saying its theoretically possible. One possiblity is the encode/decode function will be so long that in the end it takes up more memory to store the function.

There might also be another more clever and easy way to encode the state in less than 3 bytes but so far the ones I found are just not worth the extra effort to save a few more bits where in the end 32KB will just not going to be able to fit all 16030 different states.

## Minimax

Given now the best attempt to store a board state requires a minimum of 3 bytes each. It is pretty much impossible to just store all of the states with their corresponding best moves. I have actually thought of adding an external 1MB EEPROM just to store all the moves but I found a better solution.

Remember our old friend? Minimax.

The main issue with minimax (previously explored in software artile) is that there are no way to evaluate a given position other than checking for a winning condition. Thus, the algorithm simply goes in a loop infinitely. Terminating with a depth limit caused incorrect evaluation and non-optimal moves to be made. 

Now however, we have all the possible states each with their own corresponding scores. So, what if we use the precalculated states' scores as the score to evaluate a state in minimax. This would work obviously with all the state scores, but since we can't store all states in memory.

So, the strategy I came up with is to only store the states that are far from winning (most amount of moves to reach a win), since they require a lot of depth to be searched fully. 

This code snippet shows how this works:

``` c
uint8_t transformIndex;
TTT_StateEntry *foundEntry = searchEntry(board, &transformIndex);

if (foundEntry != NULL){
    if (player == TTT_PLAYER_X){
        if (foundEntry->score_x != 0x7F)
            return foundEntry->score_x + depth;
        else
            Error_Handler();
    }
    else {
        if (foundEntry->score_o != 0x7F)
            return foundEntry->score_o - depth;
        else
            Error_Handler();
    }
}
```

After minimax checks for winning conditions, it runs this code. It first checks if there is a precalculated score for the the board is stored in memory. If there is, return the score plus its depth (to prioritize moves that gets quicker to the given state). `Ox7F` was intentionally chosen as "invalid" number because the score can range from -16 to +16 including 0.

After testing this and fixing a lot of bugs along the way, at one point it was working quite well but it was still failing some edge cases for some reason. Debugging this on a microcontroller was definitely a challenge.

### Final Implementation

I changed a bunch of stuff at the end and honestly, I am not sure which exactly that fixes the edge cases problem. But, I think what fixes it was the fix I made for the python code that generates the precalculated moves and exports it to a header file.

To put simply, there was a bug when I am filtering for the states that will be exported. I had 2 functions, one that detects for duplicates and one filters states based on its scores. I messed up the order the function where it caused some of the states filtered before it should and causing incomplete state data.

The output of the python file is shown below:

```
Optimising...
1. Removed terminal states -> 16030 to 14518 (-1512)

2.0 Removed states that is 0 moves to winning -> 14518 to 14518 (-0)
2.1 Removed states that is 1 moves to winning -> 14518 to 9676 (-4842)
2.2 Removed states that is 2 moves to winning -> 9676 to 8736 (-940)
2.3 Removed states that is 3 moves to winning -> 8736 to 6874 (-1862)
2.4 Removed states that is 4 moves to winning -> 6874 to 5999 (-875)
2.5 Removed states that is 5 moves to winning -> 5999 to 4299 (-1700)
2.6 Removed states that is 6 moves to winning -> 4299 to 3829 (-470)
2.7 Removed states that is 7 moves to winning -> 3829 to 3062 (-767)
2.8 Removed states that is 8 moves to winning -> 3062 to 2638 (-424)
2.9 Removed states that is 9 moves to winning -> 2638 to 2325 (-313)

Found 82 duplicate pairs (same board, diff turn)
3. Removed duplicate states -> 2325 to 2284 (-41)
```

The final amount of states that was stored is actually only **2284 states**. 

Each of the states are stored in an array of 6 bytes, and exported to a header file `tactictoe_states.h`:

- Bytes 0 to 2 -> Encoded canonical state, 
- Bytes 3      -> Best move 4 MSB for x, 4 LSB for o 
- Bytes 4      -> Score for the state (X's turn)
- Bytes 5      -> Socre for the state (O's turn)
- The array is pre-sorted (big-endian) 

_Bytes 4 to 5 couldve actually been optimised to a single byte like how byte 3 splits 4 bits for X and 4 bits for O._ 

The code is compiled with **-Oz** (optimize for size). There is only 2.84 KB Flash left. The precalculated states array alone takes up **13.38 KB**, which is almost half of the 32 KB Flash.

![tactictoe binary size](/assets/posts_assets/tactictoe_binary-size.png)

### Result

Even with only 2284 states stored in memory, the AI plays perfectly. I verified it's moves by playing it against the online version of tactic-toe, which plays the game perfectly.

I can proudly say, I made the perfect tactic-toe AI run on a microcontroller with limited memory.

## Move Randomizer

On top of the making the AI play perfectly, I also wanted to make it play random moves if the move results in the same position symmetrically. However, there is only one best move stored in the precalculated states array. To get around this, we can utilise the symmetry to generate all of the possible best moves by comparing the resulting state.

The bigger problem is, there are no Random Number Generator (RNG) on this MCU. Unlike most high level systems you can call a function that will generate a true random number, the MCU dont have the hardware to do so.

What I did is, by using an unused ADC channel to sample voltage since it will be affected by electrical noise. In this case, I used the internal temperature sensor channel and sample its voltage, then use a modulo operator to get an index to choose random item from an array.

``` c
randSeed = HAL_ADC_GetValue(&hadc);

bestMove = symMoveList[randSeed % symMoveIndex];
```

# RGB LEDs



## PWM to send data



# Touch Sensor


## Touch Sensing Controller (TSC)


## interrupts with TSC



# Other stuff

There a bunch of other smaller things I considered and problems in the project but it would impossible to mention all of them. So here are some of my favourites.

## USB Support

Since I am using a USB C connector I thought it would be cool if it could also add an extra functionality,perhaps adding the option to re-flash the firmware through USB. Unfortunately the USB stack uses too much flash storage and it would impossible to have both USB software and the tactic-toe states at the same time.

Actually in the first version of the PCB, the USB data lines were connected to the MCU but after realising the flash storage issue, it was removed in the second version. Plus, its probably for the best since someone can view it as a security risk if they connect it to a PC. 

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




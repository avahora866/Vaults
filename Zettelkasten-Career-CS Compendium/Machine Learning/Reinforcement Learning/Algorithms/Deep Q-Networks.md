---
tags:
  - "#SECONDARY_BRANCH"
---
Q-Tables work well for simple grids (like Tic-Tac-Tow). but in the case of more complex situations it gets exponentially bigger.

Consider the game of Pac-man 
- The screen has pixels
- The ghosts move
- The fruit appears and disappears

If we tried to make a Q-Table row for every possible arrangement of pixels, the table would be larger than the memory of all computer on Earth combined.

Instead we can use a Neural Network (a brain) instead of a spreadsheet

- Old Way (Q-Table): "I am in State X. Let me look up row X in my book to find the answer."
- New Way (DQN): "I am in State X. I have never seen this exact situation before, but I can calculate an estimate based on similar situations I have seen."

We replace the Memory (Table) with a Function Approximator (Neural Network). The Neural Network looks at the screen (input) and spits out the precited Q-Values for Up, Down, Left and Right

- Q-Table: Memorizing the answer to every single multiplication problem ($2\times2=4, 2\times3=6... 89\times12=1068$). If you see a problem you haven't memorized, you are helpless.
- DQN: Learning the rules of multiplication. You don't memorize the answers; you learn how to calculate them. If you see $89\times12$, you figure it out.


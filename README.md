# 8×8 Game of Life in Digital Logic Sim

Conway’s Game of Life implemented as pure digital logic on an 8×8 LED-style grid, built in Sebastian Lague’s **Digital Logic Sim**.  
The board state is stored entirely in flip-flops / registers, and the next generation is computed using combinational logic inspired by a hardware framebuffer.

---

## Overview

This project is a small “hardware” implementation of Game of Life:

- **Grid:** 8×8 cells (64 bits of state)
- **Storage:** implemented using flip-flops and Latches
- **Edit mode:** manually toggle individual cells to draw an initial pattern
- **Run mode:** step or run the Game of Life evolution using clocked logic
- **No microcontroller / CPU:** just logic gates, counters and memory elements

I made this both as a fun logic design exercise and as a way to explore how something normally done in software can be realised with simple synchronous logic.

---

## Features

- 🎛 **Interactive cell editing**
  - Select a cell within the 8×8 input grid
  - Toggle cell states and store the pattern in hardware (by activating "Set")
- ⚙️ **Game of Life update logic**
  - For each cell, the next state is computed based on:
    - its current value, and
    - the count of its 8 neighbours
  - Uses the classic Game of Life rules:
    - Live cell only survives with 2 or 3 neighbours
    - Dead cell becomes live only with exactly 3 neighbours
- ⏱ **Clocked updates**
  - Mode input to enable clock which advances one generation at a time
- 🔁 **Stable storage**
  - The initial grid is held in flip-flops, so the pattern is kept while the simulation is running

---

## How it works (high level)

At the core of the design are three main pieces:

1. **Grid state memory**

   - The 8×8 grid is stored as 64 bits of state.
   - Internally this is implemented as 64 toggle latches.
   - The display and the Game of Life logic both read from this stored state.

2. **Edit vs Run data path**

   Each cell’s flip-flop input comes from a 64bit 2-to-1 multiplexer:

   - **Edit mode:** input driven by the “toggle cell” logic (so the user can set or clear cells).
   - **Run mode:** input driven by the Game of Life “next state” logic.

   A single mode signal selects between these two sources, so the board can be safely edited when stopped and then advanced while locked from direct user changes.

3. **Game of Life combinational logic**

   For each cell:

   - The current cell value and its 8 neighbours are fed into combinational logic cell that:
     - counts the number of live neighbours, and
     - applies the Game of Life rule to determine the next state.
   - The next state is stored back into the grid on the active clock edge.

This is implemented as:

- a grid of identical **cell modules** (one per board cell)

---

## How to run on your device

1. **Download the project**
   - Download the ZIP archive of this repository.
   - Decompress it on your computer.

2. **Move the project into Digital Logic Sim’s folder**
   - Locate the Digital Logic Sim v2 projects directory (for example on macOS):
     - `~/Library/Application Support/com.DefaultCompany.2DProject/Projects`
   - Move the decompressed project folder into this `Projects` directory.

3. **Open the project in Digital Logic Sim**
   - Launch **Digital Logic Sim v2** by Sebastian Lague.
   - On the start screen, click **Open Project**.
   - Select **“Game of life by Alex”** from the list.

4. **Open the main Game of Life circuit**
   - Inside the project, go to **Library → Game collection → Game chip**.
   - Open `Game` chip to view and run the 8×8 Game of Life circuit.


## Developed by Aleksandre Meskhi ##

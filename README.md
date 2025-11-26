# FPGA Flappy Bird (Verilog)

A hardware-based implementation of a "Flappy Bird" style obstacle game, written in Verilog for Altera/Intel DE-Series FPGA boards. This project utilizes a custom VGA controller for graphics, PS/2 keyboard for input, and 7-segment displays for score tracking.

> **⚠️ ACADEMIC INTEGRITY NOTICE**
> To comply with university academic integrity policies regarding the sharing of solution code:
> * The **source code (Verilog/VHDL) is NOT included** in this repository.
> * This repository provides the **compiled bitstream (`.sof` file)** only.
> * This allows users to download and play the game on the hardware without exposing the underlying implementation logic to future students.

## 🎮 Game Overview
Unlike the traditional gravity-based Flappy Bird, this version uses **directional controls**. The player controls the bird's altitude directly to navigate through moving gaps in the pipes.

* **Resolution:** 640x480 (VGA)
* **Frame Rate:** 60Hz (Driven by 25MHz pixel clock)
* **Input:** PS/2 Keyboard
* **Output:** VGA Monitor & 7-Segment Hex Displays

## 🛠 Hardware Requirements
To run the `.sof` file provided in the releases, you need the specific board used for compilation:

* **FPGA Board:** DE1-SoC (Cyclone V 5CSEMA5F31C6 Device)
    * *Note: The .sof file is compiled specifically for this Cyclone V chip. It will not work on the DE10-Lite, DE2-115, or other boards without recompiling the source.*
* **VGA Monitor:** Standard 640x480 capability (Connected to the board's VGA port).
* **PS/2 Keyboard:** Connected to the PS/2 port on the FPGA board.

## 📥 How to Load the Game
Since the source code is hidden, you do not need to compile anything. You simply "flash" the board.

1.  **Download** the `.sof` file from this repository in the output_files folder.
2.  Connect your FPGA board to your computer via USB (USB-Blaster).
3.  Open **Quartus Prime Programmer**.
4.  Click **"Hardware Setup"** and select your USB-Blaster.
5.  Click **"Add File"** and select the `final_flappy.sof`.
6.  Check the **"Program/Configure"** box.
7.  Click **Start**.

## 🕹 Controls

| Key | Action |
| :--- | :--- |
| **Spacebar** | **Start Game** (from Start Screen) or **Restart** (from Game Over) |
| **Up Arrow** | Move Bird **Up** |
| **Down Arrow** | Move Bird **Down** |
| **KEY[0]** | **Hardware Reset** (Active Low) |

## 🚀 Key Features (Architecture)
Although the source code is private, the system was designed using the following modular architecture:

* **VGA Controller:** Custom driver generating standard 640x480 timing signals (H-Sync/V-Sync).
* **Collision Detection:** Real-time logic checks for pixel overlap between the bird and pipes/boundaries.
* **LFSR Randomizer:** Uses a Linear Feedback Shift Register to generate pseudo-random pipe gap heights, ensuring no two games are exactly the same.
* **Rolling Score Display:**
    * During gameplay: Shows current score.
    * **Game Over:** Implements a custom "rolling" animation that scrolls the **High Score** (marked 'H') and **Last Score** (marked 'U') across all 6 HEX displays.
* **PS/2 Driver:** Decodes make/break codes from the keyboard to handle smooth input without "ghosting."

## 🧠 Finite State Machine (FSM)
The core logic follows a 3-state system:
1.  **START (00):** Waits for Spacebar. Displays start screen. Resets score.
2.  **PLAY (01):** Enables physics and pipe movement. Checks for collisions (pipes, floor, ceiling).
3.  **GAMEOVER (02):** Stops movement. Updates High Score if beaten. Activates the `rollingScoreDisplay` animation.

## 📸 Demo
<h3>Live Demo</h3>
<table>
  <tr>
    <th width="50%">VGA Monitor View</th>
    <th width="50%">FPGA Board View</th>
  </tr>
  <tr>
    <td><img src="gif\flappy.GIF" width="100%"></td>
    <td><img src="gif\score.GIF" width="100%"></td>
  </tr>
</table>

<h3>Game States</h3>
<table>
  <tr>
    <th width="33%">Start Screen</th>
    <th width="33%">Gameplay</th>
    <th width="33%">Game Over</th>
  </tr>
  <tr>
    <td><img src="gif\start_screen.png" width="100%"></td>
    <td><img src="gif\play_screen.png" width="100%"></td>
    <td><img src="gif\gameover_screen.png" width="100%"></td>
  </tr>
</table>
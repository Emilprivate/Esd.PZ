# Chess.com Autobot / Stockfish Assistant

A lightweight system to connect Chess.com live and bot games with a local Stockfish engine, displaying and optionally autoplaying the engine's best moves directly on the board.

## Features
- Sends your current game moves (UCI) to Stockfish via a Python+Flask backend
- Highlights engine recommendations with arrows and square outlines on Chess.com
- Optional native autoplay using PyAutoGUI
- Supports Bot and Human modes
- Configurable time controls: Bullet (depth 5), Blitz (depth 10), Rapid (depth 15)

## Prerequisites
- Linux OS
- Python 3.7+
- [PyAutoGUI](https://pyautogui.readthedocs.io/), Flask, flask-cors
- Stockfish installed and in `$PATH`
- Browser with Tampermonkey or similar userscript manager

![](resources/demo.png)

## Installation
1. Clone or download this repository.
2. In the project root, install Python dependencies:
   ```bash
   pip3 install flask flask-cors pyautogui
   ```
3. Ensure `stockfish` is available:
   ```bash
   stockfish --help
   ```

## Calibration & Backend Setup
1. Run the Python backend:
   ```bash
   python3 backend.py
   ```
2. Follow the prompts to click the **top-left corner** of top left square and the one to the right of it on your Chess.com board. So top left corner of the two top left squares.
3. The console will confirm your board origin and tile size.
4. The server listens on http://localhost:5000/bestmove

## Frontend (Userscript)
1. Install Tampermonkey (or Greasemonkey) in your browser.
2. Create a new userscript and replace its content with the code from `frontend.js`.
3. Ensure the `API` URL in the script is set to `http://localhost:5000/bestmove`.
4. Reload a Chess.com game (Bot or Live).

## Usage
- **Mode**: select `Bot` for vs. computer, `Human` for vs. live opponents
- **Time Control**: `1min Bullet`, `3min Blitz`, `10min Rapid` (adjusts engine depth & autoplay speed)
- **Configure** (Bot only): toggles Chess.com move notation to text mode
- **Start**: begins polling your move list, sends positions to backend
- **Autoplay**: when enabled, moves pieces automatically
- **Restart**: stops polling, clears highlights/arrows, resets state

## Troubleshooting
- If suggestions do not appear, open DevTools → Console → check for errors.
- Verify move list selector: run
  ```js
  document.querySelectorAll('.play-controller-moveList .node-highlight-content')
  ```
- For Human mode, test:
  ```js
  document.querySelectorAll('#live-game-tab-scroll-container .node-highlight-content')
  ```
- If PyAutoGUI drags land off-target, rerun calibration with clear prompts.

## License & Attribution
MIT License. Uses Stockfish under its own license. Frontend built as a userscript; backend powered by Python & Flask. Feel free to customize and improve.

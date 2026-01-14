# Hnefatafl (Copenhagen Variant)

A fully responsive, browser-based implementation of **Hnefatafl** (Viking Chess), specifically the "Copenhagen" variant. This project is built entirely in **Vanilla JavaScript** and **HTML5 Canvas** with zero external dependencies, featuring complex algorithmic rule validation and state management.

[**🔗 Play the Live Demo**][https://hnefataflweb.vercel.app](https://hnefataflweb.vercel.app)

## 📖 About the Game

Hnefatafl is an ancient asymmetrical strategy game. The **Defenders (White)** act as the "king's guard," attempting to escort the King to one of the four corner squares. The **Attackers (Black)** outnumber the defenders 2:1 and must capture the King before he escapes.

This implementation strictly adheres to the **Copenhagen Ruleset**, which introduces complex topological victory conditions such as "Encirclement" and "Shieldwalls."

## 🛠️ Technical Highlights

This project demonstrates core computer science concepts applied to game development:

### 1. Algorithmic Win Detection (Flood Fill / BFS)
To detect the **"Encirclement"** victory condition (where Attackers form a sealed ring around the Defenders), the game utilizes a **Flood Fill / Breadth-First Search (BFS)** algorithm.
* **Logic:** Rather than checking if the white pieces are trapped, the algorithm attempts to flow "oxygen" from the board edges inward.
* **Result:** If the recursive search cannot reach the King or any Defender from the edge, the state is flagged as an Encirclement victory for Black.
* **Application:** The same BFS logic is applied inversely to detect **"Exit Forts"** (a White victory condition where the King builds an impregnable bunker against the edge).

### 2. State Hashing & Cycle Detection
To prevent infinite loops and stalling, the engine implements a **Perpetual Repetition** rule.
* **Serialization:** The board state is serialized into a unique string hash string after every move.
* **Cycle Detection:** A `Map` data structure tracks the frequency of every board state. If a specific hash is encountered three times, the game enforces a loss on the stalling player (Defenders).
* **Undo/Redo Stack:** This serialization allows for a robust, memory-efficient history stack, enabling "Undo" functionality without deep-copying the entire game object repeatedly.

### 3. Vector-Based Capture Logic (Shieldwalls)
Standard captures use coordinate checking, but the "Shieldwall" rule (capturing a whole row of pieces against the edge) requires complex vector scanning.
* The engine scans all four board edges dynamically.
* It validates "bracketing" conditions (enemy pieces at both ends of a row) and "blocking" conditions (enemies directly in front of the row).

### 4. Custom Engine Architecture
* **Canvas API:** Rendering is handled via the HTML5 Canvas API for high-performance, 60FPS visuals.
* **Asset Management:** A custom loader handles asynchronous fetching of sprites and audio, ensuring resources are ready before the render loop begins.
* **Hybrid Rendering:** Users can toggle strictly between "Symbolic" (geometric shapes) and "Sprite" modes instantly without reloading the game state.

## 🎮 How to Play

### Controls
* **Click and Drag** to move pieces.
* **R** to Reset the board.
* **U** to Undo a move.
* **S** to Toggle Sprites/Symbols.
* **H** to View Rules/Help.

### Key Rules (Copenhagen)
1.  **Movement:** Rooks movement (orthogonal, any distance).
2.  **Capture:** Custodian capture (sandwiching a piece between two enemies).
3.  **The King:** Captured by being surrounded on 4 sides. Escapes by reaching a corner.
4.  **Shieldwall:** A row of pieces on the edge can be captured if bracketed at both ends and blocked from the front.
5.  **Encirclement:** If Black forms a closed loop containing all White pieces, Black wins.

## 🚀 Installation & Setup

Since this project uses Vanilla JS, no build steps (NPM/Webpack) are required.

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/yourusername/hnefatafl-web.git](https://github.com/yourusername/hnefatafl-web.git)
    ```
2.  **Run locally:**
    * Simply open `index.html` in your browser.
    * *Note:* For audio assets to load correctly without CORS issues, it is recommended to use a local server (e.g., VS Code "Live Server" extension or Python `http.server`).

    ```bash
    # Python 3 example
    python -m http.server 8000
    ```

## 📂 Project Structure

```text
/
├── index.html          # Main entry point (DOM structure)
├── assets/
│   ├── images/         # Piece sprites (King, Rook, etc.)
│   └── sounds/         # WAV files for moves and captures
└── README.md           # Documentation

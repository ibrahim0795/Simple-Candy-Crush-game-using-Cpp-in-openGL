# 🍬 Simple Candy Crush Game - C++ with OpenGL

A feature-rich implementation of the popular Candy Crush game using C++ and OpenGL graphics library. This project demonstrates graphics programming concepts including 2D rendering, user interaction, and game state management.

![Language](https://img.shields.io/badge/Language-C%2B%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Complete-success)

## 📋 Table of Contents

- [Features](#features)
- [Game Overview](#game-overview)
- [System Requirements](#system-requirements)
- [Installation](#installation)
- [Compilation & Building](#compilation--building)
- [How to Play](#how-to-play)
- [Game Controls](#game-controls)
- [Project Structure](#project-structure)
- [Architecture](#architecture)
- [Scoring System](#scoring-system)
- [Game Levels](#game-levels)
- [Code Documentation](#code-documentation)
- [Contributing](#contributing)
- [Authors](#authors)

## 🎮 Features

### Core Gameplay
- **8x8 Grid System**: Classic match-3 gameplay on an 8x8 grid
- **5 Different Candy Types**: 
  - Blue Squares
  - Green Triangles
  - Red Diamonds
  - Purple Hexagons
  - Brown Circles

### Game Mechanics
- **Swap & Match**: Right-click to select and swap adjacent candies
- **Automatic Cascade**: Candies fall down when matches are made
- **Score Tracking**: Real-time score updates
- **Move System**: Limited moves per level to create challenge
- **Level Progression**: Two difficulty levels with increasing targets

### Features
- 🎯 **Two Level System**: 
  - Level 1: Score target 1500 points
  - Level 2: Score target 3600 points
- 💾 **Save/Load Game**: Save and load game progress
- 💡 **Hint System**: Get hints for possible matches (costs 400 points)
- ⏸️ **Pause Functionality**: Pause and resume gameplay
- 🎨 **Colorful UI**: Vibrant graphics and intuitive interface
- 📊 **Game Statistics**: Track player name and score

## 🎯 Game Overview

This is a Candy Crush clone that tests players' strategic thinking and pattern recognition skills. Players must create matches of three or more identical candies to:
- Earn points
- Clear the board
- Progress through levels

The game features a main menu, player profile system, and multiple game states for an engaging experience.

## 💻 System Requirements

### Minimum Requirements
- **OS**: Linux/Unix (with X11 support)
- **Compiler**: GCC/G++ with C++11 support
- **Architecture**: 32-bit or 64-bit

### Required Libraries
- **GLUT** (OpenGL Utility Toolkit)
- **OpenGL** (GL and GLU libraries)
- **FreeImage** (for image processing)
- **X11** (for window management on Linux)
- **PThread** (POSIX threads library)

### Optional
- **CImg Library** (included in repository for header-only usage)

## 📦 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/ibrahim0795/Simple-Candy-Crush-game-using-Cpp-in-openGL.git
cd Simple-Candy-Crush-game-using-Cpp-in-openGL
```

### 2. Install Required Libraries (Ubuntu/Debian)

```bash
sudo apt-get update
sudo apt-get install freeglut3 freeglut3-dev libglew-dev libx11-dev libfreeimage-dev libfreeimage3
```

For Fedora/RHEL:
```bash
sudo dnf install freeglut-devel libX11-devel freeimage-devel
```

### 3. Run Installation Script (Optional)

```bash
cd Projecti23_Candy_Crush/Projecti23
chmod +x install-libraries.sh
./install-libraries.sh
```

## 🔧 Compilation & Building

### Using Makefile

```bash
cd Projecti23_Candy_Crush/Projecti23

# Compile the project
make all

# Or simply
make

# Clean build artifacts
make clean

# Rebuild
make clean && make all
```

### Manual Compilation

```bash
g++ -g3 -Wall -fmessage-length=0 -c util.cpp -o util.o
g++ -g3 -Wall -fmessage-length=0 -c game.cpp -o game.o
g++ -o game util.o game.o -L/usr/X11R6/lib -lglut -lGLU -lGL -lX11 -lfreeimage -pthread
```

### Running the Game

```bash
cd Projecti23_Candy_Crush/Projecti23
./game
```

## 🕹️ How to Play

### Main Menu
1. **New Game [G]** - Start a fresh game with a new player profile
2. **Load Game [L]** - Load a previously saved game
3. **High Scores [H]** - View high scores
4. **Help [T]** - Display game instructions
5. **Exit [E]** - Exit the game

### Gameplay

1. **Select a Candy**: Right-click on a candy on the grid
2. **Swap**: Right-click on an adjacent candy to swap them
3. **Match**: Make matches of 3 or more identical candies
4. **Earn Points**: Each match gives you 20 points
5. **Progress**: Reach the target score to complete the level

## ⌨️ Game Controls

| Key | Action |
|-----|--------|
| **G** | New Game |
| **L** | Load Game |
| **H** | High Scores |
| **T** | Help / Tutorial |
| **P** | Pause Game |
| **C** | Continue (during pause) |
| **M** | Main Menu (during pause) |
| **B** | Back (in menus) |
| **I** | Hint |
| **S** | Save Game |
| **E** | Exit Game |
| **Backspace** | Delete character (in name entry) |
| **Enter** | Confirm (in name entry) |
| **Right Mouse Click** | Select/Swap Candies |

## 📁 Project Structure

```
Simple-Candy-Crush-game-using-Cpp-in-openGL/
├── README.md                          # This file
├── REPOSITORY_NOTES.md                # Technical documentation
├── Projecti23_Candy_Crush/
│   └── Projecti23/
│       ├── game.cpp                   # Main game engine and logic
│       ├── game.o                     # Compiled object file
│       ├── util.cpp                   # Utility functions implementation
│       ├── util.h                     # Utility functions header
│       ├── util.o                     # Compiled utility object
│       ├── CImg.h                     # CImg image processing library
│       ├── Makefile                   # Build configuration
│       ├── game                       # Executable file
│       ├── gameState.txt              # Saved game state
│       └── install-libraries.sh       # Library installation script
```

## 🏗️ Architecture

### Game State Management
The game uses a centralized `GameState` structure to manage:
- 8x8 game grid array
- Player score
- Moves remaining
- Player name
- Selected candy index

### Key Components

#### game.cpp
- **Main Application**: Window initialization and OpenGL setup
- **Game Logic**: Match detection and scoring
- **Rendering**: Drawing candies and UI elements
- **Input Handling**: Keyboard and mouse events
- **Game States**: Menu, gameplay, pause, level progression

#### util.cpp / util.h
- **Graphics Primitives**: Draw shapes (circles, squares, triangles, rectangles)
- **Color Management**: Extensive color palette support
- **Text Rendering**: Display strings on screen
- **Utility Functions**: Random number generation, math operations

### Rendering Pipeline
1. **Initialization**: Setup OpenGL projection and camera
2. **Clear**: Clear color buffer
3. **Draw Grid**: Render 8x8 game grid
4. **Draw Candies**: Render each candy type based on array values
5. **Draw UI**: Render score, moves, and level information
6. **Swap Buffers**: Display rendered frame

## 🎯 Scoring System

| Action | Points |
|--------|--------|
| Match 3 candies | +20 points |
| Hint Usage | -400 points |

### Level Requirements
- **Level 1**: Reach 1500 points (30 moves)
- **Level 2**: Reach 3600 points

## 📊 Game Levels

### Level 1: Beginner
- **Objective**: Score 1500 points
- **Moves**: 30 moves
- **Candy Types**: 5 different types
- **Difficulty**: Introduction to mechanics

### Level 2: Advanced
- **Objective**: Score 3600 points
- **Moves**: Unlimited (continued from Level 1 progress)
- **Candy Types**: 5 different types
- **Difficulty**: Requires strategic play

## 📚 Code Documentation

### Main Function Structure

```cpp
GameState gameState;  // Global game state

void main()           // Initialize window and start game loop
void drawMenu()       // Display menus and screens
void GameDisplay()    // Render game scene
void DrawGrid()       // Draw 8x8 grid and candies
void scoring()        // Check matches and update score
void MouseClicked()   // Handle candy selection
void handleKeypress() // Process keyboard input
void Timer()          // Game update loop (12 FPS)
void saveGameState()  // Save to file
void loadGameState()  // Load from file
```

### Candy Type Mapping

| Value | Type | Shape | Color |
|-------|------|-------|-------|
| 1 | Square | Square | Blue |
| 2 | Triangle | Triangle | Green |
| 3 | Diamond | Diamond | Red |
| 4 | Hexagon | Hexagon | Purple |
| 5 | Circle | Circle | Brown |

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 👥 Authors

- **Designed and Developed by**
  - i230795 (Muhammad Ibrahim)
  - i230713

## 📝 License

This project is open source and available under the MIT License.

## 🐛 Known Issues & Limitations

- Linux/Unix only (X11 requirement)
- Limited to 8x8 grid size (can be modified in code)
- No network/multiplayer support
- Single player only
- OpenGL 1.x only (deprecated on modern systems)

## 🚀 Future Enhancements

- [ ] Cross-platform support (Windows, macOS)
- [ ] Power-ups and special candies
- [ ] Sound effects and background music
- [ ] Leaderboard system
- [ ] Additional levels
- [ ] Mobile version
- [ ] Multiplayer support
- [ ] Modern OpenGL (3.x+) support
- [ ] Time-based challenges

## 💬 Feedback & Support

For issues, questions, or suggestions:
- Open an issue on GitHub
- Review the code comments for implementation details
- Check the REPOSITORY_NOTES.md for technical documentation
- See README for additional setup guides

## 📖 Additional Documentation

For detailed technical information about the implementation, architecture, and code analysis, see [REPOSITORY_NOTES.md](REPOSITORY_NOTES.md).

---

**Happy Gaming! 🍬✨**

Last Updated: June 07, 2026  
Version: 1.0.0  
Repository: https://github.com/ibrahim0795/Simple-Candy-Crush-game-using-Cpp-in-openGL

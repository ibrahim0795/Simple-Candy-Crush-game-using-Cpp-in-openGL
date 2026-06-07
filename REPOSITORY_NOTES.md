# 📖 Repository Notes & Documentation

## Overview
This document provides comprehensive notes about the repository structure, implementation details, and technical documentation for the Simple Candy Crush game in C++ with OpenGL.

---

## 🎓 Project Information

**Project Name**: Simple Candy Crush Game using C++ in OpenGL  
**Language**: C++ (100%)  
**Type**: Graphics Programming / Game Development  
**Platform**: Linux/Unix with X11 support  
**Graphics API**: OpenGL with GLUT  
**Created**: September 26, 2024  

---

## 🏗️ Repository Structure

### Directory Layout

```
repository/
├── README.md                              # Main documentation
├── REPOSITORY_NOTES.md                    # This file
└── Projecti23_Candy_Crush/
    └── Projecti23/
        ├── Source Files
        │   ├── game.cpp                   # Main game implementation (~1063 lines)
        │   ├── util.cpp                   # Utility functions
        │   └── util.h                     # Utility headers & declarations
        │
        ├── Build Files
        │   ├── Makefile                   # GNU Makefile configuration
        │   ├── game.o                     # Compiled game object
        │   ├── util.o                     # Compiled utility object
        │   └── game                       # Final executable
        │
        ├── Data Files
        │   └── gameState.txt              # Persistent game save data
        │
        ├── Libraries
        │   └── CImg.h                     # CImg image library (~2.8MB)
        │
        └── Scripts
            └── install-libraries.sh       # Dependency installation script
```

---

## 🔧 Technical Stack

### Core Technologies

| Component | Details |
|-----------|---------|
| **Language** | C++11 |
| **Graphics** | OpenGL 1.x / GLUT |
| **Window Manager** | FreeGLUT 3.0+ |
| **Image Library** | FreeImage, CImg |
| **Threading** | POSIX threads (pthread) |
| **Build System** | GNU Make |
| **Compilation** | GCC/G++ |

### Dependencies

#### Graphics Libraries
- `libglut-dev` / `freeglut3-dev` - OpenGL Utility Toolkit
- `libGLU-dev` - GLU utilities
- `libGL-dev` - OpenGL core
- `libX11-dev` - X Window System

#### Additional Libraries
- `libfreeimage-dev` - Image processing
- `pthread` - Threading support

---

## 💻 Core Implementation Details

### Game State Structure

```cpp
struct GameState {
    int arr[8][8];           // 8x8 game grid
    int selectedIndex = 0;   // Current screen/state index
    int userscore = 0;       // Player score
    int moves = 30;          // Moves remaining
    string playerName;       // Player profile name
};
```

### Main Game Loop (12 FPS)

```
Initialize Window & OpenGL
    ↓
Main Loop (GLUT Main Loop)
    ├─→ Timer Callback (every 83.33ms = 12 FPS)
    │   ├─→ Update Game State
    │   ├─→ Check for Matches
    │   ├─→ Update Score
    │   └─→ Request Redisplay
    ├─→ Display Callback
    │   └─→ Render Current Frame
    ├─→ Keyboard Callback
    │   └─→ Handle User Input
    └─→ Mouse Callback
        └─→ Handle Click/Movement Events
```

### Game States (selectedIndex)

| Index | State | Purpose |
|-------|-------|---------|
| 0 | Splash Screen | Initial loading animation |
| 1 | Main Menu | Menu selection screen |
| 2 | Player Profile | Enter player name |
| 3 | Help Screen | Game instructions |
| 4 | Gameplay | Active game state |
| 5 | Pause Menu | Pause options |
| 6 | Level 1 Intro | Level 1 introduction |
| 7 | Level Completion | Level completed screen |
| 8 | Game Over | Game over screen |

---

## 🎮 Game Mechanics Implementation

### Candy System

**Candy Types & Representation**:
```cpp
enum CandyType {
    BLUE_SQUARE = 1,      // DrawSquare()
    GREEN_TRIANGLE = 2,   // DrawTriangle()
    RED_DIAMOND = 3,      // DrawDiamond()
    PURPLE_HEXAGON = 4,   // DrawHexagon()
    BROWN_CIRCLE = 5      // DrawCircle()
};
```

### Grid Initialization

- **Size**: 8x8 cells (64 total candies)
- **Cell Size**: 75 pixels (600px grid / 8)
- **Grid Position**: Centered at (200, 200)
- **Rendering**: Each cell shows appropriate shape based on value

### Match Detection Algorithm

#### Horizontal Matching
```
FOR each row i = 0 to 7:
    FOR each column j = 0 to 5:
        IF arr[i][j] == arr[i][j+1] == arr[i][j+2]:
            MARK as match
            SHIFT candies down
            ADD 20 points
```

#### Vertical Matching
```
FOR each column j = 0 to 7:
    FOR each row i = 0 to 5:
        IF arr[i][j] == arr[i+1][j] == arr[i+2][j]:
            MARK as match
            SHIFT candies down
            ADD 20 points
```

### Cascade System

When candies are matched:
1. Matched candies are removed
2. Candies above fall down (gravity simulation)
3. New random candies spawn at top
4. Check for new matches (chain reactions)

---

## 🖥️ Rendering Details

### OpenGL Setup

```cpp
// Window Configuration
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
gluOrtho2D(0, 1020, 0, 840);  // 2D orthographic projection

// Screen Setup
SCREEN_WIDTH = 1020
SCREEN_HEIGHT = 840
```

### Rendering Sequence

1. **Clear Buffer**
   ```cpp
   glClear(GL_COLOR_BUFFER_BIT);
   glClearColor(0.0, 0.0, 0.0, 0.0);  // Black background
   ```

2. **Draw Game Elements**
   ```
   DrawRectangle()      → Background
   DrawGrid()           → Grid lines and candies
   DrawLine()           → Grid borders
   DrawRectangle()      → Score/moves boxes
   DrawString()         → Text labels
   DrawCircle()         → Pause button indicator
   ```

3. **Swap Buffers**
   ```cpp
   glutSwapBuffers();   // Double buffering
   ```

---

## ⌨️ Input System

### Keyboard Events

**Printable Keys** (via `glutKeyboardFunc`)
- Game menu navigation (G, L, H, T, E, B, C, M, P, S, I)
- Player name entry (a-z, A-Z, space)
- Backspace handling (ASCII 8)
- Enter confirmation (ASCII 13)

**Special Keys** (via `glutSpecialFunc`)
- Arrow keys (not currently implemented)
- Function keys (reserved for future)

### Mouse Events

**Right-Click Functionality** (via `glutMouseFunc`)
```cpp
void MouseClicked(int button, int state, int x, int y)
{
    if (button == GLUT_RIGHT_BUTTON && state == GLUT_DOWN)
    {
        // Convert pixel coordinates to grid coordinates
        int col = (x / CELL_SIZE) - 2;
        int row = (GRID_SIZE - 1 - (y / CELL_SIZE)) + 1;
        
        // Select first candy
        if (selectedRow == -1 && selectedCol == -1)
        {
            selectedRow = row;
            selectedCol = col;
        }
        // Swap with adjacent candy
        else if (abs(row - selectedRow) + abs(col - selectedCol) == 1)
        {
            // Perform swap and check for matches
        }
    }
}
```

**Motion Tracking** (via `glutMotionFunc`)
- Tracks mouse movement (currently just logs coordinates)

---

## 💾 Save/Load System

### File Format: `gameState.txt`

```
<8x8 grid array>
<user score>
<player name>
```

**Example**:
```
1 2 3 4 5 1 2 3
2 3 4 5 1 2 3 4
...
1500
John
```

### Save Function
```cpp
void saveGameState()
{
    ofstream outFile("gameState.txt");
    // Write 8x8 grid
    // Write score
    // Write player name
}
```

### Load Function
```cpp
void loadGameState()
{
    ifstream inFile("gameState.txt");
    // Read 8x8 grid
    // Read score
    // Read player name
}
```

---

## 🎨 Color System

### Color Enumeration

The utility header defines 169+ color constants:

```cpp
enum ColorNames {
    MAROON, DARK_RED, BROWN, FIREBRICK, CRIMSON,
    RED, TOMATO, CORAL, INDIAN_RED, LIGHT_CORAL,
    // ... (many more)
    WHITE, BLACK, SLATE_BM
};

// Each color maps to RGB values in float[3]
// Example: colors[BLUE] = {0.0, 0.0, 1.0}
```

### Candy Colors in Game

| Candy | Color Code | RGB Value |
|-------|-----------|-----------|
| Square | BLUE | (0, 0, 1) |
| Triangle | GREEN | (0, 1, 0) |
| Diamond | RED | (1, 0, 0) |
| Hexagon | MEDIUM_ORCHID | (0.73, 0.33, 0.83) |
| Circle | BROWN | (0.65, 0.16, 0.16) |

---

## 📊 Scoring & Progression

### Points System

- **Match 3 Candies**: +20 points
- **Hint Usage**: -400 points
- **Level 1 Target**: 1500 points (Level 2 unlock)
- **Level 2 Target**: 3600 points (Game completion)

### Move System

- **Level 1**: 30 moves to reach 1500 points
- **Game Over**: When moves reach 0 before score target

---

## 🐛 Known Limitations

1. **Platform Specific**
   - Only Linux/Unix with X11 support
   - No Windows or macOS native support

2. **Grid Fixed**
   - Hard-coded 8x8 grid
   - Would need refactoring to support variable sizes

3. **Performance**
   - 12 FPS game loop (can cause lag on slower systems)
   - No optimization for large grids

4. **Gameplay**
   - Single player only
   - No networking capabilities
   - Limited special power-ups/items

5. **Graphics**
   - OpenGL 1.x API (deprecated on modern systems)
   - No shaders support
   - Fixed function pipeline only

---

## 🔍 Code Quality Analysis

### Strengths
✅ Clear game logic implementation  
✅ Modular color and utility functions  
✅ State machine for game flow  
✅ Save/load persistence  
✅ Comprehensive match detection  

### Areas for Improvement
⚠️ Could use more error handling  
⚠️ Some code duplication in scoring logic  
⚠️ Limited comments in complex sections  
⚠️ Magic numbers should be constants  
⚠️ Could benefit from refactoring into classes  

---

## 📚 Key Functions Reference

| Function | Purpose | Location |
|----------|---------|----------|
| `main()` | Initialize window & loop | game.cpp:50 |
| `drawMenu()` | Render menu screens | game.cpp:714 |
| `GameDisplay()` | Render game scene | game.cpp:690 |
| `DrawGrid()` | Draw 8x8 grid & candies | game.cpp:480 |
| `scoring()` | Check matches & update score | game.cpp:189 |
| `MouseClicked()` | Handle candy selection | game.cpp:640 |
| `handleKeypress()` | Process keyboard input | game.cpp:883 |
| `Timer()` | Main game loop (12 FPS) | game.cpp:703 |
| `saveGameState()` | Save to file | game.cpp:1014 |
| `loadGameState()` | Load from file | game.cpp:1039 |
| `CheckForMatches()` | Validate swap moves | game.cpp:608 |
| `moveCandiesDown()` | Gravity simulation | (declared but not shown) |

---

## 🚀 Compilation Notes

### Build Process

```bash
# Step 1: Preprocess & compile source files
g++ -g3 -Wall -fmessage-length=0 -c game.cpp -o game.o
g++ -g3 -Wall -fmessage-length=0 -c util.cpp -o util.o

# Step 2: Link object files and libraries
g++ -o game game.o util.o -lglut -lGLU -lGL -lX11 -lfreeimage -pthread

# Step 3: Run executable
./game
```

### Compiler Flags

- `-g3`: Include debug symbols (level 3)
- `-Wall`: Enable all warnings
- `-fmessage-length=0`: No limit on error message length
- `-L/usr/X11R6/lib`: Specify library search paths
- `-l{glut,GLU,GL,X11,freeimage}`: Link required libraries
- `-pthread`: Link POSIX thread library

---

## 📋 Debugging Tips

### Enable Verbose Output
```cpp
// Add cout statements to track:
cout << "Clicked on row: " << row << ", col: " << col << endl;
cout << "Score: " << gameState.userscore << endl;
cout << "Moves remaining: " << gameState.moves << endl;
```

### Debug Information
- Game state printed to console via cout
- Coordinate system: origin at top-left
- Grid indexing: 0-based (0 to 7)

### Common Issues

1. **OpenGL Libraries Not Found**
   - Solution: Install development packages for GLUT, GLU, GL

2. **X11 Display Errors**
   - Solution: Ensure X11 forwarding enabled or display variable set

3. **Crashes on Startup**
   - Check: All required libraries installed
   - Verify: Correct file permissions on executable

---

## 📈 Performance Characteristics

### Resource Usage

- **Memory**: ~20-30 MB (including libraries)
- **CPU**: Minimal (only active during timer callbacks)
- **GPU**: Minimal (simple 2D rendering)
- **Frame Rate**: 12 FPS (fixed)

### Optimization Opportunities

1. Use vertex buffers for grid rendering
2. Implement frustum culling (if scaling up)
3. Reduce timer frequency for slower systems
4. Cache drawable lists between frames

---

## 🎯 Development Roadmap Ideas

**Short Term**
- Add sound effects
- Improve UI responsiveness
- Add more color themes

**Medium Term**
- Port to modern OpenGL (3.x)
- Cross-platform support (Windows, macOS)
- Mobile version (Android, iOS)

**Long Term**
- Multiplayer support
- Cloud-based leaderboard
- Power-up system
- Level creation tool

---

## 📞 Maintenance Notes

### Regular Tasks
- Update documentation with new features
- Test on different Linux distributions
- Monitor for deprecated API warnings
- Keep library dependencies updated

### Contributing Guidelines
1. Follow existing code style
2. Add comments for complex logic
3. Test thoroughly before submitting
4. Update documentation
5. Create feature branches for new work

---

**Last Updated**: September 26, 2024  
**Version**: 1.0.0  
**Maintainer**: i230795, i230713

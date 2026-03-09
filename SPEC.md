# Minesweeper Game Specification

## 1. Project Overview

- **Project Name**: Classic Minesweeper
- **Type**: Web-based game (Single HTML file)
- **Core Functionality**: A fully playable Minesweeper game with adjustable difficulty levels and responsive design for both desktop and mobile browsers
- **Target Users**: Casual gamers looking for a classic Minesweeper experience on any device

## 2. UI/UX Specification

### Layout Structure

**Desktop Layout (>768px)**
- Centered game container, max-width 600px
- Header with game title
- Control panel (difficulty selector, new game button, timer, mine counter)
- Game board centered
- Footer with instructions

**Mobile Layout (≤768px)**
- Full-width responsive design
- Compact control panel
- Touch-optimized cell sizes (minimum 36px)
- Stacked layout for controls

**Responsive Breakpoints**
- Desktop: > 768px
- Mobile: ≤ 768px

### Visual Design

**Color Palette**
- Background: #1a1a2e (deep navy)
- Game board background: #16213e (darker navy)
- Cell unrevealed: #0f3460 (medium blue)
- Cell revealed: #e8e8e8 (light gray)
- Cell hover: #1a4a7a (highlighted blue)
- Mine cell (game over): #ff4757 (red)
- Flag color: #ffa502 (orange)
- Number colors:
  - 1: #3498db (blue)
  - 2: #27ae60 (green)
  - 3: #e74c3c (red)
  - 4: #9b59b6 (purple)
  - 5: #e67e22 (orange)
  - 6: #1abc9c (teal)
  - 7: #34495e (dark gray)
  - 8: #7f8c8d (gray)
- Primary accent: #e94560 (coral red)
- Secondary accent: #0f3460 (navy)
- Text primary: #ffffff
- Text secondary: #a0a0a0

**Typography**
- Font family: "JetBrains Mono", "Fira Code", monospace (for retro computer feel)
- Title: 2rem, bold, letter-spacing 2px
- Controls: 0.9rem
- Cell numbers: 1rem, bold
- Timer/Counter: 1.2rem, monospace

**Spacing System**
- Container padding: 20px
- Cell gap: 2px
- Control panel margin-bottom: 20px
- Button padding: 10px 20px

**Visual Effects**
- Cell unrevealed: subtle inner shadow (inset 2px 2px 4px rgba(255,255,255,0.1), inset -2px -2px 4px rgba(0,0,0,0.3))
- Cell revealed: flat with 1px border #ccc
- Buttons: smooth hover transition (0.2s), slight scale on active (0.95)
- Win/Lose: fade-in animation for overlay (0.3s)
- Cell reveal: subtle scale animation (0.1s)
- Number appearance: quick fade-in (0.15s)

### Components

**Header**
- Game title "MINESWEEPER" with subtle glow effect

**Control Panel**
- Difficulty dropdown selector:
  - Beginner (9×9, 10 mines)
  - Intermediate (16×16, 40 mines)
  - Expert (30×16, 99 mines)
  - Custom (user-defined)
- New Game button (restart icon + text)
- Mine counter (shows remaining flags)
- Timer (starts on first click, stops on win/lose)
- Best times display (per difficulty)

**Game Board**
- Grid of cells
- Cell states: unrevealed, revealed, flagged, question mark, mine (exploded)
- Left-click to reveal
- Right-click to flag (desktop) / long-press to flag (mobile)
- Middle-click / double-tap to chord (reveal neighbors if numbers match)

**Cell States**
- Unrevealed: raised 3D appearance
- Revealed: flat, shows number or empty
- Flagged: shows 🚩 emoji or flag icon
- Question mark: shows ? (optional, toggle in settings)
- Mine: shows 💣 emoji
- Exploded mine: red background with 💥

**Custom Difficulty Modal**
- Width input (9-30)
- Height input (9-30)
- Mines input (1 to width×height-1)
- Save and Cancel buttons
- Validation with error messages

**Win/Lose Overlay**
- Semi-transparent dark overlay
- Win: "🎉 YOU WIN!" message with time and best time comparison
- Lose: "💥 GAME OVER" message
- "Play Again" button
- For win: option to save time to best times (localStorage)

**Instructions Panel**
- Collapsible on mobile
- Desktop: visible below game board

## 3. Functionality Specification

### Core Features

**Game Mechanics**
1. Grid generation with random mine placement (after first click - first click is always safe)
2. Number calculation for each cell (adjacent mine count)
3. Recursive reveal for empty cells (flood fill)
4. Flag placement to mark suspected mines
5. Win condition: all non-mine cells revealed
6. Lose condition: mine cell clicked

**Difficulty Levels**
- Beginner: 9×9 grid, 10 mines
- Intermediate: 16×16 grid, 40 mines
- Expert: 30×16 grid, 99 mines
- Custom: user-defined dimensions and mine count

**Controls**
- Left click: reveal cell
- Right click: toggle flag (desktop)
- Long press: toggle flag (mobile, 500ms)
- Double click/tap on revealed number: chord (reveal all neighbors if enough flags placed)

**Timer**
- Starts on first cell click
- Pauses on win/lose
- Max display: 999 seconds

**Mine Counter**
- Shows: total mines - placed flags
- Can go negative if over-flagged

**Best Times (localStorage)**
- Store top 5 times per difficulty
- Display format: MM:SS
- Highlight new records

### User Interactions and Flows

1. **Start Game Flow**
   - Page loads → Show difficulty selector (default: Beginner)
   - Select difficulty → Generate grid → Ready to play

2. **Gameplay Flow**
   - Click first cell → Start timer → Reveal cell
   - Continue playing → Place flags as needed
   - Win → Stop timer → Show overlay → Save best time if applicable
   - Lose → Stop timer → Show overlay with revealed mines

3. **Restart Flow**
   - Click "New Game" → Reset timer and counter → Generate new grid

4. **Change Difficulty Flow**
   - Select new difficulty → Confirm if game in progress → Reset game

### Data Handling

**localStorage Keys**
- `minesweeper_best_times`: JSON object with best times per difficulty
- `minesweeper_custom_difficulty`: Last used custom settings

**Data Structure**
```json
{
  "beginner": [{ "time": 45, "date": "2024-01-01" }],
  "intermediate": [],
  "expert": [],
  "custom": []
}
```

### Edge Cases

1. First click always reveals a safe cell (never a mine)
2. If first click is on empty cell, auto-reveal surrounding empty cells
3. Prevent flagging revealed cells
4. Prevent revealing flagged cells
5. Validate custom difficulty inputs (min/max bounds)
6. Handle window resize gracefully
7. Prevent text selection during gameplay (mobile)
8. Handle rapid clicking gracefully

## 4. Acceptance Criteria

### Visual Checkpoints
- [ ] Game loads with dark theme as specified
- [ ] Title has glow effect
- [ ] Cells have 3D raised appearance
- [ ] Numbers display in correct colors
- [ ] Responsive layout works on mobile (≤768px)
- [ ] Win/Lose overlay appears with animation

### Functional Checkpoints
- [ ] All three preset difficulties work correctly
- [ ] Custom difficulty modal opens and validates input
- [ ] First click is always safe
- [ ] Empty cells flood-fill correctly
- [ ] Right-click (desktop) and long-press (mobile) place flags
- [ ] Timer starts on first click and stops on game end
- [ ] Mine counter updates correctly
- [ ] Win detection works (all non-mine cells revealed)
- [ ] Lose detection works (mine clicked)
- [ ] Best times save to localStorage and display correctly
- [ ] Game restarts properly with "New Game" button

### Mobile-Specific
- [ ] Touch targets are at least 36px
- [ ] Long-press (500ms) triggers flag
- [ ] No horizontal scroll
- [ ] Viewport meta tag prevents zoom issues
- [ ] Double-tap to chord works

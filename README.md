# The Game

A 2D top-down pixel art game built with HTML5 Canvas and vanilla JavaScript. Features player movement, NPCs, minigames, a shop system, and an inventory.

## 🎮 How to Play

### Starting the Game

**Option 1: Using the Start Script (Recommended)**

```bash
./scripts/start-server.sh
```

This will automatically:

- Find an available HTTP server (Python, Node.js, or http-server)
- Start the server on port 8080
- Open your browser to the game

**Option 2: Direct File**
Simply open `game.html` in your browser (some features may not work).

**Option 3: Node.js Server**

```bash
node server.js
```

**Option 4: Python Server**

```bash
python -m http.server 8080
```

**Option 5: Live Server**
If you have VS Code/Cursor with Live Server extension:

1. Open the workspace file: `the-game.code-workspace`
2. Right-click on `game.html` → "Open with Live Server"

### Controls

- **WASD / Arrow Keys**: Move player
- **Shift**: Sprint (drains stamina)
- **E**: Open/close inventory
- **Q**: Interact (NPCs, red X tiles for minigames)
- **1-5**: Buy items in shop (when shop is open)
- **SPACE**: Used in brushing minigame

### Gameplay Tips

1. Find red X tiles on the ground and press Q to start a minigame
2. Complete minigames to earn money
3. Visit the shopkeeper (yellow sprite) to buy better shovels
4. Better shovels = better rewards from minigames
5. Talk to NPCs by getting close and pressing Q

## 🎯 Features

### Core Mechanics

- **Player Movement**: Move with WASD or Arrow Keys, sprint with Shift
- **Stamina System**: Sprint drains stamina, regenerates when not sprinting
- **Procedural World**: 2400x2000 pixel world with procedurally generated tiles
- **Camera System**: Smooth camera follow with pixel-perfect rendering

### NPCs & Interactions

- **Shopkeeper**: Buy shovels and equipment at the shop
- **Old Guy**: Friendly NPC who greets you
- **Dialogue System**: Press Q to interact with NPCs and progress dialogue

### Minigames

- **Balancing Minigame** (50% chance): Balance a pot on a stick for 15 seconds using arrow keys
- **Brushing Minigame** (50% chance): Hold SPACE to brush dirt clean, release before it turns red (5-10 seconds)

### Shop & Inventory

- **Shop System**: Purchase shovels of different tiers (Tier 1-5)
- **Inventory**: Press E to open/close inventory
- **Equipment**: Equipped shovels affect minigame rewards

### Money System

- Earn money by completing minigames
- Money displayed in top-right corner with decorative border
- Shovel multipliers increase rewards:
  - Tier 1: 1.5x (success: 4.5x)
  - Tier 2: 2.0x (success: 6.0x)

## 🛠️ Technical Details

- **Rendering**: Canvas 2D API with pixelated rendering
- **Camera**: Smooth follow with lerp (0.12), rounded to prevent jitter
- **Collision**: Circle-based collision detection
- **Performance**: 60fps using requestAnimationFrame
- **No Dependencies**: Pure HTML/CSS/JavaScript

## 📁 Project Structure

```
the-game/
├── assets/
│   ├── sprites/           # All PNG sprite files
│   │   ├── player*.png    # Player sprites (idle, walk, back)
│   │   ├── pot.png        # Balancing minigame
│   │   ├── brush.png      # Brushing minigame
│   │   ├── dirt.png       # Brushing minigame
│   │   ├── money_border.png
│   │   ├── shopkeeper.png
│   │   ├── oldguy.png
│   │   └── shovel.png     # Tier 1 shovel
│   └── piskel/            # Source files for sprites
├── scripts/
│   ├── start-server.sh    # Start game server
│   └── open-game.sh       # Open game in browser
├── game.html              # Main game file (all code in one file)
├── index.html             # Entry point
├── server.js              # Node.js server (optional)
├── .cursorrules           # Coding guidelines
└── the-game.code-workspace # VS Code workspace config
```

## 🚀 Development

This game was built as a standalone HTML file with no build process required. All game logic, rendering, and assets are self-contained.

### Key Features Implemented

- ✅ Player movement with momentum and friction
- ✅ Stamina system with exhaustion mechanics
- ✅ Procedural world generation
- ✅ Shop system with dialogue
- ✅ Inventory system
- ✅ Two different minigames (balancing and brushing)
- ✅ Money system
- ✅ Equipment system (shovels)
- ✅ NPCs with dialogue
- ✅ Tutorial system

## 🔧 Development Setup

### Git Configuration

This project uses **Git Bash** as the default terminal (configured in `.vscode/settings.json`).

### GitHub CLI Setup (No Admin Required)

If you don't have admin rights, you can install GitHub CLI locally:

```bash
# Create local bin directory
mkdir -p ~/bin

# Download and extract GitHub CLI
cd ~/bin
curl -L -o gh.zip https://github.com/cli/cli/releases/latest/download/gh_2.83.2_windows_amd64.zip
unzip -q gh.zip && rm gh.zip

# Add to PATH for current session
export PATH="$HOME/bin/bin:$PATH"

# Authenticate with GitHub
gh auth login
```

**Note:** Add `export PATH="$HOME/bin/bin:$PATH"` to your `~/.bashrc` to make it permanent.

### Git Credential Helper

Configure Git to use GitHub CLI for authentication:

```bash
git config --global credential.helper ""
git config --global credential.helper "!gh auth git-credential"
```

### Fork Syncing

This project has two remotes:

- `origin` - Main repository (nyrobovines-bit/the-game)
- `fork` - Fork repository (runningsammy/the-game)

**To push changes:**

1. **Push to origin (direct push):**

   ```bash
   git push origin fork-main:main
   ```

2. **Sync to fork (if direct push fails):**
   ```bash
   export PATH="$HOME/bin/bin:$PATH"  # If using local gh installation
   gh repo sync runningsammy/the-game --force
   ```

The fork is configured to sync from upstream, so use `gh repo sync` instead of direct `git push` to the fork remote.

## 📝 License

This is a personal project.

## 📚 Code Documentation

### World & Camera System

- **World Size**: 2400x2064 pixels (larger than canvas to enable camera movement)
- **Camera**: Smooth follow using linear interpolation (lerp = 0.12). Smaller values = slower follow, larger = snappier
- **Coordinate System**: World coordinates (where objects exist) vs Screen coordinates (where to draw). Always translate: `screenX = worldX - camera.x`

### Sprite Loading

All sprites use `imageSmoothingEnabled = false` for pixelated rendering. Sprites are loaded as `Image()` objects and must be checked for `complete` before rendering.

### Pot Sprites

- Regular pot: `Sprites for the Game-3 (1).png`
- Tall pot: `Sprites for the Game-5 (1).png` (more susceptible to gravity and balancing input)
- Short pot: `Sprites for the Game-4 (2).png` (less susceptible to gravity and balancing input)

### Minigame States

**Balancing Minigame:**
- `potAngle`: Current rotation in radians (0 = balanced)
- `potAngularVelocity`: Rotation speed
- `timeRemaining`: Seconds left (15 seconds)
- `potType`: 'regular', 'tall', or 'short' (affects gravity/input multipliers)

**Brushing Minigame:**
- `dirtLevel`: 0 = clean, 1.0 = fully dirty
- `redness`: 0 = normal, 1.0 = fully red (gradual transition)
- `redTriggerTime`: Random 3-8 seconds when dirt turns red
- `perfectReleaseWindow`: 0.5 seconds after fully red for perfect timing

**Declaration Minigame:**
- `realExcerpt`: The authentic text (left side)
- `testExcerpt`: The potentially fake text with typos (right side)
- `isFake`: Whether test excerpt contains typos
- `baseReward`: +$50 for correct answer
- `penalty`: -$30 for wrong answer (calling fake real)

### Game Loop Pattern

1. Update game state (physics, AI, input)
2. Clear canvas
3. Render world (sprites, backgrounds)
4. Render UI (menus, HUD)
5. Request next frame with `requestAnimationFrame`

### Rendering Best Practices

- Always use `ctx.save()` and `ctx.restore()` for transformations
- Round positions with `Math.round()` for pixel-perfect rendering
- Check sprite `complete` property before drawing
- Use `image-rendering: pixelated` in CSS for canvas

## Credits

Built with vanilla JavaScript, HTML5 Canvas, and pixel art sprites.

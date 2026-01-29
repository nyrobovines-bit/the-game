# NPC Jake, Fork artifact, build/distribution fixes

## New NPC: Jake (Warning Guy)

- Added new NPC "Jake" positioned to the left of spawn (mirrors Uncle Bernie on right).
- Sprite: `assets/sprites/Warning_guy.png.png`
- Dialogue sequence (first time):
  1. "Hey, you there, are you really an archeologist?"
  2. "You are? Good. Can't believe the nerve of some people."
  3. "They come here with their metal detector's and shovels, trying to find some souvenir or something."
  4. "Don't they know you have to be given permission to excavate an area?"
- After intro: "So long as you have a permit, have fun!"
- Full collision/interaction system (same pattern as other NPCs).
- Fixed softlock bug where repeat dialogue wouldn't close.

## New Artifact: Fork

- Added "Fork" as common-tier artifact.
- Sprite: `assets/sprites/Fork.png`
- Registered in `ARTIFACT_NAMES` and `artifactTiers` (common).
- Added to minigame reward distribution (15% chance on normal X tiles).
- Works in inventory, museum displays, etc.

## Bug Fixes

### Ghost X's (radar markers persisting across days)
- Problem: Yellow X markers from radar discoveries persisted after new day regenerated tiles.
- Fix: Clear `radar.discoveredSecretX` and `radar.rings` in `startNewDay()` and `startNewGameFromControls()`.

### Multiple Cannons bug
- Problem: Could obtain multiple Cannon artifacts after placing one in museum.
- Fix: Added `playerOrMuseumHasCannon()` helper to check inventory + museum display.
- Updated reward logic: If Cannon already owned, give Cannon Shards instead.
- Applied to both secret-X and normal-X reward paths.

### Tutorial popup blocking minigame interaction
- Problem: Could click minigame UI while tutorial popup was visible.
- Fix: Added early return in canvas click handler when `tutorial.active && dayTime.currentDay === 0`.

### Delete button labels
- Added red "Delete" text above delete buttons in both inventory screens:
  - Regular inventory (overworld)
  - Museum build-mode inventory panel

## Build/Distribution Fixes

### Fixed zip structure for proper Electron runtime
- Problem: Extracted zip had exe and DLLs in separate folders → "ffmpeg.dll was not found" error.
- Fix: Zip now includes entire `dist/win-unpacked/` folder contents (all DLLs + exe together).
- Created `Dig_dig_dig_2026.zip` with proper structure for easy sharing.
- Updated `Dig_dig_dig_game_win.zip` with same fix.

### Scripts updated
- `scripts/build-exe.sh`: Full Electron build + sync to Game folder.
- `scripts/sync-existing-exe.sh`: Quick sync of code/assets to existing exe folder (avoids rebuild).
- `scripts/start-server.sh`: Improved browser opening on Windows Git Bash.
- `scripts/open-game.sh`: Better Windows compatibility.

## Notes

- Fork artifact distribution: 15% on normal X, not included in secret-X distribution (kept original ratios).
- Cannon uniqueness enforced globally: checks both inventory slots and museum cannon display.
- All NPCs now follow same collision/interaction pattern for consistency.

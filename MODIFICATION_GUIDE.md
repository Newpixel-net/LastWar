# 🎮 LastWar - Practical Modification Guide

## 📍 Quick Navigation

This guide tells you **EXACTLY** where to make common changes in `LastWar_MODULAR.html`.

---

## 🎯 Common Modifications

### 1. **Change Game Balance** (Enemy HP, Damage, Credits)

**Location:** Search for `// ⚖️ BALANCE CONFIG`
**Line:** ~4700-4900

```javascript
// In Enemy class constructor:
case 'drone':
    this.hp = 30 + (level * 5);        // ← Change base HP here
    this.contactDamage = 15;            // ← Change contact damage
    this.creditValue = 5 + level;       // ← Change credit drops
    break;
```

**Example changes:**
- Make enemies harder: Increase `hp` values
- Reduce coins: Reduce `creditValue` values
- More damage: Increase `contactDamage` values

---

### 2. **Change Upgrade Costs**

**Location:** Search for `// 💰 UPGRADE COSTS`
**Line:** ~3000-3100

```javascript
// In renderUpgrades() method:
const costs = {
    armor: [100, 200, 400, 800],       // ← Change armor costs
    firepower: [150, 300, 600, 1200],  // ← Change firepower costs
    attackSpeed: [120, 240, 480, 960], // ← Change attack speed costs
    health: [200, 400, 800, 1600]      // ← Change health costs
};
```

**Example changes:**
- More expensive: Increase all values
- Cheaper: Decrease all values
- Different progression: Change the multiplier pattern

---

### 3. **Add or Modify Ships**

**Location:** Search for `// 🚀 SHIP CONFIGURATION`
**Line:** ~2139-2344

```javascript
const SHIP_TYPES = {
    yourNewShip: {
        id: 'yourNewShip',
        name: 'YOUR SHIP NAME',
        icon: '🛸',
        cost: 3000,                    // ← Price to unlock
        speed: 1.2,                    // ← Movement speed multiplier
        size: 1.0,                     // ← Size multiplier
        desc: 'Description here',
        legendary: true,               // ← Makes it glow purple
        ability: {
            name: 'ABILITY NAME',
            desc: 'What it does',
            icon: '✨'
        },
        render: (ctx, x, y, w, h, theme) => {
            // Draw your ship here
        }
    }
};
```

**To modify existing ship:**
- Change `cost` to adjust unlock price
- Change `speed` to make faster/slower
- Change `desc` to update description

---

### 4. **Change Power-Up Drop Rates**

**Location:** Search for `// 🎁 POWER-UP DROPS`
**Line:** ~3700-3800

```javascript
// In enemyDeath handling:
if (Math.random() < 0.15) {  // ← 15% drop chance (change this)
    const types = ['electric', 'fire', 'fireworks', 'shield'];
    const type = types[Math.floor(Math.random() * types.length)];
    this.spawnPowerUp(enemy.lane, enemy.y, type);
}
```

**Example changes:**
- More power-ups: Increase to `0.25` (25%)
- Fewer power-ups: Decrease to `0.08` (8%)

---

### 5. **Change Power-Up Duration**

**Location:** Search for `// ⚡ POWER-UP EFFECTS`
**Line:** ~5374-5410

```javascript
class PowerUp {
    constructor(lane, y, type, game) {
        // ...
        this.shots = type === 'fireworks' ? 40 : 50;  // ← Change shot count
    }
}
```

**Or in Player.collectPowerUp()** - Search for `// 🎁 POWER-UP COLLECTION`

```javascript
collectPowerUp(powerUp) {
    if (powerUp.type === 'shield') {
        this.shields++;
    } else {
        this.currentPowerUp = powerUp;
        this.powerUpShots = powerUp.type === 'fireworks' ? 40 : 50;  // ← Change here too
    }
}
```

---

### 6. **Change Level Progression**

**Location:** Search for `// 🏁 LEVEL PROGRESSION`
**Line:** ~2600-2700

```javascript
// Target distance per level:
const targetDistance = 12000 + (this.currentLevel * 2000);  // ← Change formula

// Boss spawn threshold:
if (this.levelProgress >= 0.95 && !this.bossSpawned) {  // ← Change 0.95 to spawn earlier/later
    this.spawnBoss();
}
```

**Example changes:**
- Longer levels: Increase multiplier (`* 3000` instead of `* 2000`)
- Boss spawns earlier: Change `0.95` to `0.85` (85% instead of 95%)

---

### 7. **Change Combo System**

**Location:** Search for `// 🎯 COMBO SYSTEM`
**Line:** ~3500-3600

```javascript
// Combo timer (how long before combo resets):
this.comboTimer -= deltaTime;
if (this.comboTimer <= 0) {
    this.combo = 0;  // Timer currently is ~2 seconds
}

// Super missile unlock:
if (this.combo >= 10 && !this.superMissileReady) {  // ← Change 10 to require more/fewer kills
    this.superMissileReady = true;
}
```

---

### 8. **Add UI Text or Change Labels**

**Location:** Multiple locations - search for the specific text

**Common locations:**
- **"COMMAND CENTER"**: Line ~741 (HTML) and ~2800 (render)
- **"LAUNCH MISSION"**: Line ~765 (HTML button)
- **"MISSION COMPLETE"**: Line ~801 (HTML)
- **HUD text**: Search for `// 🎮 HUD RENDERING` (~4100-4300)

**To change button text:**
```html
<!-- In HTML section -->
<button class="button" onclick="game.startSelectedLevel()">🚀 YOUR TEXT HERE</button>
```

**To change screen titles:** Edit the `<h1>` tags in the HTML section or the `ctx.fillText()` calls in render methods.

---

### 9. **Change Achievement Requirements**

**Location:** Search for `// 🏆 ACHIEVEMENT DEFINITIONS`
**Line:** ~2349-2430

```javascript
this.achievements = {
    firstBlood: {
        id: 'firstBlood',
        name: 'First Blood',
        desc: 'Complete Level 1',  // ← Change description
        icon: '🎯',
        unlocked: false
    },
    // Add new achievements here
};
```

**To add new achievement:**
```javascript
yourAchievement: {
    id: 'yourAchievement',
    name: 'Achievement Name',
    desc: 'How to unlock it',
    icon: '✨',
    unlocked: false
}
```

**Then add unlock logic** - Search for `// 🏆 CHECK ACHIEVEMENTS` (~3900-4000)

---

### 10. **Change Player Stats**

**Location:** Search for `// 👤 PLAYER STATS`
**Line:** ~4385-4450

```javascript
class Player {
    constructor(lane, game) {
        this.maxHp = 100 + (game.upgrades.health * 30);     // ← Base HP + upgrade bonus
        this.hp = this.maxHp;
        this.baseDamage = 10 + (game.upgrades.firepower * 5); // ← Base damage
        this.shootCooldown = 500 - (game.upgrades.attackSpeed * 50); // ← Fire rate
    }
}
```

---

## 🔍 Module Reference

| Module | Purpose | Line Range |
|--------|---------|-----------|
| **Configuration** | Ship types, themes, constants | 1661-2348 |
| **Systems** | Audio, achievements, canvas | 835-2428 |
| **Core Game** | Main game logic, loop | 2430-4384 |
| **Entities** | Player, enemies, projectiles | 4385-5409 |
| **Visual Effects** | Particles, floating text | 5410-5470 |

---

## 🎯 Workflow for Making Changes

### **Professional Workflow:**

1. **Identify what you want to change** (balance, UI, features, etc.)
2. **Use Ctrl+F** to search for the relevant section in this guide
3. **Open the game file** and search for the marker (e.g., `// ⚖️ BALANCE CONFIG`)
4. **Make ONLY the specific change** at that location
5. **Test the game** to verify the change works
6. **Document your change** (optional: add a comment)

### **Example Task: "Make coins 50% less common"**

**Steps:**
1. Search this guide for "coins" or "credits"
2. Find "Change Game Balance" section
3. Open game file, search for `// ⚖️ BALANCE CONFIG`
4. Find `creditValue` lines
5. Change: `this.creditValue = 5 + level;` → `this.creditValue = Math.floor((5 + level) * 0.5);`
6. Save and test

---

## 🚨 Critical Rules

For EVERY code update, my TODO list will ALWAYS start with:

1. ✅ Update version number (FIRST!)
2. ✅ Make code changes
3. ✅ Test changes
4. ✅ Commit and push

### ✅ **SAFE TO CHANGE:**
- Numeric values (HP, damage, costs, timers, drop rates)
- Text strings in UI (labels, titles, descriptions)
- Color values
- Formulas (as long as you don't break syntax)
- Add new ships/achievements/power-ups using existing patterns

### ❌ **DO NOT CHANGE:**
- Class names or structure
- Method signatures
- UI element positions (unless you know what you're doing)
- Core game loop logic
- Rendering system
- Audio system (unless adding sounds)

### ⚠️ **CHANGE CAREFULLY:**
- Add new features: Follow existing patterns
- Remove features: Make sure nothing else depends on them
- Change game states: Test thoroughly

---

## 💡 Tips

1. **Make one change at a time** - Easier to debug if something breaks
2. **Keep a backup** - Copy the file before major changes
3. **Test immediately** - Don't make 10 changes before testing
4. **Comment your changes** - Add `// CHANGED: reason` next to modifications
5. **Use browser console** - Press F12 to see error messages

---

## 🆘 If Something Breaks

1. **Check browser console** (F12) for error messages
2. **Undo your last change** and test again
3. **Compare with backup** to see what changed
4. **Search for typos** - Missing brackets, semicolons, quotes
5. **Ask for help** - Describe exactly what you changed and what error you see

---

## 📞 Quick Commands

**Open game:** Double-click the .html file
**View code:** Open with any text editor (VS Code, Notepad++, etc.)
**Find in file:** `Ctrl+F` (Windows/Linux) or `Cmd+F` (Mac)
**Save:** `Ctrl+S` (Windows/Linux) or `Cmd+S` (Mac)
**Reload game:** `F5` or `Ctrl+R` in browser

---

**Remember:** The restructured file has clear module boundaries marked with ASCII art. Always search for the section you need before making changes!

# SnowFree

A browser game inspired by classic downhill skiing games: you are a loose snowboard causing trouble at Black Pine Resort. All artwork is drawn with original Canvas shapes.

## Play

Open `index.html` in a modern browser. No installation or build step required.

- **Left/right arrows / A D:** steer. **Down arrow / S:** brake. Full acceleration is automatic; W/up adds no further boost.
- **Touch:** drag toward your desired position; hold near the bottom of the slope to brake.
- **Space / left-click / tap on the slope:** a small, 0.45-second hop that clears only rocks, stumps, and rough snow. Trees, grounded skiers, patrol, and yetis still collide with you; airborne skiers are above hop height. Hops carry your sideways momentum from takeoff, so hopping while moving right or left continues in that direction. You cannot change direction until landing or stack jumps. Hops have a short recovery (0.75 seconds between takeoffs), a smaller visual lift, and no speed boost.
- **P:** pause/resume. Switching away automatically pauses.
- The board always accelerates as though W were held, with an extra acceleration bonus when it points straight downhill. The bonus tapers away as you turn. Gentle carving preserves momentum; hard turns modestly scrub speed and braking slows you down. The internal speed cap is unchanged (72 km/h on the calibrated speedometer).
- A classic boxed HUD shows active run time, downhill distance in meters, speed in km/h, and your **1.0×–2.5× speed multiplier** (1× at 28.8 km/h or below, 2.5× at the 72 km/h cap). Time and distance freeze when paused. The slope camera is zoomed in **1.75×**, including touch-coordinate scaling.
- Hit roaming skiers for **100 base points**, multiplied by your speed at impact.
- Each hit adds heat; every three hits fills another wanted star, up to five. The first hit alerts patrol.
- Patrol arrives four seconds after the first skier hit, with an onscreen countdown. They enter visibly from the sides just downhill of the board, rather than spawning unseen above the camera. Further hits do not restart that delay. Reinforcements also arrive less frequently.
- Full-height patrolers in red rescue jackets get faster and more numerous with higher wanted levels. They anticipate lateral turns and adjust downhill speed to keep chasing instead of skiing past you.
- Stay away from pursuing patrol for **8 seconds** to clear your wanted level and earn **500 points per peak wanted star**, multiplied by your speed when you escape. Reinforcements stop once existing patrols lose contact, allowing the escape countdown to finish. New skier hits reset the escape timer.
- Rocks and trees damage the board. Rocks halve your speed on impact. Rough snow takes 20% of your speed on first contact and adds moderate friction over its visible bumps. Five layouts range from single moguls to pairs, winding strips, and irregular clusters. Collision areas follow the individual bumps. Yeti attacks inflict greater damage.
- Four tree silhouettes include small pines, medium pines, tall narrow firs, and broad pines, with collision widths scaled to their size.
- Small and large pixel-art tree stumps act as ground obstacles.
- Rare rainbow ramps (1.5% of terrain spawns) give a strong takeoff boost: 1.6× current speed plus 60 internal speed units, capped at maximum speed. They launch the board for roughly 1.2–1.9 seconds, depending on boosted speed. While airborne, you clear ground obstacles, rough snow, grounded skiers, patrol, and yetis, but can hit airborne skiers for points. The board rises and grows toward the camera, with a ground shadow and a snow puff on landing. Ground collisions resume on landing; ramps cannot relaunch you during the same crossing. Jumps travel straight downhill: lateral momentum stops at takeoff, and left/right steering resumes only on landing.
- Dry, bare-branched trees ignite on contact, including during small hops. Pixel flames grow through the branches, flicker, and emit rising smoke and embers. High rainbow jumps clear them without ignition.
- Every rainbow ramp reserves a straight landing corridor free of rocks, trees, stumps, and rough snow. The corridor extends past maximum-speed airtime and stays reserved as new terrain loads, even after the ramp scrolls offscreen.
- Skiers also launch from rainbow ramps, raising their arms and spreading their skis. They travel straight downhill while airborne. Their sprites rise and enlarge over a ground shadow, then resume normal directional movement on landing. A grounded or small-hopping snowboard passes underneath airborne skiers; a rainbow-jumping snowboard can knock them down with a mid-air hit.
- Patrol contact reduces speed by 65% with a 0.9-second contact cooldown, without damaging board condition. If contact leaves the board below **10 km/h on the current speedometer scale**, the run ends with **SKI PATROL GOT YOU**. Low speed alone does not cause capture without patrol contact.
- Skiers wear red hats, purple jackets, and green pants. Straight, diagonal, and sideways poses follow their actual travel direction, mirrored for left/right movement. Knockdowns leave scattered yellow skis, a lost hat, snow puffs, and an “OUCH!” caption.
- At zero board condition, the run ends. Your personal best is stored locally when available.

## Project

The camera keeps the snowboard centered horizontally and 30% down from the top, leaving more of the downhill slope visible ahead. The mountain extends endlessly to either side and downhill, generating coordinate-seeded terrain chunks around the camera and unloading distant chunks to keep memory bounded. Skiers also roam without screen-edge barriers.

Each 256-pixel terrain chunk generates up to three objects, keeping the slope open with 40% fewer spawns than the original five-object layout.

Movement and distance tracking retain their original tuning. The HUD speedometer reads one-third of the former value to better match the perceived pace. Patrol's 10 km/h capture threshold uses this calibrated speed scale.

Dependency-free HTML, CSS, and JavaScript prototype. `game.js` contains the simulation and original pixel-sprite renderer, `style.css` the classic desktop-style interface. White snow, bright limited-color sprites, and a monospace statistics box evoke early downhill games. No external fonts or assets are needed. TypeScript migration can follow once gameplay is settled.

## Share with friends

`snowfree-standalone.html` is a single self-contained file with the CSS and JavaScript inlined. Send it as an attachment and it runs offline in any modern browser by double-clicking it — no server, install, or internet connection required. Rebuild it after changing the source files:

```powershell
$root = 'E:\Games\Projeto Padrão\SnowFree'
$utf8 = New-Object System.Text.UTF8Encoding($false)
$html = [System.IO.File]::ReadAllText((Join-Path $root 'index.html'), $utf8)
$css = [System.IO.File]::ReadAllText((Join-Path $root 'style.css'), $utf8)
$js = [System.IO.File]::ReadAllText((Join-Path $root 'game.js'), $utf8)
$html = $html.Replace('<link rel="stylesheet" href="style.css">', "<style>`n$css`n</style>")
$html = $html.Replace('<script src="game.js"></script>', "<script>`n$js`n</script>")
[System.IO.File]::WriteAllText((Join-Path $root 'snowfree-standalone.html'), $html, $utf8)
```

Read and write with explicit UTF-8. `Get-Content`/`Set-Content` can double-encode the `—`, `★`, `×`, and `☆` characters.


## Planned tuning

Playtest patrol speed, heat thresholds, obstacle density, and scoring. Add sound, more terrain, and snowboard tricks after validating the core loop.

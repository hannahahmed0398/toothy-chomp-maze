import React, { useCallback, useEffect, useRef, useState } from "react";

const CELL = 28;
const RAW_MAZE = [
  "#####################",
  "#.........#.........#",
  "#.###.###.#.###.###.#",
  "#o###.###.#.###.###o#",
  "#...................#",
  "#.###.#.#####.#.###.#",
  "#.....#...#...#.....#",
  "#####.### # ###.#####",
  "    #.#       #.#    ",
  "#####.# ## ## #.#####",
  "     .  #GGG#  .     ",
  "#####.# ##### #.#####",
  "    #.#       #.#    ",
  "#####.# ##### #.#####",
  "#.........#.........#",
  "#.###.###.#.###.###.#",
  "#o..#.....P.....#..o#",
  "###.#.#.#####.#.#.###",
  "#.....#...#...#.....#",
  "#.#######.#.#######.#",
  "#...................#",
  "#.........#.........#",
  "#####################",
];

const ROWS = RAW_MAZE.length;
const COLS = RAW_MAZE[0].length;
const WIDTH = COLS * CELL;
const HEIGHT = ROWS * CELL;

const DIRS = {
  up: { x: 0, y: -1, name: "up" },
  down: { x: 0, y: 1, name: "down" },
  left: { x: -1, y: 0, name: "left" },
  right: { x: 1, y: 0, name: "right" },
  none: { x: 0, y: 0, name: "none" },
};

const KEY_DIRS = {
  ArrowUp: DIRS.up,
  KeyW: DIRS.up,
  ArrowDown: DIRS.down,
  KeyS: DIRS.down,
  ArrowLeft: DIRS.left,
  KeyA: DIRS.left,
  ArrowRight: DIRS.right,
  KeyD: DIRS.right,
};

const DIRECTIONS = [DIRS.up, DIRS.down, DIRS.left, DIRS.right];
const PLAYER_SPEED = 138;
const GHOST_SPEED = 95;
const EYES_SPEED = 185;
const POWER_TIME = 6500;
const RESPAWN_DELAY = 2500;

const GHOST_STARTS = [
  { x: 9, y: 10 },
  { x: 10, y: 10 },
  { x: 11, y: 10 },
];
const GHOST_RELEASE_DELAYS = [900, 0, 1800];
const GHOST_DOOR = { x: 10, y: 9 };
const GHOST_EXIT = { x: 10, y: 8 };
const SCATTER_TARGETS = [
  { x: 1, y: 1 },
  { x: COLS - 2, y: 1 },
  { x: Math.floor(COLS / 2), y: ROWS - 2 },
];
const GHOSTS = [
  { body: "#ff4d6d", glow: "rgba(255,77,109,0.65)", name: "Blink" },
  { body: "#4ddcff", glow: "rgba(77,220,255,0.65)", name: "Zip" },
  { body: "#b56cff", glow: "rgba(181,108,255,0.65)", name: "Shade" },
];

function key(tile) {
  return tile.x + "," + tile.y;
}

function center(tile) {
  return { x: tile.x * CELL + CELL / 2, y: tile.y * CELL + CELL / 2 };
}

function copyTile(tile) {
  return { x: tile.x, y: tile.y };
}

function parseMaze() {
  const walls = [];
  const pellets = new Set();
  const power = new Set();
  let playerStart = { x: 10, y: 16 };

  for (let y = 0; y < ROWS; y += 1) {
    const row = [];
    for (let x = 0; x < COLS; x += 1) {
      const char = RAW_MAZE[y][x];
      row.push(char === "#" ? 1 : 0);
      if (char === ".") pellets.add(key({ x, y }));
      if (char === "o") power.add(key({ x, y }));
      if (char === "P") playerStart = { x, y };
    }
    walls.push(row);
  }

  return { walls, pellets, power, playerStart };
}

const MAZE = parseMaze();
const TOTAL_PELLETS = MAZE.pellets.size + MAZE.power.size;
const ROAM_TILES = [];
for (let y = 1; y < ROWS - 1; y += 1) {
  for (let x = 1; x < COLS - 1; x += 1) {
    const inCage = y === 10 && x >= 8 && x <= 12;
    if (MAZE.walls[y][x] !== 1 && !inCage) ROAM_TILES.push({ x, y });
  }
}

function isWall(walls, tile) {
  if (tile.x < 0 || tile.x >= COLS || tile.y < 0 || tile.y >= ROWS) return true;
  return walls[tile.y][tile.x] === 1;
}

function canMove(walls, tile, dir) {
  if (!dir || dir.name === "none") return false;
  return !isWall(walls, { x: tile.x + dir.x, y: tile.y + dir.y });
}

function opposite(a, b) {
  return a.x === -b.x && a.y === -b.y;
}

function manhattan(a, b) {
  return Math.abs(a.x - b.x) + Math.abs(a.y - b.y);
}

function validDirs(walls, tile) {
  return DIRECTIONS.filter((dir) => canMove(walls, tile, dir));
}

function makeActor(tile, speed, dir) {
  const pos = center(tile);
  return {
    x: pos.x,
    y: pos.y,
    tile: copyTile(tile),
    targetTile: null,
    dir,
    nextDir: dir,
    speed,
    startTile: copyTile(tile),
    mode: "normal",
    releaseAt: 0,
    releaseIsAbsolute: false,
    immuneUntil: 0,
    roamTarget: null,
    nextRoamAt: 0,
  };
}

function makeGhost(index, level) {
  const startingDir = index === 1 ? DIRS.up : index === 0 ? DIRS.right : DIRS.left;
  const ghost = makeActor(GHOST_STARTS[index], GHOST_SPEED + level * 4 + index * 7, startingDir);
  ghost.mode = "caged";
  ghost.releaseAt = GHOST_RELEASE_DELAYS[index];
  ghost.releaseIsAbsolute = false;
  return ghost;
}

function newGame() {
  return {
    walls: MAZE.walls,
    pellets: new Set(MAZE.pellets),
    power: new Set(MAZE.power),
    player: makeActor(MAZE.playerStart, PLAYER_SPEED, DIRS.right),
    ghosts: [0, 1, 2].map((index) => makeGhost(index, 1)),
    score: 0,
    lives: 3,
    level: 1,
    status: "ready",
    message: "Press Start or choose a direction",
    startedAt: 0,
    powerUntil: 0,
    tick: 0,
    hudDirty: true,
  };
}

function resetRound(game) {
  game.player = makeActor(MAZE.playerStart, PLAYER_SPEED + game.level * 4, DIRS.right);
  game.ghosts = [0, 1, 2].map((index) => makeGhost(index, game.level));
  game.powerUntil = 0;
  game.startedAt = 0;
}

function chooseRoamTarget(index) {
  const chunk = Math.ceil(ROAM_TILES.length / 3);
  const start = index * chunk;
  const localTiles = ROAM_TILES.slice(start, start + chunk);
  const pool = localTiles.length ? localTiles : ROAM_TILES;
  return copyTile(pool[Math.floor(Math.random() * pool.length)] || SCATTER_TARGETS[index]);
}

function shortestDirection(walls, start, target, currentDir, allowReverse) {
  if (start.x === target.x && start.y === target.y) return DIRS.none;
  const queue = [{ tile: start, first: null }];
  const seen = new Set([key(start)]);

  for (let head = 0; head < queue.length; head += 1) {
    const current = queue[head];
    for (const dir of DIRECTIONS) {
      if (!allowReverse && current.first === null && opposite(dir, currentDir)) continue;
      const next = { x: current.tile.x + dir.x, y: current.tile.y + dir.y };
      if (isWall(walls, next)) continue;
      const nextKey = key(next);
      if (seen.has(nextKey)) continue;
      const first = current.first || dir;
      if (next.x === target.x && next.y === target.y) return first;
      seen.add(nextKey);
      queue.push({ tile: next, first });
    }
  }
  return DIRS.none;
}

function greedyDirection(walls, tile, target, currentDir, allowReverse) {
  let options = validDirs(walls, tile);
  if (!allowReverse) {
    const filtered = options.filter((dir) => !opposite(dir, currentDir));
    if (filtered.length) options = filtered;
  }
  if (!options.length) return DIRS.none;
  options.sort((a, b) => {
    const nextA = { x: tile.x + a.x, y: tile.y + a.y };
    const nextB = { x: tile.x + b.x, y: tile.y + b.y };
    return manhattan(nextA, target) - manhattan(nextB, target);
  });
  return options[0];
}

function ghostDirection(game, ghost, index, now, vulnerable) {
  if (ghost.mode === "eyes") {
    return shortestDirection(game.walls, ghost.tile, ghost.startTile, ghost.dir, true);
  }

  if (ghost.mode === "leaving") {
    const target = ghost.tile.y <= GHOST_DOOR.y && ghost.tile.x === GHOST_DOOR.x ? GHOST_EXIT : GHOST_DOOR;
    if (ghost.tile.y <= GHOST_EXIT.y) return index === 0 ? DIRS.left : index === 1 ? DIRS.up : DIRS.right;
    return shortestDirection(game.walls, ghost.tile, target, ghost.dir, true);
  }

  const playerTile = game.player.tile;
  const cycle = Math.floor((now + index * 1300) / 4700) % 4;
  let target = playerTile;

  if (vulnerable) {
    target = SCATTER_TARGETS[index];
  } else if (cycle === 0) {
    if (!ghost.roamTarget || now > ghost.nextRoamAt || manhattan(ghost.tile, ghost.roamTarget) <= 1) {
      ghost.roamTarget = chooseRoamTarget(index);
      ghost.nextRoamAt = now + 2500 + index * 600;
    }
    target = ghost.roamTarget;
  } else if (index === 1) {
    target = {
      x: Math.max(1, Math.min(COLS - 2, playerTile.x + game.player.dir.x * 5)),
      y: Math.max(1, Math.min(ROWS - 2, playerTile.y + game.player.dir.y * 5)),
    };
    if (isWall(game.walls, target)) target = playerTile;
  } else if (index === 2) {
    target = cycle === 1 ? { x: COLS - playerTile.x - 1, y: ROWS - playerTile.y - 1 } : SCATTER_TARGETS[index];
  }

  const randomChance = index === 0 ? 0.08 : index === 1 ? 0.16 : 0.25;
  const options = validDirs(game.walls, ghost.tile).filter((dir) => !opposite(dir, ghost.dir));
  if (options.length && Math.random() < randomChance) return options[Math.floor(Math.random() * options.length)];
  return greedyDirection(game.walls, ghost.tile, target, ghost.dir, false);
}

function startStep(actor, walls) {
  if (canMove(walls, actor.tile, actor.nextDir)) actor.dir = actor.nextDir;
  if (!canMove(walls, actor.tile, actor.dir)) {
    actor.targetTile = null;
    return;
  }
  actor.targetTile = { x: actor.tile.x + actor.dir.x, y: actor.tile.y + actor.dir.y };
}

function moveActor(actor, walls, dt) {
  if (!actor.targetTile) startStep(actor, walls);
  if (!actor.targetTile) return;

  const target = center(actor.targetTile);
  const dx = target.x - actor.x;
  const dy = target.y - actor.y;
  const distance = Math.hypot(dx, dy);
  const step = actor.speed * dt;

  if (distance <= step || distance < 0.05) {
    actor.x = target.x;
    actor.y = target.y;
    actor.tile = copyTile(actor.targetTile);
    actor.targetTile = null;
  } else {
    actor.x += (dx / distance) * step;
    actor.y += (dy / distance) * step;
  }
}

function distancePixels(a, b) {
  return Math.hypot(a.x - b.x, a.y - b.y);
}

function roundedRect(ctx, x, y, w, h, r) {
  ctx.beginPath();
  ctx.moveTo(x + r, y);
  ctx.arcTo(x + w, y, x + w, y + h, r);
  ctx.arcTo(x + w, y + h, x, y + h, r);
  ctx.arcTo(x, y + h, x, y, r);
  ctx.arcTo(x, y, x + w, y, r);
  ctx.closePath();
}

function makeStaticCanvas(walls, dpr) {
  const canvas = document.createElement("canvas");
  canvas.width = WIDTH * dpr;
  canvas.height = HEIGHT * dpr;
  const ctx = canvas.getContext("2d");
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

  const bg = ctx.createRadialGradient(WIDTH / 2, HEIGHT / 2, 40, WIDTH / 2, HEIGHT / 2, HEIGHT * 0.8);
  bg.addColorStop(0, "#111c3a");
  bg.addColorStop(0.58, "#080b1d");
  bg.addColorStop(1, "#02030a");
  ctx.fillStyle = bg;
  ctx.fillRect(0, 0, WIDTH, HEIGHT);

  ctx.save();
  ctx.shadowColor = "#2d7cff";
  ctx.shadowBlur = 12;
  for (let y = 0; y < ROWS; y += 1) {
    for (let x = 0; x < COLS; x += 1) {
      if (walls[y][x] !== 1) continue;
      const px = x * CELL;
      const py = y * CELL;
      const wall = ctx.createLinearGradient(px, py, px + CELL, py + CELL);
      wall.addColorStop(0, "#133eff");
      wall.addColorStop(0.55, "#3616b8");
      wall.addColorStop(1, "#0df2ff");
      roundedRect(ctx, px + 2, py + 2, CELL - 4, CELL - 4, 8);
      ctx.fillStyle = wall;
      ctx.fill();
      ctx.strokeStyle = "rgba(181,244,255,0.75)";
      ctx.lineWidth = 1.2;
      ctx.stroke();
    }
  }
  ctx.restore();
  return canvas;
}

function drawPellets(ctx, game) {
  ctx.save();
  ctx.shadowColor = "rgba(255,246,173,0.75)";
  ctx.shadowBlur = 7;
  ctx.fillStyle = "rgba(255,246,173,0.95)";
  game.pellets.forEach((pelletKey) => {
    const parts = pelletKey.split(",").map(Number);
    const pos = center({ x: parts[0], y: parts[1] });
    ctx.beginPath();
    ctx.arc(pos.x, pos.y, 3.1, 0, Math.PI * 2);
    ctx.fill();
  });
  game.power.forEach((pelletKey) => {
    const parts = pelletKey.split(",").map(Number);
    const pos = center({ x: parts[0], y: parts[1] });
    const pulse = 1 + Math.sin(game.tick / 9) * 0.2;
    ctx.fillStyle = "white";
    ctx.shadowColor = "#fffb8f";
    ctx.shadowBlur = 16;
    ctx.beginPath();
    ctx.arc(pos.x, pos.y, 7.4 * pulse, 0, Math.PI * 2);
    ctx.fill();
  });
  ctx.restore();
}

function drawRose(ctx, x, y, scale = 1) {
  ctx.save();
  ctx.translate(x, y);
  ctx.scale(scale, scale);
  ctx.shadowColor = "rgba(255,77,109,0.8)";
  ctx.shadowBlur = 6;
  ctx.fillStyle = "#ff4d6d";
  for (let i = 0; i < 5; i += 1) {
    const angle = (Math.PI * 2 * i) / 5;
    ctx.beginPath();
    ctx.ellipse(Math.cos(angle) * 3.2, Math.sin(angle) * 3.2, 3.2, 4.5, angle, 0, Math.PI * 2);
    ctx.fill();
  }
  ctx.fillStyle = "#ffd1dc";
  ctx.beginPath();
  ctx.arc(0, 0, 2.4, 0, Math.PI * 2);
  ctx.fill();
  ctx.shadowBlur = 0;
  ctx.strokeStyle = "#39d98a";
  ctx.lineWidth = 1.4;
  ctx.beginPath();
  ctx.moveTo(-1, 5);
  ctx.quadraticCurveTo(-4, 8, -7, 7);
  ctx.stroke();
  ctx.fillStyle = "#39d98a";
  ctx.beginPath();
  ctx.ellipse(-6.5, 7, 2.5, 1.5, -0.4, 0, Math.PI * 2);
  ctx.fill();
  ctx.restore();
}

function drawPlayer(ctx, player, tick, powered) {
  const radius = CELL * 0.43;
  const bite = 0.25 + Math.abs(Math.sin(tick / 6)) * 0.18;
  const dir = player.dir.name === "none" ? DIRS.right : player.dir;

  ctx.save();
  ctx.translate(player.x, player.y);
  if (dir.name === "left") ctx.scale(-1, 1);
  if (dir.name === "up") ctx.rotate(-Math.PI / 2);
  if (dir.name === "down") ctx.rotate(Math.PI / 2);

  const grad = ctx.createRadialGradient(-5, -6, 4, 0, 0, radius + 4);
  grad.addColorStop(0, "#fff6a6");
  grad.addColorStop(0.55, "#ffd629");
  grad.addColorStop(1, "#ff9f1c");
  ctx.shadowColor = powered ? "rgba(255,255,255,0.85)" : "rgba(255,225,44,0.72)";
  ctx.shadowBlur = powered ? 22 : 15;
  ctx.beginPath();
  ctx.moveTo(0, 0);
  ctx.arc(0, 0, radius, bite, Math.PI * 2 - bite);
  ctx.closePath();
  ctx.fillStyle = grad;
  ctx.fill();
  ctx.strokeStyle = "rgba(255,255,255,0.45)";
  ctx.lineWidth = 2;
  ctx.stroke();

  ctx.shadowBlur = 0;
  ctx.fillStyle = "#101426";
  ctx.beginPath();
  ctx.arc(2, -radius * 0.45, 3.3, 0, Math.PI * 2);
  ctx.fill();

  ctx.fillStyle = "white";
  const teeth = [
    [radius * 0.45, -radius * 0.12, radius * 0.76, -radius * 0.35, radius * 0.78, -radius * 0.03],
    [radius * 0.18, -radius * 0.04, radius * 0.5, -radius * 0.22, radius * 0.5, radius * 0.04],
    [radius * 0.45, radius * 0.12, radius * 0.76, radius * 0.35, radius * 0.78, radius * 0.03],
    [radius * 0.18, radius * 0.04, radius * 0.5, radius * 0.22, radius * 0.5, -radius * 0.04],
  ];
  teeth.forEach((tooth) => {
    ctx.beginPath();
    ctx.moveTo(tooth[0], tooth[1]);
    ctx.lineTo(tooth[2], tooth[3]);
    ctx.lineTo(tooth[4], tooth[5]);
    ctx.closePath();
    ctx.fill();
  });

  // Drawn inside Toothy's own rotation/mirror transform, so the flower stays
  // behind the eye on the back/top of his head in every movement direction.
  drawRose(ctx, -radius * 0.48, -radius * 0.72, 0.9);
  ctx.restore();
}

function drawGhost(ctx, ghost, index, vulnerable, tick) {
  const bob = Math.sin(tick / 10 + index) * 2;
  ctx.save();
  ctx.translate(ghost.x, ghost.y + bob);

  const r = CELL * 0.39;
  if (ghost.mode === "eyes") {
    ctx.shadowColor = "rgba(255,255,255,0.7)";
    ctx.shadowBlur = 12;
    ctx.fillStyle = "white";
    ctx.beginPath();
    ctx.ellipse(-r * 0.35, -r * 0.18, 5.5, 7, 0, 0, Math.PI * 2);
    ctx.ellipse(r * 0.35, -r * 0.18, 5.5, 7, 0, 0, Math.PI * 2);
    ctx.fill();
    ctx.shadowBlur = 0;
    ctx.fillStyle = "#101426";
    ctx.beginPath();
    ctx.arc(-r * 0.35 + ghost.dir.x * 2.6, -r * 0.18 + ghost.dir.y * 2.6, 2.6, 0, Math.PI * 2);
    ctx.arc(r * 0.35 + ghost.dir.x * 2.6, -r * 0.18 + ghost.dir.y * 2.6, 2.6, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
    return;
  }

  const color = vulnerable ? "#3555ff" : GHOSTS[index].body;
  const glow = vulnerable ? "rgba(90,140,255,0.75)" : GHOSTS[index].glow;
  const grad = ctx.createLinearGradient(0, -r, 0, r);
  grad.addColorStop(0, vulnerable ? "#88a8ff" : color);
  grad.addColorStop(1, vulnerable ? "#2032a0" : "#101426");
  ctx.shadowColor = glow;
  ctx.shadowBlur = 15;
  ctx.beginPath();
  ctx.arc(0, -3, r, Math.PI, 0);
  ctx.lineTo(r, r * 0.62);
  for (let i = 0; i < 4; i += 1) {
    ctx.lineTo(r - (i + 0.5) * ((r * 2) / 4), i % 2 === 0 ? r * 0.92 : r * 0.58);
  }
  ctx.lineTo(-r, r * 0.62);
  ctx.closePath();
  ctx.fillStyle = grad;
  ctx.fill();
  ctx.strokeStyle = "rgba(255,255,255,0.35)";
  ctx.lineWidth = 1.5;
  ctx.stroke();

  ctx.shadowBlur = 0;
  ctx.fillStyle = "white";
  ctx.beginPath();
  ctx.ellipse(-r * 0.35, -r * 0.18, 4.5, 6, 0, 0, Math.PI * 2);
  ctx.ellipse(r * 0.35, -r * 0.18, 4.5, 6, 0, 0, Math.PI * 2);
  ctx.fill();
  ctx.fillStyle = "#101426";
  ctx.beginPath();
  ctx.arc(-r * 0.35 + ghost.dir.x * 2.2, -r * 0.18 + ghost.dir.y * 2.2, 2.2, 0, Math.PI * 2);
  ctx.arc(r * 0.35 + ghost.dir.x * 2.2, -r * 0.18 + ghost.dir.y * 2.2, 2.2, 0, Math.PI * 2);
  ctx.fill();

  if (vulnerable) {
    ctx.strokeStyle = "white";
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.moveTo(-8, 7);
    ctx.lineTo(-4, 10);
    ctx.lineTo(0, 7);
    ctx.lineTo(4, 10);
    ctx.lineTo(8, 7);
    ctx.stroke();
  }
  ctx.restore();
}

function drawOverlay(ctx, game) {
  if (game.status === "playing") return;
  ctx.save();
  ctx.fillStyle = "rgba(2,3,10,0.62)";
  ctx.fillRect(0, 0, WIDTH, HEIGHT);
  ctx.textAlign = "center";
  ctx.fillStyle = "white";
  ctx.shadowColor = "#8be9fd";
  ctx.shadowBlur = 16;
  ctx.font = "700 28px Inter, system-ui, sans-serif";
  const title = game.status === "gameover" ? "TOOTHY CHOMP GOT CAUGHT" : game.status === "win" ? "MAZE CLEARED" : "TOOTHY CHOMP";
  ctx.fillText(title, WIDTH / 2, HEIGHT / 2 - 22);
  ctx.font = "500 15px Inter, system-ui, sans-serif";
  ctx.fillText(game.message, WIDTH / 2, HEIGHT / 2 + 12);
  ctx.restore();
}

function updateGame(game, dt, now) {
  game.tick += dt * 60;
  if (game.status !== "playing") return;

  const powered = now < game.powerUntil;
  moveActor(game.player, game.walls, dt);

  const playerKey = key(game.player.tile);
  if (game.pellets.delete(playerKey)) {
    game.score += 10;
    game.hudDirty = true;
  }
  if (game.power.delete(playerKey)) {
    game.score += 50;
    game.powerUntil = now + POWER_TIME;
    game.hudDirty = true;
  }

  if (game.pellets.size + game.power.size === 0) {
    game.score += 500;
    game.status = "win";
    game.message = "Level cleared! Press Restart to play again.";
    game.hudDirty = true;
    return;
  }

  game.ghosts.forEach((ghost, index) => {
    if (ghost.mode === "eyes") {
      if (!ghost.targetTile && ghost.tile.x === ghost.startTile.x && ghost.tile.y === ghost.startTile.y) {
        const rebuilt = makeGhost(index, game.level);
        Object.assign(ghost, rebuilt);
        ghost.releaseAt = now + RESPAWN_DELAY + index * 300;
        ghost.releaseIsAbsolute = true;
        ghost.immuneUntil = game.powerUntil;
        return;
      }
      if (!ghost.targetTile) ghost.nextDir = ghostDirection(game, ghost, index, now, false);
      moveActor(ghost, game.walls, dt);
      return;
    }

    if (ghost.mode === "caged") {
      const absoluteRelease = ghost.releaseIsAbsolute ? ghost.releaseAt : game.startedAt + ghost.releaseAt;
      if (now < absoluteRelease) return;
      ghost.mode = "leaving";
      ghost.targetTile = null;
      ghost.nextDir = ghostDirection(game, ghost, index, now, false);
    }

    if (!ghost.targetTile) {
      if (ghost.mode === "leaving" && ghost.tile.y <= GHOST_EXIT.y) {
        ghost.mode = "normal";
        ghost.roamTarget = chooseRoamTarget(index);
        ghost.nextRoamAt = now + 1400 + index * 500;
      }
      const vulnerable = powered && ghost.mode !== "caged" && now >= ghost.immuneUntil;
      ghost.nextDir = ghostDirection(game, ghost, index, now, vulnerable);
    }
    moveActor(ghost, game.walls, dt);
  });

  for (let i = 0; i < game.ghosts.length; i += 1) {
    const ghost = game.ghosts[i];
    if (distancePixels(game.player, ghost) >= CELL * 0.58) continue;
    const vulnerable = powered && ghost.mode !== "caged" && ghost.mode !== "eyes" && now >= ghost.immuneUntil;
    if (vulnerable) {
      game.score += 200;
      ghost.mode = "eyes";
      ghost.speed = EYES_SPEED + game.level * 5;
      ghost.targetTile = null;
      ghost.nextDir = shortestDirection(game.walls, ghost.tile, ghost.startTile, ghost.dir, true);
      ghost.immuneUntil = game.powerUntil;
      game.hudDirty = true;
    } else if (ghost.mode !== "eyes") {
      game.lives -= 1;
      game.hudDirty = true;
      if (game.lives <= 0) {
        game.status = "gameover";
        game.message = "Game over. Press Restart to try again.";
      } else {
        resetRound(game);
        game.status = "ready";
        game.message = "Ouch! Press Start or choose a direction.";
      }
    }
    break;
  }
}

function drawFrame(ctx, staticCanvas, game, dpr, now) {
  ctx.setTransform(1, 0, 0, 1, 0, 0);
  ctx.clearRect(0, 0, WIDTH * dpr, HEIGHT * dpr);
  ctx.drawImage(staticCanvas, 0, 0);
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);

  const powered = now < game.powerUntil;
  drawPellets(ctx, game);
  game.ghosts.forEach((ghost, index) => {
    const vulnerable = powered && ghost.mode !== "caged" && ghost.mode !== "eyes" && now >= ghost.immuneUntil;
    drawGhost(ctx, ghost, index, vulnerable, game.tick);
  });
  drawPlayer(ctx, game.player, game.tick, powered);
  drawOverlay(ctx, game);
}

export default function ToothyChompMazeGame() {
  const canvasRef = useRef(null);
  const frameRef = useRef(null);
  const staticCanvasRef = useRef(null);
  const lastTimeRef = useRef(0);
  const hudTimeRef = useRef(0);
  const gameRef = useRef(newGame());

  const [hud, setHud] = useState({
    score: 0,
    lives: 3,
    level: 1,
    status: "ready",
    remaining: TOTAL_PELLETS,
  });

  const syncHud = useCallback(() => {
    const game = gameRef.current;
    setHud({
      score: game.score,
      lives: game.lives,
      level: game.level,
      status: game.status,
      remaining: game.pellets.size + game.power.size,
    });
    game.hudDirty = false;
  }, []);

  const startGame = useCallback(() => {
    const game = gameRef.current;
    if (game.status === "gameover" || game.status === "win") {
      gameRef.current = newGame();
      gameRef.current.status = "playing";
      gameRef.current.message = "";
      gameRef.current.startedAt = performance.now();
    } else {
      game.status = "playing";
      game.message = "";
      if (!game.startedAt) game.startedAt = performance.now();
      game.hudDirty = true;
    }
    syncHud();
  }, [syncHud]);

  const resetGame = useCallback(() => {
    gameRef.current = newGame();
    syncHud();
  }, [syncHud]);

  const pauseGame = useCallback(() => {
    const game = gameRef.current;
    if (game.status === "playing") {
      game.status = "paused";
      game.message = "Paused";
    } else if (game.status === "paused") {
      game.status = "playing";
      game.message = "";
    }
    game.hudDirty = true;
    syncHud();
  }, [syncHud]);

  const changeDirection = useCallback((dir) => {
    const game = gameRef.current;
    game.player.nextDir = dir;
    if (game.status === "ready") {
      game.status = "playing";
      game.message = "";
      game.startedAt = performance.now();
    }
    game.hudDirty = true;
    syncHud();
  }, [syncHud]);

  useEffect(() => {
    const handleKeyDown = (event) => {
      if (event.code === "Space") {
        event.preventDefault();
        pauseGame();
        return;
      }
      if (event.code === "Enter") {
        event.preventDefault();
        if (gameRef.current.status === "gameover" || gameRef.current.status === "win") resetGame();
        else startGame();
        return;
      }
      const dir = KEY_DIRS[event.code];
      if (dir) {
        event.preventDefault();
        changeDirection(dir);
      }
    };
    window.addEventListener("keydown", handleKeyDown);
    return () => window.removeEventListener("keydown", handleKeyDown);
  }, [changeDirection, pauseGame, resetGame, startGame]);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return undefined;
    const ctx = canvas.getContext("2d", { alpha: false });
    const dpr = Math.min(window.devicePixelRatio || 1, 2);
    canvas.width = WIDTH * dpr;
    canvas.height = HEIGHT * dpr;
    canvas.style.width = WIDTH + "px";
    canvas.style.height = HEIGHT + "px";
    staticCanvasRef.current = makeStaticCanvas(gameRef.current.walls, dpr);

    const loop = (now) => {
      const last = lastTimeRef.current || now;
      const dt = Math.min((now - last) / 1000, 0.033);
      lastTimeRef.current = now;
      const game = gameRef.current;
      updateGame(game, dt, now);
      drawFrame(ctx, staticCanvasRef.current, game, dpr, now);
      if (game.hudDirty || now - hudTimeRef.current > 180) {
        hudTimeRef.current = now;
        syncHud();
      }
      frameRef.current = requestAnimationFrame(loop);
    };

    frameRef.current = requestAnimationFrame(loop);
    return () => cancelAnimationFrame(frameRef.current);
  }, [syncHud]);

  const progress = Math.round(((TOTAL_PELLETS - hud.remaining) / TOTAL_PELLETS) * 100);

  return (
    <div className="min-h-screen w-full bg-slate-950 text-white flex items-center justify-center p-4 overflow-hidden">
      <div
        className="absolute inset-0 pointer-events-none opacity-40"
        style={{
          background: "radial-gradient(circle at 20% 20%, rgba(45,124,255,0.35), transparent 30%), radial-gradient(circle at 80% 10%, rgba(181,108,255,0.24), transparent 28%), radial-gradient(circle at 50% 90%, rgba(255,214,41,0.12), transparent 35%)",
        }}
      />

      <main className="relative w-full max-w-6xl grid gap-5 lg:grid-cols-[auto_320px] items-center">
        <section className="rounded-[2rem] border border-cyan-300/20 bg-white/5 shadow-2xl shadow-cyan-500/10 backdrop-blur p-3 sm:p-4 mx-auto">
          <canvas
            ref={canvasRef}
            className="rounded-[1.5rem] border border-white/10 max-w-full h-auto shadow-inner shadow-black/50"
            aria-label="Toothy Chomp maze game canvas"
          />
        </section>

        <aside className="rounded-[2rem] border border-white/10 bg-white/10 backdrop-blur-xl p-5 shadow-2xl shadow-black/30">
          <div className="mb-5">
            <p className="text-cyan-200 text-sm font-semibold tracking-[0.3em] uppercase">Arcade</p>
            <h1 className="text-3xl font-black mt-1 bg-gradient-to-r from-yellow-200 via-amber-300 to-cyan-200 bg-clip-text text-transparent">
              Toothy Chomp
            </h1>
            <p className="text-slate-300 mt-2 text-sm leading-6">
              Eat pellets, chomp blue ghosts, and watch their eyes take the shortest route back home.
            </p>
          </div>

          <div className="grid grid-cols-3 gap-3 mb-5">
            <div className="rounded-2xl bg-black/30 border border-white/10 p-3">
              <p className="text-xs text-slate-400">Score</p>
              <p className="text-2xl font-black text-yellow-200">{hud.score}</p>
            </div>
            <div className="rounded-2xl bg-black/30 border border-white/10 p-3">
              <p className="text-xs text-slate-400">Lives</p>
              <p className="text-2xl font-black text-rose-200">{"❤".repeat(Math.max(0, hud.lives))}</p>
            </div>
            <div className="rounded-2xl bg-black/30 border border-white/10 p-3">
              <p className="text-xs text-slate-400">Level</p>
              <p className="text-2xl font-black text-cyan-200">{hud.level}</p>
            </div>
          </div>

          <div className="mb-5 rounded-2xl bg-black/30 border border-white/10 p-4">
            <div className="flex items-center justify-between text-sm mb-2">
              <span className="text-slate-300">Pellets cleared</span>
              <span className="font-bold text-cyan-100">{progress}%</span>
            </div>
            <div className="h-3 rounded-full bg-slate-800 overflow-hidden">
              <div
                className="h-full rounded-full bg-gradient-to-r from-yellow-300 via-amber-400 to-cyan-300 transition-all duration-300"
                style={{ width: progress + "%" }}
              />
            </div>
          </div>

          <div className="grid grid-cols-2 gap-3 mb-5">
            <button
              onClick={hud.status === "gameover" || hud.status === "win" ? resetGame : startGame}
              className="rounded-2xl px-4 py-3 bg-yellow-300 text-slate-950 font-black shadow-lg shadow-yellow-300/20 hover:scale-[1.02] active:scale-95 transition"
            >
              {hud.status === "playing" ? "Playing" : hud.status === "gameover" || hud.status === "win" ? "Restart" : "Start"}
            </button>
            <button
              onClick={pauseGame}
              className="rounded-2xl px-4 py-3 bg-cyan-300 text-slate-950 font-black shadow-lg shadow-cyan-300/20 hover:scale-[1.02] active:scale-95 transition"
            >
              Pause
            </button>
          </div>

          <div className="rounded-2xl bg-black/30 border border-white/10 p-4 mb-5">
            <p className="text-sm font-bold text-white mb-3">Controls</p>
            <p className="text-sm text-slate-300 leading-6">Use arrow keys or WASD. Space pauses. Enter starts or restarts.</p>
          </div>

          <div className="grid grid-cols-3 gap-2 max-w-52 mx-auto select-none">
            <span />
            <button onClick={() => changeDirection(DIRS.up)} className="rounded-xl bg-white/10 hover:bg-white/20 border border-white/10 py-3 font-black">↑</button>
            <span />
            <button onClick={() => changeDirection(DIRS.left)} className="rounded-xl bg-white/10 hover:bg-white/20 border border-white/10 py-3 font-black">←</button>
            <button onClick={() => changeDirection(DIRS.down)} className="rounded-xl bg-white/10 hover:bg-white/20 border border-white/10 py-3 font-black">↓</button>
            <button onClick={() => changeDirection(DIRS.right)} className="rounded-xl bg-white/10 hover:bg-white/20 border border-white/10 py-3 font-black">→</button>
          </div>

          <div className="mt-5 flex items-center justify-center gap-3 text-xs text-slate-400">
            {GHOSTS.map((ghost) => (
              <span key={ghost.name} className="inline-flex items-center gap-1.5">
                <span className="size-2.5 rounded-full" style={{ backgroundColor: ghost.body }} />
                {ghost.name}
              </span>
            ))}
          </div>
        </aside>
      </main>
    </div>
  );
}

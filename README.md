
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Speed Racer</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body {
  margin: 0;
  background: #222;
  overflow: hidden;
  font-family: Arial, sans-serif;
}
#road {
  position: relative;
  width: 300px;
  height: 100vh;
  margin: auto;
  background: #555;
  overflow: hidden;
}
.line {
  position: absolute;
  width: 10px;
  height: 80px;
  background: white;
  left: 145px;
}
#player {
  position: absolute;
  width: 50px;
  height: 90px;
  background: red;
  bottom: 20px;
  left: 125px;
  border-radius: 10px;
}
.enemy {
  position: absolute;
  width: 50px;
  height: 90px;
  background: blue;
  top: -100px;
  border-radius: 10px;
}
#score {
  position: fixed;
  top: 10px;
  left: 10px;
  color: white;
  font-size: 18px;
}
#gameOver {
  display: none;
  position: fixed;
  top: 40%;
  width: 100%;
  text-align: center;
  color: white;
}
button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 20px;
}
</style>
</head>

<body>
<div id="score">Score: 0</div>

<div id="road">
  <div id="player"></div>
</div>

<div id="gameOver">
  <h1>GAME OVER</h1>
  <button onclick="location.reload()">Restart</button>
</div>

<script>
const road = document.getElementById("road");
const player = document.getElementById("player");
const scoreText = document.getElementById("score");
const gameOverText = document.getElementById("gameOver");

let speed = 5;
let score = 0;
let gameRunning = true;
let playerX = 125;

let touchX = null;

// Road lines
for (let i = 0; i < 5; i++) {
  let line = document.createElement("div");
  line.className = "line";
  line.style.top = (i * 150) + "px";
  road.appendChild(line);
}

// Enemy car
let enemy = document.createElement("div");
enemy.className = "enemy";
enemy.style.left = Math.random() * 250 + "px";
road.appendChild(enemy);
let enemyY = -100;

// Touch controls
road.addEventListener("touchstart", e => {
  touchX = e.touches[0].clientX;
});
road.addEventListener("touchmove", e => {
  if (!touchX) return;
  let deltaX = e.touches[0].clientX - touchX;
  playerX += deltaX * 0.5;
  if (playerX < 0) playerX = 0;
  if (playerX > 250) playerX = 250;
  player.style.left = playerX + "px";
  touchX = e.touches[0].clientX;
});

function gameLoop() {
  if (!gameRunning) return;

  // Move road lines
  document.querySelectorAll(".line").forEach(line => {
    let top = parseInt(line.style.top);
    top += speed;
    if (top > 600) top = -100;
    line.style.top = top + "px";
  });

  // Move enemy
  enemyY += speed + 2;
  enemy.style.top = enemyY + "px";

  if (enemyY > 700) {
    enemyY = -120;
    enemy.style.left = Math.random() * 250 + "px";
    score += 10;
    speed += 0.2;
  }

  // Collision detection
  let p = player.getBoundingClientRect();
  let e = enemy.getBoundingClientRect();

  if (
    p.left < e.right &&
    p.right > e.left &&
    p.top < e.bottom &&
    p.bottom > e.top
  ) {
    gameRunning = false;
    gameOverText.style.display = "block";
  }

  scoreText.innerText = "Score: " + score;

  requestAnimationFrame(gameLoop);
}

gameLoop();
</script>
</body>
</html>

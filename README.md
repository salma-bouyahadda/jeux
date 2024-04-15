<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>لعب التقاط المكعبات</title>
<style>
  body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #776e6e;
}

#game-container {
  width: 450px;
  height: 410px;
  margin: 50px auto;
  border: 4px solid #000000;
  background-color: bisque;
  border-radius: 15%;
  position: relative;
}

.cube {
  width: 50px;
  height: 50px;
  background-color: #fff980;
  position: absolute;
  top: 0;
  border-radius: 50%;
}

#player {
  width: 60px;
  height : 60px;
  background-color: rgb(77, 108, 228);
  position: absolute;
  margin-bottom: -33cap;
  bottom: 0;
  border-radius: 20%;
  left: 0;
  transform: translateX(0PX);
  border: 1px solid black;
}

#score {
  position: absolute;
  top: 20px;
  right: 20px;
  font-size: 25px;
  color: #000;
  border: 2px solid black;
  border-radius: 5px;
  background-color: #3CB371;
}

#restart-btn {
  position: absolute;
  bottom: 20px;
  margin:50px 50px;
  background-color: #3CB371;
  color: #fff;
  font-size: 18px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  display: none;
}

#message {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 30px;
  font-weight: bold;
  color: #fff;
  display: none;
}
</style>
</head>
<body>
<div id="game-container">
  <div id="player"></div>
</div>
<div id="score">Score: 0</div>
<button id="restart-btn" onclick="restartGame()">Restart</button>
<div id="message"></div>
<script>
document.addEventListener('DOMContentLoaded', () => {
  const gameContainer = document.getElementById('game-container');
  const player = document.getElementById('player');
  const scoreDisplay = document.getElementById('score');
  const restartButton = document.getElementById('restart-btn');
  const message = document.getElementById('message');
  let score = 0;
  let cubesCollected = 0;
  let fallingIntervals = [];

  let playerX = gameContainer.offsetWidth / 2 - player.offsetWidth / 2;
  let playerY = gameContainer.offsetHeight - player.offsetHeight;
  player.style.left = `${playerX}px`;
  player.style.bottom = `${playerY}px`;

  document.addEventListener('keydown', (event) => {
    const key = event.key;
    const step = 10;

    if (key === 'ArrowLeft') {
      playerX -= step;
      if (playerX < 0) playerX = 0;
    } else if (key === 'ArrowRight') {
      playerX += step;
      if (playerX + player.offsetWidth > gameContainer.offsetWidth) {
        playerX = gameContainer.offsetWidth - player.offsetWidth;
      }
    }
    player.style.left = `${playerX}px`;
  });

  function createCube() {
    const cube = document.createElement('div');
    cube.classList.add('cube');
    cube.style.left = `${Math.random() * (gameContainer.offsetWidth - 50)}px`;
    gameContainer.appendChild(cube);

    const fallingInterval = setInterval(() => {
      const cubeBottom = cube.offsetTop + cube.offsetHeight;
      const playerBottom = player.offsetTop + player.offsetHeight;
      const playerLeft = player.offsetLeft;
      const playerRight = playerLeft + player.offsetWidth;

      if (cubeBottom >= gameContainer.offsetHeight) {
        clearInterval(fallingInterval);
        gameContainer.removeChild(cube);
        cubesCollected++;
        if (cubesCollected === 5) {
          message.innerText = 'Perdu! Vous avez perdu';
          message.style.display = 'block';
          restartButton.style.display = 'block';
          clearInterval(cubeInterval);
        }
      } else {
        cube.style.top = `${cube.offsetTop + 5}px`;
        if (
          cubeBottom >= player.offsetTop &&
          cube.offsetTop <= playerBottom &&
          playerLeft <= cube.offsetLeft &&
          playerRight >= cube.offsetLeft + cube.offsetWidth
        ) {
          clearInterval(fallingInterval);
          gameContainer.removeChild(cube);
          score++;
          scoreDisplay.innerText = `Score: ${score}`;
          cubesCollected++;
          if (score === 3) {
            message.innerText = 'Félicitations! Vous avez gagné';
            message.style.display = 'block';
            restartButton.style.display = 'block';
            clearInterval(cubeInterval);
          }
        }
      }
    }, 20);

    fallingIntervals.push(fallingInterval);
  }

  let cubeInterval = setInterval(createCube, 1500);

  restartButton.addEventListener('click', () => {
    score = 0;
    cubesCollected = 0;
    scoreDisplay.innerText = 'Score: 0';
    message.style.display = 'none';
    restartButton.style.display = 'none';
    player.style.left = `${gameContainer.offsetWidth / 2 - player.offsetWidth / 2}px`;
    fallingIntervals.forEach(interval => clearInterval(interval));
    cubeInterval = setInterval(createCube, 1500);
  });
});

function restartGame() {
  window.location.reload();
}
</script>
</body>
</html>

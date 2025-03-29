<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dino Run</title>
    <style>
        body {
            text-align: center;
            font-family: Arial, sans-serif;
        }
        canvas {
            border: 2px solid black;
            background-color: lightgray;
        }
    </style>
</head>
<body>
    <h1>Dino Run Game</h1>
    <canvas id="gameCanvas"></canvas>
    <p>กด <strong>Spacebar</strong> เพื่อกระโดด!</p>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        canvas.width = 800;
        canvas.height = 200;

        let dino = { x: 50, y: 150, width: 30, height: 30, velocityY: 0, jumping: false };
        let gravity = 0.5;
        let obstacles = [];
        let gameOver = false;
        let score = 0;

        function drawDino() {
            ctx.fillStyle = "black";
            ctx.fillRect(dino.x, dino.y, dino.width, dino.height);
        }

        function drawObstacles() {
            ctx.fillStyle = "red";
            obstacles.forEach(obs => ctx.fillRect(obs.x, obs.y, obs.width, obs.height));
        }

        function updateObstacles() {
            if (Math.random() < 0.02) {
                obstacles.push({ x: canvas.width, y: 150, width: 20, height: 30 });
            }
            obstacles.forEach(obs => obs.x -= 5);
            obstacles = obstacles.filter(obs => obs.x + obs.width > 0);
        }

        function checkCollision() {
            obstacles.forEach(obs => {
                if (dino.x < obs.x + obs.width &&
                    dino.x + dino.width > obs.x &&
                    dino.y < obs.y + obs.height &&
                    dino.y + dino.height > obs.y) {
                    gameOver = true;
                }
            });
        }

        function update() {
            if (gameOver) {
                ctx.fillStyle = "black";
                ctx.font = "30px Arial";
                ctx.fillText("Game Over!", canvas.width / 2 - 70, canvas.height / 2);
                return;
            }

            ctx.clearRect(0, 0, canvas.width, canvas.height);

            dino.y += dino.velocityY;
            if (dino.y + dino.height >= 150) {
                dino.y = 150;
                dino.jumping = false;
            } else {
                dino.velocityY += gravity;
            }

            updateObstacles();
            checkCollision();

            drawDino();
            drawObstacles();

            score++;
            ctx.fillStyle = "black";
            ctx.font = "20px Arial";
            ctx.fillText("Score: " + score, 10, 20);

            requestAnimationFrame(update);
        }

        document.addEventListener("keydown", (e) => {
            if (e.code === "Space" && !dino.jumping) {
                dino.jumping = true;
                dino.velocityY = -10;
            }
        });

        update();
    </script>
</body>
</html>
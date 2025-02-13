<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juego del Pong Vertical</title>
    <style>
        body {
            text-align: center;
            font-family: Arial, sans-serif;
        }
        canvas {
            border: 2px solid black;
            display: block;
            margin: auto;
        }
        button {
            margin: 10px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>Juego del Pong Vertical</h1>
    <h2>Jugador: <span id="playerScore">0</span> - IA: <span id="aiScore">0</span></h2>
    <canvas id="gameCanvas" width="400" height="600"></canvas>
    <br>
    <button id="startButton">Iniciar Juego</button>
    <button id="resetButton" style="display: none;">Reiniciar Juego</button>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const startButton = document.getElementById("startButton");
        const resetButton = document.getElementById("resetButton");
        
        let paddleWidth = 80, paddleHeight = 10;
        let playerX, aiX, ballX, ballY, ballSpeedX, ballSpeedY, playerSpeed;
        let playerScore = 0, aiScore = 0;
        let gameInterval;
        
        function initializeGame() {
            playerX = (canvas.width - paddleWidth) / 2;
            aiX = (canvas.width - paddleWidth) / 2;
            ballX = canvas.width / 2;
            ballY = canvas.height / 2;
            ballSpeedX = 5;
            ballSpeedY = 5;
            playerSpeed = 0;
        }
        
        document.addEventListener("keydown", function(event) {
            if (event.key === "ArrowLeft") playerSpeed = -8;
            else if (event.key === "ArrowRight") playerSpeed = 8;
        });
        document.addEventListener("keyup", function() {
            playerSpeed = 0;
        });
        
        function drawGame() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            ctx.fillStyle = "black";
            ctx.fillRect(playerX, canvas.height - paddleHeight, paddleWidth, paddleHeight);
            ctx.fillRect(aiX, 0, paddleWidth, paddleHeight);
            
            ctx.beginPath();
            ctx.arc(ballX, ballY, 10, 0, Math.PI * 2);
            ctx.fill();
            
            playerX += playerSpeed;
            if (playerX < 0) playerX = 0;
            if (playerX > canvas.width - paddleWidth) playerX = canvas.width - paddleWidth;
            
            ballX += ballSpeedX;
            ballY += ballSpeedY;
            
            if (ballX <= 0 || ballX >= canvas.width) ballSpeedX *= -1;
            
            if (ballY >= canvas.height - paddleHeight && ballX > playerX && ballX < playerX + paddleWidth) {
                ballSpeedY *= -1;
                playerScore++;
                document.getElementById("playerScore").innerText = playerScore;
            }
            if (ballY <= paddleHeight && ballX > aiX && ballX < aiX + paddleWidth) {
                ballSpeedY *= -1;
                aiScore++;
                document.getElementById("aiScore").innerText = aiScore;
            }
            
            if (ballY < 0 || ballY > canvas.height) {
                clearInterval(gameInterval);
                startButton.style.display = "none";
                resetButton.style.display = "inline-block";
            }
            
            aiX = ballX - paddleWidth / 2;
        }
        
        startButton.addEventListener("click", function() {
            initializeGame();
            gameInterval = setInterval(drawGame, 30);
            startButton.style.display = "none";
            resetButton.style.display = "none";
        });
        
        resetButton.addEventListener("click", function() {
            playerScore = 0;
            aiScore = 0;
            document.getElementById("playerScore").innerText = playerScore;
            document.getElementById("aiScore").innerText = aiScore;
            initializeGame();
            gameInterval = setInterval(drawGame, 30);
            resetButton.style.display = "none";
        });
    </script>
</body>
</html>


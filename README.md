# My-project-number-1
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Classic Snake Game</title>
    <style>
        body {
            background-color: #121212;
            color: white;
            font-family: 'Arial', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            margin-bottom: 10px;
            color: #4CAF50;
        }
        #score-board {
            font-size: 24px;
            margin-bottom: 20px;
        }
        canvas {
            background-color: #1e1e1e;
            border: 4px solid #4CAF50;
            box-shadow: 0px 0px 20px rgba(76, 175, 80, 0.5);
        }
        .instructions {
            margin-top: 15px;
            color: #888;
            font-size: 14px;
        }
    </style>
</head>
<body>

    <h1>SNAKE GAME</h1>
    <div id="score-board">Score: <span id="score">0</span></div>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <div class="instructions">Use Arrow Keys (Up, Down, Left, Right) to Play!</div>

    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const scoreElement = document.getElementById("score");

        const gridSize = 20;
        const tileCount = canvas.width / gridSize;

        let snake = [{x: 10, y: 10}];
        let food = {x: 5, y: 5};
        let dx = 0;
        let dy = 0;
        let score = 0;
        let gameSpeed = 100;

        // Main game loop
        function main() {
            if (hasGameEnded()) {
                alert("Game Over! Aapka Score: " + score);
                resetGame();
            }

            setTimeout(function onTick() {
                clearCanvas();
                drawFood();
                moveSnake();
                drawSnake();
                main();
            }, gameSpeed);
        }

        // Start the game loop
        main();

        // Canvas ko clean karne ke liye
        function clearCanvas() {
            ctx.fillStyle = "#1e1e1e";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
        }

        // Saanp banane ke liye
        function drawSnake() {
            snake.forEach((part, index) => {
                ctx.fillStyle = index === 0 ? "#4CAF50" : "#81C784"; // Head dark green, body light green
                ctx.fillRect(part.x * gridSize, part.y * gridSize, gridSize - 2, gridSize - 2);
            });
        }

        // Saanp ko chalane ke liye
        function moveSnake() {
            const head = {x: snake[0].x + dx, y: snake[0].y + dy};
            snake.unshift(head);

            // Agar saanp khana kha leta hai
            if (snake[0].x === food.x && snake[0].y === food.y) {
                score += 10;
                scoreElement.innerText = score;
                generateFood();
            } else {
                snake.pop(); // Piche ka hissa hatado agar khana nahi khaya
            }
        }

        // Random jagah par khana generate karne ke liye
        function generateFood() {
            food.x = Math.floor(Math.random() * tileCount);
            food.y = Math.floor(Math.random() * tileCount);
            
            // Khana saanp ke upar na generate ho jaye
            snake.forEach(part => {
                if (part.x === food.x && part.y === food.y) {
                    generateFood();
                }
            });
        }

        // Khana (Apple) draw karne ke liye
        function drawFood() {
            ctx.fillStyle = "#FF5252"; // Red Color
            ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize - 2, gridSize - 2);
        }

        // Game over check karne ke liye
        function hasGameEnded() {
            // Agar deewar se takraye
            if (snake[0].x < 0 || snake[0].x >= tileCount || snake[0].y < 0 || snake[0].y >= tileCount) {
                return true;
            }
            // Agar khud ki body se takraye
            for (let i = 1; i < snake.length; i++) {
                if (snake[i].x === snake[0].x && snake[i].y === snake[0].y) {
                    return true;
                }
            }
            return false;
        }

        // Game ko restart karne ke liye
        function resetGame() {
            snake = [{x: 10, y: 10}];
            dx = 0;
            dy = 0;
            score = 0;
            scoreElement.innerText = score;
            generateFood();
        }

        // Controls (Arrow Keys)
        document.addEventListener("keydown", changeDirection);

        function changeDirection(event) {
            const keyPressed = event.keyCode;
            const goingUp = dy === -1;
            const goingDown = dy === 1;
            const goingRight = dx === 1;
            const goingLeft = dx === -1;

            if (keyPressed === 37 && !goingRight) { dx = -1; dy = 0; } // Left
            if (keyPressed === 38 && !goingDown) { dx = 0; dy = -1; } // Up
            if (keyPressed === 39 && !goingLeft) { dx = 1; dy = 0; }  // Right
            if (keyPressed === 40 && !goingUp) { dx = 0; dy = 1; }    // Down
        }
    </script>
</body>
</html>

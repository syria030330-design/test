<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>霓虹貪食蛇 | Neon Snake</title>
    <style>
        :root {
            --bg-color: #1a1a2e;
            --grid-color: #16213e;
            --snake-color: #0f3460;
            --snake-head: #e94560;
            --food-color: #4caf50;
            --text-color: #ffffff;
            --accent-color: #e94560;
            --panel-bg: rgba(255, 255, 255, 0.1);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            overflow: hidden;
        }

        .game-container {
            position: relative;
            padding: 20px;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
        }

        header {
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 10px;
        }

        h1 {
            font-size: 1.5rem;
            color: var(--accent-color);
            text-shadow: 0 0 10px rgba(233, 69, 96, 0.5);
        }

        .score-board {
            display: flex;
            gap: 20px;
            font-size: 1rem;
            font-weight: bold;
        }

        .score-item span {
            color: var(--accent-color);
        }

        canvas {
            background-color: var(--grid-color);
            border-radius: 8px;
            box-shadow: inset 0 0 20px rgba(0, 0, 0, 0.5);
            border: 2px solid #333;
            max-width: 100%;
            display: block;
            position: relative; /* 關鍵：使按鈕能相對定位 */
        }

        .controls-hint {
            font-size: 0.85rem;
            color: #888;
            margin-top: 5px;
            text-align: center;
        }

        /* 移動端專屬提示 */
        .mobile-hint {
            display: none;
            font-size: 0.85rem;
            color: #888;
            margin-top: 5px;
            text-align: center;
        }

        /* 修正：按鈕覆蓋在畫布右側 */
        .mobile-controls {
            display: none;
            position: absolute;
            top: 50%;
            right: 10px;
            transform: translateY(-50%);
            grid-template-columns: repeat(3, 1fr);
            grid-template-rows: repeat(3, 1fr);
            gap: 10px;
            z-index: 10;
            background: rgba(0, 0, 0, 0.3);
            padding: 15px;
            border-radius: 12px;
            backdrop-filter: blur(4px);
        }

        .d-btn {
            width: 50px;
            height: 50px;
            background: var(--panel-bg);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.4rem;
            color: white;
            cursor: pointer;
            transition: all 0.15s ease;
            touch-action: manipulation;
        }

        .d-btn:active {
            background: var(--accent-color);
            transform: scale(0.92);
            box-shadow: 0 0 12px var(--accent-color);
        }

        /* 修正：按鈕佈局為十字鍵 */
        .d-btn.up { grid-column: 2; grid-row: 1; }
        .d-btn.left { grid-column: 1; grid-row: 2; }
        .d-btn.down { grid-column: 2; grid-row: 3; }
        .d-btn.right { grid-column: 3; grid-row: 2; }

        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(26, 26, 46, 0.85);
            backdrop-filter: blur(4px);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 10;
            border-radius: 15px;
            transition: opacity 0.3s;
        }

        .overlay.hidden {
            opacity: 0;
            pointer-events: none;
        }

        .overlay h2 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            color: var(--accent-color);
        }

        .overlay p {
            margin-bottom: 25px;
            font-size: 1.1rem;
            color: #ccc;
        }

        .btn {
            padding: 12px 30px;
            font-size: 1.2rem;
            background: linear-gradient(135deg, #e94560, #c0394d);
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(233, 69, 96, 0.4);
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(233, 69, 96, 0.6);
        }

        .btn:active {
            transform: translateY(1px);
        }

        .pause-btn {
            background: transparent;
            border: 1px solid rgba(255,255,255,0.3);
            color: white;
            width: 30px;
            height: 30px;
            border-radius: 4px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        @media (max-width: 768px) {
            .mobile-controls {
                display: grid;
            }
            .controls-hint {
                display: none;
            }
            .mobile-hint {
                display: block;
            }
            h1 {
                font-size: 1.2rem;
            }
            .game-container {
                padding: 15px;
                width: 95%;
            }
            .d-btn {
                width: 45px;
                height: 45px;
                font-size: 1.2rem;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <header>
            <h1>霓虹貪食蛇</h1>
            <div style="display: flex; align-items: center; gap: 15px;">
                <div class="score-board">
                    <div class="score-item">分數: <span id="score">0</span></div>
                    <div class="score-item">最高: <span id="highScore">0</span></div>
                </div>
                <button id="pauseBtn" class="pause-btn" title="暫停/繼續">||</button>
            </div>
        </header>

        <canvas id="gameCanvas" width="400" height="400"></canvas>

        <!-- 修正：按鈕覆蓋在畫布右側 -->
        <div class="mobile-controls">
            <div class="d-btn up" data-dir="ArrowUp">▲</div>
            <div class="d-btn left" data-dir="ArrowLeft">◀</div>
            <div class="d-btn down" data-dir="ArrowDown">▼</div>
            <div class="d-btn right" data-dir="ArrowRight">▶</div>
        </div>

        <!-- 桌面端提示（WASD優先表述） -->
        <p class="controls-hint">使用 WASD 鍵或方向鍵控制移動，空白鍵暫停</p>
        <!-- 移動端專屬提示 -->
        <p class="mobile-hint">點擊右側按鈕控制貪食蛇</p>

        <!-- 遊戲狀態覆蓋層 -->
        <div id="overlay" class="overlay">
            <h2 id="overlayTitle">準備開始</h2>
            <p id="overlayMessage">盡可能吃掉更多的綠色果實</p>
            <button id="startBtn" class="btn">開始遊戲</button>
        </div>
    </div>

<script>
    /**
     * 遊戲配置與變數（完全保留原有邏輯）
     */
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const scoreEl = document.getElementById('score');
    const highScoreEl = document.getElementById('highScore');
    const overlay = document.getElementById('overlay');
    const overlayTitle = document.getElementById('overlayTitle');
    const overlayMessage = document.getElementById('overlayMessage');
    const startBtn = document.getElementById('startBtn');
    const pauseBtn = document.getElementById('pauseBtn');

    const GRID_SIZE = 20;
    const TILE_COUNT = canvas.width / GRID_SIZE;

    let score = 0;
    let highScore = localStorage.getItem('snakeHighScore') || 0;
    let snake = [];
    let food = { x: 0, y: 0 };
    let velocity = { x: 0, y: 0 };
    let nextVelocity = { x: 0, y: 0 };
    let gameLoopId;
    let isGameRunning = false;
    let isPaused = false;
    let gameSpeed = 150;
    let particles = [];

    highScoreEl.textContent = highScore;

    function initGame() {
        snake = [
            { x: 10, y: 10 },
            { x: 10, y: 11 },
            { x: 10, y: 12 }
        ];
        score = 0;
        scoreEl.textContent = score;
        velocity = { x: 0, y: -1 };
        nextVelocity = { x: 0, y: -1 };
        gameSpeed = 150;
        particles = [];
        placeFood();
        isGameRunning = true;
        isPaused = false;
        pauseBtn.textContent = "||";
        overlay.classList.add('hidden');
        if (gameLoopId) clearTimeout(gameLoopId);
        gameLoop();
    }

    function gameLoop() {
        if (!isGameRunning || isPaused) return;
        update();
        draw();
        gameLoopId = setTimeout(gameLoop, gameSpeed);
    }

    function update() {
        velocity = { ...nextVelocity };
        const head = { x: snake[0].x + velocity.x, y: snake[0].y + velocity.y };

        if (head.x < 0 || head.x >= TILE_COUNT || head.y < 0 || head.y >= TILE_COUNT || isSnakeCollision(head)) {
            gameOver();
            return;
        }

        snake.unshift(head);

        if (head.x === food.x && head.y === food.y) {
            score += 10;
            scoreEl.textContent = score;
            createParticles(head.x * GRID_SIZE + GRID_SIZE/2, head.y * GRID_SIZE + GRID_SIZE/2, '#4caf50');
            placeFood();
            increaseSpeed();
        } else {
            snake.pop();
        }

        updateParticles();
    }

    function draw() {
        ctx.fillStyle = '#16213e';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        ctx.shadowBlur = 15;
        ctx.shadowColor = "#4caf50";
        ctx.fillStyle = '#4caf50';
        ctx.beginPath();
        ctx.arc(
            food.x * GRID_SIZE + GRID_SIZE/2, 
            food.y * GRID_SIZE + GRID_SIZE/2, 
            GRID_SIZE/2 - 2, 0, Math.PI * 2
        );
        ctx.fill();
        ctx.shadowBlur = 0;

        snake.forEach((segment, index) => {
            if (index === 0) {
                ctx.fillStyle = '#e94560';
                ctx.shadowBlur = 10;
                ctx.shadowColor = "#e94560";
            } else {
                ctx.fillStyle = `hsl(214, 70%, ${50 - (index * 2)}%)`;
                ctx.shadowBlur = 0;
            }
            ctx.fillRect(
                segment.x * GRID_SIZE + 1, 
                segment.y * GRID_SIZE + 1, 
                GRID_SIZE - 2, 
                GRID_SIZE - 2
            );
        });
        ctx.shadowBlur = 0;
        drawParticles();
    }

    function placeFood() {
        let valid = false;
        while (!valid) {
            food.x = Math.floor(Math.random() * TILE_COUNT);
            food.y = Math.floor(Math.random() * TILE_COUNT);
            valid = !snake.some(segment => segment.x === food.x && segment.y === food.y);
        }
    }

    function isSnakeCollision(pos) {
        return snake.some(segment => segment.x === pos.x && segment.y === pos.y);
    }

    function gameOver() {
        isGameRunning = false;
        if (score > highScore) {
            highScore = score;
            localStorage.setItem('snakeHighScore', highScore);
            highScoreEl.textContent = highScore;
        }
        overlayTitle.textContent = "遊戲結束";
        overlayMessage.textContent = `你的分數: ${score}`;
        startBtn.textContent = "再玩一次";
        overlay.classList.remove('hidden');
    }

    function increaseSpeed() {
        if (gameSpeed > 80) {
            gameSpeed -= 2;
        }
    }

    function togglePause() {
        if (!isGameRunning) return;
        isPaused = !isPaused;
        if (isPaused) {
            overlayTitle.textContent = "已暫停";
            overlayMessage.textContent = "休息一下吧";
            startBtn.textContent = "繼續遊戲";
            overlay.classList.remove('hidden');
            pauseBtn.textContent = "▶";
        } else {
            overlay.classList.add('hidden');
            pauseBtn.textContent = "||";
            gameLoop();
        }
    }

    function createParticles(x, y, color) {
        for (let i = 0; i < 8; i++) {
            particles.push({
                x: x,
                y: y,
                vx: (Math.random() - 0.5) * 4,
                vy: (Math.random() - 0.5) * 4,
                life: 1.0,
                color: color
            });
        }
    }

    function updateParticles() {
        for (let i = particles.length - 1; i >= 0; i--) {
            let p = particles[i];
            p.x += p.vx;
            p.y += p.vy;
            p.life -= 0.1;
            if (p.life <= 0) {
                particles.splice(i, 1);
            }
        }
    }

    function drawParticles() {
        particles.forEach(p => {
            ctx.globalAlpha = p.life;
            ctx.fillStyle = p.color;
            ctx.beginPath();
            ctx.arc(p.x, p.y, 3, 0, Math.PI * 2);
            ctx.fill();
        });
        ctx.globalAlpha = 1.0;
    }

    function handleInput(key) {
        if (!isGameRunning || isPaused) return;
        switch(key) {
            case 'ArrowUp':
            case 'w':
            case 'W':
                if (velocity.y !== 1) nextVelocity = { x: 0, y: -1 };
                break;
            case 'ArrowDown':
            case 's':
            case 'S':
                if (velocity.y !== -1) nextVelocity = { x: 0, y: 1 };
                break;
            case 'ArrowLeft':
            case 'a':
            case 'A':
                if (velocity.x !== 1) nextVelocity = { x: -1, y: 0 };
                break;
            case 'ArrowRight':
            case 'd':
            case 'D':
                if (velocity.x !== -1) nextVelocity = { x: 1, y: 0 };
                break;
        }
    }

    document.addEventListener('keydown', (e) => {
        if (e.code === 'Space') {
            if (isGameRunning) {
                if (overlay.classList.contains('hidden')) {
                    togglePause();
                } else {
                    togglePause();
                }
            } else {
                initGame();
            }
            e.preventDefault();
        } else {
            handleInput(e.key);
            if(['ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight', ' '].includes(e.key)) {
                e.preventDefault();
            }
        }
    });

    startBtn.addEventListener('click', () => {
        if (isPaused) {
            togglePause();
        } else {
            initGame();
        }
    });

    pauseBtn.addEventListener('click', togglePause);

    document.querySelectorAll('.d-btn').forEach(btn => {
        btn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            handleInput(btn.dataset.dir);
        });
        btn.addEventListener('mousedown', (e) => {
            handleInput(btn.dataset.dir);
        });
    });

    // 初始畫面
    ctx.fillStyle = '#16213e';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    ctx.font = "20px Arial";
    ctx.fillStyle = "#333";
    ctx.textAlign = "center";
    ctx.fillText("準備好了嗎？", canvas.width/2, canvas.height/2);
</script>
</body>
</html>
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mini Minecraft</title>
<link rel="stylesheet" href="style.css">
</head>
<body>
<h1>Mini Minecraft 2D</h1>
<div id="game">
    <canvas id="gameCanvas" width="400" height="400"></canvas>
</div>
<div id="controls">
    <button onclick="currentBlock='grass'">🌱 Herbe</button>
    <button onclick="currentBlock='dirt'">🟫 Terre</button>
    <button onclick="currentBlock='stone'">🪨 Pierre</button>
</div>
<script src="script.js"></script>
</body>
</html>

body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #87CEEB; /* ciel bleu */
    margin: 0;
    padding: 20px;
}

h1 {
    color: #333;
}

#game {
    display: inline-block;
    margin: 20px 0;
    border: 4px solid #333;
    background-color: #a0d8f1;
}

canvas {
    display: block;
}

#controls {
    margin-top: 10px;
}

button {
    font-size: 18px;
    padding: 10px 20px;
    margin: 5px;
    cursor: pointer;
}


const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

const tileSize = 20;
const rows = canvas.height / tileSize;
const cols = canvas.width / tileSize;

let currentBlock = 'grass';

// Grille du monde (2D array)
let world = [];
for (let r = 0; r < rows; r++) {
    world[r] = [];
    for (let c = 0; c < cols; c++) {
        if (r > 15) world[r][c] = 'dirt';
        else if (r === 15) world[r][c] = 'grass';
        else world[r][c] = null;
    }
}

// Couleurs pour les blocs
const colors = {
    grass: '#4CAF50',
    dirt: '#8B4513',
    stone: '#808080'
};

// Dessine le monde
function drawWorld() {
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            let block = world[r][c];
            if (block) {
                ctx.fillStyle = colors[block];
                ctx.fillRect(c * tileSize, r * tileSize, tileSize, tileSize);
                ctx.strokeStyle = '#555';
                ctx.strokeRect(c * tileSize, r * tileSize, tileSize, tileSize);
            } else {
                ctx.clearRect(c * tileSize, r * tileSize, tileSize, tileSize);
            }
        }
    }
}

drawWorld();

// Placer ou casser un bloc avec la souris
canvas.addEventListener('click', (e) => {
    const rect = canvas.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;

    const col = Math.floor(x / tileSize);
    const row = Math.floor(y / tileSize);

    if (world[row][col]) {
        // Casser le bloc
        world[row][col] = null;
    } else {
        // Placer le bloc
        world[row][col] = currentBlock;
    }

    drawWorld();
});

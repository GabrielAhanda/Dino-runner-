# Dino-runner-
The famous Dino runner game by chrome coding by using html,canvas,and javascript


Créer un jeu Dino Runner en utilisant HTML, CSS et JavaScript implique plusieurs étapes. Voici un exemple complet et bien organisé pour vous aider à démarrer :

### 1. HTML

Créez un fichier `index.html` pour la structure de base de votre jeu.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dino Runner</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div id="game-container">
        <canvas id="gameCanvas" width="800" height="200"></canvas>
        <div id="score">Score: 0</div>
    </div>
    <script src="game.js"></script>
</body>
</html>
```

### 2. CSS

Créez un fichier `styles.css` pour styliser le jeu.

```css
body {
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f4f4f4;
    font-family: Arial, sans-serif;
}

#game-container {
    position: relative;
    text-align: center;
}

#score {
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 24px;
}
```

### 3. JavaScript

Créez un fichier `game.js` pour la logique du jeu.

```javascript
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let dino = {
    x: 50,
    y: 150,
    width: 50,
    height: 50,
    dy: 0,
    gravity: 1.5,
    jumpPower: -15,
    isJumping: false,
};

let obstacles = [];
let frame = 0;
let score = 0;
const obstacleFrequency = 100; // Lower value means more frequent obstacles

function drawDino() {
    ctx.fillStyle = 'green';
    ctx.fillRect(dino.x, dino.y, dino.width, dino.height);
}

function drawObstacles() {
    ctx.fillStyle = 'red';
    obstacles.forEach(obstacle => {
        ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
    });
}

function updateObstacles() {
    if (frame % obstacleFrequency === 0) {
        let obstacle = {
            x: canvas.width,
            y: canvas.height - 50,
            width: 20,
            height: 50,
        };
        obstacles.push(obstacle);
    }
    obstacles = obstacles.filter(obstacle => obstacle.x + obstacle.width > 0);
    obstacles.forEach(obstacle => {
        obstacle.x -= 5;
    });
}

function checkCollision() {
    obstacles.forEach(obstacle => {
        if (
            dino.x < obstacle.x + obstacle.width &&
            dino.x + dino.width > obstacle.x &&
            dino.y < obstacle.y + obstacle.height &&
            dino.y + dino.height > obstacle.y
        ) {
            alert('Game Over! Your score: ' + score);
            document.location.reload();
        }
    });
}

function updateScore() {
    score++;
    document.getElementById('score').innerText = 'Score: ' + score;
}

function updateDino() {
    if (dino.isJumping) {
        dino.dy += dino.gravity;
        dino.y += dino.dy;
        if (dino.y > canvas.height - dino.height) {
            dino.y = canvas.height - dino.height;
            dino.isJumping = false;
            dino.dy = 0;
        }
    }
}

function jump() {
    if (!dino.isJumping) {
        dino.isJumping = true;
        dino.dy = dino.jumpPower;
    }
}

document.addEventListener('keydown', (e) => {
    if (e.key === ' ') {
        jump();
    }
});

function gameLoop() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawDino();
    drawObstacles();
    updateDino();
    updateObstacles();
    checkCollision();
    updateScore();
    frame++;
    requestAnimationFrame(gameLoop);
}

gameLoop();
```

### Explications:

1. **HTML**: 
   - Structure de base avec un canevas pour le jeu et un élément pour afficher le score.

2. **CSS**: 
   - Style simple pour centrer le jeu sur la page et afficher le score.

3. **JavaScript**:
   - Gestion du dino avec la gravité et le saut.
   - Génération et mise à jour des obstacles.
   - Détection des collisions entre le dino et les obstacles.
   - Incrémentation et affichage du score.
   - Boucle principale du jeu utilisant `requestAnimationFrame` pour des animations fluides.

Avec ces fichiers, vous aurez un jeu de type Dino Runner fonctionnel. N'hésitez pas à enrichir et à personnaliser le jeu selon vos besoins !

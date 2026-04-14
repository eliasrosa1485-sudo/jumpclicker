<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title>Jump Clicker</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body {
    margin: 0;
    overflow: hidden;
    font-family: Arial;
}

#ui {
    position: absolute;
    width: 100%;
    top: 10px;
    display: flex;
    justify-content: space-between;
    padding: 0 20px;
    color: white;
}
</style>
</head>

<body>

<div id="ui">
    <div id="coins">💰 0</div>
    <div id="meters">0 m</div>
</div>

<canvas id="game"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let y = canvas.height - 150;
let velocity = 0;
let gravity = 0.6;

let cameraY = 0;
let meters = 0;
let coins = 0;

// stickman
function drawStickman(x, y) {
    ctx.lineWidth = 4;
    ctx.strokeStyle = "black";

    ctx.beginPath();
    ctx.arc(x, y - 30, 15, 0, Math.PI*2);
    ctx.stroke();

    ctx.beginPath();
    ctx.moveTo(x, y - 15);
    ctx.lineTo(x, y + 20);
    ctx.stroke();

    ctx.beginPath();
    ctx.moveTo(x - 20, y);
    ctx.lineTo(x + 20, y);
    ctx.stroke();

    ctx.beginPath();
    ctx.moveTo(x, y + 20);
    ctx.lineTo(x - 15, y + 40);
    ctx.moveTo(x, y + 20);
    ctx.lineTo(x + 15, y + 40);
    ctx.stroke();
}

// controle
window.addEventListener("mousedown", ()=>{
    velocity = -15;
});

window.addEventListener("touchstart", ()=>{
    velocity = -15;
});

// loop
function update() {

    velocity += gravity;
    y += velocity;

    // trampolim
    if (y > canvas.height - 150) {
        y = canvas.height - 150;
        velocity = -15;
    }

    meters += Math.abs(velocity * 5);

    draw();
    requestAnimationFrame(update);
}

function draw() {
    ctx.clearRect(0,0,canvas.width,canvas.height);

    // câmera suave
    cameraY += (y - canvas.height/2 - cameraY) * 0.08;
    ctx.setTransform(1,0,0,1,0,-cameraY);

    // fundo gradiente
    let gradient = ctx.createLinearGradient(0, cameraY, 0, cameraY + canvas.height);
    gradient.addColorStop(0, "#000");
    gradient.addColorStop(1, "#87CEEB");
    ctx.fillStyle = gradient;
    ctx.fillRect(0, cameraY, canvas.width, canvas.height);

    // sol
    if (meters > 50000) {
        ctx.fillStyle = "orange";
        ctx.beginPath();
        ctx.arc(canvas.width/2, cameraY + 100, 100, 0, Math.PI*2);
        ctx.fill();
    }

    // trampolim
    ctx.fillStyle = "blue";
    ctx.fillRect(canvas.width/2 - 100, canvas.height - 100 + cameraY, 200, 20);

    drawStickman(canvas.width/2, y);

    ctx.setTransform(1,0,0,1,0,0);

    document.getElementById("meters").innerText = Math.floor(meters) + " m";
    document.getElementById("coins").innerText = "💰 " + coins;
}

update();
</script>

</body>
</html>

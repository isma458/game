<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Juego Avanzado Offline</title>
<style>
body {
  margin: 0;
  overflow: hidden;
  font-family: Arial, sans-serif;
  background-color: black;
}
canvas {
  display: block;
  margin: 0 auto;
  background-color: black;
}
</style>
</head>
<body>
<canvas id="gameCanvas" width="800" height="400"></canvas>
<script>
// --- Configuración ---
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let gameState = 'menu'; // menu, jugando
let personaje = localStorage.getItem('personaje') || 'astro';
let nivel = parseInt(localStorage.getItem('nivel')) || 1;
let puntaje = parseInt(localStorage.getItem('puntaje')) || 0;
let velocidad = 4;

// Jugador
let player = { x: 50, y: 300, width: 50, height: 50, dy: 0, saltando: false };

// Obstáculos
let obstaculos = [];

// Misiones diarias (ejemplo)
let misiones = [`Nivel ${nivel}: Evita 5 obstáculos`];
let misionesCompletadas = 0;

// --- Funciones ---
function crearObstaculo() {
  obstaculos.push({ x: canvas.width, y: 350, width: 30 + Math.random()*20, height: 50 });
}

// --- Eventos ---
document.addEventListener('keydown', e => {
  if(gameState === 'menu') gameState = 'jugando';
  if(gameState === 'jugando' && e.code === 'Space' && !player.saltando){
    player.dy = -15;
    player.saltando = true;
  }
});

// --- Loop principal ---
function gameLoop() {
  // Fondo dinámico (estrellas)
  ctx.fillStyle = 'black';
  ctx.fillRect(0,0,canvas.width,canvas.height);
  for (let i = 0; i < 50; i++) {
    ctx.fillStyle = 'white';
    ctx.fillRect(Math.random()*canvas.width, Math.random()*canvas.height, 2, 2);
  }

  if(gameState === 'menu'){
    ctx.fillStyle = 'white';
    ctx.font = '30px Arial';
    ctx.fillText('Juego Avanzado Offline 🚀', 150, 100);
    ctx.font = '20px Arial';
    ctx.fillText('Presiona cualquier tecla para jugar', 200, 200);
    ctx.fillText(`Personaje actual: ${personaje}`, 280, 250);
  } else if(gameState === 'jugando'){
    // Jugador
    ctx.fillStyle = personaje==='astro'?'yellow':'cyan';
    ctx.fillRect(player.x, player.y, player.width, player.height);

    // Física
    player.dy += 1;
    player.y += player.dy;
    if(player.y >= 300){
      player.y = 300;
      player.dy = 0;
      player.saltando = false;
    }

    // Obstáculos
    ctx.fillStyle = 'red';
    obstaculos.forEach((obs, index) => {
      obs.x -= velocidad;
      ctx.fillRect(obs.x, obs.y, obs.width, obs.height);

      // Colisión
      if(player.x < obs.x + obs.width &&
         player.x + player.width > obs.x &&
         player.y < obs.y + obs.height &&
         player.y + player.height > obs.y){
           alert(`¡Game Over! Puntaje: ${puntaje}`);
           obstaculos = [];
           player.y = 300;
           velocidad = 4;
           puntaje = 0;
           misionesCompletadas = 0;
           gameState='menu';
      }

      // Si pasa
      if(obs.x + obs.width < 0){
        obstaculos.splice(index,1);
        puntaje++;
        misionesCompletadas++;
        velocidad += 0.3; // aumenta dificultad
        localStorage.setItem('puntaje', puntaje);
        localStorage.setItem('nivel', nivel);
      }
    });

    // Dibujar info
    ctx.fillStyle='white';
    ctx.font='20px Arial';
    ctx.fillText(`Nivel: ${nivel} Puntaje: ${puntaje} Velocidad: ${velocidad.toFixed(1)}`, 10,30);
    ctx.fillText(`Misión diaria: ${misiones[0]} Completadas: ${misionesCompletadas}`, 10,60);
  }

  requestAnimationFrame(gameLoop);
}

// --- Obstáculos periódicos ---
setInterval(() => { if(gameState==='jugando') crearObstaculo(); }, 2000);

gameLoop();
</script>
</body>
</html>

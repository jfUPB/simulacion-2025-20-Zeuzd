# Unidad 1

## 🛠 Fase: Apply

### Actividad 8

Esta obra generativa e interactiva mezcla la visualizacion de las emociones con la interaccion para crear una obra con la cual todos se puede representar ya que muestra como funcionan las emociones.

Como funciona?
Dependiendo de la tecla que presiones se va a activar una emocion.

1-Tranquilidad

2-Estres

3-Tranquilidad con episodios de ira cortos


``` js

let cols, rows;
let cellSize = 4; // MÁS resolución: más celdas, menos pixelado
let estado = 'perlin';

let z = 0;
let grid = [];
let ataque = false;
let duracionAtaque = 15;
let contadorAtaque = 0;
let tiempoSiguienteAtaque = 0;

function setup() {
  createCanvas(600, 600);
  cols = floor(width / cellSize);
  rows = floor(height / cellSize);
  colorMode(RGB, 255);
  initGrid();
  tiempoSiguienteAtaque = frameCount + getTiempoRandom();
  noStroke();
}

function initGrid() {
  for (let x = 0; x < cols; x++) {
    grid[x] = [];
    for (let y = 0; y < rows; y++) {
      grid[x][y] = {
        actual: color(127),
        destino: color(127)
      };
    }
  }
}

function draw() {
  background(0);

  if (estado === 'perlin') {
    actualizarPerlin();
  } else if (estado === 'gauss') {
    actualizarGauss();
  } else if (estado === 'levy') {
    actualizarLevy();
  }

  dibujarCeldas();

  z += 0.01;
}

function actualizarPerlin() {
  for (let x = 0; x < cols; x++) {
    for (let y = 0; y < rows; y++) {
      let n = noise(x * 0.1, y * 0.1, z);
      let c = color(n * 255, n * 200 + 55, n * 180 + 75);
      grid[x][y].destino = c;
    }
  }
}

function actualizarGauss() {
  for (let x = 0; x < cols; x++) {
    for (let y = 0; y < rows; y++) {
      let r = constrain(randomGaussian(127, 30), 0, 255);
      let g = constrain(randomGaussian(127, 30), 0, 255);
      let b = constrain(randomGaussian(127, 30), 0, 255);
      grid[x][y].destino = color(r, g, b);
    }
  }
}

function actualizarLevy() {
  if (ataque) {
    for (let x = 0; x < cols; x++) {
      for (let y = 0; y < rows; y++) {
        let c = color(random(255), random(100), random(100));
        grid[x][y].destino = c;
      }
    }
    contadorAtaque++;
    if (contadorAtaque > duracionAtaque) {
      ataque = false;
      tiempoSiguienteAtaque = frameCount + getTiempoRandom();
      contadorAtaque = 0;
    }
  } else {
    actualizarPerlin(); // calma por defecto
    if (frameCount >= tiempoSiguienteAtaque) {
      ataque = true;
    }
  }
}

function dibujarCeldas() {
  for (let x = 0; x < cols; x++) {
    for (let y = 0; y < rows; y++) {
      let celda = grid[x][y];
      celda.actual = lerpColor(celda.actual, celda.destino, 0.1);
      fill(celda.actual);
      rect(x * cellSize, y * cellSize, cellSize, cellSize);
    }
  }
}

function getTiempoRandom() {
  return int(random(100, 300)); // entre 2 y 5 segundos
}

function keyPressed() {
  if (key === '1') estado = 'perlin';
  if (key === '2') estado = 'gauss';
  if (key === '3') estado = 'levy';
}

```

Enlace al link en p5.js: https://editor.p5js.org/Zeuzd/sketches/kGu_zB_Qw

<img width="747" height="516" alt="image" src="https://github.com/user-attachments/assets/7ba87775-43d1-4471-aa73-3293312b8b01" />


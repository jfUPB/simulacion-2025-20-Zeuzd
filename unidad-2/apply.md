# Unidad 2


## 🛠 Fase: Apply

### Actividad 8

Mi obra generativa va a ser una orilla de la playa vista desde arriba cuando llegan olas que se desvanecen entre mas salen del mar.

Espero aplicar el motion 101 con la posición vertical de cada ola actualizandola y sumándole su velocidad en cada frame.

Tambien espero utilizar un algoritmo senoidal para darle la forma a las olas y que vayan variando con el tiempo.

Cada vez que se presione la tecla "o" va a salir una ola diferente.

``` js
let olas = [];

function setup() {
  createCanvas(800, 600);
  noFill();
}

function draw() {
  background(10, 20, 30, 50);

  for (let i = olas.length - 1; i >= 0; i--) {
    let ola = olas[i];
    ola.y += ola.speed; // Motion 101

    stroke(100, 200, 255, map(ola.y, 0, height, 255, 0));
    strokeWeight(2);
    beginShape();
    for (let x = 0; x <= width; x += 10) {
      let angle = (x / ola.waveSize) * TWO_PI + frameCount * 0.02 + ola.offset;
      let ySeno = sin(angle) * ola.amp;
      vertex(x, ola.y + ySeno);
    }
    endShape();

    if (ola.y > height) {
      olas.splice(i, 1); // Eliminar ola al salir de pantalla
    }
  }
}

function keyPressed() {
  if (key === 'o' || key === 'O') {
    olas.push({
      y: 0,
      speed: random(1, 2),
      amp: random(10, 30),
      waveSize: random(100, 200),
      offset: random(1000)
    });
  }
}
```

Enlace al codigo en p5.js: https://editor.p5js.org/Zeuzd/sketches/vu3RbTPi1

<img width="911" height="633" alt="image" src="https://github.com/user-attachments/assets/babff805-c2f7-42d2-8280-15c4a015ad9c" />


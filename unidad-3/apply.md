# Unidad 3


## 🛠 Fase: Apply

En esta obra quise representar el nacimiento de una "constelacion", como una nebulosa que cada parte brilla con vida y tamaño propios.

En esta obra utilice la distribucion normal, y el ruido perlin.

Distribución normal: las estrellas iniciales nacen cerca del centro con randomGaussian(), simulando una nebulosa concentrada.

Ruido Perlin: con noise() se controla color y transparencia, haciendo que las estrellas parpadeen suavemente.

La interaccion es con el mouse y cuando se oprime con el click izquierdo del mouse se agrega una nueva particula a el sistema y al mover el mouse de derecha a izquierda se afecta la gravedad.

Problema de los n-cuerpos: cada estrella atrae a las demás con la ley de Newton, generando órbitas y constelaciones efímeras.

Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/FF3lyfGa6

```js
let bodies = [];
let G = 1; 
let noiseOffset = 0;

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 255, 255, 255);
  
  // "El caos primordial": estrellas iniciales
  for (let i = 0; i < 25; i++) {
    let x = randomGaussian(width / 2, width / 6);
    let y = randomGaussian(height / 2, height / 6);
    bodies.push(new Body(x, y, random(3, 10)));
  }
}

function draw() {
  background(10, 10, 20, 40); 
  
  // Narrativa visual: "La danza cósmica"
  for (let i = 0; i < bodies.length; i++) {
    for (let j = i + 1; j < bodies.length; j++) {
      let force = bodies[j].attract(bodies[i]);
      bodies[i].applyForce(force);
      bodies[j].applyForce(force.mult(-1));
      
      // "El destino estelar": conexiones si están cerca
      let d = dist(bodies[i].pos.x, bodies[i].pos.y, bodies[j].pos.x, bodies[j].pos.y);
      if (d < 150) {
        stroke(200, 180, 255, map(d, 0, 150, 255, 0));
        line(bodies[i].pos.x, bodies[i].pos.y, bodies[j].pos.x, bodies[j].pos.y);
      }
    }
  }

  // Actualizar y mostrar estrellas
  for (let b of bodies) {
    b.update();
    b.show();
  }

  noiseOffset += 0.01;
}

// Crear nueva estrella con el mouse
function mousePressed() {
  let m = dist(mouseX, mouseY, pmouseX, pmouseY) * 0.5 + 5;
  bodies.push(new Body(mouseX, mouseY, m));
}

// Controlar gravedad con el movimiento del mouse
function mouseMoved() {
  G = map(mouseX, 0, width, 0.1, 2);
}

class Body {
  constructor(x, y, m) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(0.5, 2));
    this.acc = createVector(0, 0);
    this.mass = m;
    this.r = sqrt(this.mass) * 3;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  attract(other) {
    let force = p5.Vector.sub(this.pos, other.pos);
    let d = constrain(force.mag(), 5, 25);
    let strength = (G * this.mass * other.mass) / (d * d);
    force.setMag(strength);
    return force;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  show() {
    let n = noise(this.pos.x * 0.01, this.pos.y * 0.01, noiseOffset);
    let hue = map(n, 0, 1, 180, 320);
    let alpha = map(n, 0, 1, 150, 255);

    noStroke();
    fill(hue, 200, 255, alpha);
    ellipse(this.pos.x, this.pos.y, this.r);
  }
}
```

<img width="715" height="482" alt="image" src="https://github.com/user-attachments/assets/33da6b9b-8cb6-4385-98a6-28f0a6dd7748" />

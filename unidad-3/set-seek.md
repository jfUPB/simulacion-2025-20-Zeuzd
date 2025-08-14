# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 9

### Friccion
```js
let pos;         // Posición del centro de la línea
let vel;         // Velocidad
let angle = 0;   // Ángulo de la línea
let speed = 0;   // Velocidad en la dirección del ángulo
let friction = 0.95; // Factor de fricción (0 = se detiene rápido, 1 = no se frena)
let lineLength = 80; // Largo de la línea

function setup() {
  createCanvas(800, 600);
  background(255);
  pos = createVector(width / 2, height / 2);
  vel = createVector(0, 0);
}

function draw() {
  // Control de rotación
  if (keyIsDown(LEFT_ARROW)) {
    angle -= 0.05;
  }
  if (keyIsDown(RIGHT_ARROW)) {
    angle += 0.05;
  }

  // Acelerar hacia adelante o atrás
  if (keyIsDown(UP_ARROW)) {
    let force = p5.Vector.fromAngle(angle).mult(0.2);
    vel.add(force);
  }
  if (keyIsDown(DOWN_ARROW)) {
    let force = p5.Vector.fromAngle(angle).mult(-0.2);
    vel.add(force);
  }

  // Aplicar fricción (reduce la velocidad gradualmente)
  vel.mult(friction);

  // Actualizar posición
  pos.add(vel);

  // Dibujar línea en su posición y ángulo
  stroke(0);
  strokeWeight(4);
  let p1 = p5.Vector.fromAngle(angle).mult(lineLength / 2).add(pos);
  let p2 = p5.Vector.fromAngle(angle + PI).mult(lineLength / 2).add(pos);
  line(p1.x, p1.y, p2.x, p2.y);

  // Mantener la posición dentro del lienzo (opcional)
  if (pos.x < 0) pos.x = width;
  if (pos.x > width) pos.x = 0;
  if (pos.y < 0) pos.y = height;
  if (pos.y > height) pos.y = 0;
}
```

Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/AoqPwpq-Y

#### Resistencia del aire y de fluidos

```js
let movers = [];
let liquid;

function setup() {
  createCanvas(600, 400);

  // Crear pelotas
  for (let i = 0; i < 8; i++) {
    movers.push(new Mover(random(0.5, 3), random(width), random(0, 50)));
  }

  // Zona de fluido (agua)
  liquid = new Liquid(0, height / 2, width, height / 2, 0.2); // c = 0.2
}

function draw() {
  background(255);

  // Dibujar líquido
  liquid.display();

  for (let m of movers) {
    // Gravedad
    let gravity = createVector(0, 0.1 * m.mass);
    m.applyForce(gravity);

    // Resistencia del aire (pequeña, siempre presente)
    let air = m.velocity.copy();
    air.mult(-1);
    let c_air = 0.01; // coeficiente pequeño
    air.normalize();
    air.mult(c_air * m.velocity.magSq());
    m.applyForce(air);

    // Resistencia del fluido (solo si está dentro)
    if (liquid.contains(m)) {
      let drag = liquid.drag(m);
      m.applyForce(drag);
    }

    m.update();
    m.display();
    m.checkEdges();
  }
}

class Mover {
  constructor(m, x, y) {
    this.mass = m;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  display() {
    stroke(0);
    fill(175, 200);
    ellipse(this.position.x, this.position.y, this.mass * 16, this.mass * 16);
  }

  checkEdges() {
    if (this.position.y > height - this.mass * 8) {
      this.position.y = height - this.mass * 8;
      this.velocity.y *= -0.5; // Rebote con pérdida
    }
  }
}

class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c; // coeficiente de arrastre del fluido
  }

  contains(m) {
    let l = m.position;
    return (l.x > this.x && l.x < this.x + this.w &&
            l.y > this.y && l.y < this.y + this.h);
  }

  drag(m) {
    let speed = m.velocity.mag();
    let dragMagnitude = this.c * speed * speed;

    let drag = m.velocity.copy();
    drag.mult(-1);
    drag.normalize();
    drag.mult(dragMagnitude);
    return drag;
  }

  display() {
    noStroke();
    fill(0, 0, 255, 100);
    rect(this.x, this.y, this.w, this.h);
  }
}
```
Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/b9AjGHcHs

#### Fuerza gravitacional

```js
let posY;          // Posición vertical del personaje
let velY;          // Velocidad vertical
let gravity = 0.2; // Gravedad lunar (más baja que la terrestre ~1)
let onGround = true;
let groundY;

function setup() {
  createCanvas(600, 400);
  groundY = height - 50; // Suelo
  posY = groundY;
  velY = 0;
}

function draw() {
  background(30, 30, 50);

  // Dibujar suelo
  fill(80, 80, 100);
  rect(0, groundY, width, height - groundY);

  // Aplicar gravedad
  if (!onGround) {
    velY += gravity;
  }

  // Actualizar posición
  posY += velY;

  // Detectar suelo
  if (posY >= groundY) {
    posY = groundY;
    velY = 0;
    onGround = true;
  }

  // Dibujar personaje
  fill(200, 200, 255);
  rect(width / 2 - 15, posY - 40, 30, 40);

  // Texto informativo
  fill(255);
  textAlign(CENTER);
  text("Presiona ESPACIO para saltar", width / 2, 30);
}

function keyPressed() {
  if (key === ' ' && onGround) {
    velY = -8;   // Fuerza del salto
    onGround = false;
  }
}

```
Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/Lu0FFJkhA

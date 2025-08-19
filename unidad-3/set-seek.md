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

En esta obra se utiliza la friccion para mostrar patornes de como uno los quiera y que se vean mas fluidos, utilizando una linea que se puede mover a lo largo del canvas y pinta por donde pasa.

Para modelar la friccion en mi codigo simplemente utilice vel.mult(friction);, eso significa que en cada frame la velocidad se reduce multiplicándola por 0.95, haciendo que el objeto se frene poco a poco.

Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/AoqPwpq-Y

<img width="878" height="697" alt="image" src="https://github.com/user-attachments/assets/e29a3fe9-0d9c-4b6a-9034-1b9c4a18c0ad" />


#### Resistencia del aire y de fluidos

```jslet particles = [];

function setup() {
  createCanvas(600, 400);
  for (let i = 0; i < 20; i++) {
    particles.push(new Particle(20, random(height)));
  }
}

function draw() {
  background(240);

  // Dibujar la zona de fluido
  noStroke();
  fill(100, 150, 255, 150);
  rect(width/2, 0, width/2, height);

  for (let p of particles) {
    // Fuerza del "viento" (empuja hacia la derecha)
    let wind = createVector(0.05, 0);
    p.applyForce(wind);

    // Resistencia del aire (en todo el lienzo)
    let air = p.velocity.copy();
    air.normalize();
    let cAir = 0.01;
    let speedSq = p.velocity.magSq();
    air.mult(-cAir * speedSq);
    p.applyForce(air);

    // Resistencia de fluido (solo al entrar en la mitad derecha)
    if (p.position.x > width/2) {
      let fluid = p.velocity.copy();
      fluid.normalize();
      let cFluid = 0.1;
      let speedSq2 = p.velocity.magSq();
      fluid.mult(-cFluid * speedSq2);
      p.applyForce(fluid);
    }

    p.update();
    p.display();
  }
}

class Particle {
  constructor(x, y) {
    this.mass = 1;
    this.position = createVector(x, y);
    this.velocity = createVector(random(2, 4), random(-1, 1));
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
    fill(200, 50, 50, 200);
    ellipse(this.position.x, this.position.y, 12, 12);
  }
}

```

En el ejemplo, la resistencia del aire se aplica todo el tiempo con un coeficiente pequeño, lo que genera una fuerza opuesta al movimiento que frena lentamente al objeto. Por otro lado, la resistencia de fluidos solo aparece cuando el objeto entra en el rectángulo de "agua", usando un coeficiente mayor para frenar más fuerte.

Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/AjfaHIdfw

<img width="746" height="487" alt="image" src="https://github.com/user-attachments/assets/0908913c-f9df-4c0d-88db-75130a265553" />

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

En esta obra utilize la gravedad para simular una persona en la luna, en donde hay menos gravedad para que en el ejemplo sea mas notorio. Se puede ver como al oprimir espacio la persona salta pero va lento, no como en la tierra.

En este ejemplo se modelo la gravedad sumando constantemente un valor (gravity) a la velocidad vertical (velY) cuando el objeto no está en el suelo, eso hace que acelere hacia abajo hasta tocar el piso.

Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/Lu0FFJkhA

<img width="748" height="492" alt="image" src="https://github.com/user-attachments/assets/a4397e7a-e120-4432-917b-71cf0d2172ae" />


# Evidencias de la unidad 7

## Actividad 1

La palabra karma, utiliza las flechas de la letra K para mostrar como todo lo que das se te devuelve.

La palabra tetris, coloca las letras como en el juego encajandolas al fondo.

La palabra tsunami, utiliza la letra S para mostrar una grande ola.

La palabra mando por ejemplo, pero me refiero a un mando de consola tipo Play station, y que las vocales sean los joysticks.

La palabra teclado por ejemplo pero que todas las letras o cada 2 letras sean una tecla del mismo teclado.


## Actividad 2

MouseConstraint para poder interactuar con los cuerpos usando el mouse.

```js
const { Engine, World, Bodies, Mouse, MouseConstraint, Runner } = Matter;

let engine, world, runner;
let boxes = [];
let ground;
let mConstraint;
let canvasMouse;

function setup() {
  const canvas = createCanvas(windowWidth, windowHeight);

  engine = Engine.create();
  world = engine.world;

  runner = Runner.create();
  Runner.run(runner, engine);

  // Suelo
  ground = Bodies.rectangle(width / 2, height - 20, width, 40, { isStatic: true });
  World.add(world, ground);

  // Cajas
  for (let i = 0; i < 6; i++) {
    let box = Bodies.rectangle(random(100, width - 100), random(50, 200), 60, 60);
    World.add(world, box);
    boxes.push(box);
  }

  // MouseConstraint bien configurado
  canvasMouse = Mouse.create(canvas.elt);
  canvasMouse.pixelRatio = pixelDensity(); // importante para pantallas HiDPI

  const options = {
    mouse: canvasMouse,
    constraint: {
      stiffness: 0.2,
      render: { visible: false }
    }
  };

  mConstraint = MouseConstraint.create(engine, options);
  World.add(world, mConstraint);
}

function draw() {
  background(30);

  // Dibujar las cajas
  fill(0, 200, 255);
  noStroke();
  for (let box of boxes) {
    push();
    translate(box.position.x, box.position.y);
    rotate(box.angle);
    rectMode(CENTER);
    rect(0, 0, 60, 60);
    pop();
  }

  // Suelo
  fill(150);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width, 40);

  // Cuerpo arrastrado
  if (mConstraint.body) {
    const pos = mConstraint.body.position;
    fill(255, 0, 0, 150);
    ellipse(pos.x, pos.y, 80);
  }

  // 🔁 Actualizar la posición del mouse en cada frame
  canvasMouse.mouseupPosition.x = mouseX;
  canvasMouse.mouseupPosition.y = mouseY;
  canvasMouse.mousedownPosition.x = mouseX;
  canvasMouse.mousedownPosition.y = mouseY;
  canvasMouse.position.x = mouseX;
  canvasMouse.position.y = mouseY;
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

```

<img width="762" height="402" alt="image" src="https://github.com/user-attachments/assets/7e6d66dd-5de2-430a-a736-d5b86493ab04" />


Pelotas rebotando

```js
const { Engine, World, Bodies, Runner } = Matter;

let engine;
let world;
let ground;
let balls = [];
let runner;

function setup() {
  createCanvas(windowWidth, windowHeight);

  // Crear motor y mundo
  engine = Engine.create();
  world = engine.world;

  // 🌍 Ajustar gravedad (por defecto es 1)
  world.gravity.y = 0.3; // más suave

  // Crear Runner y ejecutarlo
  runner = Runner.create();
  Runner.run(runner, engine);

  // Suelo estático
  ground = Bodies.rectangle(width / 2, height - 20, width, 40, { isStatic: true });
  World.add(world, ground);

  // Crear pelotas
  for (let i = 0; i < 10; i++) {
    let ball = Bodies.circle(random(100, width - 100), random(-200, -50), random(20, 40), {
      restitution: 0.8, // rebote
      friction: 0.2
    });
    World.add(world, ball);
    balls.push(ball);
  }
}

function draw() {
  background(20);

  // Dibujar suelo
  fill(150);
  noStroke();
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, width, 40);

  // Dibujar pelotas
  fill(0, 200, 255);
  for (let ball of balls) {
    push();
    translate(ball.position.x, ball.position.y);
    ellipse(0, 0, ball.circleRadius * 2);
    pop();
  }
}
```

No se ve pero las pelotas salen desde arriba y rebotan unas con otras

<img width="802" height="285" alt="image" src="https://github.com/user-attachments/assets/5ef7cff8-7eb0-482a-988d-7494daa0214e" />


### Engine: 

Como su nombre lo dice, es el motor del programa, el que hace que funciona todo, se encarga de actualizar todas las fuerzas, velocidades, colisiones y posiciones de los cuerpos en el mundo.

### World:

Es un tipo de escenario, el mundo en donde pasa todo, el espacio en el que viven los cuerpos, restricciones, etc.

### Bodies:

Se encarga de crear formas como rectángulos, círculos o polígonos que pueden moverse, chocar y rotar.

###  Constraint:

Es como una cuerda que se encarga de unir cuerpos o limitar su movimiento.

### MouseConstraint:

Permite agarrar objetos con el mouse, este mismo utiliza un constraint entre el mouse y el objeto.

### Runner:

Se encarga de actualizar en cada frame el motor.

----------------------

Creo que mi unica dificultad fue como acostrumbrarme a utilizar un monton de elementos y conceptos nuevos asi de golpe pero con un poco de tiempo y paciencia me fui acostumbrando.


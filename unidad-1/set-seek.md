# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 1
La aleatoriedad en el arte generativo es una de las cosas mas importantes ya que es la que permite que este arte sea "infinito" de cierta manera e impredecible pero al mismo tiempo hermoso a su manera.

### Actividad 2
La aleatoriedad en la obra de Sofi para mi es mostrar que, asi como las raices de las plantas, su comportamiento es organico e impredecible y permite que la actividad nunca sea la misma.

Ya que mi perfil profesional es mas por la parte electronica de las experiencias y en el "como" y no tanto en el "que" la aleatoriedad me podria ayudar en proyectos como en los comportamientos de los se sores para diferentes tipos de distancias o variables a medir, por ejemplo, si tengo sensores de proximidad, y quiero que con el tiempo la distancia a la que el sensor se activa sea variable ya que la dinamica de la experiencia depende de ello.

### Actividad 3 

#### Codigo original:
``` .js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```
#### Codigo modificado:
``` .js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(3));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```
Antes de ejecutarlo: Creo que lo que va a pasar es que el cuadrado solo va a ir para los lados y abajo, no hacia arriba ya que no puede restarle a y.

Despues de ejecutarlo: Sucedio lo que esperaba creo que por la misma razon que dije antes, ya que no se le puede restar a y entonces solo puede ir hacia abajo o a los lados.

### Actividad 4

Una distribucion uniforme es cuando todas las opciones tienen la misma posibilidad de salir y una no uniforme es cuando hay opciones mas favorecidas y unas menos favorecidas a salir.

``` .js
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(max(0, randomGaussian(0, 3)));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```

### Actividad 5

``` .js
function setup() {
  createCanvas(640, 240);
  background(255);
  noStroke();
}

function draw() {
  let x = random(width);
  let y = random(height);

  let mean = 24;
  let sd = 8; // desviación estándar
  let r = abs(randomGaussian(mean, sd)); 

  fill(100, 100, 255, 50);
  ellipse(x, y, r, r);
}

```

Enlace al sketch de p5.js: https://editor.p5js.org/Zeuzd/sketches/sQ_TkyJTs

<img width="803" height="300" alt="image" src="https://github.com/user-attachments/assets/cd560e05-48d8-44f9-b896-2e2298ec3e0b" />

### Actividad 6

``` .js
function setup() {
  createCanvas(640, 240);
  background(255);
  noStroke();
}

function draw() {
  let x = random(width);
  let y = random(height);

  
  let r;
  let chance = random(1);
  if (chance < 0.01) {
    r = random(50, 120);
  } else {
    r = random(2, 20);   
  }

  fill(100, 100, 255, 50);
  ellipse(x, y, r, r);
}

```

Modifique el codigo del punto anterior para que cada vez que se genera un circulo, tenga un 1% de probabilidad de que sea mucho mas grande de lo normal.

Enlace a codigo en p5.js: https://editor.p5js.org/Zeuzd/sketches/zN-lXYFww

<img width="803" height="297" alt="image" src="https://github.com/user-attachments/assets/52d2837f-db71-4aab-bced-aef3174f70d0" />

### Actividad 7

``` .js
let yoff = 0;

function setup() {
  createCanvas(640, 240);
  noFill();
}

function draw() {
  background(255);
  stroke(0);

  beginShape();

  let xoff = 0;
  for (let x = 0; x <= width; x += 5) {
    let y = noise(xoff, yoff) * height;
    vertex(x, y);
    xoff += 0.02; // control del detalle horizontal
  }

  endShape();

  yoff += 0.01; // esto hace que la onda se "desplace" con el tiempo
}

```

Este es un codigo el cual hace un efecto de "ola en movimiento" con el ruido perlin utilizando el noise() en un vector.

Enlace al codigo en p5.js: https://editor.p5js.org/Zeuzd/sketches/LtzLLoy6r

<img width="803" height="307" alt="image" src="https://github.com/user-attachments/assets/e8a6af0e-8a0c-47ef-97bb-cba1b071a38a" />



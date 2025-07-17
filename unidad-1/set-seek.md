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

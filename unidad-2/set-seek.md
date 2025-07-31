# Unidad 2

## 🔎 Fase: Set + Seek

### Actividad 1

La suma de dos vectores en p5.js es como la suma normal de un vector, que se suma cada uno de sus componentes, y se hace llamando a la funcion .add, no se puede con un operador convencional de + ya que no funciona en javascript para suma de vectores.

### Actividad 2
En este caso quise recrear la simulacion de random walker del ejercicio 0.1 utilizando vectores.
Para lograrlo simplemente utilize vectores en vez de simplemente puntos pero los dos funcionan de la misma manera y con el mismo walker.

``` js
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
    // En vez de usar this.x y this.y, usamos un vector
    this.position = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    // Creamos un vector de movimiento aleatorio
    let step = createVector(0, 0);
    
    const choice = floor(random(4));
    if (choice == 0) {
      step.x = 1;
    } else if (choice == 1) {
      step.x = -1;
    } else if (choice == 2) {
      step.y = 1;
    } else {
      step.y = -1;
    }

    // Sumamos el vector de paso a la posición
    this.position.add(step);
  }
}

```

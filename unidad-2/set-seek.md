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
### Actividad 3

El resultado que espero es que simplemente aparezcan en consola dos mensajes, y si paso asi, pero eran mensajes de la posicion del vector.

Paso por valor: 
``` js
let a = 10;

function cambiar(x) {
  x = 20;
}

cambiar(a);
console.log(a); // ➜ 10 (no cambia)
```

Paso por referencia:
``` js
let persona = { nombre: "Ana" };

function renombrar(p) {
  p.nombre = "Luis";
}

renombrar(persona);
console.log(persona.nombre); // ➜ "Luis" (sí cambia)

```

La diferencia entre el paso por valor y el paso por referencia es que el de por valor solo afecta al objeto el cual se le da el paso en cambio el de referencia afecta tanto al que da el paso como al que le dice que lo de, osea que los dos dan el paso.

En el primer codigo se esta utilizando un paso por referencia.

Aprendi la importancia y la diferencia de los pasos por referencia y los pasos por valor y en que ocasiones se utiliza uno o el otro.

### Actividad 4

El metodo mag() devuelve la magnitud de un vector, y el magsq() devuelve la magnitud al cuadrado de un vector, ninguna es mas eficiente, solo hace cosas diferentes.

El metodo normalize() escala los componentes de un vector para que su magnitud sea 1.

El metodo dot() sirve para calcular el producto escalar de dos vectores.

La diferencia entre la version estatica y la instancia es la manera es como se llaman pero las dos cumplen con la misma funcion, solo que si ya tienen un vector instanciado conviene mas hacerlo por el metodo de instancia.

El producto cruz de dos vectores da como resultado un vector perpendicular a ambos (orientación determinada por la regla de la mano derecha).
Su magnitud representa el área del paralelogramo formado por los dos vectores.
Si los vectores son paralelos, el producto cruz es cero (sin dirección ni magnitud).

El metodo dist() calcula la distancia entre dos puntos de vectores.

El metodo limit() limita la magnitud de un vector hasta un numero maximo.

### Actividad 5

```js
let t=0;

function setup() {
    createCanvas(700, 700);
}

function draw() {
    background(200);

    let v0 = createVector(50, 50);
    let v1 = createVector(500, 0);
    let v2 = createVector(0, 500);
    let v3 = p5.Vector.lerp(v1, v2, 0.5);
    
    let vR = p5.Vector.add(v0, v1);  // extremo rojo
    let vB = p5.Vector.add(v0, v2);  // extremo azul
    let v5 = lerpColor('red','blue' , (sin(t) + 1) / 2);
    let vGreen = p5.Vector.sub(vB, vR); // de rojo a azul

    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(vR, vGreen, 'green');
  
    let vPurple = p5.Vector.lerp(vR, vB, (sin(t) + 1) / 2); //t oscila entre 0 y 1
    drawArrow(v0, p5.Vector.sub(vPurple, v0), v5);

    
    t += 0.02;
    
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```

El lerp() funciona dandole dos vectores, un inicio un final de distancia y cuanto va variando con el tiempo y el lerpcolor() tambien da una combiancion, dos colores y una variable con la que va cambiando de color.

El drawArrow() dibuja una flecha desde un punto base en la dirección de un vector.
Usa `translate()` para mover el origen y `rotate()` para orientar la punta de la flecha.
Dibuja una línea con `line()` y una punta triangular con `triangle()`.

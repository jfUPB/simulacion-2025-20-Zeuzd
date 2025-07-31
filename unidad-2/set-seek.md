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

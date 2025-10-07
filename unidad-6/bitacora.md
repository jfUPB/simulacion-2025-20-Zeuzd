# Evidencias de la unidad 6

## Set

### Actividad 1

<img width="1242" height="632" alt="image" src="https://github.com/user-attachments/assets/122d889c-8834-4731-99d4-d42b1055f036" />

Esta imagen me parece muy interesante ya que me recuerda algo que hace mucho no pensaba, y es que las experiencias no siemre tienen que ser un programa y se van o escuchen cosas de una pantalla, sino que tambien pueden ser cosas con incluso robotica.

<img width="1247" height="631" alt="image" src="https://github.com/user-attachments/assets/1ae99c79-c7d3-43fa-9f31-33cb7e39afd7" />

Este frame tambien me llamo mucho la atencion ya que el en su proceso creativo utiliza papel y colores para empezar a crear, y es algo que me gustaria empezar a hacer a futuro para proximos proyectos.

De su trabajo me inspira que el no tiene nunca planeado cual va a ser su punto final, solo como empezar pero no sabe exactamente a que va a llegar ni como.

## Seek 

### Actividad 2

Una steering force es una fuerza que corrige la velocidad de un agente para guiarlo hacia un objetivo o comportamiento deseado.
Se calcula como la diferencia entre la velocidad deseada y la velocidad actual, limitada por una fuerza máxima.

La diferencia es que las fuerzas fisicas provienen de la simulación de la naturaleza. Están definidas por leyes físicas, actúan de manera pasiva sobre el agente y no tienen “intención”. En cambio las steering forces son fuerzas artificiales de comportamiento. No vienen de la física, sino que se calculan para que un agente “decida” hacia dónde moverse (buscar, huir, alinearse). Representan intenciones, no leyes naturales.

La relacion es directa ya que el propuso la idea de que cuando se simularan bandadas de animales en lugar de usar solo fuerzas físicas clásicas, propuso que cada agente tuviera “reglas locales” (separación, alineación, cohesión), implementadas como steering forces que guían su velocidad.

### Actividad 3

2. Se utiliza un array 2D, Cada elemento es un p5.Vector que indica la dirección del flujo en esa celda del espacio, mediante Perlin Noise se calcula el angulo. Para cada celda (i, j) se calcula un ángulo theta = noise(xoff, yoff) * TWO_PI * 2.
Ese ángulo se convierte en un vector unitario con p5.Vector.fromAngle(theta).

3. El vehículo toma su posición actual y convierte esas coordenadas continuas en índices de celda de la cuadrícula del flow field. Primero escala ese vector deseado a la velocidad máxima y despues resta la velocidad actual del vehículo, esto produce un vector que apunta en la dirección en que debe corregir su movimiento.
   
4. Resolución del campo de flujo
   
   Se refiere al tamaño de las celdas de la cuadrícula, que determina cuán “fino” es el campo de vectores.
   ```js
   let cols, rows;        // cantidad de columnas y filas
    let resolution = 20;   // tamaño de cada celda (pixels)
   ```
   
   Velocidad máxima y fuerza máxima de los agentes

   En la clase del agente (Vehicle)
   ```js
   this.maxspeed = 4;   // velocidad máxima del agente
    this.maxforce = 0.1; // fuerza máxima (steering force)
   ```
   
5. En mi caso puse la resolucion mucho mas gruesa
   
   ```js
   let resolution = 5;  // celdas pequeñas → campo muy detallado
   cols = floor(width / resolution);
   rows = floor(height / resolution);
   ```
   
   Cuando la resolución es muy gruesa, veo que los agentes siguen trayectorias más suaves y coherentes, con menos curvas, se notan patrones globales 
   claros pero pierden detalle en sus movimientos individuales.
   
### Actividad 4

2.
  Separacion
   ```js
   separate(boids) {
   let desiredSeparation = 25.0;
   let steer = createVector(0, 0);
   let count = 0;

   for (let other of boids) {
    let d = p5.Vector.dist(this.position, other.position);
    if ((d > 0) && (d < desiredSeparation)) {
      // Vector que apunta lejos del vecino
      let diff = p5.Vector.sub(this.position, other.position);
      diff.normalize();
      diff.div(d); // Más fuerte cuanto más cerca
      steer.add(diff);
      count++;
    }
   }
   if (count > 0) {
    steer.div(count);
   }

   if (steer.mag() > 0) {
    steer.setMag(this.maxspeed);
    steer.sub(this.velocity);
    steer.limit(this.maxforce);
    }
   return steer;
   }

   ```
  Alineación

  ```js
  align(boids) {
  let neighbordist = 50;
  let sum = createVector(0, 0);
  let count = 0;

  for (let other of boids) {
    let d = p5.Vector.dist(this.position, other.position);
    if ((d > 0) && (d < neighbordist)) {
      sum.add(other.velocity); // Acumula direcciones
      count++;
    }
  }
  if (count > 0) {
    sum.div(count);
    sum.setMag(this.maxspeed);
    let steer = p5.Vector.sub(sum, this.velocity);
    steer.limit(this.maxforce);
    return steer;
  } else {
    return createVector(0, 0);
  }
  }

  ```

  Cohesion

  ```js
  cohesion(boids) {
  let neighbordist = 50;
  let sum = createVector(0, 0);
  let count = 0;

  for (let other of boids) {
    let d = p5.Vector.dist(this.position, other.position);
    if ((d > 0) && (d < neighbordist)) {
      sum.add(other.position); // Acumula posiciones
      count++;
    }
  }
  if (count > 0) {
    sum.div(count);
    return this.seek(sum); // Fuerza hacia el centro de masa
  } else {
    return createVector(0, 0);
  }
  }
  ```

3. Separacion: El objetivo es que los agentes no choquen entre si o se amontonen y su logica es que cuando detecta a un vecino demasiado cerca crea un vector de fuerza pero en direccion contraria.

   Alineacion: El objetivo es que cada boid ajuste su dirección y velocidad para coincidir con la de los demas boids para que se muevan como una manada,
   y su logica es que promedia la direccion y velocidad de todos y se las da a cada uno como una nueva direccion y velocidad.

   Cohesion: El objetivo es que todos permanezcan unidos y que si uno se queda atras se atrae al grupo, la logica es que mira a todos los demas boids vecinos y promedia sus posiciones y con esta genera una fuerza con direccion.

4. Radio:
   ```js
   // En separate()
    let desiredSeparation = 25.0; 

    // En align() y cohesion()
    let neighbordist = 50;

   ```

   Pesos o multiplicadores de las tres reglas:
   ```js
   flock(boids) {
    let sep = this.separate(boids);   // Separación
   let ali = this.align(boids);      // Alineación
   let coh = this.cohesion(boids);   // Cohesión

   // Ajustar pesos relativos
   sep.mult(1.5);   // separación más fuerte
   ali.mult(1.0);   // alineación normal
   coh.mult(1.0);   // cohesión normal

   // Aplicar fuerzas al boid
   this.applyForce(sep);
   this.applyForce(ali);
   this.applyForce(coh);
   }
   ```

   Velocidad máxima y fuerza máxima:
   ```js
   constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D();
    this.velocity.setMag(random(2, 4));
    this.acceleration = createVector(0, 0);

    this.maxforce = 0.05;  // Fuerza máxima de dirección
    this.maxspeed = 3;     // Velocidad máxima
    }

   ```

5. Fragmento de codigo cambiado:
   ```js
   let desiredSeparation = 300;
   ```

   Al correr el codigo se me muy interesante ya que al tener tantas particulas hacen efectos de que se agrupan y se desagrupan muchas veces ya que se intentan separan unas de otras pero en eso se encuentran y juntan con otras.

   <img width="765" height="292" alt="image" src="https://github.com/user-attachments/assets/82889057-138a-4901-948d-2405c9ea3b62" />

### Actividad 5

Empecé creando un sistema de partículas que reaccionaba al ritmo de la música, después hice que cambiaran de dirección cuando el ritmo variaba y añadí rastros que mostraban cómo el color de cada partícula iba cambiando. Luego conecté la velocidad de las partículas con el volumen de la canción para darles más energía, y al final convertí el mouse en un círculo de fuerza que interactúa y choca con las partículas.

```js
// === Variables globales ===
let flock;
let song, fft, amplitude;

// === Precarga de la canción ===
function preload() {
  song = loadSound("assets/enemigos.mp3");
}

// === Setup ===
function setup() {
  createCanvas(800, 600);
  song.loop();

  fft = new p5.FFT();
  amplitude = new p5.Amplitude();

  flock = new Flock();
  for (let i = 0; i < 250; i++) {
    flock.addBoid(new Boid(random(width), random(height)));
  }
}

// === Draw ===
function draw() {
  // Fondo con transparencia (rastro)
  background(0, 20); 

  let spectrum = fft.analyze();
  let bass = fft.getEnergy("bass");
  let treble = fft.getEnergy("treble");
  let level = amplitude.getLevel();

  // Dibujar el círculo de fuerza del mouse
  noFill();
  stroke(255, 100, 200, 200);
  ellipse(mouseX, mouseY, 80, 80);

  flock.run(bass, treble, level);
}

// === Flock ===
class Flock {
  constructor() {
    this.boids = [];
  }
  run(bass, treble, level) {
    for (let b of this.boids) {
      b.run(this.boids, bass, treble, level);
    }
  }
  addBoid(b) {
    this.boids.push(b);
  }
}

// === Boid ===
class Boid {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector();
    this.r = 3;
    this.maxspeed = 3;
    this.maxforce = 0.05;

    this.lastBass = 0; // detectar cambios de ritmo
  }

  run(boids, bass, treble, level) {
    // Velocidad depende del volumen
    this.maxspeed = map(level, 0, 0.5, 2, 8);  
    this.maxspeed = constrain(this.maxspeed, 2, 8);

    this.flock(boids, bass, treble, level);
    this.mouseRepel(); // nueva fuerza de repulsión con el mouse
    this.update();
    this.borders();
    this.show(level);
  }

  applyForce(force) {
    this.acc.add(force);
  }

  flock(boids, bass, treble, level) {
    let sep = this.separate(boids);
    let ali = this.align(boids);
    let coh = this.cohesion(boids);

    // separación ↔ agudos
    let separationStrength = map(treble, 0, 255, 0.5, 3.5);
    sep.mult(separationStrength);

    // cohesión ↔ volumen
    let cohesionStrength = map(level, 0, 0.5, 0.2, 3.0);
    coh.mult(cohesionStrength);

    // alineación neutra
    ali.mult(1.0);

    // si cambia el ritmo fuerte → invertir dirección
    if (abs(bass - this.lastBass) > 80) {
      this.vel.mult(-1);
    }
    this.lastBass = bass;

    this.applyForce(sep);
    this.applyForce(ali);
    this.applyForce(coh);

    // ruido extra
    let randomForce = p5.Vector.random2D().mult(0.1);
    this.applyForce(randomForce);
  }

  // === Repulsión con el mouse ===
  mouseRepel() {
    let mouse = createVector(mouseX, mouseY);
    let d = p5.Vector.dist(this.pos, mouse);
    let radius = 40; // radio de colisión (mitad del círculo dibujado)
    if (d < radius + this.r) {
      let repel = p5.Vector.sub(this.pos, mouse);
      repel.normalize();
      repel.mult(0.5); // fuerza de repulsión
      this.applyForce(repel);
    }
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxspeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  borders() {
    if (this.pos.x < -this.r) this.pos.x = width + this.r;
    if (this.pos.y < -this.r) this.pos.y = height + this.r;
    if (this.pos.x > width + this.r) this.pos.x = -this.r;
    if (this.pos.y > height + this.r) this.pos.y = -this.r;
  }

  // === Dibujo con rastros progresivos ===
  show(level) {
    let col = level > 0.2 ? color(180, 0, 255) : color(0, 255, 100);
    fill(col);
    stroke(col);
    ellipse(this.pos.x, this.pos.y, this.r * 3, this.r * 3);
  }

  // === Reglas ===
  separate(boids) {
    let desiredSeparation = 20;
    let steer = createVector();
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (d > 0 && d < desiredSeparation) {
        let diff = p5.Vector.sub(this.pos, other.pos);
        diff.normalize();
        diff.div(d);
        steer.add(diff);
        count++;
      }
    }
    if (count > 0) steer.div(count);
    if (steer.mag() > 0) {
      steer.setMag(this.maxspeed);
      steer.sub(this.vel);
      steer.limit(this.maxforce * 2);
    }
    return steer;
  }

  align(boids) {
    let neighbordist = 50;
    let sum = createVector();
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (d > 0 && d < neighbordist) {
        sum.add(other.vel);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      sum.setMag(this.maxspeed);
      let steer = p5.Vector.sub(sum, this.vel);
      steer.limit(this.maxforce);
      return steer;
    } else {
      return createVector();
    }
  }

  cohesion(boids) {
    let neighbordist = 50;
    let sum = createVector();
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (d > 0 && d < neighbordist) {
        sum.add(other.pos);
        count++;
      }
    }
    if (count > 0) {
      sum.div(count);
      return this.seek(sum);
    } else {
      return createVector();
    }
  }

  seek(target) {
    let desired = p5.Vector.sub(target, this.pos);
    desired.setMag(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.vel);
    steer.limit(this.maxforce);
    return steer;
  }
}
```

Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/bsflyOk2j

<img width="997" height="682" alt="image" src="https://github.com/user-attachments/assets/9d37fc86-5426-4463-bd32-e823b7794d12" />

## Reflect

1. Un agente autonomo es un programa capaz de tomar decisiones por si mismo según las reglas que le doy, puede percibir su entorno, reaccionar y actuar sin que yo intervenga directamente.
En el arte generativo, estos agentes hacen que las obras cobren vida y cambien constantemente para que cada ejecución puede ser distinta, impredecible y llena de movimiento.

2. El comportamiento emergente es cuando surgen patrones complejos a partir de reglas simples,
no es como que se programa directamente el resultado, sino las interacciones básicas entre los elementos.

3. La verdad nisiquiera tengo instalado blender en mi computador pero me parece  muy interesante la idea de que tambien se pueda aplicar ahi.

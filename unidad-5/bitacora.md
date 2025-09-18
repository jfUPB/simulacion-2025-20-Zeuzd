# Evidencias de la unidad 5

## Seek

### Ejemplo 4.2 – Single Particle

En esta simulación solo hay una partícula suelta.

Creación: se instancia directamente un objeto Particle con new.

Desaparición: no desaparece nunca porque no hay lógica de vida útil todavía.

Memoria: como solo hay un objeto, la gestión es mínima. Permanece en memoria durante toda la ejecución del sketch.

#### + Motion 101
Apliqué Motion 101 agregando aceleración constante hacia la derecha.
Lo hice para mostrar cómo velocidad y posición cambian paso a paso desde lo más básico.

```js
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0.05, 0); // Motion 101: aceleración constante
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
  }

  show() {
    stroke(0);
    fill(127);
    ellipse(this.pos.x, this.pos.y, 20);
  }
}

let p;

function setup() {
  createCanvas(400, 200);
  p = new Particle(width / 4, height / 2);
}

function draw() {
  background(220);
  p.update();
  p.show();
}

```
Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/Cbu7zFmMQ

Creación: solo existe una partícula, creada con new al inicio (setup).

Desaparición: no hay desaparición porque nunca la elimino; la partícula vive toda la simulación.

Memoria: el consumo es mínimo ya que siempre hay un solo objeto en memoria.

<img width="485" height="223" alt="image" src="https://github.com/user-attachments/assets/a8057ef2-9d9d-43cc-97d1-0d545b30ac2a" />


### Ejemplo 4.4 – An Array of Particles

En este caso ya hay un arreglo (array) de partículas.

Creación: cada Particle se instancia con new y se guarda en el array.

Desaparición: en este ejemplo tampoco hay “muerte” de partículas. Simplemente el array crece con cada nueva partícula.

Memoria: la memoria se va llenando indefinidamente porque los objetos nunca se eliminan del array.

#### + Distribución Normal

Usé distribución normal para la posición inicial en x con randomGaussian.
Así logré que la mayoría de partículas aparezcan en el centro y pocas en los extremos.
```js
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
  }

  update() {
    this.pos.y += 1;
  }

  show() {
    fill(127, 150);
    stroke(0);
    ellipse(this.pos.x, this.pos.y, 16);
  }
}

let particles = [];

function setup() {
  createCanvas(400, 200);
}

function draw() {
  background(220);

  let mean = width / 2;
  let sd = 60;
  let x = randomGaussian(mean, sd);
  particles.push(new Particle(x, 0));

  for (let p of particles) {
    p.update();
    p.show();
  }
}

```
Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/8ctzrN4IG

Creación: en cada frame creo una nueva partícula y la guardo en un array. Su posición en x se calcula con randomGaussian.

Desaparición: en este ejemplo no programé la muerte de partículas, por lo que todas permanecen en el array.

Memoria: la memoria crece indefinidamente porque nunca elimino partículas.

<img width="498" height="231" alt="image" src="https://github.com/user-attachments/assets/8b022a5b-cd20-4235-87d9-741180b97871" />


### Ejemplo 4.5 – A Particle System

Aquí aparece la clase ParticleSystem.

Creación: se van generando partículas en cada frame a partir de un origen definido.

Desaparición: cada partícula tiene una vida útil (lifespan). Cuando llega a cero, esa partícula se elimina del ArrayList.

Memoria: al remover las partículas muertas, el programa evita que el array crezca infinitamente. Eso libera memoria de objetos que ya no se usan.

#### + Friccion

Agregué fricción aplicando una fuerza opuesta y proporcional a la velocidad.
Esto hizo que las partículas desaceleren de forma realista en lugar de moverse indefinidamente.
```js
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(2, 5));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    // aplicar fricción
    let friction = this.vel.copy();
    friction.mult(-0.05);
    this.applyForce(friction);

    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2;
  }

  show() {
    noStroke();
    fill(127, this.lifespan);
    ellipse(this.pos.x, this.pos.y, 16);
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

class ParticleSystem {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.show();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

let ps;

function setup() {
  createCanvas(400, 200);
  ps = new ParticleSystem(width / 2, 50);
}

function draw() {
  background(220);
  ps.addParticle();
  ps.run();
}

```
Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/wK2ru21Mn

Creación: cada frame se añade una nueva partícula en el ParticleSystem.

Desaparición: cada partícula tiene una variable lifespan que disminuye en cada actualización. Cuando llega a 0, esa partícula se elimina del array con splice.

Memoria: al eliminar las partículas muertas, libero espacio y evito acumulación infinita en el array.

<img width="492" height="227" alt="image" src="https://github.com/user-attachments/assets/b2a34200-752d-41aa-a479-29990ecec4ab" />


### Ejemplo 4.6 – Multiple Particle Systems

Aquí ya hay varios sistemas de partículas funcionando a la vez.

Creación: cada sistema crea sus propias partículas, todas con su lifespan.

Desaparición: se eliminan partículas muertas de cada ArrayList, igual que en el 4.5.

Memoria: la memoria se mantiene controlada porque cada sistema se encarga de limpiar su propio array, aunque haya varios corriendo al tiempo.

#### + Fuerza Gravitacional

Introduje fuerza gravitacional como un vector que atrae hacia abajo en cada frame.
Lo hice para dar coherencia física y que los sistemas se sintieran más naturales.
```js
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(2, 4));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
  }

  applyForce(force) {
    this.acc.add(force);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2;
  }

  show() {
    fill(127, this.lifespan);
    noStroke();
    ellipse(this.pos.x, this.pos.y, 16);
  }

  isDead() {
    return this.lifespan < 0;
  }
}

class ParticleSystem {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.applyForce(createVector(0, 0.1)); // gravedad
      p.update();
      p.show();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

let systems = [];

function setup() {
  createCanvas(400, 200);
}

function draw() {
  background(220);

  if (mouseIsPressed) {
    systems.push(new ParticleSystem(mouseX, mouseY));
  }

  for (let ps of systems) {
    ps.addParticle();
    ps.run();
  }
}

````
Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/rCzN8LmOP

Creación: cada sistema de partículas (cuando hago clic) genera su propio conjunto de partículas nuevas en cada frame.

Desaparición: igual que en el 4.5, las partículas tienen un lifespan y se eliminan del array al morir.

Memoria: aunque hay múltiples sistemas corriendo, cada uno limpia sus partículas muertas, así que la memoria no crece descontroladamente.

<img width="490" height="241" alt="image" src="https://github.com/user-attachments/assets/e5c21ac9-cda3-4951-aae4-bb95a83342eb" />


### Ejemplo 4.7 – Inheriting from a Class

Aquí se introduce herencia: distintas clases de partículas (como Confetti) que heredan de Particle.

Creación: las partículas se crean igual, con new, pero ahora pueden ser de diferentes tipos gracias al polimorfismo.

Desaparición: cada partícula sigue teniendo su lifespan y se elimina del array cuando muere.

Memoria: se gestiona igual que antes: limpieza periódica de partículas muertas, pero ahora con la flexibilidad de manejar distintos comportamientos sin cambiar la lógica de gestión.

#### + Ruido Perlin

Apliqué ruido Perlin en el movimiento de partículas tipo Confetti.
Sirvió para generar trayectorias más orgánicas y menos caóticas que con random.
```js
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.lifespan = 255;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2;
  }

  show() {
    fill(127, this.lifespan);
    noStroke();
    ellipse(this.pos.x, this.pos.y, 16);
  }

  isDead() {
    return this.lifespan < 0;
  }
}

class Confetti extends Particle {
  constructor(x, y) {
    super(x, y);
    this.noiseOffset = random(1000);
  }

  update() {
    super.update();
    let n = noise(this.noiseOffset) * TWO_PI * 2;
    this.pos.x += cos(n);
    this.pos.y += sin(n);
    this.noiseOffset += 0.01;
  }

  show() {
    push();
    rectMode(CENTER);
    fill(200, 100, 255, this.lifespan);
    translate(this.pos.x, this.pos.y);
    rotate(frameCount / 10.0);
    rect(0, 0, 12, 12);
    pop();
  }
}

class ParticleSystem {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Confetti(this.origin.x, this.origin.y));
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.update();
      p.show();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

let ps;

function setup() {
  createCanvas(400, 200);
  ps = new ParticleSystem(width / 2, height / 2);
}

function draw() {
  background(220);
  ps.addParticle();
  ps.run();
}

```
Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/SNnjdvZXmU

Creación: las partículas se instancian con new, pero ahora pueden ser de distintas clases gracias a la herencia (Particle y Confetti).

Desaparición: todas siguen usando la lógica de lifespan y se eliminan al llegar a 0.

Memoria: al ir borrando cada partícula muerta del array, el sistema mantiene estable el uso de memoria, aunque las trayectorias sean más complejas (con ruido Perlin).

<img width="490" height="230" alt="image" src="https://github.com/user-attachments/assets/88defbd8-c0b6-47d5-b8eb-3f5e9de39c96" />

## Apply

### Concepto 
Quiero hacer un sistema como el "ciclo de la vida" como nace-vive-se reproduce-muere con el sistema de particulas en el cual las particulas puedan ser hombre o mujer simulando a una persona y si estos se chocan puede nacer una nueva particula.

Para crear esto voy a utilizar los conceptos de interactividad, polimorfismo, herencia, ruido perlin, friccion, distribucion normal, motion 101, gestion del tiempo de vida de las partículas y la memoria.

### Codigo

Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/wt_IWVwKl

```js
let particles = [];
const MAX_PARTICLES = 200;

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(20, 40, 60, 40);

  // Crear nuevas partículas con el mouse
  if (mouseIsPressed && particles.length < MAX_PARTICLES) {
    if (random() < 0.5) {
      particles.push(new BlueCloud(mouseX, mouseY)); // azul (hombre)
    } else {
      particles.push(new PinkCloud(mouseX, mouseY)); // rosada (mujer)
    }
  }

  // Actualizar y mostrar partículas
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.applyPerlinForce();
    p.update();
    p.display(); // polimorfismo: depende del tipo de partícula

    // Reproducción solo entre azul y rosada
    for (let j = i - 1; j >= 0; j--) {
      let other = particles[j];

      if (
        particles.length < MAX_PARTICLES &&
        p.collides(other) &&
        p.canReproduce() &&
        other.canReproduce()
      ) {
        // Verificar que sean de distinto tipo (Blue + Pink)
        if (
          (p instanceof BlueCloud && other instanceof PinkCloud) ||
          (p instanceof PinkCloud && other instanceof BlueCloud)
        ) {
          // Hijo: azul o rosado al azar
          if (random() < 0.5) {
            particles.push(
              new BlueCloud((p.pos.x + other.pos.x) / 2, (p.pos.y + other.pos.y) / 2)
            );
          } else {
            particles.push(
              new PinkCloud((p.pos.x + other.pos.x) / 2, (p.pos.y + other.pos.y) / 2)
            );
          }

          p.resetCooldown();
          other.resetCooldown();
          p.children++;
          other.children++;
        }
      }
    }

    // Eliminar partículas muertas
    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }
}

// ---------------------
// Clase base Particle
// ---------------------
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);

    this.lifespan = 180;

    // Distribución normal para el tamaño inicial
    this.size = abs(randomGaussian(10, 3));
    this.growthRate = 0.05;
    this.noiseOffset = random(1000);

    this.cooldown = 0;
    this.children = 0;
    this.maxChildren = 3;
  }

  applyForce(f) {
    this.acc.add(f);
  }

  applyPerlinForce() {
    let angle = noise(this.noiseOffset, frameCount * 0.01) * TWO_PI;
    let force = p5.Vector.fromAngle(angle).mult(0.05);
    this.applyForce(force);
  }

  update() {
    // Motion 101 + fricción
    this.vel.add(this.acc);
    this.vel.mult(0.9);
    this.pos.add(this.vel);
    this.acc.mult(0);

    // Crecimiento limitado
    if (this.size < 30) {
      this.size += this.growthRate;
    }

    // Envejecimiento
    this.lifespan -= 0.5;

    // Reducir cooldown
    if (this.cooldown > 0) this.cooldown--;
  }

  display() {
    // Polimorfismo: sobreescrito en subclases
    noStroke();
    fill(255, this.lifespan);
    ellipse(this.pos.x, this.pos.y, this.size);
  }

  isDead() {
    return this.lifespan <= 0;
  }

  collides(other) {
    let d = dist(this.pos.x, this.pos.y, other.pos.x, other.pos.y);
    return d < (this.size + other.size) / 2;
  }

  canReproduce() {
    return this.cooldown === 0 && this.children < this.maxChildren;
  }

  resetCooldown() {
    this.cooldown = 60;
  }
}

// ---------------------
// Subclase BlueCloud (hombre)
// ---------------------
class BlueCloud extends Particle {
  display() {
    noStroke();
    fill(100, 150, 255, this.lifespan); // azul
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}

// ---------------------
// Subclase PinkCloud (mujer)
// ---------------------
class PinkCloud extends Particle {
  display() {
    noStroke();
    fill(255, 150, 200, this.lifespan); // rosado
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}
```
<img width="725" height="487" alt="image" src="https://github.com/user-attachments/assets/cff3c2cb-c12f-45c3-ae92-2d6cc68eb337" />

### Conceptos utilizados 

#### Distribucion normal

En el constructor de la clase base Particle:
```js
this.size = abs(randomGaussian(10, 3));
```
Usé randomGaussian() para que el tamaño inicial no sea fijo, sino que siga una distribución normal con media 10 y desviación estándar 3.

#### Motion 101

En el método update() de Particle:
```js
this.vel.add(this.acc);
this.vel.mult(0.9);  // ← aquí entra la fricción
this.pos.add(this.vel);
this.acc.mult(0);
```
La partícula aplica aceleración - velocidad - posición.

#### Friccion

En update():
```js
this.vel.mult(0.9);
```
En cada frame la velocidad se reduce un poco simulando la resistencia del fondo para que no se muevan indefinidamente.

#### Ruido perlin

En el método applyPerlinForce() de Particle.

```js
let angle = noise(this.noiseOffset, frameCount * 0.01) * TWO_PI;
let force = p5.Vector.fromAngle(angle).mult(0.05);
this.applyForce(force);

```
Se usa noise() para calcular un ángulo suave y continuo y despues se aplica a la fuerza para crear un movimiento natural y fluido.

#### Polimorfismo

En los métodos display() de BlueCloud y PinkCloud:

```js
class BlueCloud extends Particle {
  display() {
    fill(100, 150, 255, this.lifespan); // azul
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}

class PinkCloud extends Particle {
  display() {
    fill(255, 150, 200, this.lifespan); // rosado
    ellipse(this.pos.x, this.pos.y, this.size);
  }
}

```

Cada subclase redefine display() y cuando el programa llama a p.display(), se ejecuta la versión correcta según el tipo real de partícula.

#### Herencia

En la declaración de subclases:

```js
class BlueCloud extends Particle { ... }
class PinkCloud extends Particle { ... }

```

Ambas heredan de Particle, comparten toda la lógica común y solo cambian lo que necesitan (el color en display()).

#### Tiempo de vida

En update() de Particle:

```js
this.lifespan -= 0.5;
```

Y en isDead():

```js
return this.lifespan <= 0;
```

Cada frame se reduce la vida de la partícula y cuando llega a 0, isDead() devuelve true.

#### Gestión de la memoria

En el draw():

```js
if (p.isDead()) {
  particles.splice(i, 1);
}

```

Si la partícula muere, se elimina del array particles.

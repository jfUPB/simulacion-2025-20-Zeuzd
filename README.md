# Bitácoras de aprendizaje

Esta bitácoras de aprendizaje buscan evidenciar tu proceso y tus resultados de aprendizaje.

## Realización de actividades

Cada unidad del curso tiene una carpeta asociada. Dentro de esa carpeta encontrarás las bitácoras de aprendizaje de cada una de las fases
de la unidad:

| Fase         | Enfoque                                                    | Qué se evidencia                                |
| ------------ | ---------------------------------------------------------- | ----------------------------------------------- |
| **Set-Seek** | Exploración del contexto y formulación de preguntas        | Comprensión inicial, intereses, hipótesis       |
| **Apply**    | Aplicación de lo aprendido en contextos reales o simulados | Capacidad de análisis, uso integrado de saberes |
| **Reflect**  | Evaluación y reconstrucción del conocimiento               | Metacognición, transferencia, juicio crítico    |

Las bitácoras de cada fase te permitirán documentar tu proceso de aprendizaje y evidenciar tus procesos coginitivos y resultados 
del aprendizaje. 

Cada fase tiene actividades. Para reportar los resultados de esas actividades usarás este formato:

``` md
### Actividad XX
Contenido de la actividad
```

Para insertar una imagen:

``` md
![decripción de la imagen](URL de la imagen)
```
Para insertar un hipervínculo:

``` md
[texto hipervinculado](URL del enlace)
```

Para insertar código

  ```
    ``` lenguaje
      function setup(){
      }
    ```
  ```

En lenguaje puedes colocar: py para python, js para javascript, bash para comandos de consola, cpp para c++. Por ejemplo, para insertar código 
javascript:

  ```
    ``` js
      function setup(){
      
      }
    ```
  ```

El resultado será este:

``` js
function setup(){

}
```

## Consideraciones de diseño de la bitácora

Para el diseño de estas bitácoras se consideró el modelo pedagógico integrado (MPI) de la UPB:

* El MPI, "privilegia el aprendizaje, la posición activa del estudiante en la construcción de su propio 
conocimiento, el papel mediador del profesor, la relación profesor–estudiante basada en el diálogo 
y guiada por el respeto y el reconocimiento de la dignidad del otro como persona."
* La formación es por compentencias. Estas se definen en la UPB como "la actuación integral para identificar, 
interpretar, argumentar y resolver problemas del contexto integrando conceptos y teorías, actitudes y valores, 
y habilidades procedimentales y técnicas. 
* Para la formación en com­petencias, la UPB opta por "el **aprender a aprender** como el proceso de estructura­ción y 
transformación que el estudiante hace del conocimiento desde la investiga­ción, y no como la simple asimilación 
del mismo. Desde esta concepción de apren­dizaje se desprenden las lógicas de la en­señanza y sus didácticas que 
posibiliten el aprendizaje significativo.

## Implicaciones

✔ No basta con saber contenido: debes demostrar que puedes usar ese conocimiento de forma integrada para resolver problemas reales.  
✔ La investigación no es un componente adicional, sino el motor del proceso de *aprender a aprender*.  
✔ Actuación integral: se espera una combinación de saber conceptual (qué sé), procedimental (qué sé hacer) y actitudinal (cómo actúo).  
✔ Eres el protagonista del proceso: no se trata de repetir contenido, sino de reorganizar y transformar el conocimiento en función de 
tu propio proceso investigativo.  
✔ **Aprender a aprender**: se espera que desarrolles autonomía, metacognición y capacidad para transferir lo aprendido a nuevas situaciones.  
✔ El profesor es un mediador: mi papel es diseñar contextos de aprendizaje ricos en significado, conectados con tus intereses y realidades.  
✔ Se favorecen metodologías activas.

-------

### Actividad 1

Yo escogi los ejemplos 2 y 3 de Sonar+D y Le Parody.

Vi que los visuales responden a la musica con algoritmos, por ejemplo con cada beat se muestra unas particulas, con los agudos se muestran unas lineas que se van moviendo conforme al ritmo de la musica, osea todo esta entrelazado ya que responde al ritmo, a la intensida y a las frecuencias.

En los primeros minutos del ejemplo de Le Parody me llamo mucho la atencion las parte del medio ya que aparecen unas particulas que se van moviendo en las que creo que afectan la voz de la muchacha, el ritmo de la musica y la frecuencia, y todo esto acompañado de los puntos en los alrededores que van con el beat de la cancion. 

Tambien en este mismo ejemplo se ve como el algoritmo va cambiando y no se queda en esa misma "fase" despues se libera y se desplaza por toda la pantalla las particulas que solo estaban en el centro, y pareciera que hay varias capas ya que aparecen como partes que afectan de una manera la imagen de abajo, es muy interesante, y claro, cada performance sera diferente aunque la musica fuera la misma, por eso se llama "arte generativo".

El hecho de que todo sea en live siento que es algo muy nuevo y llamativo ya que agrega dificultad al artista para estar pendiente de tantas cosas y es una nueva forma de "ser unico" ya que cada persona haria las cosas de manera diferente, y todo esto conlleva una preparacion, no es solamente una musica de fondo, ya es jugar con tu propio algortimo en tiempo real frente a cientos o miles de personas.

### Actividad 2

Yo elegi esta cancion: 
https://www.youtube.com/watch?v=JjPtDl6EJ3o&list=RDJjPtDl6EJ3o&start_radio=1

Esta es una cancion de punk brasileño, entonces voy a manejar como el concepto de caos y sensaciones fuertes, por ejemplo yo siempre utilizo esta cancion cuando me quiero levantar el animo o cuando quiero motivacion en el gym, entonces voy a manejarme alrededor de ese concepto.

Voy a utilizar flocking, friccion y motion 101 ya que siento que son las que mas facil pueden representar el caos si se saben utilizar de manera adecuada.

Planeo tener un sistema de particulas y un fondo y todo se mueva con el ritmo de la canciony vaya cambiando de color y con las teclas pueda "jugar" con las particulas.

### Actividad 3

```js

let particles = [];
let song, amp, fondo;
let playing = false;
let brilloFondo = 150;

let attractToCenter = false;
let centerExplosion = false;

let tornadoActive = false;
let tornadoDirection = 1;
let tornadoRadius = 200;
let tornadoCenter;

let freezeParticles = false; // congelar con "b"
let ondas = []; // lista de ondas expansivas

// Nuevo: color global para todas las partículas
let globalHue = 200;

function preload() {
  song = loadSound("punk.mp3");
  fondo = loadImage("portada.jpg");
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB);
  noStroke();

  amp = new p5.Amplitude();
  amp.setInput(song);

  for (let i = 0; i < 250; i++) {
    particles.push(new Particle(random(width), random(height)));
  }

  tornadoCenter = createVector(width / 2, height / 2);

  let playButton = select("#playButton");
  playButton.mousePressed(() => {
    if (!playing) {
      song.loop();
      playButton.hide();
      playing = true;
    }
  });
}

function draw() {
  if (!playing) return;

  let level = amp.getLevel();
  let beat = map(level, 0, 0.4, 0, 1);
  beat = constrain(beat, 0, 1);

  // Actualizar color global según el beat (puedes cambiar los límites)
  // Aquí pasa de 120 (verde) a 240 (azul) según la intensidad
  globalHue = map(beat, 0, 1, 120, 240, true);

  if (level > 0.15) {
    brilloFondo = map(level, 0.15, 1, 130, 255, true);
  } else {
    brilloFondo = lerp(brilloFondo, 50, 0.05);
  }

  if (fondo) {
    tint(brilloFondo, brilloFondo, brilloFondo, 255);
    let zoom = map(level, 0, 0.4, 0.9, 1.1, true);
    let fw = width * zoom;
    let fh = height * zoom;
    let fx = (width - fw) / 2;
    let fy = (height - fh) / 2;
    image(fondo, fx, fy, fw, fh);
    noTint();
  } else {
    background(0);
  }

  // 🌈 Dibujar ondas expansivas visuales
  noFill();
  for (let o of ondas) {
    strokeWeight(o.grosor);
    stroke(o.hue, 100, 100, o.alpha / 255);
    ellipse(o.x, o.y, o.r * 2);
    o.r += 12;
    o.alpha -= 8;
  }
  ondas = ondas.filter(o => o.alpha > 0);

  // 🔹 Dibujar partículas
  for (let p of particles) {
    if (!freezeParticles) p.update(beat);
    p.show(beat);
  }
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(1, 3));
    this.acc = createVector();
    this.maxSpeed = random(2, 5);
    this.baseSize = random(6, 10);
    // ya no guardamos hue por partícula; usamos globalHue
  }

  update(beat) {
    let steer = createVector(0, 0);
    let center = createVector(width / 2, height / 2);

    let neighbors = 0;
    for (let other of particles) {
      let d = dist(this.pos.x, this.pos.y, other.pos.x, other.pos.y);
      if (d > 0 && d < 80) {
        let diff = p5.Vector.sub(this.pos, other.pos);
        diff.normalize();
        diff.div(d * 0.6);
        steer.add(diff);
        neighbors++;
      }
    }
    if (neighbors > 0) steer.div(neighbors);
    steer.limit(0.4);

    let attractionStrength = map(beat, 0, 1, 0.003, -0.00005, true);
    let toCenterBeat = p5.Vector.sub(center, this.pos).mult(attractionStrength);

    this.acc.add(steer);
    this.acc.add(toCenterBeat);

    if (attractToCenter) {
      let toCenter = p5.Vector.sub(center, this.pos);
      toCenter.setMag(10);
      this.acc.add(toCenter);
    }

    if (centerExplosion) {
      let away = p5.Vector.sub(this.pos, center);
      away.setMag(random(5, 10));
      this.vel.add(away);
    }

    if (tornadoActive) {
      let toParticle = p5.Vector.sub(this.pos, tornadoCenter);
      let currentDistance = toParticle.mag();

      let radiusError = currentDistance - tornadoRadius;
      let radialForce = toParticle.copy().normalize().mult(-radiusError * 0.05);
      this.acc.add(radialForce);

      let tangent = createVector(-toParticle.y, toParticle.x).normalize();
      tangent.mult(2 * tornadoDirection);
      this.acc.add(tangent);
    }

    let speedFactor = lerp(0.5, 6, pow(beat, 2));
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed * speedFactor);
    this.pos.add(this.vel);
    this.acc.mult(0);

    if (this.pos.x < 0 || this.pos.x > width) this.vel.x *= -1;
    if (this.pos.y < 0 || this.pos.y > height) this.vel.y *= -1;
  }

  show(beat) {
    let s = this.baseSize * map(beat, 0, 1, 0.7, 4);
    let hue = globalHue; // <-- usa el hue global
    let sat = map(beat, 0, 1, 50, 100);
    let bright = map(beat, 0, 1, 40, 100);

    stroke(hue, sat, bright);
    strokeWeight(2);
    fill(0);

    let cornerRadius = map(beat, 0, 1, s / 1.5, 0, true);
    rectMode(CENTER);
    rect(this.pos.x, this.pos.y, s, s, cornerRadius);
  }
}

function keyPressed() {
  if (key === 'c' || key === 'C') {
    attractToCenter = true;
    centerExplosion = false;
  }

  if (key === 'v' || key === 'V') {
    tornadoActive = true;
    tornadoDirection = random([1, -1]);
  }

  if (key === 'b' || key === 'B') {
    freezeParticles = true;
  }
}

function keyReleased() {
  if (key === 'c' || key === 'C') {
    attractToCenter = false;
    centerExplosion = true;
    setTimeout(() => {
      centerExplosion = false;
    }, 100);
  }

  if (key === 'v' || key === 'V') {
    tornadoActive = false;
  }

  if (key === 'b' || key === 'B') {
    freezeParticles = false;
    for (let p of particles) {
      let speed = p.vel.mag();
      p.vel = p5.Vector.random2D().mult(speed);
    }
  }
}

// 💣 Clic = explosión + onda expansiva visual (usa globalHue)
function mousePressed() {
  let clickCenter = createVector(mouseX, mouseY);

  // Empuje a partículas cercanas
  for (let p of particles) {
    let d = dist(mouseX, mouseY, p.pos.x, p.pos.y);
    if (d < 150) {
      let force = p5.Vector.sub(p.pos, clickCenter);
      force.setMag(map(d, 0, 150, 10, 0));
      p.vel.add(force);
    }
  }

  // Onda expansiva visual — usa globalHue directamente
  ondas.push({
    x: mouseX,
    y: mouseY,
    r: 0,
    hue: globalHue,
    alpha: 255,
    grosor: random(8, 14)
  });
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

```

Enlace a p5.js: https://editor.p5js.org/Zeuzd/sketches/Pq97cDPNp

<img width="792" height="730" alt="image" src="https://github.com/user-attachments/assets/e4b9d6a8-0490-4adf-8c68-ff2d0f0ae3a9" />

<img width="798" height="736" alt="image" src="https://github.com/user-attachments/assets/ddebdc0d-73cb-4f46-a091-0bac4680f0fa" />


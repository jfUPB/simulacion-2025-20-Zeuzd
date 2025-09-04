# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Aplique el concepto de resortes haciendo que cada vez que las "luciernagas" salieran de cierto perimetro hubieran resortes que las devuelven hacia adentro para asi tenerlas controladas. Todo esto lo hice aplicando la ley de Hooke.

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Aplique el concepto de friccion haciendo que cada "luciernaga" al desplazarse tenga una reduccion de fuernza en cada frame y de el efecto de que no solo sigue al mouse sino que antes de cambiar de orientacion se desplaza un poco mas alla.

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Aplique el concepto de motion 101 hciendo que las "luciérnagas" no se muevan directamente hacia el mouse, sino que acumulen fuerzas en su aceleración y de ahí en su velocidad.

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Tu respuesta aquí:
> Aplique el concepto de distribucion normal y la use cuando definí la posicion donde inician las "luciernagas" y características físicas de cada "luciérnaga", por ejemplo la masa, que afecta su movimiento.

## ¿Cómo resolviste la interacción?
> Tu respuesta aquí:
> Lo resolvi utilizando el mouse, simulando con la narrativa que el mouse es como la "luciernaga reina" y que todas las demas la seguian por eso.

## Enlace a la obra en el editor de p5.js

[https://editor.p5js.org/Zeuzd/sketches/cF0vc_Ljq](URL)

## Código de la obra 

``` js
let luciernagas = [];
let numLuciernagas = 80;
let centro;

function setup() {
  createCanvas(800, 600);
  centro = createVector(width / 2, height / 2);

  for (let i = 0; i < numLuciernagas; i++) {
    let x = random(width);
    let y = random(height);
    luciernagas.push(new Luciernaga(x, y));
  }
}

function draw() {
  background(0);

  for (let l of luciernagas) {
    // --- Fuerza hacia el mouse (pero suave) ---
    let target = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(target, l.pos);
    dir.normalize();
    dir.mult(0.02 * l.mass); // más débil que antes
    l.applyForce(dir);

    // --- Resortes: límite invisible del bosque ---
    let toCenter = p5.Vector.sub(centro, l.pos);
    let distCenter = toCenter.mag();
    let springThreshold = min(width, height) * 0.32;
    let springActive = false;

    if (distCenter > springThreshold) {
      springActive = true;
      toCenter.normalize();
      let k = 0.02; 
      let extension = distCenter - springThreshold;
      let springForce = toCenter.mult(k * extension * l.mass);
      l.applyForce(springForce);

      // Dibuja línea resorte
      stroke(100, 150);
      line(l.pos.x, l.pos.y, centro.x, centro.y);
    }

    // --- Ruido aleatorio (wander) ---
    let angle = noise(frameCount * 0.01 + l.noiseOffset) * TWO_PI * 2;
    let wander = p5.Vector.fromAngle(angle).mult(0.05);
    l.applyForce(wander);

    // --- Separación entre luciérnagas ---
    let separation = createVector(0, 0);
    let desiredSeparation = 20;
    let count = 0;
    for (let other of luciernagas) {
      let d = p5.Vector.dist(l.pos, other.pos);
      if (other != l && d < desiredSeparation && d > 0) {
        let diff = p5.Vector.sub(l.pos, other.pos);
        diff.normalize();
        diff.div(d); // más cerca = más fuerza
        separation.add(diff);
        count++;
      }
    }
    if (count > 0) {
      separation.div(count);
      separation.setMag(0.1); // fuerza de separación
      l.applyForce(separation);
    }

    l.update();
    l.display(springActive);
  }
}

class Luciernaga {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.mass = random(0.5, 2);
    this.maxSpeed = 3;
    this.damping = 0.02;
    this.phase = random(TWO_PI);
    this.freq = random(0.01, 0.03);
    this.noiseOffset = random(1000);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.mult(1 - this.damping);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  display(springActive) {
    noStroke();
    let flicker = map(sin(frameCount * this.freq + this.phase), -1, 1, 0.5, 1);

    let baseCol = springActive ? color(255, 100, 100) : color(255, 255, 150);

    // núcleo brillante
    fill(red(baseCol), green(baseCol), blue(baseCol), 200 * flicker);
    ellipse(this.pos.x, this.pos.y, 8 * this.mass, 8 * this.mass);

    // halo difuso
    for (let i = 1; i <= 3; i++) {
      let alpha = 35 / i;
      fill(red(baseCol), green(baseCol), blue(baseCol), alpha * flicker);
      ellipse(this.pos.x, this.pos.y, 20 * i * this.mass, 20 * i * this.mass);
    }
  }
}

```

## Captura de pantalla representativa

<img width="885" height="717" alt="image" src="https://github.com/user-attachments/assets/444f44b9-0272-49d6-aa27-7ab0c318fdd8" />







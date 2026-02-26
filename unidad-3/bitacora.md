# Unidad 3

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
- Para esta obra me quise inspirar un poco en marco aurelio y una cita en la cual comparaba las generaciones humanas como hojas que caen de un arbol y pues la caida representa el destino de cada generacion de como estan destinidos a "caer" y dar paso a las generaciones nuevas el mouse representa el intento de estas generaciones por mantenerse y tratar de cambiar el rumbo apesar de que pueden cambiar un poco el rumbo no pueden cambiar su destino el cual es terminar de caer y dar el pazo a las nuevas generaciones cuando damos click el movimiento se hace un poco mas fuerte lo cual representa esas generaciones apasionadas que trataron de cambiar este rumbo "esas generaciones revolucionarias" mas sin embargo aunque cambiaron un poco el movimiento su destino termino siendo el mismo 
``` JS
let leaves = [];
let windStrength = 0.4;   
let dragC = 0.02;
let gust = 0;

function setup() {
  createCanvas(900, 600);

  for (let i = 0; i < 40; i++) {
    leaves.push(new Leaf(random(width), random(-height, 0)));
  }
}

function draw() {
  background(245);

  
  let n = noise(frameCount * 0.01);
  let windDir = map(mouseX, 0, width, -1.5, 1.5);
  let wind = createVector(
    (windDir * windStrength) +
    map(n, 0, 1, -0.1, 0.1) +
    gust,
    0
  );

  fill(0);
  noStroke();
  text("Mueve el mouse: viento. Mantén click: tormenta. Espacio: ráfaga fuerte. R: reiniciar.", 14, 20);

  for (let l of leaves) {

    // 1) GRAVEDAD
    let gravity = createVector(0, 0.12 * l.mass);
    l.applyForce(gravity);

    // 2) VIENTO
    let w = wind.copy();
    if (mouseIsPressed) w.mult(4);  
    l.applyForce(w);

    // 3) DRAG (resistencia del aire)
    let speed = l.vel.mag();
    if (speed > 0.0001) {
      let drag = l.vel.copy();
      drag.normalize();
      drag.mult(-1);
      let dragMag = dragC * speed * speed;
      drag.mult(dragMag);
      l.applyForce(drag);
    }

    // 4) FRICCIÓN SUAVE
    let friction = l.vel.copy();
    if (friction.mag() > 0.0001) {
      friction.normalize();
      friction.mult(-0.004);
      l.applyForce(friction);
    }

    l.update();
    l.edges();
    l.show();
  }


  gust *= 0.92;
}

function keyPressed() {
  if (key === ' ') {
    gust = random(-2, 2);  
  }

  if (key === 'r' || key === 'R') {
    leaves = [];
    for (let i = 0; i < 40; i++) {
      leaves.push(new Leaf(random(width), random(-height, 0)));
    }
  }
}

class Leaf {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(random(-1, 1), random(0, 2));
    this.acc = createVector(0, 0);

    this.mass = random(0.7, 2.5);
    this.size = this.mass * 10;

    this.angle = random(TWO_PI);
    this.spin = random(-0.06, 0.06);
    this.wobble = random(0.01, 0.03);
  }

  applyForce(force) {
    let f = force.copy().div(this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(7);
    this.pos.add(this.vel);

    this.angle += this.spin;
    this.pos.x += sin(frameCount * this.wobble + this.angle) * 0.8;

    this.acc.mult(0);
  }

  edges() {
    if (this.pos.y > height + this.size) {
      this.pos.y = -this.size;
      this.pos.x = random(width);
      this.vel = createVector(random(-1, 1), random(0.5, 2.5));
      this.mass = random(0.7, 2.5);
      this.size = this.mass * 10;
    }

    if (this.pos.x < -this.size) this.pos.x = width + this.size;
    if (this.pos.x > width + this.size) this.pos.x = -this.size;
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    rotate(this.angle + this.vel.x * 0.3);

    noStroke();
    fill(60, 140, 80, 180);
    ellipse(0, 0, this.size * 1.3, this.size);

    fill(40, 110, 60, 200);
    triangle(
      this.size * 0.6, 0,
      this.size * 1.2, -this.size * 0.2,
      this.size * 1.2, this.size * 0.2
    );

    pop();
  }
}
 ```
link: https://editor.p5js.org/LuisFernandoParra/full/fEpBPu4cB
<img width="673" height="445" alt="image" src="https://github.com/user-attachments/assets/ab63c671-e220-4951-997a-3803606e3b7d" />


## Bitácora de reflexión

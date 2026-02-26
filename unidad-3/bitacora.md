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
El marco de movimiento Motion 101 es como la base para entender cómo se mueve cualquier objeto, y yo lo entiendo como una cadena donde todo empieza con la fuerza. La fuerza es lo que provoca que algo cambie su estado, y según la fórmula, esa fuerza genera una aceleración (si hay más masa, la aceleración es menor). Luego, la aceleración no mueve directamente el objeto, sino que cambia su velocidad, es decir, la velocidad aumenta, disminuye o cambia de dirección gracias a la aceleración. Después, la velocidad es la que realmente cambia la posición, porque la posición simplemente indica dónde está el objeto en el espacio. Entonces lo veo como una secuencia: la fuerza produce aceleración, la aceleración modifica la velocidad y la velocidad cambia la posición. Si no hay fuerza, no hay aceleración, si no hay aceleración, la velocidad no cambia; y si no hay velocidad, la posición permanece igual.

```JS
/*
 * Brisa Digital: Homenaje a Calder (Versión Lenta)
 * Ajustado para un movimiento más elegante y pesado.
 */

let mob; 
let wind; 
let noiseOffset = 0; 

function setup() {
  createCanvas(windowWidth, windowHeight);
  let rootPosition = createVector(width / 2, 50);
  // Bajamos a 4 niveles para que sea menos caótico
  mob = new Mobile(rootPosition, 4); 
}

function draw() {
  background(240);

  // 1. VIENTO MÁS LENTO
  // Al bajar el incremento de noiseOffset (de 0.01 a 0.002), 
  // el cambio en la dirección del viento es casi imperceptible.
  let windSpeed = map(noise(noiseOffset), 0, 1, -0.02, 0.02);
  wind = createVector(windSpeed, 0);
  noiseOffset += 0.002; 

  // 2. ACTUALIZACIÓN
  mob.applyForce(wind); 
  mob.update(); 

  // 3. DIBUJO
  mob.show(); 
}

class Mobile {
  constructor(origin, levels) {
    this.origin = origin; 
    this.levels = levels; 

    this.angle = 0; 
    this.aVelocity = 0; 
    this.aAcceleration = 0; 

    this.len = random(120, 220) / (5 - levels);

    // AMORTIGUACIÓN (Damping)
    // Subimos de 0.98 a 0.95. Esto hace que el aire sea más "denso"
    // y la pieza pierda energía más rápido, evitando que gire locamente.
    this.damping = 0.95;

    this.leftChild = null;
    this.rightChild = null;

    if (levels > 1) {
      this.leftChild = new Mobile(createVector(0, 0), levels - 1);
      this.rightChild = new Mobile(createVector(0, 0), levels - 1);
    } else {
      this.finalShape = new AbstractShape(this.len);
    }
  }

  applyForce(force) {
    // TORQUE REDUCIDO
    // Multiplicamos por un valor mucho más pequeño (0.01) para que el viento
    // apenas logre empujar la masa de la escultura.
    let torque = force.x * this.len * 0.01;
    this.aAcceleration += torque; 

    if (this.leftChild) this.leftChild.applyForce(force);
    if (this.rightChild) this.rightChild.applyForce(force);
  }

  update() {
    this.aVelocity += this.aAcceleration; 
    this.aVelocity *= this.damping; 
    
    // LIMITADOR DE VELOCIDAD
    // Forzamos a que la velocidad angular nunca supere un límite bajo
    this.aVelocity = constrain(this.aVelocity, -0.02, 0.02);
    
    this.angle += this.aVelocity; 
    this.aAcceleration = 0; 

    if (this.leftChild) this.leftChild.update();
    if (this.rightChild) this.rightChild.update();
  }

  show() {
    push(); 
    translate(this.origin.x, this.origin.y); 
    rotate(this.angle); 

    stroke(50); 
    strokeWeight(map(this.levels, 1, 4, 1.5, 4)); 
    line(-this.len, 0, this.len, 0); 
    
    line(0, 0, 0, 20);

    if (this.levels > 1) {
      this.leftChild.origin = createVector(-this.len, 60);
      this.rightChild.origin = createVector(this.len, 60);
      
      line(-this.len, 0, -this.len, 60);
      line(this.len, 0, this.len, 60);

      this.leftChild.show();
      this.rightChild.show();
    } else {
      line(0, 0, 0, 50); 
      translate(0, 50); 
      this.finalShape.show();
    }

    pop(); 
  }
}

class AbstractShape {
  constructor(size) {
    this.size = size * 1.2; 
    this.colors = [
      color(200, 30, 30), // Rojo
      color(20),          // Negro
      color(240, 200, 50), // Amarillo
      color(40, 80, 160)   // Azul (añadido para variedad)
    ];
    this.color = random(this.colors); 
    this.shapeType = floor(random(2)); 
  }

  show() {
    noStroke();
    fill(this.color);

    if (this.shapeType === 0) {
      ellipse(0, 0, this.size, this.size * 0.8);
    } else {
      beginShape();
      let numPoints = 5;
      for (let i = 0; i < numPoints; i++) {
        let angle = map(i, 0, numPoints, 0, TWO_PI);
        let r = this.size * 0.5 + random(-5, 5);
        let x = r * cos(angle);
        let y = r * sin(angle);
        vertex(x, y);
      }
      endShape(CLOSE);
    }
  }
}
```

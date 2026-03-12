# Unidad 4

## Bitácora de proceso de aprendizaje
### Actividad 5

estuve explorando cómo salir de la rigidez de las coordenadas cartesianas para trabajar con ángulos de una forma más natural usando coordenadas polares. Al principio, la idea de pasar de un sistema de cuadrícula (x y y) a uno basado en la distancia desde el centro (r) y la dirección (θ) parece compleja, pero al ver el código todo cobra sentido. En la simulación inicial, entendí que la relación matemática fundamental para que la computadora dibuje en la pantalla es que x es el radio multiplicado por el coseno del ángulo y y es el radio por el seno del ángulo.  Al aplicar esto manualmente en el draw(), logré que el círculo orbitara perfectamente, pero luego quise experimentar con las herramientas nativas de p5.js. Al intentar simplificar el proceso usando p5.Vector.fromAngle(theta), me llevé una sorpresa: el círculo parecía haber desaparecido o quedarse estático en el centro. Lo que ocurrió es que este método, por defecto, crea un vector unitario, lo que significa que el radio es apenas de 1 píxel, haciendo el movimiento casi imperceptible a simple vista. Además, al borrar las variables manuales de x y y, la función line() dejó de funcionar porque ya no encontraba esas referencias. La verdadera revelación llegó con la segunda modificación, donde utilicé p5.Vector.fromAngle(theta, r). Aquí comprendí que p5.js permite pasarle la magnitud como segundo argumento, lo que hace que el vector calcule internamente toda la trigonometría por nosotros. Al usar las propiedades v.x y v.y del objeto vector, el código se volvió mucho más limpio y profesional. Aprendí que las coordenadas polares son la clave para cualquier movimiento circular o espiral, y que usar vectores no solo ahorra líneas de código, sino que hace que la lógica sea mucho más fácil de leer y escalar para proyectos más complejos

## Bitácora de aplicación 

``` JS
let recuerdos = [];
let angulo = 0;

function setup() {
  createCanvas(windowWidth, windowHeight);
}

function draw() {
  background(10, 15, 30, 50); // El azul profundo del tiempo

  // REGLA 1: EL BROTE (Coordenadas Polares)
  let radioBrote = 40;
  let x = mouseX + radioBrote * cos(angulo);
  let y = mouseY + radioBrote * sin(angulo);
  
  // Cap 0: Aleatoriedad en la aparición
  if (random(1) > 0.15) {
    recuerdos.push(new Recuerdo(x, y));
  }

  for (let i = recuerdos.length - 1; i >= 0; i--) {
    let r = recuerdos[i];

    // REGLA 3: EL OLVIDO (Cap 2 - Fricción)
    // La fricción es una fuerza opuesta a la velocidad
    let friccion = r.vel.copy();
    friccion.normalize();
    friccion.mult(-0.05); // Coeficiente de fricción
    r.applyForce(friccion);

    // REGLA 4: EL RESCATE (Cap 2 - Atracción)
    if (mouseIsPressed) {
      let centro = createVector(mouseX, mouseY);
      let atraccion = p5.Vector.sub(centro, r.pos);
      atraccion.setMag(0.6); 
      r.applyForce(atraccion);
    }

    r.update();
    r.display();

    if (r.estaMuerto()) {
      recuerdos.splice(i, 1);
    }
  }

  angulo += 0.2; 
}

class Recuerdo {
  constructor(x, y) {
    this.pos = createVector(x, y);
    // Cap 1: Vector de velocidad inicial aleatorio
    this.vel = p5.Vector.random2D().mult(random(2, 5));
    this.acc = createVector(0, 0);
    this.vida = 255;
    this.tamanio = random(3, 10); // Cap 0: Aleatoriedad
  }

  applyForce(f) {
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0); 
    this.vida -= 2;
  }

  display() {
    noStroke();
    // El color se intensifica cuando el recuerdo está cerca del mouse
    let d = dist(this.pos.x, this.pos.y, mouseX, mouseY);
    let brillo = map(d, 0, 200, 255, 100);
    fill(brillo, 200, 255, this.vida);
    circle(this.pos.x, this.pos.y, this.tamanio);
  }

  estaMuerto() {
    return this.vida < 0;
  }
}
```
- link: https://editor.p5js.org/LuisFernandoParra/full/jlKe1kRH1
- <img width="456" height="314" alt="image" src="https://github.com/user-attachments/assets/e15ddb00-a8e0-4545-94bb-2b468669cd43" />
- <img width="375" height="329" alt="image" src="https://github.com/user-attachments/assets/e6a54889-5279-4192-996a-2bf06d6875e5" />

## Bitácora de reflexión


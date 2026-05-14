# Unidad 4

## Bitácora de proceso de aprendizaje
### Actividad 5

estuve explorando cómo salir de la rigidez de las coordenadas cartesianas para trabajar con ángulos de una forma más natural usando coordenadas polares. Al principio, la idea de pasar de un sistema de cuadrícula (x y y) a uno basado en la distancia desde el centro (r) y la dirección (θ) parece compleja, pero al ver el código todo cobra sentido. En la simulación inicial, entendí que la relación matemática fundamental para que la computadora dibuje en la pantalla es que x es el radio multiplicado por el coseno del ángulo y y es el radio por el seno del ángulo.  Al aplicar esto manualmente en el draw(), logré que el círculo orbitara perfectamente, pero luego quise experimentar con las herramientas nativas de p5.js. Al intentar simplificar el proceso usando p5.Vector.fromAngle(theta), me llevé una sorpresa: el círculo parecía haber desaparecido o quedarse estático en el centro. Lo que ocurrió es que este método, por defecto, crea un vector unitario, lo que significa que el radio es apenas de 1 píxel, haciendo el movimiento casi imperceptible a simple vista. Además, al borrar las variables manuales de x y y, la función line() dejó de funcionar porque ya no encontraba esas referencias. La verdadera revelación llegó con la segunda modificación, donde utilicé p5.Vector.fromAngle(theta, r). Aquí comprendí que p5.js permite pasarle la magnitud como segundo argumento, lo que hace que el vector calcule internamente toda la trigonometría por nosotros. Al usar las propiedades v.x y v.y del objeto vector, el código se volvió mucho más limpio y profesional. Aprendí que las coordenadas polares son la clave para cualquier movimiento circular o espiral, y que usar vectores no solo ahorra líneas de código, sino que hace que la lógica sea mucho más fácil de leer y escalar para proyectos más complejos

## Bitácora de aplicación 
Para esta obra me quise inspirar en cómo nos olvidamos de las cosas y de cómo el tiempo hace que todo se pierda si no le ponemos atención. El mouse representa nuestro presente y las ganas de mantener vivo un recuerdo, por eso usé coordenadas polares para que las partículas broten en un círculo alrededor del mouse, representando esos recuerdos que están ahí cerquita de nosotros tratando de nacer.  Para el movimiento usé vectores (Capítulo 1) para que cada recuerdo tenga su propia velocidad y dirección al salir, pero lo que realmente define la obra es la fricción (Capítulo 2), porque esa fuerza representa el olvido; es lo que hace que los fragmentos se vayan frenando y se queden quietos hasta desaparecer si uno no hace nada. El mouse es el intento de uno por tratar de que no se detengan, y cuando damos click, se activa una fuerza de atracción que representa esa voluntad de rescatar lo que se está perdiendo. A pesar de que la fricción siempre está ahí tratando de frenar todo, el click hace que el movimiento sea más fuerte y traiga los fragmentos de vuelta al centro para que brillen otra vez, aunque sepamos que apenas soltemos el click, el destino de la partícula será volver a frenarse por la resistencia del tiempo.

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

`1. El código como narrativa (Diseño de Experiencias)
Aprendí que mi fuerte no es solo programar, sino traducir emociones a reglas físicas. En el entretenimiento digital, la oportunidad está en que el usuario "sienta" la historia a través del control. Si quiero representar soledad, no pongo un texto, subo la fricción (Cap. 2) para que al jugador le cueste moverse y sienta el peso del ambiente. Es usar la física para generar empatía.

2. Interfaces que se sienten naturales
Con las coordenadas polares, aprendí que puedo romper la pantalla cuadrada tradicional. Como ingeniero, puedo diseñar menús o interfaces interactivas que nazcan del usuario, que lo rodeen y que se muevan de forma circular. Eso en juegos o apps de entretenimiento se ve mucho más pro y orgánico que un montón de botones quietos.

3. Comportamientos vivos, no grabados
Aprendí que los vectores (Cap. 1) son los que mandan si quiero que algo se vea real. En lugar de animar un objeto para que vaya del punto A al B, le doy una fuerza. Esto me sirve para que, si estoy haciendo un juego, los enemigos no parezcan robots, sino que tengan inercia, que les cueste frenar o que se vean atraídos por el jugador de forma fluida. Es pasar de "animar" a "simular vida".

4. Optimización con sentido
Aprendí que ser un buen ingeniero también es saber cuándo limpiar la casa. El bloque del splice y el ciclo hacia atrás me enseñó que para que una experiencia de entretenimiento sea buena, tiene que ser fluida. No puedo dejar que el sistema se cargue de "basura" (fragmentos muertos), porque eso daña la inmersión del usuario.



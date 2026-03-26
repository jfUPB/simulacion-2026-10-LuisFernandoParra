# Unidad 5
## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 


###  Mapa de desiciones
| Elemento | Decisión de Diseño | Significado Narrativo / Justificación |
| :--- | :--- | :--- |
| **Emisión (Lluvia)** | Cenizas constantes cayendo desde el borde superior (`y = -10`) con un intervalo de `frameCount % 15`. | Representa la **Entropía Universal**: la materia física que llega constantemente al mundo, destinada a ocupar un lugar en el plano terrenal. |
| **Fuerzas (Gravedad)** | Vector vertical hacia abajo `(0, 0.2)` aplicado a las partículas tipo `Ceniza`. | Simboliza el **Peso de la Existencia**: la fuerza inevitable que atrae la materia hacia la quietud, la inercia y el reposo en la tierra. |
| **Visualización** | Contraste entre círculos grises opacos (Ceniza) y elipses neón con transparencia (Flama). | Representa el paso de lo **Inerte** a lo **Vital**; el color naranja/amarillo es la chispa de energía o "alma" que se consume mientras asciende. |
| **Interacción** | Cambio de estado de `Ceniza` a `Flama` por proximidad al cursor (`dist < 30`). | Representa el **Acto de Animación**: El usuario no crea materia de la nada, sino que "despierta" lo que ya existe. Es el catalizador que otorga propósito. |
| **Condición de Muerte** | La flama estalla en un destello visual y se elimina al llegar al límite superior o agotar su `vida`. | Simboliza la **Liberación Final**: A diferencia del cuerpo, la energía se expande y se funde con el vacío, cumpliendo su ciclo de trascendencia. |

---


<img width="1002" height="199" alt="image" src="https://github.com/user-attachments/assets/f4275c18-6dea-42de-aa4b-3018075bb8f0" />


- link: https://editor.p5js.org/LuisFernandoParra/full/J_8Q3NjtV
```JS
let particulas = [];
let gravedad;

function setup() {
  createCanvas(windowWidth, windowHeight);
  gravedad = createVector(0, 0.2); // Fuerza constante

  // Nacimiento inicial de cenizas en el suelo para empezar
  for (let i = 0; i < 80; i++) {
    particulas.push(new Ceniza(random(width), height - 10, false));
  }
}

function draw() {
  background(10, 10, 20);

  // EL SOPLO DE VIDA (Interacción)
  noFill(); 
  stroke(255, 50); 
  ellipse(mouseX, mouseY, 60);

  // LLUVIA CONSTANTE: Caen cenizas desde el cielo para mantener el ciclo vivo
  // Estas SÍ van a llegar al piso
  if (frameCount % 15 === 0 && particulas.length < 200) {
    particulas.push(new Ceniza(random(width), -10, true));
  }

  // Recorrido en reversa para gestionar la memoria correctamente
  for (let i = particulas.length - 1; i >= 0; i--) {
    let p = particulas[i];
    p.update();
    p.display();

    // INTERACCIÓN: Transformar ceniza del suelo en Flama ascendente
    if (p.constructor.name === "Ceniza" && !p.cayendo) {
      if (dist(mouseX, mouseY, p.pos.x, p.pos.y) < 30) {
        particulas[i] = new Flama(p.pos.x, p.pos.y);
      }
    }

    // CICLO DE VIDA: Muerte y Eliminación
    if (p.isDead()) {
      // LO QUE PEDISTE: Si es la que está subiendo (Flama), explota en el aire y desaparece
      if (p instanceof Flama) {
        // Explosión visual de energía en el aire (destello naranja/blanco)
        fill(255, 200, 100, 200); 
        ellipse(p.pos.x, p.pos.y, p.tam * 4); 
        fill(255, 255, 255, 150); 
        ellipse(p.pos.x, p.pos.y, p.tam * 2); 
        
        // Fíjate que AQUÍ YA NO NACEN CENIZAS NUEVAS. Solo explota y muere.
      }
      
      // Se elimina la partícula (sea la flama que explotó o la ceniza que se desvaneció en el piso)
      particulas.splice(i, 1); 
    }
  }

  // Línea visual del suelo
  stroke(100); 
  line(0, height, width, height);
}

// ==========================================
// CLASE BASE: CENIZA (La materia física)
// ==========================================
class Ceniza {
  constructor(x, y, cayendo) {
    this.pos = createVector(x, y);
    // Si nace cayendo, tiene velocidad. Si no, nace quieta.
    this.vel = cayendo ? createVector(random(-1, 1), random(0, 2)) : createVector(0, 0);
    this.cayendo = cayendo;
    this.tam = random(4, 7);
    this.vida = 255; 
  }

  update() {
    if (this.cayendo) {
      // Gravedad jalando las cenizas de la lluvia hacia abajo
      this.vel.add(gravedad); 
      this.pos.add(this.vel);
      
      // Cuando tocan el piso, se detienen y se vuelven materia inerte
      if (this.pos.y >= height - 10) {
        this.pos.y = height - 10;
        this.cayendo = false; // Ya no caen
        this.vel.mult(0);     // Se detienen
      }
    } else {
      // Si están en el suelo esperando, se desvanecen muy lentamente
      this.vida -= 0.5;
    }
  }

  display() {
    noStroke();
    fill(150, 150, 150, this.vida); 
    ellipse(this.pos.x, this.pos.y, this.tam);
  }
  
  isDead() {
    return this.vida <= 0; 
  }
}

// ==========================================
// CLASE HIJA: FLAMA (Las que seleccionamos y suben)
// ==========================================
class Flama extends Ceniza {
  constructor(x, y) {
    super(x, y, false); 
    // Van hacia arriba
    this.vel = createVector(random(-1, 1), random(-2, -5)); 
    this.tam = random(10, 15);
  }

  update() {
    this.pos.add(this.vel); // Suben constantemente
    this.vida -= 3;         // Se van consumiendo en el aire
  }

  display() {
    noStroke();
    fill(255, 100, 0, this.vida); 
    ellipse(this.pos.x, this.pos.y, this.tam * 1.5);
    fill(255, 200, 0, this.vida); 
    ellipse(this.pos.x, this.pos.y, this.tam);
  }

  isDead() {
    // Mueren cuando se consumen por completo o llegan muy arriba
    return this.vida <= 0 || this.pos.y < height * 0.1;
  }
}
```
-<img width="584" height="559" alt="image" src="https://github.com/user-attachments/assets/2e05a68f-2cce-4ee5-bf14-ed9db828ce12" />
-<img width="259" height="304" alt="image" src="https://github.com/user-attachments/assets/0c27c063-3c0f-4e30-b1ce-82db11cf5923" />
-<img width="233" height="289" alt="image" src="https://github.com/user-attachments/assets/dbe0a6ed-2ed5-4404-b39d-214e2eceef3f" />



## Bitácora de reflexión

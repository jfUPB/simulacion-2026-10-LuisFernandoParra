# Unidad 5
## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 

-Inspirada en la premisa "polvo eres y en polvo te convertirás", la obra captura el ciclo eterno entre la materia inerte y la energía vital.

-Las cenizas descienden al reposo terrenal hasta que el tacto las despierta en flamas ascendentes que estallan y se liberan en el aire.

-Esta pieza busca comunicar que la existencia debe consumirse y morir para liberar la energía necesaria que permitirá el nacimiento de un nuevo ciclo.

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


## Parte 1 — Los 10 Principios Fundamentales

### 1. Una partícula es una entidad con estado
Cada partícula es un objeto independiente con su propia "memoria". No es un simple dibujo en pantalla, sino una entidad que almacena variables específicas como su posición (`pos`), velocidad (`vel`) y nivel de transparencia (`vida`).

### 2. Una partícula tiene ciclo de vida
Las partículas no son infinitas; pasan por un proceso biológico simulado. Nacen (se instancian), crecen o se transforman (cambian de Ceniza a Flama) y finalmente mueren cuando cumplen una condición lógica, como agotar su tiempo de vida.

### 3. Gestión de colecciones dinámicas de elementos
El sistema utiliza estructuras de datos flexibles (como los *Arrays*). Esto permite que el número de elementos en pantalla cambie constantemente, creciendo cuando caen cenizas y encogiéndose cuando las flamas se desvanecen, adaptándose en tiempo real.

### 4. Creación y eliminación como parte central del modelo
Eliminar una partícula no es solo un detalle técnico para optimizar; es una decisión narrativa. En esta obra, la "muerte" de la flama es el evento que permite que el ciclo de energía continúe, haciendo que la gestión de memoria sea parte de la estética.

### 5. Separación entre la lógica individual y la del sistema
Existe una división de responsabilidades: la clase (Ceniza/Flama) sabe cómo moverse y dibujarse a sí misma, mientras que el sistema central (el emisor en el `draw`) decide cuándo crearlas, cuántas pueden existir y cuándo darles la orden de actualizarse.

### 6. El emisor o "Particle System" como abstracción
El emisor es la fuente o "grifo" de la simulación. Es una entidad lógica que define el punto de origen y la frecuencia con la que la materia (cenizas) entra al sistema, actuando como el motor que mantiene el flujo constante.

### 7. Sistemas de sistemas
Es la capacidad de anidar comportamientos. Podríamos tener un sistema principal de cenizas interactuando con sub-sistemas de chispas o efectos de explosión, creando capas de complejidad que conviven en el mismo espacio.

### 8. Heterogeneidad mediante herencia y polimorfismo
Permite que el sistema gestione elementos distintos bajo una misma estructura. Gracias a la herencia, las flamas y cenizas comparten una base, pero el polimorfismo permite que cada una reaccione de forma única (unas caen y otras suben).

### 9. Respuesta a fuerzas globales y locales
Las partículas reaccionan a su entorno. En este proyecto, la **gravedad** es una fuerza global que afecta a todas las cenizas por igual, mientras que el **mouse** actúa como una fuerza local que solo afecta a las partículas en su radio de cercanía.

### 10. Independencia de la representación visual
El motor físico y algorítmico es independiente del dibujo. El cálculo de movimiento (física de vectores) seguiría siendo el mismo si en lugar de círculos usáramos imágenes, líneas o modelos 3D en el método `display`.

---

## Parte 2 — Transferencia a otra herramienta (Unity / Blender)

Si decidiera recrear la obra **"Polvo Eres"** en un motor como **Unity** (VFX Graph) o un software como **Blender** (Geometry Nodes), se presentarían los siguientes cambios y permanencias:

### ¿Qué se mantendría IGUAL?
* **La Lógica de Fuerzas:** El uso de un vector de gravedad para atraer la materia al suelo y un "atractor" (el cursor) para elevarla.
* **El Ciclo de Vida:** La regla de que cada entidad debe destruirse al cumplir un tiempo de vida o llegar a una altura específica para liberar recursos.
* **La Narrativa de Interacción:** La idea central de transformar materia inerte en energía ascendente a través de la intervención del usuario.

### ¿Qué CAMBIARÍA?
* **El Lenguaje y Herramientas:** Pasaría de escribir código en **JavaScript/p5.js** a usar **C#** (en Unity) o sistemas de **nodos visuales** (en Blender).
* **La Dimensión del Espacio:** El proyecto pasaría de coordenadas 2D $(x, y)$ a un entorno 3D $(x, y, z)$, permitiendo que la lluvia de cenizas tenga profundidad y volumen.
* **Capacidad de Renderizado:** En Unity podría usar *Shaders* avanzados para que las flamas emitan luz real y tengan efectos de post-procesamiento como el *Bloom* (brillo intenso).

### Elementos independientes de la herramienta
El diseño del **comportamiento** es totalmente independiente. La tasa de emisión de la lluvia, la velocidad de ascenso de las flamas y la decisión estética de que la muerte sea un estallido visual son decisiones de **Diseño de Experiencia** que trascienden el código y pueden aplicarse en cualquier software.

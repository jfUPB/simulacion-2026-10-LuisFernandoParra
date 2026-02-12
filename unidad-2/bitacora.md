# Unidad 2

## Bitácora de proceso de aprendizaje
### Actividad 2
- si se refiere a como se suman pues componente a componente y si es en progrmacion se llama a la funcion add y dentro se la de el ootro vector el cual se desea sumar
- no funciona porque se estan sumando como si fueran numeros normales y se tiene que hacer es con la funcion add


### Actividad 4
- la verdad no sabia que esperar
- que los vectores cambiaron de componentes en x y en y
- es un paso por referencia
- aprendi a como manipular y cambiar el valor de los vectores de forma permanente y de forma rapida y facil con la funcion playing vector

### Actividad 7
- Concepto e interpretación geométrica:
El marco Motion 101 es el algoritmo fundamental de la física digital que establece una jerarquía de movimiento basada en la acumulación: la aceleración modifica la velocidad y la velocidad, a su vez, modifica la posición. Geométricamente, se interpreta como una suma sucesiva de vectores, donde la posición es un punto en el espacio que se desplaza mediante la suma del vector velocidad (una flecha de dirección y magnitud), el cual es constantemente alterado en su forma por el vector aceleración (un empuje externo).

- Aplicación en el ejemplo:
En el ejemplo de la clase Mover, este marco se aplica de manera cíclica dentro del método update() mediante tres pasos técnicos: primero, se usa velocity.add(acceleration) para que el motor del objeto (aceleración) cambie su rapidez o dirección; segundo, se aplica velocity.limit(topSpeed) para actuar como un regulador físico que impide que el objeto acelere infinitamente; y finalmente, se ejecuta position.add(velocity) para trasladar visualmente el objeto a su nueva ubicación en el lienzo de p5.y basándose en la velocidad resultante.
## Bitácora de aplicación 
### ACtividad 9
-mi obra lo que busca representar es algo mas psicologico las bolas en este caso representan los problemas de la vida y el mouse nos representa a nosotros nosotros en esta obra podemos hacer dos cosas como los seres humanos en la vida real podemos desplazar el mouse a diferentes partes del lienzo que representa basicamente huir de nuestros problemas el problema es que como vemos ellos siempre nos van a perseguir a donde vayamos como en la vida real mas sin embargo tambien tenemos otra opcion que es enfrentarlos que se manifiesta cuando ya dejamos de desplazarnos si no que decidimos presionar al mouse lo cual es un papel mucho mas activo y cuando eso pasa como sucede en la vida real las bolas que representan nuestros problemas se alejan precisamente la regla que aplique de aceleracion se basa basicamente en calcular la direccion entre dos puntos no es una aceleracion como la gravedad que va hacia abajo si no que en vez de eso es van hacia un punto o un objetivo que en este caso es el mouse
``` js
let partículas = [];
let cantidad = 100; 
function setup() {
  createCanvas(windowWidth, windowHeight);
 
  for (let i = 0; i < cantidad; i++) {
    partículas.push(new Mover());
  }
}

function draw() {

  background(20, 20, 30, 50);

  for (let p of partículas) {
    p.update();
    p.checkEdges();
    p.show();
  }
}

class Mover {
  constructor() {
 
    this.position = createVector(random(width), random(height));
    
    this.velocity = p5.Vector.random2D();
    this.velocity.mult(random(2, 5));

    this.acceleration = createVector(0, 0);
    this.topSpeed = 6; 
    this.size = random(4, 12);
  }

  update() {
    
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.position);

  
    dir.normalize(); 
    

    if (mouseIsPressed) {
      dir.mult(-0.5); 
    } else {
      dir.mult(0.2); 
    }

    this.acceleration = dir;

    
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
  }

  show() {
    noStroke();
   
    let speedColor = map(this.velocity.mag(), 0, this.topSpeed, 100, 255);
    fill(speedColor, 100, 255, 150);
    circle(this.position.x, this.position.y, this.size);
  }

  checkEdges() {
    
    if (this.position.x > width || this.position.x < 0) {
      this.velocity.x *= -1;
    }
    if (this.position.y > height || this.position.y < 0) {
      this.velocity.y *= -1;
    }
  }
}
``
```
https://editor.p5js.org/LuisFernandoParra/full/5zKQQ4lGn


## Bitácora de reflexión


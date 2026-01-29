# Unidad 1

## Bitácora de proceso de aprendizaje
### Actividad 1
La aleatoriedad en el arte generativo mete un poco de caos creativo que hace que cada pieza salga distinta y sorprendente, incluso para quien la crea.
### Actividad 2
* Lo que modifique fueron los patrones de x++ y de y++, y lo cambie por valores negativos.
* lo que espero que suceda es que la linea vaya hacia abajo y a la izquierda hasta salir del lienzo
* la linea si se movio en la direccion izquierda hasta salir del lienzo en direccion hacia arriba
* En parte si ocurrio lo que esperaba ya que la linea si se movio a la izquierda mas sin embargo en vez de hacerlo hacia abajo fue hacia arriba y esto ocurrio porque me confundi con el valor cartesiano en "y del ++ y del --" 
### Actividad 3
* que una distribucion uniforme todos los numeros van saliendo por igual pues todos los numeros tienen la misma probabilidad de salir mientras que una distribucion no uniforme es que hay un numero o una cantidad de numeros que tienden a salir mucho mas que otros
*
```

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 0) {
      this.x--;
    } else if (choice == 0) {
      this.y++;
    } else {
      this.y--;
    }
  }
}
```
### Actividad 4
*
```
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

function setup() {
  createCanvas(640, 240);
  background(200);
}

function draw() {
  //{!1} A normal distribution with mean 320 and standard deviation 60
  let x = randomGaussian(320, 20);
  let y = randomGaussian(120, 10);
  noStroke();
  fill(0, 10);
  circle(x, y, 16);
}
```
* https://editor.p5js.org/LuisFernandoParra/sketches/Sp3iwLRWh


 <img width="1064" height="479" alt="image" src="https://github.com/user-attachments/assets/5cb62ed5-4ffc-45c6-aeef-c11b648cad0f" />

### Activudad 5
*use esta tecnica ya que espero poder simular que la maquina en P5 este escribiendo su propio lienzo como si fuera un humano trazando cosas al azar y que de pronto pueda construir algo interesante
*
```
let walker;

function setup() {
  createCanvas(640, 240);
  // Creating the Walker object!
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const r = random(1);
    // A 40% of moving to the right!
    if (r < 0.4) {
      this.x++;
    } else if (r < 0.6) {
      this.x--;
    } else if (r < 0.8) {
      this.y++;
    } if (r < 0.1){
      this.x = random(-100, 100);
      this.y = random(-100, 100);  
    } else {
      this.y--;
    }
    this.x = constrain(this.x, 0, width - 1);
    this.y = constrain(this.y, 0, height - 1);
  }
}
```
* https://editor.p5js.org/natureofcode/full/iAjs_70DF
 <img width="197" height="197" alt="image" src="https://github.com/user-attachments/assets/62b453ee-2148-46ff-8c05-076c70e4b0e3" />

<img width="555" height="593" alt="image" src="https://github.com/user-attachments/assets/36eb362b-02c0-4fe0-9ee1-353d6e637429" />

### Actividad 6
* Lo que espero es que suceda es generar un circulo el cual de la ilusion que tiene vida o que se mueve por si solo espero principalmente que solo se mueva de izquierda a derecha
```js
let t = 0;

function setup() {
  createCanvas(400, 200);
}

function draw() {
  background(220);
  let n = noise(t);
  let x = map(n, 0, 1, 0, width);
  circle(x, height / 2, 20);
  t += 0.01;
}
```
* https://editor.p5js.org/LuisFernandoParra/full/WM58ZSl2Z
* <img width="899" height="311" alt="image" src="https://github.com/user-attachments/assets/0f3823cd-e292-4c0f-8642-6a7bf0e2f7bc" />
## Bitácora de aplicación 
### ACTIVIDAD 7
* Este diseño genera una composición interactiva donde un punto se desplaza continuamente por el lienzo siguiendo un movimiento suave controlado por ruido de Perlin; su tamaño varía de forma natural mediante una distribución gaussiana, concentrándose la mayoría de las veces alrededor de un valor medio. Al presionar cualquier tecla, el sistema introduce un salto largo inspirado en una caminata tipo Lévy, alterando de manera puntual la trayectoria y permitiendo que el espectador intervenga activamente en la generación de la obra en tiempo real.
 ``` JS
  let x, y;
let tx = 0;
let ty = 1;

function setup() {
  createCanvas(600,600 );
  background(20);
  x = width / 2;
  y = height / 2;
  noStroke();
}

function draw() {
 
  let nx = noise(tx);
  let ny = noise(ty);
  
  x = map(nx, 0, 1, 0, width);
  y = map(ny, 0, 1, 0, height);

  
  let diametro = randomGaussian(15, 5);

  fill(100, 150, 255, 150);
  circle(x, y, diametro);

 
  tx += 0.005;
  ty += 0.005;
}

function keyPressed() {
 
  let magnitud = 200; 
  tx += magnitud;
  ty += magnitud;
  
 
}
```
* https://editor.p5js.org/natureofcode/full/O7PsvcpQ3
<img width="1054" height="520" alt="image" src="https://github.com/user-attachments/assets/f93a62dc-bc6b-4347-8ed7-76a1e4363b41" />


## Bitácora de reflexión
### Actividad 8
* La diferencia fundamental entre random() y el Ruido Perlin (noise()) es la relación entre los valores que generan.
random() produce números completamente independientes, por lo que cada valor es impredecible y genera cambios bruscos. En cambio, noise() genera valores que varían de forma continua, haciendo que la aleatoriedad se perciba más suave y orgánica.

Usaría random() cuando necesito resultados caóticos o cambios repentinos, y noise() cuando busco movimientos fluidos o simulaciones que se asemejen a fenómenos naturales.



* Una distribución de probabilidad indica qué tan probable es que ocurra cada posible resultado dentro de un conjunto de valores.

En una caminata con distribución uniforme, todas las direcciones o valores tienen la misma probabilidad, lo que visualmente produce trayectorias más dispersas y desordenadas.
En cambio, una caminata con distribución normal concentra la mayoría de los valores alrededor de un promedio, lo que genera visualmente agrupaciones y movimientos más controlados, con menos extremos.



* La aleatoriedad cumple un papel fundamental en el arte generativo porque permite que cada resultado sea único y no completamente predecible.
Además, introduce variación visual y ayuda a simular comportamientos naturales, logrando un equilibrio entre control del autor y resultados emergentes.

* En mi obra utilicé el Ruido Perlin (noise()) como concepto principal de aleatoriedad para generar el movimiento de los elementos en el lienzo. Elegí este tipo de aleatoriedad porque permite transiciones suaves y continuas, lo que hace que el recorrido del punto se perciba orgánico y no caótico. A diferencia del azar puro, el Ruido Perlin mantiene una relación entre los valores, lo que ayudó a construir una trayectoria coherente y visualmente fluida, acorde al efecto que buscaba en la obra.


* Una caminata o “walk” en el contexto de la simulación es un proceso en el que un elemento se desplaza paso a paso siguiendo reglas probabilísticas.
La caminata tipo Lévy flight se caracteriza por la presencia de muchos pasos cortos y, ocasionalmente, saltos largos e inesperados, lo que genera patrones con concentraciones y desplazamientos abruptos, similares a los que se observan en la naturaleza.












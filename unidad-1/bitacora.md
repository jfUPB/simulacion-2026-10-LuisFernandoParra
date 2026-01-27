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
## Actividad 4
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

## Activudad 5
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


## Bitácora de aplicación 



## Bitácora de reflexión







# Unidad 1

## Bitácora de proceso de aprendizaje

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
  
## Bitácora de aplicación 



## Bitácora de reflexión

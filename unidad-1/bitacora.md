# Unidad 1

## Bitácora de proceso de aprendizaje

### Actividad 01
La aletoriedad influye en la creación de secuencias lo que permite variabilidad y resultados diferentes.

### Actividad 02

**Resultados que espero**
Creo que va a crear un punto en la mitad del canva, después de eso avanzará según el número random en el espacio ya sea arriba, abajo, izquierda o derecha.


```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;                    //Se crea la variable

function setup() {            //Se crea el espacio
  createCanvas(640, 240);
  walker = new Walker();     //Se crea una nueva instancia
  background(255);
}

function draw() {          //Bucle
  walker.step();           //Mueve el objeto
  walker.show();           //Crea la línea
}

class Walker {           //Se crea la clase 
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {            //Se mostrará un punto en la posición establecida
    stroke(0);
    point(this.x, this.y);
  }

  step() {          //Se crea una constante que varía entre el 0 y el 3 y según el valor que tome cambiará en el plano cartesiano la posición
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;    //Derecha si el número aleatorio es 0
    } else if (choice == 1) {
      this.x--;    //Izquierda si el número aleatorio es 1
    } else if (choice == 2) {
      this.y++;   //Arriba si el número aleatorio es 2
    } else {
      this.y--;  //Abajo el número aleatorio es 3
    }
  }
}
```

**Lo que sucede**
<img width="629" height="231" alt="image" src="https://github.com/user-attachments/assets/b4240dbd-f593-469f-93f3-66d1e49034f9" />


**¿Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?**

Si ocurrió lo que esperaba porque se observa como se crea un punto y una línea que va "caminando" alrededor del canva. Aún así me di cuenta que me confundí al explicar el código y no lo comprendí completamente. Cuando se hace this.y++ en realidad va hacia abajo y con this.y-- va hacia arriba porque sube en píxeles. También  

### Actividad 03

**Diferencia entre una distribución uniforme y una no uniforme de números aleatorios**

**Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.**

## Bitácora de aplicación 



## Bitácora de reflexión


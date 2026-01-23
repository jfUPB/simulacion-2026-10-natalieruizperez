# Unidad 1

## Bitácora de proceso de aprendizaje

# Pendiente
-Actividad 03 explicación
-Actividad 04 explicación
-Actividad 05 tarea hacer una distribución inventada con saltos

### Actividad 01
La aletoriedad influye en la creación de secuencias lo que permite variabilidad y resultados diferentes.

### Actividad 02

**Resultados que espero**
Creo que si sale el 0 va a ir en línea recta hacia la derecha porque va a tomar únicamente el primer argumento de if, si sale 1 irá hacia la izquierda o arriba y si sale 2 o 3 irá hacia arriba.


```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(0);
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
    stroke(floor(random(255)),floor(random(255)),floor(random(255)));
    square(this.x, this.y,10);
    fill(255,0,128)
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 0) {
      this.y++;
    } else {
      this.y--;
    }
  }
}

```

**Lo que sucede**

<img width="629" height="233" alt="image" src="https://github.com/user-attachments/assets/954e8adc-2ba8-4bde-81cf-3ab00a78fd0d" />



**¿Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?**

No ocurrió lo que esperaba porque en la foto se observa como el cuadrado serpentea hacía arriba de de derecha a izquierda por lo que puedo concluir que no hace únicamente la primera línea de if si no que a su vez trabaja con todas las demás que tengan else if y cumplan con la condición. En el resultado obtuve que pareciera que se va por igual tanto a izquierda como derecha.


### Actividad 03

**Diferencia entre una distribución uniforme y una no uniforme de números aleatorios**
Una distribución normal son las que parecen unas montañas y los valores se aglomeran alrededor de la punta de la montaña que sería la media. Una distribución uniforme es cuando todos los números tienen la misma probabilidad de salir y no uniforme que hay unos más probables que otros. randomGaussian() es una distribución no uniforme porque hay más probabilidad que los valores estén cerca de la media.

**Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.**
En el código considero que favorece el movimiento hacia la derecha porque ya hay 3/5 probabilidades de que vaya hacia la derecha y los 2/5 restantes queda para los demás movimientos
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(0);
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
    stroke(floor(random(255)),floor(random(255)),floor(random(255)));
    square(this.x, this.y,10);
    fill(255,0,128)
  }

  step() {
    const choice = floor(random(6));
    if (choice == 1 | choice == 2 | choice == 3) {
      this.x++;
    } else if (choice == 4) {
      this.x--;
    } else if (choice == 4) {
      this.y++;
    } else {
      this.y--;
    }
  }
}

```

### Actividad 04
Creo que al crear la variable y voy a poder mover el cuadrardo por la pantalla y estará la mayor parte en el centro, entonces la media sería poca pero la desviación estandár mucha entonces espero que esté distribuída por casi todo el canva en y que en el centro sea más oscuro

**Sketch en p5.js que represente una distribución normal**
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  //{!1} A normal distribution with mean 320 and standard deviation 60
  let x = randomGaussian(320, 60);
  let y = randomGaussian(60, 320);
  noStroke();
  fill(0, 10);
  square(x, y, 16);
}

```
**Enlace**
https://editor.p5js.org/natalieruizperez/sketches/Qg17275qR

**Captura de pantalla**
<img width="624" height="237" alt="image" src="https://github.com/user-attachments/assets/a4e720f5-c5b1-46dc-b926-2da9d77b4cb8" />

**Análisis de resultados**


### Actividad 05

## Bitácora de aplicación 



## Bitácora de reflexión
En tu bitácora de aprendizaje. Responde con tus propias palabras a las siguientes preguntas.

**1. Describe la diferencia fundamental entre la aleatoriedad generada por random() y la apariencia de aleatoriedad del Ruido Perlin (noise()). ¿En qué tipo de situación usarías cada una?**
**2. Explica con tus palabras qué es una distribución de probabilidad. ¿Qué diferencia visual produce una caminata aleatoria con una distribución uniforme versus una con una distribución normal?**
**3. ¿Cuál es el papel de la aleatoriedad en el arte generativo? Menciona al menos dos funciones distintas que cumple**
**4. Piensa en tu obra final (Actividad 07). Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas.**
**5. ¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?**




# Unidad 1

## Bitácora de proceso de aprendizaje




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
En el código considero que favorece el movimiento hacia la derecha porque ya hay más probabilidad que ha que haga los demás movimientos.
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
    const choice = floor(random(5));
    if (choice == 1 || choice == 2 || choice == 3) {
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

Creo que al crear la variable voy a poder mover el cuadrardo por la pantalla y estará la mayor parte en el centro, entonces la media sería poca pero la desviación estandár más grande entonces espero que esté distribuída por casi todo el canva en y que en el centro sea más oscuro

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



### Actividad 05

**Análisis**

Creo que mantendra la línea recta de puntos que se concentran más en la mitadp pero que se crearán aleatoriamente puntos alrededor.

**Código**

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
 
  let x = randomGaussian(320, 60);
  let y = 120;

  
  let r = random(1);
  let xstep, ystep;

  if (r < 0.5) { 
    xstep = random(-100, 100);
    ystep = random(-100, 100);
  } else { // 99% pasos pequeños
    xstep = random(-1, 1);
    ystep = random(-1, 1);
  }

  let newX = constrain(x + xstep, 0, width);
  let newY = constrain(y + ystep, 0, height);


  noStroke();
  fill(0, 10);
  circle(newX, newY, 16);
}
```

**Captura de patalla** 

<img width="626" height="234" alt="image" src="https://github.com/user-attachments/assets/7ea1cd5e-dabc-4596-9090-f20b13bb43a3" />

**Enlace**

https://editor.p5js.org/natalieruizperez/sketches/eXLHRUD7x

### Actividad 06

**Código**

```js
let t = 0.0;

function setup() {
  createCanvas(360, 240);
}

function draw() {
  background(0); 
  let xoff = t;

  let prevY = noise(xoff) * height;
  let prevX = 0;
  xoff += 0.05;

  for (let i = 1; i < width; i++) {
    let y = noise(xoff) * height;
    xoff += 0.05;
   
    let base = map(y, 0, height, 5, 1);
    
    let pulse = map(noise(t * 5), 0, 1, -3, 3);
    
    let thick = base + pulse;

    strokeWeight(thick);
    stroke(0, 255, 0); 

    line(prevX, prevY, i, y);

    prevX = i;
    prevY = y;
  }

  t += 0.02; 
}
```

**Enlace**
https://editor.p5js.org/natalieruizperez/sketches/bBVl2oDB2

**Captura**

<img width="347" height="232" alt="image" src="https://github.com/user-attachments/assets/36fce3b9-8ee2-4cb7-9bc4-53014025359d" />

<img width="349" height="238" alt="image" src="https://github.com/user-attachments/assets/e1a41a4b-9921-468b-99b1-b18ed4f61dcd" />


## Bitácora de aplicación 

**Análisis**
Mi idea es hacer una onda como las que se ven en los aparato médicos y que se pudiera controlar al mover el mouse. Pense que podria lograr esto usando un walker que fuera en linea recta horizontal y después agregarle noise. Para que el resto de canva no se viera vacio agregúe el concepto de levy flight y asi de vez en cuando saltarian puntos aleaotorios.


**Código**
```js
let walker;
let hueValue = 0;
let tx = 0; 

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(0);
  colorMode(HSB, 360, 100, 100, 1);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = 0;
    this.y = height / 2;
  }

  show() {
    noStroke();
    fill(hueValue, 80, 100, 0.3);
    circle(this.x, this.y, 16);
  }

  step() {
    
    let noiseSpeed = map(mouseY, 0, height, 0.002, 0.05);

    
    let xstep = map(noise(tx), 0, 1, 1, 4); 
    tx += noiseSpeed;
    this.x += xstep;

   
    let amp = map(mouseY, 0, height, 5, 80);
    this.y = height / 2 + map(noise(tx + 1000), 0, 1, -amp, amp);

    
    if (random(1) < 0.1) { 
      let px = this.x + random(-50, 50); 
      let py = this.y + random(-50, 50); 
      noStroke();
      fill(hueValue, 80, 100, 0.3);
      circle(px, py, 16);
    }

    
    if (this.x > width) {
      this.x = 0;
    }
  }
}


function mousePressed() {
  hueValue = random(360);
}


function keyPressed() {
  if (key === ' ') {  
    background(0);     
    walker.x = 0;      
  }
}
```

**Enlace**
https://editor.p5js.org/natalieruizperez/sketches/XrpMOkONB


**Captura**

<img width="623" height="235" alt="image" src="https://github.com/user-attachments/assets/2f779b76-f32c-436f-a9d6-ca138779bda1" />


## Bitácora de reflexión

**1. Describe la diferencia fundamental entre la aleatoriedad generada por random() y la apariencia de aleatoriedad del Ruido Perlin (noise()). ¿En qué tipo de situación usarías cada una?**

La diferencia fundamental entre aletoriedad y ruido perlin es que cuando se usa la función random() esta puede crear saltos repentinos de un lugar a otro, en cambio perlin noise es más suave. Si yo fuera a hacer un terreno o una visual que no puede tener cambios bruscos usaría ruido perlin porque permite una transcición suave, en cambio si quisiera generar patrones de forma aleatoriea sin ninguna relación con el otro como por ejemplo puntos en la pantalla usaría random()

**2. Explica con tus palabras qué es una distribución de probabilidad. ¿Qué diferencia visual produce una caminata aleatoria con una distribución uniforme versus una con una distribución normal?**

Una distribución de probabilidad es aquella que dice que tan seguido puede cambiar la visuaol. Una caminata alatoria con una distribución uniforme variaría y tendría la misma probabilidad como su nombre lo indica para que ocurra el salto, en cambio con una distribución normal es parecida a una gaussiana en donde los elementos se van a concentrar alrededor de la media y tendrán una desviación controlada según los parámetros que se pongan.

**3. ¿Cuál es el papel de la aleatoriedad en el arte generativo? Menciona al menos dos funciones distintas que cumple**

El papel de aletoriedad en el arte generativo es muy importante porque hace que se puedan crear obras únicas y variantes lo que permite que el creador pueda concentrarse en manejar otros parámetros para crear el efecto que desea. El arte generativo a veces puede dar la sensación de ser muy complejo por tener visuales impactantes automatizadas. Se puede lograr por ejemplo con random(), randomGaussian() y también noise()

**4. Piensa en tu obra final (Actividad 07). Describe uno de los conceptos de aleatoriedad que usaste y explica por qué fue una elección adecuada para lograr el efecto que buscabas.**

Al hacer la actividad 07 noté que se veía muy vacía la línea el centro y que no se veía tan mágico así que agregué levy flight para que tuviese cierta probabilidad de hacer un salto, así habría más variedad.

**5. ¿Qué es un “paseo” o “caminata” (walk) en el contexto de la simulación? ¿Qué característica particular tiene una caminata de tipo “Lévy flight”?**

Una caminata en el contexto de simulación es una variable que crea números aleatorios y según el número que salga avanza en una direcció9n específica. La diferencia con levy flight es que cuando se habla de caminata es en línea junta y lo que varía es la dirección, en cambio en la otra se generan saltos más bruscos.









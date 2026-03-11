# Unidad 4

## Bitácora de proceso de aprendizaje

### Actividad 01

<img width="1099" height="581" alt="image" src="https://github.com/user-attachments/assets/51a1f278-c3f3-484d-ae0d-a0b0d4262c93" />

<img width="1098" height="600" alt="image" src="https://github.com/user-attachments/assets/e87f8416-85e6-4dfc-bdde-33a00b475963" />

<img width="1100" height="619" alt="image" src="https://github.com/user-attachments/assets/30a1ad6a-a638-4a97-999d-59f25d5dfa18" />

Lo que más me llamo la atención es el uso de ondas y sonidos para llegar al espectador. Además da la sensación de 3d en algunas de sus obras y yo soy amante del modelado 3d entonces no puedo evitar pensar en como logra crear esas ondas en el agua que se ven tan mágicas.

### Actividad 02

**¿Qué está pasando en esta simulación? ¿Cuál es la interacción?**

Al presionar las tecla se van rotando los elementos gráficos (círculos y líneas).


**Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?**
Creo que se usan puntos fijos porque ese 

**Cuál es la relación entre el sistema de coordenadas y la función rotate().**

Las función rotate modifica las coordenadas para que si pueda rotar, entonces traslada el sistema de coordenadas y luego lo rota.

**Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?**

Se actualiza la posición usando motion 101 pero sin fuerzas acumulativas. Se integra la velocidad para encontrar la posicición y se integra la velocidad para encontrar la aceleración.

**¿Qué hace la función heading()?**

La función heading se encarga de 

**¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.**

La función push y pop

**¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.**
**¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.**

### Actividad 03




### Actividad 04

**Identifica motion 101. ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas? Trata de recordar por qué es necesario hacer esta modificación.**

Se toma la velocidad y se entegra para obtener el ángulo y se entegra la aceleración angular para obtener la velocidad.


**Identifica dónde está el Attractor en la simulación. Cambia el color de este.**

El attractor es la bola grande del centro.

**Observa que el Attractor tiene dos atributos this.dragging y this.rollover. Estos atributos no se modifican en el código, pero permitirían mover el attractor con el mouse y cambiar su color cuando el mouse está sobre él.¿Cómo podrías modificar el código para que esto funcione? considera las funciones que ofrece p5.js para interactuar con el mouse.**

### Actividad 05

Observa de nuevo esta parte del código ¿Cuál es la relación entre r y theta con las posiciones x y y? Puedes repasar entonces la definición de coordenadas polares y cómo se convierten a coordenadas cartesianas.


### Activdad 06

**Reflexión**

La función sinusoide es la que representa el seno, t es la variable independiente, a es la amplitud de la onda punto máximo y mínimo. W es la frecuencia angular y esta relacionada con la frecuencia entonces si es muy grande se vería más de dos picos y si es pequeña menos de dos picos. Un ciclo es cada cuanto se repite la señal entonces tiene que pasar dos veces por 0. Cuando el tiempo es 0 y la fase uno entonces el seno empezaría en otr aparte entonces como que se desplazaría.

```js
let period1 = 100;
let period2 = 120;

let amplitude = 150;
let phase = 0;

function setup() {
  createCanvas(640, 240);
  // phase = TWO_PI/8;
  
}

function draw() {
  
  let y = amplitude * sin( ((TWO_PI * frameCount) / period1));
  let x2 = amplitude * sin( ((TWO_PI * frameCount) / period2) + phase);

  stroke(0);
  strokeWeight(2);
  translate(width / 2, height / 2);

  fill(0,0,255);

  circle(0, y, 48);
  
  
  fill(255,0,0);
  circle(x2, 0, 48);  
}

function keyPressed(){
  phase += TWO_PI/360;  
}
```

### Actividad 07



## Bitácora de aplicación 

Sketch
```
let startAngle = 0;
let angleVelocity = 0.2;

let movers = [];
let liquid;

function setup() {
  createCanvas(640, 240);

  liquid = new Liquid(0, height / 2, width, height / 2, 0.1);

  for (let x = 0; x <= width; x += 24) {
    movers.push(new Mover(x, height / 4, random(1, 2)));
  }
}

function draw() {
  background(255);

  liquid.show();

  let angle = startAngle;
  startAngle += 0.02;

  for (let i = 0; i < movers.length; i++) {

    let mover = movers[i];

    // fuerza que sigue la onda
    let targetY = map(sin(angle), -1, 1, 0, height);
    let waveForce = createVector(0, targetY - mover.position.y);
    waveForce.mult(0.02);
    mover.applyForce(waveForce);

    if (liquid.contains(mover)) {
      let dragForce = liquid.calculateDrag(mover);
      mover.applyForce(dragForce);
    }

    let gravity = createVector(0, 0.05 * mover.mass);
    mover.applyForce(gravity);

    mover.update();
    mover.show();
    mover.checkEdges();

    angle += angleVelocity;
  }
}
```

Liquid
```
class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c;
  }

  contains(mover) {
    let pos = mover.position;
    return (
      pos.x > this.x &&
      pos.x < this.x + this.w &&
      pos.y > this.y &&
      pos.y < this.y + this.h
    );
  }

  calculateDrag(mover) {
    let speed = mover.velocity.mag();
    let dragMagnitude = this.c * speed * speed;

    let dragForce = mover.velocity.copy();
    dragForce.mult(-1);
    dragForce.setMag(dragMagnitude);

    return dragForce;
  }

  show() {
    noStroke();
    fill(200, 220, 255, 150);
    rect(this.x, this.y, this.w, this.h);
  }
}
```

Mover
```
class Mover {
  constructor(x, y, mass) {
    this.mass = mass;
    this.radius = mass * 8;

    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127, 127);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  checkEdges() {
    if (this.position.y > height - this.radius) {
      this.velocity.y *= -0.9;
      this.position.y = height - this.radius;
    }
  }
}
```


## Bitácora de reflexión






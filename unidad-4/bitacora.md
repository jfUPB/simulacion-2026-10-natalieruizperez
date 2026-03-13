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

Bob
```
class Bob {
  constructor(x, y, m) {
    this.position = createVector(x, y);
    this.velocity = createVector();
    this.acceleration = createVector();
    this.mass = m;
    this.radius = m * 8;

    this.color = color(random(50, 255), random(50, 255), random(50, 255));
    this.dragging = false;
    this.dragOffset = createVector();
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    if (!this.dragging) {
      this.velocity.add(this.acceleration);
      this.position.add(this.velocity);
    } else {
      // Si está siendo arrastrado, se mueve naturalmente hacia la posición del cursor con fuerza
      this.velocity.add(this.acceleration);
      this.position.add(this.velocity);
    }
    this.acceleration.mult(0);
  }

  show() {
    noStroke();
    fill(this.color);
    if (this.dragging) fill(200);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  handleClick(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.radius) {
      this.dragging = true;
      this.dragOffset.x = this.position.x - mx;
      this.dragOffset.y = this.position.y - my;
    }
  }

  stopDragging() {
    this.dragging = false;
  }

  handleDrag(mx, my) {
    if (this.dragging) {
      this.position.x = mx + this.dragOffset.x;
      this.position.y = my + this.dragOffset.y;
    }
  }
}
```

Sketch
```
let bobs = [];
let springs = [];
let center;
let sunMass = 40; // aún se usa para gravedad
let G = 1;
let numBobs = 10;

function setup(){
  createCanvas(800,500);
  center = createVector(width/2,height/2);

  for(let i=0;i<numBobs;i++){
    let angle = random(TWO_PI);
    let radius = random(60,150);
    let x = center.x + cos(angle)*radius;
    let y = center.y + sin(angle)*radius;

    let b = new Bob(x,y,random(1.5,3));

    // Velocidad tangencial inicial para órbita estable
    let distance = p5.Vector.sub(b.position,center).mag();
    let speed = sqrt(G*sunMass/distance);
    let tangent = createVector(-(y-center.y),(x-center.x)).normalize();
    tangent.mult(speed);
    b.velocity = tangent;

    bobs.push(b);
    springs.push(new Spring(center.x,center.y,radius));
  }
}

function draw(){
  background(0);

  for(let i=0;i<bobs.length;i++){
    let bob = bobs[i];

    // gravedad central
    let force = p5.Vector.sub(center,bob.position);
    let distance = force.mag();
    distance = constrain(distance,20,500);
    let strength = (G*sunMass*bob.mass)/(distance*distance);
    force.setMag(strength);
    bob.applyForce(force);

    // resorte más rígido
    springs[i].connect(bob);

    bob.update();
    bob.show();
    springs[i].showLine(bob);
  }
}

// Interactividad
function mousePressed(){
  for(let bob of bobs) bob.handleClick(mouseX,mouseY);
}

function mouseReleased(){
  for(let bob of bobs) bob.stopDragging();
}

function mouseDragged(){
  for(let bob of bobs) bob.handleDrag(mouseX,mouseY);
}
```

Spring
```
class Spring {
  constructor(x, y, length){
    this.anchor = createVector(x,y);
    this.restLength = length;
    this.k = 0.2;
    this.damping = 0.9;
    this.numPoints = 100; // puntos para la onda
    this.values = [];

    // Inicializar valores aleatorios para la onda con amplitud controlada
    for(let i = 0; i < this.numPoints; i++){
      this.values[i] = random(-10, 10); // ajusta estos valores para menos/más amplitud
    }
  }

  connect(bob){
    let force = p5.Vector.sub(bob.position, this.anchor);
    let stretch = force.mag() - this.restLength;
    force.setMag(-this.k * stretch);

    let velAlongSpring = p5.Vector.dot(bob.velocity, force.copy().normalize());
    let dampingForce = force.copy().normalize().mult(-velAlongSpring * (1 - this.damping));
    force.add(dampingForce);

    bob.applyForce(force);
  }

  showLine(bob){
    stroke(bob.color);
    strokeWeight(2);

    let dir = p5.Vector.sub(bob.position, this.anchor);
    let length = dir.mag();
    let unitDir = dir.copy().normalize();

    let perp = createVector(-unitDir.y, unitDir.x);

    beginShape();
    for(let i = 0; i < this.numPoints; i++){
      let t = i / (this.numPoints - 1);
      let pos = p5.Vector.add(this.anchor, p5.Vector.mult(unitDir, length * t));

      let idx = (i + frameCount) % this.numPoints;
      let offsetMag = this.values[idx];

      let offset = perp.copy().mult(offsetMag);

      vertex(pos.x + offset.x, pos.y + offset.y);
    }
    endShape();
  }
}
```

**Enlace**

https://editor.p5js.org/natalieruizperez/sketches/lrFESDjRG

**Capturas**


## Bitácora de reflexión








# Unidad 4

## Bitácora de proceso de aprendizaje

### Actividad 02

**¿Qué está pasando en esta simulación? ¿Cuál es la interacción?**

Al presionar las tecla se van rotando los elementos gráficos (círculos y líneas).

**Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?**

Creo que se usan puntos fijos para que la rotación ocurra en el centro.

**Cuál es la relación entre el sistema de coordenadas y la función rotate().**

Las función rotate modifica las coordenadas para que si pueda rotar, entonces traslada el sistema de coordenadas y luego lo rota.

**Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?**

Se actualiza la posición usando motion 101 pero sin fuerzas acumulativas. Se integra la velocidad para encontrar la posicición y se integra la velocidad para encontrar la aceleración.

**¿Qué hace la función heading()?**

La función heading se encarga de devolver el ángulo.

**¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.**

La función push guarda y la pop restaura.
 
**¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.**

Es para que el triángulo quede en el centro.

**¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.**

El ángulo representa hacia donde se va a mover la velocidad.


### Actividad 04

**Identifica motion 101. ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas? Trata de recordar por qué es necesario hacer esta modificación.**

Se toma la velocidad y se entegra para obtener el ángulo y se entegra la aceleración angular para obtener la velocidad.


**Identifica dónde está el Attractor en la simulación. Cambia el color de este.**

El attractor es la bola grande del centro y para que cambie de color habría que modificar el fill.

**Observa que el Attractor tiene dos atributos this.dragging y this.rollover. Estos atributos no se modifican en el código, pero permitirían mover el attractor con el mouse y cambiar su color cuando el mouse está sobre él.¿Cómo podrías modificar el código para que esto funcione? considera las funciones que ofrece p5.js para interactuar con el mouse.**

Se podría usar mousePressed() y mouseReleased(). Cuando el mouse esté sobre el attractor se activa rollover, y al presionar se activa dragging para moverlo.

### Actividad 05

Observa de nuevo esta parte del código ¿Cuál es la relación entre r y theta con las posiciones x y y? Puedes repasar entonces la definición de coordenadas polares y cómo se convierten a coordenadas cartesianas.

r es la distancia y theta el ángulo. Para convertirla seria usar la fórmula x = rcos(theta) y así se podrá obtener la posición.

**Modifica la función draw(): ¿Qué ocurre? ¿Por qué?**

El punto se mueve alrededor del centro. Eso ocurre porque crea un vector con magnitud 1.

**Ahora realiza esta modificación: ¿Qué ocurre aquí? ¿Por qué?**

El punto se mueve más lejos o más cerca del centro. Esto pasa porque usa r como magnitud del vector, entonces la distancia al origen cambia según el valor de r.


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


### Actividad 08

```js
let startAngle = 0;
let angleVelocity = 0.2;

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);

  let angle = startAngle;
  startAngle += 0.02;

  for (let x = 0; x <= width; x += 24) {
    let y = map(sin(angle), -1, 1, 0, height);
    stroke(0);
    strokeWeight(2);
    fill(127, 127);
    circle(x, y, 48);
    angle += angleVelocity;
  }
}
```


## Bitácora de aplicación 

**Explicación**

**Una galaxia inestable.** Mi idea es implementar un concepto diferente de cada una de las unidades pero de forma cuidadosa para crear una obra cohesiva. Me basaré en la obra de la unidad anterior y habrán planetas que rotan alrededor del centro. Mi interacción será con el mouse y al apretar click y arrastrar los planetas se pueden hacer que reboten porque estarán unidas como si fueran resortes. Finalmente usaré noise para que sea más interesante la línea del resorte y se vea una onda cambiante.

Bob

```js
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

```js
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

```js
class Spring {
  constructor(x, y, length) {
    this.anchor = createVector(x, y);
    this.restLength = length;
    this.k = 0.2;
    this.damping = 0.9;
    this.numPoints = 100; // puntos para la onda
    this.values = [];

    // Inicializar valores de la onda (estilo Daniel Shiffman)
    for (let i = 0; i < this.numPoints; i++) {
      this.values[i] = random(-10, 10); // amplitud de la onda
    }
  }

  // Aplica la fuerza de resorte al bob
  connect(bob) {
    let force = p5.Vector.sub(bob.position, this.anchor);
    let stretch = force.mag() - this.restLength;
    force.setMag(-this.k * stretch);

    // Amortiguamiento a lo largo del resorte
    let velAlongSpring = p5.Vector.dot(bob.velocity, force.copy().normalize());
    let dampingForce = force.copy().normalize().mult(-velAlongSpring * (1 - this.damping));
    force.add(dampingForce);

    bob.applyForce(force);
  }

  // Dibuja el resorte
  showLine(bob) {
    stroke(bob.color);
    strokeWeight(2);

    let dir = p5.Vector.sub(bob.position, this.anchor);
    let length = dir.mag();
    let unitDir = dir.copy().normalize();

    // Vector perpendicular para desplazar la onda
    let perp = createVector(-unitDir.y, unitDir.x);

    beginShape();
    for (let i = 0; i < this.numPoints; i++) {
      let t = i / (this.numPoints - 1);
      let pos = p5.Vector.add(this.anchor, p5.Vector.mult(unitDir, length * t));

      // Offset usando array al estilo Shiffman
      let idx = (i + frameCount) % this.numPoints;
      let offsetMag = this.values[idx];

      let offset = perp.copy().mult(offsetMag);
      vertex(pos.x + offset.x, pos.y + offset.y);
    }
    endShape();
  }
}
}
```

**Enlace**

https://editor.p5js.org/natalieruizperez/sketches/lrFESDjRG

**Capturas**

https://imgur.com/a/Iw7Q4n3

## Bitácora de reflexión










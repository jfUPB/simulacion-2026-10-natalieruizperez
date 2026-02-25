# Unidad 3

## Bitácora de proceso de aprendizaje

### Actividad 01

**Reflexión**

Siempre ha sido difícil destacar cuando se trata de arte pero ahora más que nunca. Personalmente encuentro consuelo en ver que profesionales de otras áreas también "sufren" cuando se trata de crear porque se encuentran ante el dilema de gastar horas y horas aprendiendo o gastar unos cuentos seguros de su tiempo escribiendo un prompt para crear exactamente lo mismo. A mi me afecta especialmente en la parte del 3D y pintar, ahora la ia puede hacer modelos exactamente iguales a la foto y lo único que hay que hacer es la retopología, entonces a este paso como van las cosas parece que van a buscar únicamente profesionales que tengan las bases muy claras para asegurarse de que el resultado que da la ia sea funcional y correcto en la parte técnica. En cuanto a pintar ha sido difícil y he gastado muchas horas pero no dejaría de hacerlo, la satisfacción de crear una obra propia en la que se invirtieron muchas horas no lo cambiaría por nada, también encuentro orgullo en decir que lo hice yo. Ahora lo que queda es buscar otras alternativas de trabajo, yo llevo tiempo buscando trabajo como artista 3d en distintas empresas pero están contratando más que nada seniors. Creo que la única opción que veo para salir adelante en el mundo del arte es crearse una red social, conseguir un público y realizar comisiones. Irónicamente a estas alturas parece que sería hasta más fácil ser youtuber. 

### Actividad 02

**Reflexión**

Entre lasventajas que hay de trabajar de manera modular estan que se puede trabjar al mismo tiempo.En la unidad pasada se recalculaba la velocidad frame by frame. Ahora cualquier interacción con señales externas se aplicarán fuerzas con y se usarán métodos para mantener todo separado de forma correcta. Se definen las fuerzas y luego se integran con la velocidad y la posición pero se reiniciará la aceleración porque se actualizará cada segundo. Como el applyForce es paso por referecia se pasa la dirección de esa variable y se terminaría modificando el objeto, una solución sería crear un objeto temporal.

<img width="512" height="323" alt="image" src="https://github.com/user-attachments/assets/119ba86f-214b-478e-a3ba-50ebcbe2a43e" />


### Actividad 03

## Bitácora de aplicación 

### Actividad 04

**Explicación**

**Código**

**Enlace**

**Capturas**


## Bitácora de reflexión


🌊 Código completo adaptado a tu idea
🔹 Clase Mover
class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 8;
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
    fill(180);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  checkEdges() {
    if (this.position.y < this.radius) {
      this.position.y = this.radius;
      this.velocity.y *= -0.5;
    }
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= -0.5;
    }
  }
}
🔹 Clase Liquid
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
    fill(180, 200, 255, 150);
    rect(this.x, this.y, this.w, this.h);
  }
}
🔹 Sketch principal
let movers = [];
let liquid;
let windActive = true;

function setup() {
  createCanvas(800, 500);

  // Agua ocupa mitad inferior
  liquid = new Liquid(0, height / 2, width, height / 2, 0.15);

  // Crear círculos ENCIMA del agua
  for (let i = 0; i < 10; i++) {
    movers.push(new Mover(random(width), random(50, height / 2 - 20), random(1, 3)));
  }
}

function draw() {
  background(255);

  liquid.show();

  for (let mover of movers) {

    // 🌬️ VIENTO HACIA ARRIBA (solo si está activo y está en el aire)
    if (windActive && !liquid.contains(mover)) {
      let wind = createVector(0, -0.05 * mover.mass);
      mover.applyForce(wind);
    }

    // 🌎 GRAVEDAD (solo cuando haces click)
    if (mouseIsPressed) {
      let gravity = createVector(0, 0.2 * mover.mass);
      mover.applyForce(gravity);
    }

    // 🌊 DRAG EN EL AGUA
    if (liquid.contains(mover)) {
      let drag = liquid.calculateDrag(mover);
      mover.applyForce(drag);

      // gravedad más suave dentro del agua
      let gravity = createVector(0, 0.05 * mover.mass);
      mover.applyForce(gravity);
    }

    mover.update();
    mover.checkEdges();
    mover.show();
  }
}

function mousePressed() {
  windActive = false;
}

function mouseReleased() {
  windActive = true;
}




te voy a mostrar diferentes codigos vistos e clase de como impeplementan la fuerza en en p5js, quiero que los analices porqu despues con eso que veas y aprendiste vamosa generar una obra mas adelante

atraccion gravitacional

mvoer
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Mover {
  constructor(x, y, mass) {
    this.mass = mass;
    this.radius = mass * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(1, 0);
    this.acceleration = createVector(0, 0);
  }
  // Newton's 2nd law: F = M * A
  // or A = F / M
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    // Velocity changes according to acceleration
    this.velocity.add(this.acceleration);
    // position changes by velocity
    this.position.add(this.velocity);
    // We must clear acceleration each frame
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127, 127);
    circle(this.position.x, this.position.y, this.radius * 2);
  }
}

atractor
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// An object for a draggable attractive body in our world

class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.mass = 20;
    this.dragOffset = createVector(0, 0);
    this.dragging = false;
    this.rollover = false;
  }

  attract(mover) {
    // Calculate direction of force
    let force = p5.Vector.sub(this.position, mover.position);
    // Distance between objects
    let distance = force.mag();
    // Limiting the distance to eliminate "extreme" results for very close or very far objects
    distance = constrain(distance, 5, 25);

    // Calculate gravitional force magnitude
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    // Get force vector --> magnitude * direction
    force.setMag(strength);
    return force;
  }

  // Method to display
  show() {
    strokeWeight(4);
    stroke(0);
    if (this.dragging) {
      fill(50);
    } else if (this.rollover) {
      fill(100);
    } else {
      fill(175, 200);
    }
    circle(this.position.x, this.position.y, this.mass * 2);
  }

  // The methods below are for mouse interaction
  handlePress(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.dragging = true;
      this.dragOffset.x = this.position.x - mx;
      this.dragOffset.y = this.position.y - my;
    }
  }

  handleHover(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.rollover = true;
    } else {
      this.rollover = false;
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

sketch 
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// A Mover and an Attractor
let mover;
let attractor;

// Gravitational constant (for global scaling)
let G = 1;

function setup() {
  createCanvas(640, 240);
  mover = new Mover(300, 50, 2);
  attractor = new Attractor();
}

function draw() {
  background(255);

  let force = attractor.attract(mover);
  mover.applyForce(force);
  mover.update();

  attractor.show();
  mover.show();
}

function mouseMoved() {
  attractor.handleHover(mouseX, mouseY);
}

function mousePressed() {
  attractor.handlePress(mouseX, mouseY);
}

function mouseDragged() {
  attractor.handleHover(mouseX, mouseY);
  attractor.handleDrag(mouseX, mouseY);
}

function mouseReleased() {
  attractor.stopDragging();
}

fluid

liquid
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c;
  }

  // Is the Mover in the Liquid?
  contains(mover) {
    let pos = mover.position;
    return (
      pos.x > this.x &&
      pos.x < this.x + this.w &&
      pos.y > this.y &&
      pos.y < this.y + this.h
    );
  }

  // Calculate drag force
  calculateDrag(mover) {
    // Magnitude is coefficient * speed squared
    let speed = mover.velocity.mag();
    let dragMagnitude = this.c * speed * speed;

    // Direction is inverse of velocity
    let dragForce = mover.velocity.copy();
    dragForce.mult(-1);

    // Scale according to magnitude
    dragForce.setMag(dragMagnitude);
    return dragForce;
  }

  show() {
    noStroke();
    fill(220);
    rect(this.x, this.y, this.w, this.h);
  }
}

mover
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Mover {
  constructor(x, y, mass) {
    this.mass = mass;
    this.radius = mass * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }
  // Newton's 2nd law: F = M * A
  // or A = F / M
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    // Velocity changes according to acceleration
    this.velocity.add(this.acceleration);
    // position changes by velocity
    this.position.add(this.velocity);
    // We must clear acceleration each frame
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127, 127);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  // Bounce off bottom of window
  checkEdges() {
    if (this.position.y > height - this.radius) {
      this.velocity.y *= -0.9; // A little dampening when hitting the bottom
      this.position.y = height - this.radius;
    }
  }
}

sketch
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// Forces (Gravity and Fluid Resistence) with Vectors

// Demonstration of multiple force acting on bodies (Mover class)
// Bodies experience gravity continuously
// Bodies experience fluid resistance when in "water"

// Five moving bodies
let movers = [];

// Liquid
let liquid;

function setup() {
  createCanvas(640, 240);
  reset();
  // Create liquid object
  liquid = new Liquid(0, height / 2, width, height / 2, 0.1);
}

function draw() {
  background(255);

  // Draw liquid
  liquid.show();

  for (let i = 0; i < movers.length; i++) {
    // Is the Mover in the liquid?
    if (liquid.contains(movers[i])) {
      // Calculate drag force
      let dragForce = liquid.calculateDrag(movers[i]);
      // Apply drag force to Mover
      movers[i].applyForce(dragForce);
    }

    // Gravity is scaled by mass here!
    let gravity = createVector(0, 0.1 * movers[i].mass);
    // Apply gravity
    movers[i].applyForce(gravity);

    // Update and display
    movers[i].update();
    movers[i].show();
    movers[i].checkEdges();
  }
}

function mousePressed() {
  reset();
}

// Restart all the Mover objects randomly
function reset() {
  for (let i = 0; i < 9; i++) {
    movers[i] = new Mover(40 + i * 70, 0, random(0.5, 3));
  }
}

friction
mover
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 8;
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

  contactEdge() {
    // The mover is touching the edge when it's within one pixel
    return (this.position.y > height - this.radius - 1);
  }

  bounceEdges() {
    // A new variable to simulate an inelastic collision
    // 10% of the velocity's x or y component is lost
    let bounce = -0.9;
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= bounce;
    } else if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= bounce;
    }
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= bounce;
    }
  }

}

sketch
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let mover;

function setup() {
  createCanvas(640, 240);
  mover = new Mover(width / 2, 30, 5);
  createP('Click mouse to apply wind force.');
}

function draw() {
  background(255);

  let gravity = createVector(0, 1);
  //{!1} I should scale by mass to be more accurate, but this example only has one circle
  mover.applyForce(gravity);

  if (mouseIsPressed) {
    let wind = createVector(0.5, 0);
    mover.applyForce(wind);
  }

  if (mover.contactEdge()) {
    //{!5 .bold}
    let c = 0.1;
    let friction = mover.velocity.copy();
    friction.mult(-1);
    friction.setMag(c);

    //{!1 .bold} Apply the friction force vector to the object.
    mover.applyForce(friction);
  }

  mover.bounceEdges();
  mover.update();
  mover.show();
}

gravity scaled masses

sketch
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let moverA;
let moverB;

function setup() {
  createCanvas(640, 240);
  // A large Mover on the left side of the window
  moverA = new Mover(200, 30, 10);
  // A smaller Mover on the right side of the window
  moverB = new Mover(440, 30, 2);
  createP("Click mouse to apply wind force.");
}

function draw() {
  background(255);

  let gravity = createVector(0, 0.1);

  let gravityA = p5.Vector.mult(gravity, moverA.mass);
  moverA.applyForce(gravityA);

  let gravityB = p5.Vector.mult(gravity, moverB.mass);
  moverB.applyForce(gravityB);

  if (mouseIsPressed) {
    let wind = createVector(0.1, 0);
    moverA.applyForce(wind);
    moverB.applyForce(wind);
  }

  moverA.update();
  moverA.show();
  moverA.checkEdges();

  moverB.update();
  moverB.show();
  moverB.checkEdges();
}

mover
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 8;
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
    ellipse(this.position.x, this.position.y, this.radius * 2);
  }

  checkEdges() {
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= -1;
    } else if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= -1;
    }
    if (this.position.y > height - this.radius) {
      this.position.y = height - this.radius;
      this.velocity.y *= -1;
    }
  }

}



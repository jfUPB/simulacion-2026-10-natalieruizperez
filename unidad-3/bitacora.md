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



# Unidad 5
## Bitácora de proceso de aprendizaje

### Actividad 01

**1. ¿Qué propiedades tiene cada partícula? Clasifícalas: ¿Cuáles definen su estado físico? ¿Cuáles su estado vital?**
**2. ¿Qué condición determina que una partícula “muere”? ¿Es una muerte instantánea o gradual?**
**3. ¿Cómo se actualiza la partícula en cada frame? Identifica el patrón motion 101 dentro de la partícula.**


**4. ¿Quién crea las partículas? ¿En qué momento?**
**5. ¿Quién decide cuándo eliminar una partícula del array?**
**6. ¿Por qué se recorre el array en orden inverso para eliminar? ¿Qué pasaría si no se hiciera así?**
**7. Si no eliminaras nunca las partículas, ¿Qué pasaría con la memoria y el rendimiento? Haz el experimento: comenta la línea que elimina y observa el frame rate.**


**8. ¿Qué elementos visuales usa para representar una partícula?**
**9. ¿Cómo se conecta el “tiempo de vida” con la apariencia visual?**
**10. Si quisieras cambiar la representación visual (por ejemplo, usar líneas en vez de círculos), ¿Qué cambiarías y qué NO cambiarías?**

### Actividad 02

**1. ¿Qué responsabilidades que antes estaban en draw() ahora están dentro de la clase Emitter?**
Es para encapsular y tener sistemas de sistemas

**2. ¿Cuál es la ventaja de encapsular la lógica de emisión en una clase separada?**
Se podrán tener

**3. En este ejemplo hay un array de emitters. ¿Quién crea los emitters? ¿Quién crea las partículas dentro de cada emitter?**
Los emitters los crea 
**4. Dibuja un diagrama que muestre la jerarquía: sketch → [emitters] → [partículas]. ¿Cuántos niveles de “colección” hay?**
**5. Describe este ejemplo usando palabras que NO mencionen p5.js, JavaScript, ni ninguna herramienta específica. Usa solo términos como: entidad, estado, colección, emisor, ciclo de vida, fuerza.**

### Actividad 03

**1. ¿Qué tienen en común las subclases de partículas? ¿Qué tienen de diferente?**



**2. ¿Por qué es importante que el Emitter no necesite saber qué tipo específico de partícula está gestionando? Explica esto con tus propias palabras.**

**3. Si mañana quisieras agregar un tercer tipo de partícula, ¿Qué tendrías que crear y qué NO tendrías que modificar?**

**4. Compara con Example 4.2: ¿Cambió la lógica del Emitter? ¿Cambió la lógica de muerte? ¿Qué capa del sistema se modificó y cuáles permanecieron intactas?**

## Bitácora de aplicación 


LA INTERACTIVIDAD TIENE QUE TENER CARGA INTERACTIVA

## Bitácora de reflexión

DropParticle
class DropParticle extends Particle {
  constructor(x, y) {
    super(x, y);
    this.vel = createVector(0, 2);
  }

  update() {
    let gravity = createVector(0, 0.2);
    this.applyForce(gravity);
    super.update();
  }

  display() {
    noStroke();
    fill(180, 220, 255);
    ellipse(this.pos.x, this.pos.y, 6);
  }

  hitWater() {
    return this.pos.y >= height / 2;
  }
}

Particle
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.life = 255;
  }

  applyForce(f) {
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  isDead() {
    return this.life <= 0;
  }
}

RippleParticle
class RippleParticle extends Particle {
  constructor(x, y) {
    super(x, y);
    this.radius = 0;
  }

  update() {
    this.radius += 2;   // expansión
    this.life -= 4;     // se desvanece
  }

  display() {
    noFill();
    stroke(200, 220, 255, this.life);
    ellipse(this.pos.x, this.pos.y, this.radius);
  }
}

Sketch
let particles = [];

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(15, 25, 45);

  // agua
  fill(30, 90, 140);
  rect(0, height / 2, width, height / 2);

  // sistema de partículas
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];

    p.update();
    p.display();

    // si es gota y toca el agua → crear onda
    if (p instanceof DropParticle && p.hitWater()) {
      particles.push(new RippleParticle(p.pos.x, height / 2));
      particles.splice(i, 1);
    } 
    else if (p.isDead()) {
      particles.splice(i, 1);
    }
  }
}

// interacción
function mousePressed() {
  particles.push(new DropParticle(mouseX, 0));
}


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


## CODIGO
tengo este codigo

let lakeY;
let lakeMinY;
let lakeMaxY;
let cloud;
let drops = [];
let vapors = [];
let waveOffset = 0;
let dropCooldown = 0;

// sol
let sunX = 80;
let sunY = 80;
let sunR = 30;
let rayCount = 12;
let rayLength = 20;
let rayAngle = 0; // rotación de los rayos

function setup() {
  createCanvas(700, 400);
  lakeY = height/2 + 50;
  lakeMinY = 120;      
  lakeMaxY = height - 50; 
  cloud = new Cloud(width/2, 100);
}

function draw() {
  background(30, 30, 60);

  // dibujar sol con rayos giratorios
  drawSun();

  // lago con ondas
  drawWaves();

  // evaporación solo si presionas sobre el sol
  if(mouseIsPressed && dist(mouseX, mouseY, sunX, sunY) < sunR){
    if (cloud.vaporAmount < 100 && lakeY < lakeMaxY && frameCount % 6 === 0) {
      let vp = new EvaporationParticle(random(0, width/2), lakeY);
      vapors.push(vp);
      cloud.addVapor();

      lakeY += 2; // evaporación visible
      lakeY = constrain(lakeY, lakeMinY, lakeMaxY);
    }
  }

  // actualizar vapor
  for (let i = vapors.length-1; i>=0; i--){
    let v = vapors[i];
    v.update();
    v.display();
    if(v.isDead()) vapors.splice(i,1);
  }

  // actualizar gotas
  for (let i = drops.length-1; i>=0; i--){
    let d = drops[i];
    d.update();
    d.display();
    if(d.hitGround(height-20)) {
      drops.splice(i,1);
      if(lakeY > lakeMinY + 5){
        lakeY -= 3; 
        lakeY = constrain(lakeY, lakeMinY, lakeMaxY);
      }
    }
  }

  // nube
  cloud.display();

  // animar ondas
  waveOffset += 0.08;

  if(dropCooldown > 0) dropCooldown--;
}

function mousePressed() {
  if(dropCooldown <= 0){
    let d = cloud.releaseDrop();
    if(d){
      drops.push(d);
      dropCooldown = 10;
    }
  }
}

function drawWaves() {
  noStroke();
  fill(0, 50, 255);
  beginShape();
  for (let x = 0; x <= width/2; x += 10) {
    let y = lakeY + sin(x * 0.05 + waveOffset) * 10;
    vertex(x, y);
  }
  vertex(width/2, height);
  vertex(0, height);
  endShape(CLOSE);
}

// 🌞 sol con rayos
function drawSun() {
  push();
  translate(sunX, sunY);
  fill(255, 204, 0);
  noStroke();
  ellipse(0, 0, sunR*2);

  stroke(255, 204, 0);
  strokeWeight(2);
  for (let i = 0; i < rayCount; i++){
    let angle = TWO_PI / rayCount * i + rayAngle;
    let x1 = cos(angle) * sunR;
    let y1 = sin(angle) * sunR;
    let x2 = cos(angle) * (sunR + rayLength);
    let y2 = sin(angle) * (sunR + rayLength);
    line(x1, y1, x2, y2);
  }
  pop();

  rayAngle += 0.03; // velocidad de rotación
}
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

class EvaporationParticle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(random(-0.5,0.5), random(-2,-0.5));
    this.alpha = 200;
  }

  update() {
    this.pos.add(this.vel);
    this.alpha -= 4;
  }

  display() {
    noStroke();
    fill(255, this.alpha);
    ellipse(this.pos.x, this.pos.y, 5,5);
  }

  isDead() {
    return this.alpha <= 0;
  }
}

class DropParticle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, random(4,7));
  }

  update() {
    this.pos.add(this.vel);
  }

  display() {
    stroke(0, 150, 255);
    strokeWeight(2);
    line(this.pos.x, this.pos.y, this.pos.x, this.pos.y + 5);
  }

  hitGround(y) {
    return this.pos.y >= y;
  }
}

class DropParticle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, random(4,7));
  }

  update() {
    this.pos.add(this.vel);
  }

  display() {
    stroke(0, 150, 255);
    strokeWeight(2);
    line(this.pos.x, this.pos.y, this.pos.x, this.pos.y + 5);
  }

  hitGround(y) {
    return this.pos.y >= y;
  }
}

 lo estoy haciendo para cumplor con lo que pide el procesor

pply: Aplicación 🛠
Actividad 05: Ejercicio de diseño “Ciclos de vida”
Brief de diseño
Diseña e implementa una pieza interactiva en p5.js que use un sistema de partículas para representar un ciclo de vida de tu elección.

Un ciclo de vida es cualquier proceso donde algo nace, se transforma y desaparece. Puede ser literal (semillas → flores → pétalos que caen), metafórico (ideas → conversaciones → olvido), emocional (calma → agitación → disolución), natural (estrellas → supernovas → polvo cósmico), o social (rumores que se propagan, se fragmentan y mueren).

Requisitos conceptuales del sistema:

Al menos dos tipos de partículas con comportamientos distintos (herencia y polimorfismo).
Ciclo de vida visible: las partículas deben nacer, transformarse visualmente durante su vida, y morir. La muerte no puede ser solo “desaparecer”: debe comunicar algo.
Al menos una fuerza que afecte a las partículas (gravedad, atracción, repulsión, viento, u otra).
La interacción del usuario debe tener un propósito narrativo claro: no es “hacer click para emitir más partículas”, sino que la interacción debe tener un significado dentro del ciclo de vida que elegiste.
Gestión correcta de memoria: las partículas muertas se eliminan.
Nota

Lo que se evalúa NO es la complejidad técnica del código (eso lo puede generar una IA), sino:

La coherencia entre el concepto de ciclo de vida y las decisiones de diseño.
Que puedas explicar cómo cada elemento del sistema contribuye a comunicar la idea.
Que la pieza transmita algo reconocible al verla e interactuar con ella.
📤 Bitácora

Documenta el proceso completo:

Concepto: 2-3 frases sobre qué ciclo de vida representarás y qué emoción o idea quieres comunicar.
Bocetos: al menos 2 bocetos (pueden ser a mano) que muestren cómo imaginas la pieza antes de programarla.
Mapa de decisiones: para cada elemento del sistema, explica la decisión de diseño: ¿Por qué esa emisión, esas fuerzas, esa condición de muerte, esa visualización, qué significa la interacción del usuario dentro del concepto?
Implementación: enlace al código en el editor de p5.js + código fuente en la bitácora.
Capturas: al menos 3 capturas de momentos diferentes del ciclo de vida.

es del ccilo del agua actualemnte lo que pasa es que al hacer click se activa el sol y la nube al mismo tiempo la idea es que la nube se active al arrastrarla y que caiga el numero de gotas segun si se matiene arrastrada, ese numero de gota son las que consigeu durante la evaporacion 


# Unidad 5
## Bitácora de proceso de aprendizaje

### Actividad 01

<img width="627" height="237" alt="image" src="https://github.com/user-attachments/assets/fbef37cb-37dc-4fec-bdbe-0a4a025f0612" />


**1. ¿Qué propiedades tiene cada partícula? Clasifícalas: ¿Cuáles definen su estado físico? ¿Cuáles su estado vital?**

Tienen tiempo de vida, aceleración, velocidad y posición, la primera define su estado vital y las demás su estado físico.

**2. ¿Qué condición determina que una partícula “muere”? ¿Es una muerte instantánea o gradual?**

isDead() es la función que "mata" la partícula y sucede de forma gradual porque no baja inmediatamente a 0 si no que lo hace de poco a poco.

**3. ¿Cómo se actualiza la partícula en cada frame? Identifica el patrón motion 101 dentro de la partícula.**

Se hace igual que como se hizo en las otras unidades de que primero se toma la aceleración y se le añade a la velocidad y la velocidad se le añade a la posición.

**4. ¿Quién crea las partículas? ¿En qué momento?**

En el draw se crean las partículas con el push en cada frame.

**5. ¿Quién decide cuándo eliminar una partícula del array?**

En el mismo draw se eliminan las partículas con el isDead().

**6. ¿Por qué se recorre el array en orden inverso para eliminar? ¿Qué pasaría si no se hiciera así?**

Se recorre el array en orden inverso para eliminar los elementos mientras se recorren. Si no se hiciera asi borraría lo que no es.

**7. Si no eliminaras nunca las partículas, ¿Qué pasaría con la memoria y el rendimiento? Haz el experimento: comenta la línea que elimina y observa el frame rate.**

Si no eliminara las partícula la memoria se llenaría el rendimiento no sería bueno, podría hasta incluso ocasionar lag.

<img width="1440" height="604" alt="image" src="https://github.com/user-attachments/assets/e9a02803-83ce-421e-864e-f93b480cc877" />

Luego de probarlo se cumplió mi hipótesis, la simulación empezó a ir más lento.

**8. ¿Qué elementos visuales usa para representar una partícula?**

Se usan círculos, líneas,  relleno.

**9. ¿Cómo se conecta el “tiempo de vida” con la apariencia visual?**

Cuando está viva la partícula se ve opaca pero a medida que va disminuyendo su tiempo de vida se transparenta hasta desaparecer.

**10. Si quisieras cambiar la representación visual (por ejemplo, usar líneas en vez de círculos), ¿Qué cambiarías y qué NO cambiarías?**

En particle.js en el show() cambiaría únicamente el stroke en donde dice circle, como se está cambiando lo visual no hay que mover lo demás ni nada de la lógica.

### Actividad 02

**1. ¿Qué responsabilidades que antes estaban en draw() ahora están dentro de la clase Emitter?**

Ya en Emitter se crean, actualizan y eliminan partículas en lugar de en el draw.

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

**Concepto**

Representación del ciclo del agua como un sistema donde todo está conectado entre sí, tanto el cielo, tierra y platas. El usuario puede interactuar con el sistema para comprender mejor de forma visual que sucede.

**Bocetos**

**Mapa de decisiones**


**Cloud**

```js
class Cloud {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.vaporAmount = 0;
  }

  addVapor() {
    this.vaporAmount = constrain(this.vaporAmount + 1, 0, 100);
  }

  releaseDrop() {
    if (this.vaporAmount > 0) {
      return new DropParticle(
        this.x + random(-30, 30),
        this.y + 20
      );
    }
    return null;
  }

  contains(px, py) {
    return dist(px, py, this.x, this.y) < 40;
  }

  display() {
    noStroke();

    let size = map(this.vaporAmount, 0, 100, 60, 120);
    let c = map(this.vaporAmount, 0, 100, 230, 100);

    fill(c);

    ellipse(this.x, this.y, size, size * 0.6);
    ellipse(this.x - size * 0.3, this.y + 10, size * 0.8, size * 0.5);
    ellipse(this.x + size * 0.3, this.y + 10, size * 0.8, size * 0.5);
  }
}
```

**DropParticle**

```js
class DropParticle extends Particle {
  constructor(x, y) {
    super(x, y);

    this.vel = createVector(0, random(2, 4));

    this.state = "falling"; // falling, infiltrating, runoff, absorbed
    this.infiltrationProgress = 0;
  }

  startInfiltration() {
    this.state = "infiltrating";
    this.vel = createVector(0, 0.5);
  }

  startRunoff() {
    this.state = "runoff";
    this.vel = createVector(-1, 0);
  }

  update() {

    if (this.state === "falling") {
      this.applyForce(createVector(0, 0.2));
    }

    else if (this.state === "infiltrating") {
      this.infiltrationProgress += 0.02;
      this.vel.y = 0.5;

      if (this.infiltrationProgress >= 1) {
        this.state = "absorbed";
        this.life = 0; // muerte significativa
      }
    }

    else if (this.state === "runoff") {
      this.applyForce(createVector(-0.05, 0.02));
    }

    super.update();
  }

  display() {

    if (this.state === "falling") {
      stroke(0, 150, 255, this.life);
      line(this.pos.x, this.pos.y, this.pos.x, this.pos.y + 6);
    }

    else if (this.state === "infiltrating") {
      noStroke();
      fill(0, 120, 255, this.life);

      let size = map(this.infiltrationProgress, 0, 1, 4, 1);
      ellipse(this.pos.x, this.pos.y, size);
    }

    else if (this.state === "runoff") {
      stroke(0, 100, 255, this.life);
      line(this.pos.x, this.pos.y, this.pos.x - 6, this.pos.y);
    }
  }
}
```

**Plant**

```js
class Plant {
  constructor(x, y) {
    this.x = x;
    this.y = y;

    this.growth = 0;
    this.life = 255;

    this.waterLevel = 0.5;

    this.state = "growing"; // growing, flower, dying
  }

  water() {
    this.waterLevel += 0.2;

    if (this.state === "growing") {
      this.growth += 0.2;
      this.growth = constrain(this.growth, 0, 1);
    }
  }

  sun() {
    if (this.state === "growing" && this.growth > 0.8) {
      this.state = "flower";
    }
  }

  update() {

    // pierde agua con el tiempo
    this.waterLevel -= 0.003;

    // muerte por exceso o falta
    if (this.waterLevel < 0 || this.waterLevel > 1.2) {
      this.state = "dying";
    }

    if (this.state === "growing") {
      this.growth += 0.01;
      this.growth = constrain(this.growth, 0, 1);
    }

    if (this.state === "dying") {
      this.life -= 4;
    }
  }

  display() {
    push();
    translate(this.x, this.y);

    noStroke();

    let h = this.growth * 25;

    // tallo
    fill(50, 180, 80, this.life);
    rect(-1, -h, 2, h);

    // hojas
    ellipse(-3, -h + 5, 6, 4);
    ellipse(3, -h + 5, 6, 4);

    // flor
    if (this.state === "flower") {
      fill(255, 100, 150, this.life);
      ellipse(0, -h - 3, 8);
    }

    pop();
  }

  isDead() {
    return this.life <= 0;
  }
}
```

**EvaporationParticle**

```js
class EvaporationParticle extends Particle {
  constructor(x, y) {
    super(x, y);
    this.vel = createVector(random(-0.3, 0.3), random(-1.5, -0.5));
    this.size = random(4, 8);
  }

  update() {
    this.applyForce(createVector(0, -0.02)); // flotabilidad
    super.update();
  }

  display() {
    noStroke();

    let size = map(this.life, 255, 0, this.size, 0);
    let alpha = this.life;

    fill(255, alpha);
    ellipse(this.pos.x, this.pos.y, size);
  }
}
```

**Particle**

```js
class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);

    this.life = 255;
    this.dead = false;
  }

  applyForce(f) {
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);

    this.life -= 2;

    if (this.life <= 0) {
      this.dead = true;
    }
  }

  isDead() {
    return this.dead;
  }
}
```

**SplashParticle**

```js
class SplashParticle extends Particle {
  constructor(x, y) {
    super(x, y);
    this.vel = createVector(random(-1, 1), random(-2, -0.5));
  }

  update() {
    this.applyForce(createVector(0, 0.15));
    super.update();
  }

  display() {
    noStroke();

    let size = map(this.life, 255, 0, 3, 0);

    fill(0, 150, 255, this.life);
    ellipse(this.pos.x, this.pos.y, size);
  }
}
```

**sketch**
```js
// ================= VARIABLES =================
let lakeY, lakeMinY, lakeMaxY;
let groundX, groundY, lakeBottomY;

let clouds = [];
let draggingCloud = null;
let offsetX, offsetY;

let drops = [];
let vapors = [];
let splashes = [];
let plants = [];

let waveOffset = 0;

// cielo
let currentCloudDarkness = 0;
let currentSunLight = 0;

// sol
let sunX = 80;
let sunY = 80;
let sunR = 30;
let rayAngle = 0;

// ================= SETUP =================
function setup() {
  createCanvas(700, 400);

  groundX = width / 2;
  groundY = height - 120;
  lakeBottomY = height;

  lakeMinY = groundY + 5;
  lakeMaxY = lakeBottomY - 5;
  lakeY = lakeMinY;

  for (let i = 0; i < 3; i++) {
    clouds.push(new Cloud(
      width/2 + random(-150, 150),
      random(60, 140)
    ));
  }
}

// ================= DRAW =================
function draw() {

  // cielo dinámico
  let totalVapor = 0;
  for (let c of clouds) totalVapor += c.vaporAmount;
  let avgVapor = totalVapor / clouds.length;

  let targetCloud = avgVapor / 100;
  let targetSun = (mouseIsPressed && dist(mouseX, mouseY, sunX, sunY) < sunR) ? 1 : 0;

  currentCloudDarkness = lerp(currentCloudDarkness, targetCloud, 0.02);
  currentSunLight = lerp(currentSunLight, targetSun, 0.05);

  drawSky();
  drawSun();
  drawLake();
  drawGround();

  // evaporación
  if (mouseIsPressed && dist(mouseX, mouseY, sunX, sunY) < sunR) {
    if (frameCount % 6 === 0) {
      vapors.push(new EvaporationParticle(random(0, groundX), lakeY));

      for (let c of clouds) {
        if (c.vaporAmount < 100) c.addVapor();
      }

      lakeY += 1;
      lakeY = constrain(lakeY, lakeMinY, lakeMaxY);
    }
  }

  // arrastrar nube
  if (draggingCloud) {
    draggingCloud.x = mouseX + offsetX;
    draggingCloud.y = mouseY + offsetY;

    if (frameCount % 5 === 0 && draggingCloud.vaporAmount > 0) {
      let d = draggingCloud.releaseDrop();
      if (d) {
        drops.push(d);
        draggingCloud.vaporAmount--;
      }
    }
  }

  // ================= PARTICULAS =================

  // vapor
  for (let i = vapors.length-1; i >= 0; i--) {
    let v = vapors[i];
    v.update();
    v.display();
    if (v.isDead()) vapors.splice(i,1);
  }

  // ================= GOTAS (FIX BUENO) =================
  for (let i = drops.length-1; i >= 0; i--) {
    let d = drops[i];
    d.update();
    d.display();

    let waterSurface = getLakeSurfaceY(d.pos.x);

    // agua
    if (d.pos.x < groundX && d.pos.y >= waterSurface && d.state === "falling") {

      for (let j = 0; j < 5; j++) {
        splashes.push(new SplashParticle(d.pos.x, waterSurface));
      }

      lakeY -= 4;
      lakeY = constrain(lakeY, lakeMinY, lakeMaxY);

      drops.splice(i,1);
      continue;
    }

    // tierra
    if (d.pos.x >= groundX && d.pos.y >= groundY && d.state === "falling") {

      d.startInfiltration();

      let existing = plants.find(p => dist(p.x, p.y, d.pos.x, groundY) < 10);

      if (existing) {
        existing.water();
      } else if (random() < 0.4) {
        plants.push(new Plant(d.pos.x, groundY));
      }
    }

    // escorrentía
    if (d.state === "runoff" && d.pos.x < groundX && d.pos.y >= waterSurface) {

      for (let j = 0; j < 5; j++) {
        splashes.push(new SplashParticle(d.pos.x, waterSurface));
      }

      lakeY -= 3;
      lakeY = constrain(lakeY, lakeMinY, lakeMaxY);

      drops.splice(i,1);
      continue;
    }

    if (d.isDead()) drops.splice(i,1);
  }

  // splash
  for (let i = splashes.length-1; i >= 0; i--) {
    let s = splashes[i];
    s.update();
    s.display();
    if (s.isDead()) splashes.splice(i,1);
  }

  // plantas
  let sunActive = mouseIsPressed && dist(mouseX, mouseY, sunX, sunY) < sunR;

  for (let i = plants.length - 1; i >= 0; i--) {
    let p = plants[i];

    if (sunActive) p.sun();

    p.update();
    p.display();

    if (p.isDead()) plants.splice(i,1);
  }

  for (let c of clouds) c.display();

  waveOffset += 0.05;
}

// ================= INTERACCION =================
function mousePressed() {
  for (let c of clouds) {
    if (c.contains(mouseX, mouseY)) {
      draggingCloud = c;
      offsetX = c.x - mouseX;
      offsetY = c.y - mouseY;
      break;
    }
  }
}

function mouseReleased() {
  draggingCloud = null;
}

// ================= AGUA =================
function getLakeSurfaceY(x) {
  return lakeY + sin(x * 0.05 + waveOffset) * 6;
}

// ================= SKY =================
function drawSky() {

  for (let y = 0; y < height; y++) {

    let inter = map(y, 0, height, 0, 1);

    let topColor = color(30, 30, 60);
    let bottomColor = color(120, 180, 255);

    let darkTop = color(10, 10, 30);
    let darkBottom = color(80, 100, 140);

    topColor = lerpColor(topColor, darkTop, currentCloudDarkness);
    bottomColor = lerpColor(bottomColor, darkBottom, currentCloudDarkness);

    let lightTop = color(100, 140, 220);
    let lightBottom = color(200, 230, 255);

    topColor = lerpColor(topColor, lightTop, currentSunLight);
    bottomColor = lerpColor(bottomColor, lightBottom, currentSunLight);

    let c = lerpColor(topColor, bottomColor, inter);

    stroke(c);
    line(0, y, width, y);
  }
}

// ================= LAKE =================
function drawLake() {
  noStroke();
  fill(0, 80, 255);

  beginShape();
  for (let x = 0; x <= groundX; x += 10) {
    let y = getLakeSurfaceY(x);
    vertex(x, y);
  }
  vertex(groundX, lakeBottomY);
  vertex(0, lakeBottomY);
  endShape(CLOSE);
}

// ================= GROUND =================
function drawGround() {
  noStroke();
  fill(120, 80, 50);
  rect(groundX, groundY, width/2, height - groundY);
}

// ================= SUN (RESTAURADO) =================
function drawSun() {
  push();
  translate(sunX, sunY);

  let isHeating = mouseIsPressed && dist(mouseX, mouseY, sunX, sunY) < sunR;

  noStroke();
  fill(255, 204, 0, isHeating ? 120 : 50);
  ellipse(0, 0, sunR * (isHeating ? 4 : 3));

  fill(255, 204, 0);
  ellipse(0, 0, sunR * 2);

  stroke(255, 200, 0);
  strokeWeight(isHeating ? 3 : 2);

  let rayLength = isHeating ? 45 : 20;

  for (let i = 0; i < 12; i++) {
    let angle = TWO_PI / 12 * i + rayAngle;

    let x1 = cos(angle) * sunR;
    let y1 = sin(angle) * sunR;

    let x2 = cos(angle) * (sunR + rayLength);
    let y2 = sin(angle) * (sunR + rayLength);

    line(x1, y1, x2, y2);
  }

  pop();

  rayAngle += isHeating ? 0.06 : 0.02;
}
```

**Enlace**
https://editor.p5js.org/natalieruizperez/sketches/0j2h1_Vm3

**Capturas**

<img width="693" height="393" alt="image" src="https://github.com/user-attachments/assets/5632f414-48c8-4386-b46d-344fa58269dd" />

<img width="694" height="389" alt="image" src="https://github.com/user-attachments/assets/5db339ce-ee79-47d6-8ae1-8085990fc634" />

<img width="689" height="386" alt="image" src="https://github.com/user-attachments/assets/791f87e1-1769-45bd-aaa5-db4f90334fba" />

<img width="691" height="392" alt="image" src="https://github.com/user-attachments/assets/880c0798-bd75-4aec-aa2e-857a89a532c7" />

<img width="682" height="375" alt="image" src="https://github.com/user-attachments/assets/cb0f17e3-a34e-426b-9293-83fb18529706" />









# Unidad 6

## Bitácora de proceso de aprendizaje

### Actividad 02

**1. Explica con tus propias palabras qué es un agente autónomo.**

Es aquel que toma decisiones por si mismo según lo que percibe.

**2. Explica qué es una steering force.**

Es una fuerza que guia el agente hacia donde moverse.
   
**3. Compara una steering force con una fuerza externa como la gravedad.**

La steering force es segun el agente mientras que la gravedad aplica para todos.
   
**4. Describe por qué estas ideas son útiles para diseñar comportamiento visual y no solo para simular movimiento.**

Por ejemplo con las fuerzas externas como la gravedad estas hacen lo mismo siempre para todos mientras que gracias al steering force depende del entorno haciedno que hayan resultados unicos y vivos.

### Actividad 03

** 1. ¿Cómo está construido el campo de flujo?**

Un campo de flujo se podría entender como una cuadrícula en donde hay diferentes vectores con ciertos ángulos.

**2. ¿Qué representa cada celda o vector del campo?**

Representa la dirección y como puede pintar.

**3. ¿Cómo usa un agente su posición para consultar el campo?

La usa para saber en qué parte del campo está y la divide entre la resolución.

**4. ¿Cómo se convierte el vector consultado en una decisión de movimiento?**

Sirve de referencia.


## Bitácora de aplicación

### Actividad 08

Apply: Aplicación 🛠
Actividad 06: Diseño de un instrumento visual para un tema musical
Diseña e implementa una pieza visual generativa en p5.js para interpretar un tema musical en tiempo real. La pieza debe funcionar como un instrumento visual: debe ocupar toda la pantalla, reaccionar al audio y permitir intervención performativa durante la ejecución. No debe mostrar dashboards, sliders, botones ni instrucciones visibles en escena.

La prioridad de esta actividad no es producir código complejo por sí mismo, sino construir una propuesta visual con intención conceptual, criterio formal y posibilidad de interpretación en vivo. Primero debes diseñar la propuesta; luego puedes usar IA para ayudarte a materializarla, iterarla o depurarla, pero no para sustituir tu autoría.

Requisitos conceptuales y técnicos
Concepto visual
Qué es: la idea central que orienta la pieza. Para qué sirve: para que la visual no sea solo una demostración técnica. Ejemplo: “Quiero traducir la sensación de acumulación y desborde de la canción en una masa de agentes que se comprime y explota”.

Relación con el tema musical
Qué es: la conexión entre el sonido y las decisiones del sistema visual. Para qué sirve: para que la música no sea un fondo decorativo, sino una dimensión estructural de la pieza. Ejemplo: “En las secciones más densas de la canción, los agentes se agrupan; en los silencios, se dispersan”.

Pantalla completa
Qué es: la pieza debe ocupar toda la pantalla durante la ejecución. Para qué sirve: para construir una experiencia inmersiva y escénica. Ejemplo: el canvas llena la ventana y no comparte espacio con paneles o menús visibles.

Sin dashboards ni instrucciones en pantalla
Qué es: la visual no debe mostrar sliders, texto explicativo, botones ni paneles de control visibles. Para qué sirve: para que funcione como obra performativa y no como prototipo de laboratorio. Ejemplo: si necesitas controles, deben estar resueltos con teclado, mouse, audio, MIDI o estados internos del sistema.

Reactividad al audio
Qué es: el sistema debe responder en tiempo real a características del audio. Para qué sirve: para que la visual dialogue con la música. Ejemplo: amplitud, energía o bandas de frecuencia modifican densidad, velocidad, agrupación o intensidad del movimiento.

Interacción performativa
Qué es: la pieza debe poder ser intervenida en vivo como si se tocara un instrumento visual. Para qué sirve: para que no sea una animación automática, sino una visual interpretable. Ejemplo: el mouse perturba el sistema, unas teclas cambian modos de comportamiento o un controlador altera parámetros durante la ejecución.

Interacción con sentido musical
Qué es: la interacción no debe ser arbitraria, sino útil para interpretar la pieza. Para qué sirve: para que cada gesto tenga un valor expresivo. Ejemplo: una tecla activa una variación visual adecuada para el clímax; otra reduce densidad para un momento de pausa.

**Moodboard o referencias**
Quiero implementar algo similar para el fondo

<img width="1020" height="570" alt="image" src="https://github.com/user-attachments/assets/1ff657b8-3c65-4690-931e-ead353e93844" />

También quiero que hayan varios fuegos artificiales pero de colores específicos

<img width="547" height="350" alt="image" src="https://github.com/user-attachments/assets/ca8735df-0974-44de-a8cd-3e4311829ad0" />

Quiero que en el climax se vea muy grande y abarque gran parte de la pantalla

<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/6003ff9c-e912-4cf2-8b34-7a4b6fea3abc" />



**Bocetos**

<img width="2048" height="1536" alt="1000005807" src="https://github.com/user-attachments/assets/b4241a94-3c0f-4a6c-842d-c3e5a9992ce0" />

<img width="2048" height="1536" alt="1000005808" src="https://github.com/user-attachments/assets/6c15ba11-d801-467f-a752-ce9682b9f070" />

La idea es que cuando la música este calmada hayan fuegos artificales azules y si está movida transcicione al rojo.


Mapa de decisiones
Qué es: un esquema que relaciona cada aspecto importante de la pieza con una intención de diseño. Para qué sirve: para hacer visible que el resultado proviene de decisiones conscientes. Ejemplo:

color oscuro: atmósfera contenida;
flocking con baja cohesión: tensión e inestabilidad;
respuesta a graves: sensación de peso.
Mapa de interpretación
Qué es: un esquema de cómo se “toca” la visual. Para qué sirve: para demostrar que la interacción fue diseñada como performance y no añadida al final. Ejemplo:

mouse: perturba el flujo,
A/S/D: cambia entre modos visuales,
audio: controla energía general.
Uso justificado del algoritmo
Qué es: la explicación de por qué elegiste flow fields, flocking o una combinación. Para qué sirve: para que el algoritmo sea una decisión de diseño y no solo una obligación técnica. Ejemplo: “Elegí flow fields porque necesito una sensación de corriente continua más que un comportamiento grupal”.

Uso explícito de IA como materializador
Qué es: un registro breve de qué le pediste a la IA y para qué. Para qué sirve: para distinguir entre apoyo técnico y autoría conceptual. Ejemplo: “Usé IA para proponer variaciones de implementación del trail y depurar un error; el concepto y la dirección visual fueron míos”.

Nota

Lo que se evalúa en esta actividad no es solo que “haya código funcionando”, sino que la pieza funcione como una propuesta visual diseñada, reactiva al audio e interpretable en vivo.

📤 Bitácora

Documenta el proceso completo:

Concepto visual.
Relación entre la visual y la canción.



**Moodboard o referencias**
Quiero implementar algo similar para el fondo

<img width="1020" height="570" alt="image" src="https://github.com/user-attachments/assets/1ff657b8-3c65-4690-931e-ead353e93844" />

También quiero que hayan varios fuegos artificiales pero de colores específicos

<img width="547" height="350" alt="image" src="https://github.com/user-attachments/assets/ca8735df-0974-44de-a8cd-3e4311829ad0" />

Quiero que en el climax se vea muy grande y abarque gran parte de la pantalla

<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/6003ff9c-e912-4cf2-8b34-7a4b6fea3abc" />


**Bocetos**

<img width="2048" height="1536" alt="1000005807" src="https://github.com/user-attachments/assets/b4241a94-3c0f-4a6c-842d-c3e5a9992ce0" />

<img width="2048" height="1536" alt="1000005808" src="https://github.com/user-attachments/assets/6c15ba11-d801-467f-a752-ce9682b9f070" />

La idea es que cuando la música este calmada hayan fuegos artificales azules y si está movida transcicione al rojo.


Mapa de decisiones.


Mapa de interpretación.

**Justificación del algoritmo elegido.**


**Explicación de la relación audio-visual.**

**Evidencia del uso de IA.**

**Código fuente.**

´´´
let song;
let fft;
let amplitude; 
let fireworks = [];
let spheres = []; 
let gravity;
let manualHue = -1; 
let started = false;
let godModeForced = false; 

const MAX_PARTICLES = 2500;
const MAX_FIREWORKS = 30;

let currentMode = 0;

function preload() {
  song = loadSound('firework.mp3');
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 255, 255, 255);
  gravity = createVector(0, 0.15);
  fft = new p5.FFT();
  fft.setInput(song);
  amplitude = new p5.Amplitude();
  amplitude.setInput(song); 
  background(0);
  
  
}
function draw() {
  if (!started) {
    return;
  }

  background(0, 0, 0, 25); 

  fft.analyze();
  let lowEnergy    = fft.getEnergy("bass");
  let midEnergy    = fft.getEnergy("mid");
  let highEnergy   = fft.getEnergy("treble");
  let voiceEnergy  = midEnergy;
  let vol = amplitude.getLevel() || 0;

  let currentHue;
  let isRed = false;

  if (manualHue === -1) {
    if (voiceEnergy < 160) {
      currentHue = 220;
    } else if (voiceEnergy >= 170) {
      currentHue = 360;
      isRed = true;
    } else {
      currentHue = map(voiceEnergy, 160, 170, 220, 360);
    }
    currentHue = constrain(currentHue, 220, 360);
  } else {
    currentHue = manualHue;
    if (manualHue === 360) isRed = true;
  }

  let freqGodMode = (lowEnergy >= 240 && midEnergy >= 170 && vol >= 0.2);
  let isClimax = godModeForced || freqGodMode || (fireworks.length > 40 && isRed);

  let explodeYMin, explodeYMax;
  if (currentHue <= 240) {
    explodeYMin = height * 0.60;
    explodeYMax = height * 0.80;
  } else if (currentHue <= 310) {
    explodeYMin = height * 0.30;
    explodeYMax = height * 0.55;
  } else {
    explodeYMin = height * 0.05;
    explodeYMax = height * 0.25;
  }

  let particleSizeBase = (currentHue <= 240) ? 3.5 : (currentHue <= 310) ? 2.5 : 1.5;

  if (random(1) > 0.96) { 
    spheres.push(new MicroSphere(random(width), random(height)));
  }
  for (let i = spheres.length - 1; i >= 0; i--) {
    spheres[i].update(vol);
    spheres[i].show(vol);
    if (spheres[i].done()) spheres.splice(i, 1);
  }

  let totalParticles = fireworks.reduce((sum, fw) => sum + fw.particles.length, 0);
  if (totalParticles > MAX_PARTICLES) {
    let excess = totalParticles - MAX_PARTICLES;
    let removed = 0;
    for (let i = fireworks.length - 1; i >= 0 && removed < excess; i--) {
      for (let j = fireworks[i].particles.length - 1; j >= 0 && removed < excess; j--) {
        fireworks[i].particles[j].lifespan -= 120;
        removed++;
      }
    }
  }

  let spawnChance = isClimax ? 0.70 : 0.88;
  if (fireworks.length < MAX_FIREWORKS) {
    if (voiceEnergy > 150 && random(1) > spawnChance) {
      let explodeY = random(explodeYMin, explodeYMax);
      fireworks.push(new Firework(
        random(width), height,
        currentHue, vol, false, isClimax, particleSizeBase, explodeY, currentMode
      ));
    }
  }

  if (mouseIsPressed && fireworks.length < MAX_FIREWORKS) {
    let explodeY = random(explodeYMin, explodeYMax);
    fireworks.push(new Firework(
      mouseX, mouseY,
      currentHue, vol, true, isClimax, particleSizeBase, explodeY, currentMode
    ));
  }

  for (let i = fireworks.length - 1; i >= 0; i--) {
    fireworks[i].update();
    fireworks[i].show();
    if (fireworks[i].done()) fireworks.splice(i, 1);
  }
}

function keyPressed() {
  if (!started && keyCode === ENTER) { 
    song.play(); 
    started = true; 
    fullscreen(true); 
  }

  if (keyCode === ESCAPE) {
    fullscreen(false); 
  }

  if (key === '1') manualHue = 220;
  if (key === '2') manualHue = 280;
  if (key === '3') manualHue = 360;
  if (key === '0') { 
    manualHue = -1; 
    godModeForced = false; 
  }
  if (key === 'g' || key === 'G') godModeForced = !godModeForced;

  if (key === 'u' || key === 'U') currentMode = 0;
  if (key === 'i' || key === 'I') currentMode = 1;
  if (key === 'o' || key === 'O') currentMode = 2;
  if (key === 'p' || key === 'P') currentMode = 3;
  if (key === 'y' || key === 'Y') currentMode = 4;
}

class Firework {
  constructor(x, y, hue, vol, manual = false, gigante = false, sizeBase = 2.5, explodeY = null, mode = 0) {
    this.hue = hue;
    this.exploded = false;
    this.particles = [];
    this.vol = vol;
    this.gigante = gigante;
    this.sizeBase = sizeBase;
    this.ttl = 300;
    this.explodeY = explodeY !== null ? explodeY : height * 0.3;
    this.mode = mode;

    if (manual) {
      this.firework = new Particle(x, y, this.hue, true, 0, false, this.sizeBase, this.mode);
      this.explode(x, y);
    } else {
      let launchForce = map(vol, 0, 0.4, -11, -20);
      this.firework = new Particle(x, y, this.hue, true, launchForce, this.gigante, this.sizeBase, this.mode);
    }
  }

  update() {
    this.ttl--;
    if (!this.exploded) {
      this.firework.applyForce(gravity);
      this.firework.update();
      if (this.firework.pos.y <= this.explodeY || this.firework.vel.y >= 0) {
        this.explode(this.firework.pos.x, this.firework.pos.y);
      }
    }
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].applyForce(gravity);
      this.particles[i].update();
      if (this.particles[i].done()) this.particles.splice(i, 1);
    }
  }

  explode(x, y) {
    if (this.exploded) return;
    this.exploded = true;
    let count = this.gigante ? 550 : map(this.vol, 0, 0.5, 70, 180);
    for (let i = 0; i < count; i++) {
      this.particles.push(new Particle(x, y, this.hue, false, 0, this.gigante, this.sizeBase, this.mode));
    }
  }

  done() { return (this.exploded && this.particles.length === 0) || this.ttl < -30; }

  show() {
    if (!this.exploded) this.firework.show();
    for (let p of this.particles) p.show();
  }
}

class Particle {
  constructor(x, y, hue, isFirework, force, gigante, sizeBase = 2.5, mode = 0) {
    this.pos = createVector(x, y);
    this.origin = createVector(x, y);
    this.isFirework = isFirework;
    this.lifespan = 255;
    this.hue = hue;
    this.gigante = gigante;
    this.sizeBase = sizeBase;

    if (mode === 4) {
      this.mode = floor(random(0, 4)); 
    } else {
      this.mode = mode;
    }

    if (this.isFirework) {
      this.vel = createVector(0, force);
    } else {
      this.vel = p5.Vector.random2D();
      let mult = this.gigante ? random(12, 45) : random(3, 13);
      this.vel.mult(mult);
    }
    this.acc = createVector(0, 0);
  }

  applyForce(force) { this.acc.add(force); }

  update() {
    if (!this.isFirework) {
      if (this.mode === 1) { 
        let v = p5.Vector.sub(this.pos, this.origin);
        v.rotate(HALF_PI);
        v.setMag(0.8);
        this.acc.add(v);
      } else if (this.mode === 2) { 
        let n = noise(this.pos.x * 0.005, this.pos.y * 0.005, frameCount * 0.01);
        this.acc.add(p5.Vector.fromAngle(n * TWO_PI * 2).mult(0.6));
      } else if (this.mode === 3) { 
        let attraction = p5.Vector.sub(this.origin, this.pos);
        attraction.mult(0.01);
        this.acc.add(attraction);
      }
      this.vel.mult(this.gigante ? 0.94 : 0.91);
      this.lifespan -= this.gigante ? 3.2 : 2.5;
    }
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  show() {
    let sw = this.isFirework ? 4 : (this.gigante ? random(8, 14) : this.sizeBase);
    strokeWeight(sw);
    stroke(this.hue, 200, 255, this.lifespan);
    point(this.pos.x, this.pos.y);
  }

  done() { return this.lifespan < 0; }
}

class MicroSphere {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(0.1, 0.4));
    this.baseSize = random(5, 20); 
    this.baseBrightness = random(20, 200); 
    this.opacity = 0;
    this.maxOpacity = random(50, 150); 
    this.noiseOffset = random(1000);
    this.lifespan = random(400, 800); 
  }
  update(vol) {
    let nX = map(noise(this.noiseOffset + frameCount * 0.005), 0, 1, -0.3, 0.3);
    let nY = map(noise(this.noiseOffset + 500 + frameCount * 0.005), 0, 1, -0.3, 0.3);
    this.pos.add(this.vel);
    this.pos.x += nX; this.pos.y += nY;
    if (this.pos.x < -20) this.pos.x = width + 20;
    if (this.pos.x > width + 20) this.pos.x = -20;
    if (this.pos.y < -20) this.pos.y = height + 20;
    if (this.pos.y > height + 20) this.pos.y = -20;
    if (this.lifespan > 100) this.opacity = lerp(this.opacity, this.maxOpacity, 0.05);
    else this.opacity -= 1;
    this.lifespan--;
  }
  show(vol) {
    noStroke();
    let pulse = map(vol, 0, 0.5, 1, 2.5); 
    let b = constrain(this.baseBrightness + map(vol, 0, 0.5, 0, 100), 0, 255);
    fill(0, 0, b, this.opacity);
    ellipse(this.pos.x, this.pos.y, this.baseSize * pulse);
  }
  done() { return (this.lifespan <= 0 || this.opacity <= 0); }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}
´´´


**Enlace al sketch.**

https://editor.p5js.org/natalieruizperez/sketches/mvMvWGIZK

**Capturas o registros de momentos importantes de la pieza.**

<img width="1892" height="1079" alt="Captura de pantalla 2026-04-17 073000" src="https://github.com/user-attachments/assets/cbb52ad3-dc29-4796-918c-4c729e823da9" />

<img width="1869" height="1079" alt="Captura de pantalla 2026-04-17 072949" src="https://github.com/user-attachments/assets/fa21264f-4512-49ae-a2a1-e63d3920374e" />

<img width="1919" height="1079" alt="Captura de pantalla 2026-04-17 072946" src="https://github.com/user-attachments/assets/d8a7e1f8-9bc6-41cd-8b58-3c665fdf9dea" />

<img width="1914" height="1079" alt="Captura de pantalla 2026-04-17 072920" src="https://github.com/user-attachments/assets/3b6850eb-a1c9-4d4f-af77-c11e5b7c0970" />

<img width="1919" height="1079" alt="Captura de pantalla 2026-04-17 072903" src="https://github.com/user-attachments/assets/27a9817f-3f9f-45f6-ba70-0497234fdee7" />


### Actividad 07


Antes de ejecutar tu pieza debes explicar en 1 o 2 minutos:

**Cuál es el concepto de tu obra, cómo se relaciona con el tema musical**

**Qué tipo de comportamiento visual diseñaste y de qué manera se interpreta o ejecuta en vivo.**


## Bitácora de reflexión


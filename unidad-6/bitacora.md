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

**1. ¿Cómo está construido el campo de flujo?**

Un campo de flujo se podría entender como una cuadrícula en donde hay diferentes vectores con ciertos ángulos.

**2. ¿Qué representa cada celda o vector del campo?**

Representa la dirección y como puede pintar.

**3. ¿Cómo usa un agente su posición para consultar el campo?

La usa para saber en qué parte del campo está y la divide entre la resolución.

**4. ¿Cómo se convierte el vector consultado en una decisión de movimiento?**

Sirve de referencia.


## Bitácora de aplicación

### Actividad 06



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


**Mapa de decisiones**

Fuegos artificiales: La canción se llama firework.
Colores de azul a rojo: crear transcición y poder representar los tonos bajos, medios y altos.
Explosión: Sucede unicamente cuando los graves, medios y el volumen tienen respectivamente los valores 240, 170 y 0.2 respectivamente.
Diferentes colores de partículas: Controlar manualmente los colores azul, morado y rojo.
Dispersión: Para generar caos en el sistema cuando se active la exposión.
Modo liquido: Sucede cuando ya hay una cantidad determinada de partículas en la pantalla.
Flow field: Para generar un comportamiento indivial independiente
respuesta a graves: sensación de peso.

**Mapa de interpretación**

click: Crea fuegos artificiales.
1/2/3/0: Permite cambiar el color manualmente entre azul , morado, rojo y random respectivamente.
u: Partículas siguen el ruido
i: círculo en sentido horario
o: círculo en sentido antihorario
p: forma agrupamientos
y: crea remolinos
r: varia entre los modos explicados arriba.
audio: Si no hay volumen las partículas se dispersan.

**Justificación del algoritmo elegido.**

Elegí usar flow Fields basados en ruido de perlin con desplazamiento individual (noiseOffset) para que cada partícula reaccione al entorno de forma orgánica pero independiente, sirve para transformar la explosión de un fuego artificial en una masa líquida dinámica.

**Explicación de la relación audio-visual**
Los fuegos artificiales varían de color según los tonos bajos, medios y altos. Para la visual escogí hacer fuegos artificiales porque la canción se llama así en inglés, pero no quise hacer todo tan literal porque se volvía repetitivo y aburrido de ver así que implemente modos diferentes para las partículas para que se sintiera más vivo. La mayoría del tiempo las partículas están cercas unas de otras siguiendo un movimiento especifíco pero cuando alcanza cierto nivel de los medios, bajo y el volumen se genera una explosión dispersiva para marcar con claridad ese momento clave. Quería representar lo que sentí cuando la escuché por primera vez ya que en los momentos en los que los fuegos artificiales crecen y se dispersan estan cargados de energía y motivación.

**Evidencia del uso de IA**

Usé IA para proponer variaciones en la implementación del sistema de partículas, optimizar el rendimiento (manejo de límites, pool de objetos y control de memoria) y depurar comportamientos en los distintos modos (dispersión, líquido, silencio y cambio aleatorio). El concepto, la lógica interactiva y la dirección visual fueron desarrollados por mí.

**Código fuente.**

```js

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

let persistentParticles = [];        
let persistentPool = [];             
let liquidMode = false;
let disperseMode = false;
const PERSISTENT_THRESHOLD = 1800;   
const PERSISTENT_CAP = 4000;        
const PERSISTENT_MAX_AGE = 20 * 60;  

const JOIN_DELAY_MIN = 6;    
const JOIN_DELAY_MAX = 90;  


let randomMode = false;               
let randomModeInterval = 180;         
let lastRandomChange = 0;           

const SILENCE_THRESHOLD = 0.008;      
const SILENCE_FADE_START = 60;        
const SILENCE_MAX_ACCEL = 6.0;        
let silenceTimer = 0;

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
  noStroke();
  
  for (let i = 0; i < 1200; i++) persistentPool.push(makeEmptyPersistent());
}

function draw() {
  if (!started) return;

  
  background(0, 0, 0, 12);

  fft.analyze();
  let lowEnergy    = fft.getEnergy("bass");
  let midEnergy    = fft.getEnergy("mid");
  let highEnergy   = fft.getEnergy("treble");
  let voiceEnergy  = midEnergy;
  let vol = amplitude.getLevel() || 0;

  
  if (randomMode) updateRandomMode(vol);

  
  if (vol <= SILENCE_THRESHOLD) {
    silenceTimer++;
  } else {
    silenceTimer = 0;
  }

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

  
  if (isClimax && !disperseMode) {
    startDisperseMode(lowEnergy, midEnergy, vol);
  } else if (!isClimax && disperseMode) {
    stopDisperseMode();
  }

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

  
  updatePersistentParticles();

  
  let totalParticles = fireworks.reduce((sum, fw) => sum + fw.particles.length + (fw.exploded ? 0 : 1), 0) + persistentParticles.length;
  if (totalParticles > MAX_PARTICLES) {
    let excess = totalParticles - MAX_PARTICLES;
    
    let removed = 0;
    for (let i = fireworks.length - 1; i >= 0 && removed < excess; i--) {
      for (let j = fireworks[i].particles.length - 1; j >= 0 && removed < excess; j--) {
        fireworks[i].particles[j].lifespan -= 120;
        removed++;
      }
    }
    
    while (removed < excess && persistentParticles.length > 0) {
      recyclePersistent(persistentParticles.shift());
      removed++;
    }
  }

  
  let spawnChance = disperseMode ? 0.92 : 0.88;
  if (silenceTimer > SILENCE_FADE_START) {
    
    spawnChance = 0.995;
  }

  if (fireworks.length < MAX_FIREWORKS) {
    if (voiceEnergy > 150 && random(1) > spawnChance) {
      let explodeY = random(explodeYMin, explodeYMax);
      fireworks.push(new Firework(
        random(width), height,
        currentHue, vol, false, disperseMode, particleSizeBase, explodeY, currentMode
      ));
    }
  }

  if (mouseIsPressed && fireworks.length < MAX_FIREWORKS) {
    let explodeY = random(explodeYMin, explodeYMax);
    fireworks.push(new Firework(
      mouseX, mouseY,
      currentHue, vol, true, disperseMode, particleSizeBase, explodeY, currentMode
    ));
  }

  for (let i = fireworks.length - 1; i >= 0; i--) {
    fireworks[i].update();
    fireworks[i].show();
    if (fireworks[i].done()) {
      transferFireworkToPersistent(fireworks[i]);
      fireworks.splice(i, 1);
    }
  }

  
  if (persistentParticles.length > PERSISTENT_CAP) {
    let removeCount = persistentParticles.length - PERSISTENT_CAP;
    for (let i = 0; i < removeCount; i++) recyclePersistent(persistentParticles.shift());
  }
}



function makeEmptyPersistent() {
  return {
    x: 0, y: 0, hue: 0, size: 2, age: 0, maxAge: PERSISTENT_MAX_AGE,
    vx: 0, vy: 0, noiseOffset: 0, alive: false, id: 0,
    
    joinDelay: 0, joinTimer: 0, active: false, mode: 0
  };
}

function getPersistentFromPool() {
  if (persistentPool.length > 0) return persistentPool.pop();
  return makeEmptyPersistent();
}

function recyclePersistent(obj) {
  obj.alive = false;
  obj.age = 0;
  obj.vx = obj.vy = 0;
  obj.joinDelay = 0;
  obj.joinTimer = 0;
  obj.active = false;
  obj.mode = 0;
  persistentPool.push(obj);
}

function transferFireworkToPersistent(fw) {
  for (let p of fw.particles) {
    let pp = getPersistentFromPool();
    pp.x = p.pos.x;
    pp.y = p.pos.y;
    pp.hue = p.hue;
    pp.size = p.isFirework ? 4 : (p.gigante ? random(6, 12) : p.sizeBase);
    pp.age = 0;
    pp.maxAge = PERSISTENT_MAX_AGE;
   
    pp.vx = p.vel.x * 0.12 + random(-0.05, 0.05);
    pp.vy = p.vel.y * 0.12 + random(-0.05, 0.05);
    pp.noiseOffset = random(1000);
    pp.alive = true;
    pp.id = millis() + random(10000);
    
    pp.joinDelay = floor(random(JOIN_DELAY_MIN, JOIN_DELAY_MAX));
    pp.joinTimer = 0;
    pp.active = false;
    
    pp.mode = currentMode;
    persistentParticles.push(pp);
    if (persistentParticles.length > PERSISTENT_CAP) {
      recyclePersistent(persistentParticles.shift());
    }
  }
  fw.particles.length = 0;
}


function updatePersistentParticles() {
  let n = persistentParticles.length;
  if (n === 0) return;

  
  
  let activeCount = persistentParticles.reduce((acc, pp) => acc + (pp.alive && pp.active ? 1 : 0), 0);
  if (!liquidMode && activeCount >= PERSISTENT_THRESHOLD && !disperseMode) liquidMode = true;
  if (liquidMode && activeCount < PERSISTENT_THRESHOLD * 0.6) liquidMode = false;

  
  let cx = width * 0.5;
  let cy = height * 0.45;

  
  let silenceFactor = 1.0;
  if (silenceTimer > SILENCE_FADE_START) {
    
    let t = constrain((silenceTimer - SILENCE_FADE_START) / (SILENCE_FADE_START * 2), 0, 1);
    silenceFactor = lerp(1.0, SILENCE_MAX_ACCEL, t);
  }

  
  for (let i = persistentParticles.length - 1; i >= 0; i--) {
    let pp = persistentParticles[i];
    if (!pp || !pp.alive) continue;

    
    pp.age += silenceFactor;
    if (pp.age > pp.maxAge) {
      recyclePersistent(persistentParticles.splice(i, 1)[0]);
      continue;
    }

    
    if (!pp.active) {
      pp.joinTimer += 1 * silenceFactor; 
      
      pp.x += sin((pp.joinTimer + pp.noiseOffset) * 0.02) * 0.12;
      pp.y += cos((pp.joinTimer + pp.noiseOffset) * 0.02) * 0.12;

      
      let t = pp.joinDelay > 0 ? constrain(pp.joinTimer / pp.joinDelay, 0, 1) : 1;
      applyModeInfluence(pp, cx, cy, 0.06 * t);

      if (pp.joinTimer >= pp.joinDelay) {
        pp.active = true;
       
        pp.vx += random(-0.25, 0.25);
        pp.vy += random(-0.25, 0.25);
      }
     
    }

    
    if (disperseMode) {
      
      let dx = pp.x - cx;
      let dy = pp.y - cy;
      let d = sqrt(dx*dx + dy*dy) + 0.001;
      let strength = map(d, 0, max(width, height), 2.2, 0.6);
      pp.vx += (dx / d) * strength * random(0.6, 1.2);
      pp.vy += (dy / d) * strength * random(0.6, 1.2);

      
      let nval = noise(pp.noiseOffset + frameCount * 0.02);
      pp.vx += map(nval, 0, 1, -0.4, 0.4);
      pp.vy += map(nval, 0, 1, -0.4, 0.4);

      
      pp.vx *= 0.985;
      pp.vy *= 0.985;
    } else if (liquidMode) {
      
      let nval = noise((pp.x + pp.noiseOffset) * 0.0015, (pp.y + pp.noiseOffset) * 0.0015, frameCount * 0.003);
      let angle = map(nval, 0, 1, 0, TWO_PI);
      
      pp.vx += cos(angle) * 0.10;
      pp.vy += sin(angle) * 0.10;

      
      let toCx = (cx - pp.x) * 0.0006;
      let toCy = (cy - pp.y) * 0.0006;
      pp.vx += toCx;
      pp.vy += toCy;

     
      applyModeInfluence(pp, cx, cy, 0.18);

      
      pp.vx *= 0.985;
      pp.vy *= 0.985;
    } else {
      
      pp.vx += random(-0.02, 0.02);
      pp.vy += random(-0.02, 0.02);
      
      applyModeInfluence(pp, cx, cy, 0.03);
      pp.vx *= 0.995;
      pp.vy *= 0.995;
    }

   
    pp.vx *= (silenceFactor > 1.0) ? 0.98 : 1.0;
    pp.vy *= (silenceFactor > 1.0) ? 0.98 : 1.0;

    pp.x += pp.vx;
    pp.y += pp.vy;

    
    if (pp.x < 0) { pp.x = 0; pp.vx *= -0.4; }
    if (pp.x > width) { pp.x = width; pp.vx *= -0.4; }
    if (pp.y < 0) { pp.y = 0; pp.vy *= -0.4; }
    if (pp.y > height) { pp.y = height; pp.vy *= -0.4; }
  }

  
  push();
  for (let i = 0; i < persistentParticles.length; i++) {
    let pp = persistentParticles[i];
    if (!pp || !pp.alive) continue;
    
    let alphaBase = map(pp.age, 0, pp.maxAge, 255, 40);
    if (silenceTimer > SILENCE_FADE_START) {
      
      alphaBase = alphaBase / silenceFactor;
    }
    if (!pp.active) {
      strokeWeight(pp.size + 1);
      stroke(pp.hue, 200, 255, map(pp.joinTimer, 0, pp.joinDelay, 80, min(160, alphaBase)));
      point(pp.x, pp.y);
    } else {
      strokeWeight(pp.size);
      stroke(pp.hue, 200, 255, constrain(alphaBase, 0, 255));
      point(pp.x, pp.y);
    }
  }
  pop();
}


function applyModeInfluence(pp, cx, cy, strength) {
  
  let dx = pp.x - cx;
  let dy = pp.y - cy;
  let d = sqrt(dx*dx + dy*dy) + 0.001;

  switch (pp.mode) {
    case 1: 
      // vector perpendicular (clockwise): (dy, -dx)
      pp.vx += (dy / d) * strength * random(0.6, 1.1);
      pp.vy += (-dx / d) * strength * random(0.6, 1.1);
      break;
    case 2: 
      pp.vx += (-dy / d) * strength * random(0.6, 1.1);
      pp.vy += (dx / d) * strength * random(0.6, 1.1);
      break;
    case 3: 
      let pull = map(d, 0, max(width, height), 0.02, 0.6) * strength;
      pp.vx += (-dx / d) * pull * random(0.8, 1.2);
      pp.vy += (-dy / d) * pull * random(0.8, 1.2);
      break;
    case 4: 
      let n = noise(pp.noiseOffset + frameCount * 0.01);
      let swirlAngle = map(n, 0, 1, -PI, PI);
      pp.vx += cos(swirlAngle) * strength * 0.6;
      pp.vy += sin(swirlAngle) * strength * 0.6;
      
      pp.vx += (dy / d) * strength * 0.25;
      pp.vy += (-dx / d) * strength * 0.25;
      break;
    default:
      
      break;
  }
}


function startDisperseMode(low, mid, vol) {
  disperseMode = true;
  liquidMode = false;
  
  let sample = min(600, persistentParticles.length);
  for (let k = 0; k < sample; k++) {
    let idx = floor(random(persistentParticles.length));
    let pp = persistentParticles[idx];
    if (!pp || !pp.alive) continue;
    
    if (!pp.active) {
      pp.joinDelay = max(3, floor(pp.joinDelay * 0.25));
    }
    let angle = random(TWO_PI);
    let mag = map(vol, 0, 0.5, 1.2, 5.0) * random(0.8, 1.4);
    pp.vx += cos(angle) * mag;
    pp.vy += sin(angle) * mag;
    
    pp.maxAge = max(30, floor(pp.maxAge * 0.5));
  }
}


function stopDisperseMode() {
  disperseMode = false;
  if (persistentParticles.length >= PERSISTENT_THRESHOLD * 0.8) liquidMode = true;
}

function updateRandomMode(vol) {
  
  

  if (frameCount - lastRandomChange >= randomModeInterval) {
    lastRandomChange = frameCount;
    
    let newMode = currentMode;
    let attempts = 0;
    while (newMode === currentMode && attempts < 8) {
      newMode = floor(random(0, 5)); // 0..4
      attempts++;
    }
    setMode(newMode);
  }
}



function keyPressed() {
  if (!started && keyCode === ENTER) { 
    
    if (song && !song.isPlaying()) {
      song.play();
    }
    started = true;
    
    let c = document.querySelector('canvas');
    if (c && c.requestFullscreen) {
      c.requestFullscreen().catch(() => {});
    } else if (c && c.webkitRequestFullscreen) {
      c.webkitRequestFullscreen();
    } else if (c && c.mozRequestFullScreen) {
      c.mozRequestFullScreen();
    } else if (c && c.msRequestFullscreen) {
      c.msRequestFullscreen();
    }
  }

  
  if (key === '1') manualHue = 220;
  if (key === '2') manualHue = 280;
  if (key === '3') manualHue = 360;
  if (key === '0') { 
    manualHue = -1; 
    godModeForced = false; 
  }
  if (key === 'g' || key === 'G') godModeForced = !godModeForced;

  if (key === 'u' || key === 'U') setMode(0);
  if (key === 'i' || key === 'I') setMode(1);
  if (key === 'o' || key === 'O') setMode(2);
  if (key === 'p' || key === 'P') setMode(3);
  if (key === 'y' || key === 'Y') setMode(4);

  if (key === 'r' || key === 'R') {
    randomMode = !randomMode;
    if (randomMode) {
      lastRandomChange = frameCount - randomModeInterval; 
    }
  }
}

function setMode(m) {
  currentMode = m;
  
  for (let pp of persistentParticles) {
    if (pp && pp.alive) {
      pp.mode = m;
      
      if (!pp.active) pp.joinDelay = max(3, floor(pp.joinDelay * 0.4));
      
      pp.vx += random(-0.2, 0.2);
      pp.vy += random(-0.2, 0.2);
    }
  }
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
      
      if (silenceTimer > SILENCE_FADE_START) {
        this.particles[i].lifespan -= floor(map(silenceTimer, SILENCE_FADE_START, SILENCE_FADE_START*3, 1, 6));
      }
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

  done() { 
    return (this.exploded && this.particles.every(p => p.lifespan < 20)) || this.ttl < -30; 
  }

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
      // si hay silencio prolongado, reducir lifespan más rápido
      if (silenceTimer > SILENCE_FADE_START) {
        this.lifespan -= map(silenceTimer, SILENCE_FADE_START, SILENCE_FADE_START*3, 2.5, 12);
      } else {
        this.lifespan -= this.gigante ? 3.2 : 2.5;
      }
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
    // si hay silencio prolongado, reducir brillo y opacidad
    let alpha = this.opacity;
    if (silenceTimer > SILENCE_FADE_START) {
      alpha = alpha / lerp(1, SILENCE_MAX_ACCEL, constrain((silenceTimer - SILENCE_FADE_START) / (SILENCE_FADE_START*2), 0, 1));
    }
    fill(0, 0, b, alpha);
    ellipse(this.pos.x, this.pos.y, this.baseSize * pulse);
  }
  done() { return (this.lifespan <= 0 || this.opacity <= 0); }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

```


**Enlace al sketch.**

[https://editor.p5js.org/natalieruizperez/sketches/mvMvWGIZK](https://editor.p5js.org/natalieruizperez/sketches/JzYlo0-Zi)

**Capturas o registros de momentos importantes de la pieza.**



<img width="889" height="749" alt="image" src="https://github.com/user-attachments/assets/323823ab-d2e9-4e93-afa3-6bf20d9bc625" />

<img width="906" height="760" alt="image" src="https://github.com/user-attachments/assets/1a838ee7-8c60-46f0-9a94-2f8a78b3e023" />

<img width="914" height="773" alt="image" src="https://github.com/user-attachments/assets/c6263aca-41a6-4fbb-8541-5a1d9b29d7ba" />

<img width="899" height="768" alt="image" src="https://github.com/user-attachments/assets/5e11391f-c232-4125-96ba-4e3525a0924e" />


<img width="931" height="793" alt="image" src="https://github.com/user-attachments/assets/f26119a2-a9a5-42f9-a358-d16b5fbfad68" />




## Bitácora de reflexión


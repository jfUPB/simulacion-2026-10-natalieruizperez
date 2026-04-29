# Unidad 7

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 

**Palabra elegida**

Cohete

**Justificación conceptual**

La pieza busca representar la ascensión y el desprendimiento. El cohete no es solo un vehículo, es una palabra que se va desintegrando (perdiendo letras) a medida que avanza en su viaje. Conceptualmente, simboliza el proceso de dejar atrás cargas para alcanzar un objetivo (la Luna),

**Análisis de su significado visual y comportamental**

Visualmente use estrellas, diferentes planetas con alto contraste y un fondo negro para imitar el espacio. El cohete está construido por la palabra.

Comportamental: El movimiento es vertical como si se tratase de un cohete real. La velocidad del espacio no es lineal, sino que depende de la energía de los graves, generando una sensación de esfuerzo físico y resistencia mediante vibraciones relacionadas con las frecuencias altas.

**Moodboard o referencias**

<img width="563" height="1000" alt="image" src="https://github.com/user-attachments/assets/17f01e3c-b77a-4ff4-82c8-12393f0cce69" />

<img width="576" height="1024" alt="image" src="https://github.com/user-attachments/assets/c2d974f2-fd47-4fc3-b82e-049723da58b6" />

<img width="900" height="1600" alt="image" src="https://github.com/user-attachments/assets/128aff60-aac6-4559-ba15-326d122b8b17" />


Bocetos

<img width="1080" height="1920" alt="1000005835" src="https://github.com/user-attachments/assets/59ec0663-6d27-4e13-9dfa-7bd06ca9fed5" />

<img width="1080" height="1920" alt="1000005834" src="https://github.com/user-attachments/assets/2d9bf995-9a53-4087-822e-54cc0d84a8fa" />


**Mapa de decisiones**

Despegue: El cohete parte de la tierra para comenzar el viaje.

Propulsión: Si los graves > 100, la velocidad del espacio aumenta proporcionalmente.

Planetas Volcánicos: Se generan cada 45 frames solo si la velocidad es alta. Su color se decide por los medios si es < 90 esa marillo, si está entre 90 - 170 es naranja y si es > 170 es rojo, lo hice pensando en que como hay más energía sería una especie de erupción volcánica.

Aterrizaje: Si la canción termina o el volumen baja de 0.005 tras haber comenzado, se activa el modoAterrizaje y la luna aparece en pantalla.

Vibración: El "shaking" del cohete aumenta con las frecuencias agudas para simular turbulencia.

Cambio de colores en la llama: De esta forma la obra toma mas vida y deja de ser monótona.

Desprendimiento: Para simular un cohete de verdad.

**Mapa de interpretación**

Enter: Activa el audio, el modo pantalla completa y despega el cohete.

Espacio: Desprende la última letra del cohete, convirtiéndola en un objeto físico que cae y desaparece.

B: Lanza una estrella fugaz con trayectoria,

Mouse click y drag: Cambia manualmente el color la llama, el hue se modiffica cuando se mueve el mouse de forma horizontal y la saturacion cuando se mueve vertical según la posición en pantalla.

**Explicación de la relación entre audio y comportamiento**

La música dicta la física del entorno. Los graves controlan la velocidad a la que pasan las estrellas y planetas para simular aceleración. Los medios afectan el pulso de los cráteres y el color de los planetas, mientras que los agudos ensucian el fuego con ruido visual y hacen vibrar el cohete. El silencio final es la señal para encontrar la Luna y aterrizar.

**Evidencia del uso de IA**
Usé IA para integrar Matter.js con p5.js, específicamente para gestionar la memoria y crear la clase de las estrellas fugaces con rastro. También para el comportamiento de los planetas y que el lerp de la velocidad se sintiera orgánico y no a saltos. Además la use para la lógica del desprendimiento de las letras, la IA me ayudó a que al presionar espacio la letra se convierta en un cuerpo físico con gravedad y rotación independiente. El concepto de la palabra desintegrándose y la visual son míos, además de las decisiones relacionadas con los cambios de colores y tamaños.

**Código fuente**

```p5js

const { Engine, World, Bodies, Body } = Matter;

let engine, world;
let coheteBloque;
let palabraOriginal = "COHETE";
let letrasRestantes = "COHETE"; 
let letrasCaidas = []; 

let song, fft, amplitude;
let started = false;
let songHasStartedPlaying = false; 

let estrellas = [];
let polvoEspacial = []; 
let obstaculos = []; 
let estrellasFugaces = []; 
let velocidadEspacio = 0;

let tierra, luna;
let modoAterrizaje = false; 
let aterrizado = false; 
let currentHue = 20;   
let currentSat = 90;   

let lowEnergySuave = 0;
let midEnergySuave = 0;

function preload() {
  song = loadSound('rocket.mp3');
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  engine = Engine.create();
  world = engine.world;
  world.gravity.y = 0; 

  fft = new p5.FFT();
  fft.setInput(song);
  amplitude = new p5.Amplitude();
  amplitude.setInput(song);

  initFisica();
  initFondo();

  tierra = { x: width / 2, y: height * 0.5 + 400, size: 1500 };
  luna = { x: width / 2, y: height + 1000, size: 1400 };
}

function initFisica() {
  if (coheteBloque) World.remove(world, coheteBloque);
  coheteBloque = Bodies.rectangle(width / 2, height * 0.5, 60, 300, {
    frictionAir: 0.1,
    inertia: Infinity
  });
  World.add(world, coheteBloque);
}

function initFondo() {
  estrellas = [];
  polvoEspacial = [];
  for (let i = 0; i < 200; i++) {
    estrellas.push({ x: random(width), y: random(height), s: random(0.5, 2.5) });
  }
  for (let i = 0; i < 35; i++) {
    polvoEspacial.push({ x: random(width), y: random(height), s: random(3, 6), col: random(80, 120) });
  }
}

function draw() {
  if (!started) {
    background(2);
    fill(255);
    textAlign(CENTER);
    textSize(20);
    return;
  }

  background(3, 3, 10); 
  fft.analyze();
  
  let lowRaw = fft.getEnergy("bass");
  let midRaw = fft.getEnergy("mid");
  let highRaw = fft.getEnergy("treble");
  let volActual = amplitude.getLevel();

  if (song.isPlaying() && volActual > 0.01) {
    songHasStartedPlaying = true;
  }

  lowEnergySuave = lerp(lowEnergySuave, lowRaw, 0.15);
  midEnergySuave = lerp(midEnergySuave, midRaw, 0.15);

  Engine.update(engine);

  let propulsionDestino = (song.isPlaying() && volActual > 0.001) ? map(lowRaw, 100, 255, 1, 30, true) : 0;
  velocidadEspacio = lerp(velocidadEspacio, propulsionDestino, 0.05); 

  gestionarFondo();
  dibujarTierra();
  
  gestionarObjetosFisicos(midRaw, highRaw);

  if (songHasStartedPlaying && (!song.isPlaying() || volActual < 0.005)) {
    modoAterrizaje = true; 
  }

  if (modoAterrizaje) {
    dibujarLuna();
  }

  for (let i = estrellasFugaces.length - 1; i >= 0; i--) {
    estrellasFugaces[i].update();
    estrellasFugaces[i].display();
    if (estrellasFugaces[i].isDead()) estrellasFugaces.splice(i, 1);
  }

  dibujarLetrasCaidas();
  
  let vibracionEscala = map(highRaw, 0, 255, 0, 2.5) * (velocidadEspacio / 10);
  actualizarCoheteYDibujar(highRaw, lowEnergySuave, midEnergySuave, vibracionEscala);
}

function dibujarLuna() {
  let destinoY = height * 0.5 + 400;
  if (luna.y > destinoY) {
    luna.y -= 2.5; 
  } else {
    aterrizado = true;
  }

  push(); 
  translate(luna.x, luna.y); 
  noStroke();
  for (let i = 8; i > 0; i--) { 
    fill(200, 220, 255, map(i, 0, 8, 0, 20)); 
    ellipse(0, 0, luna.size + (i * 20)); 
  }
  fill(180, 183, 185); 
  ellipse(0, 0, luna.size);
  fill(150, 153, 155); 
  ellipse(-200, -100, 150); 
  ellipse(150, 50, 100); 
  ellipse(50, -250, 80);
  fill(0, 40); 
  ellipse(0, -40, luna.size);
  pop();
}

function gestionarObjetosFisicos(midEnergy, highEnergy) {
  let estamosAterrizando = (luna.y < height + 600);
  
  if (!estamosAterrizando && tierra.y > height + 200 && frameCount % 45 === 0 && velocidadEspacio > 5) {
    let randomVal = random();
    let tipo = randomVal > 0.90 ? "PLANETA_VOLCAN" : (randomVal > 0.80 ? "PLANETA_GRIS" : "METEORITO");
    let esPlaneta = tipo.includes("PLANETA");
    let size = esPlaneta ? random(300, 550) : random(20, 60);
    let crateres = [];
    let colorAsignado;

    if (tipo === "PLANETA_VOLCAN") {
      // IMPLEMENTACIÓN DE COLORES RANDOM EN TONOS CÁLIDOS
      // Según la intensidad, el "rango" de aleatoriedad cambia
      if (midEnergy < 90) {
        // AMARILLOS/OCRES (Calma)
        colorAsignado = color(random(180, 220), random(140, 180), random(40, 80));
      } else if (midEnergy < 170) {
        // NARANJAS (Medio)
        colorAsignado = color(random(180, 240), random(80, 120), random(20, 50));
      } else {
        // ROJOS/FUEGO (Intenso)
        colorAsignado = color(random(150, 200), random(20, 50), random(20, 40));
      }

      let numC = floor(random(8, 12));
      for(let i = 0; i < numC; i++) {
        let relSize = random(0.08, 0.18);
        let angulo = random(TWO_PI);
        let dist = random(0, (size/2) * 0.7); 
        crateres.push({ relX: cos(angulo) * dist, relY: sin(angulo) * dist, relSize: relSize });
      }
    } else {
        // Color para meteoritos o planetas grises
        colorAsignado = color(random(100, 150), random(100, 140), random(140, 180));
    }

    obstaculos.push({ 
      x: random(width), y: esPlaneta ? -700 : -100, tipo: tipo, size: size, crateres: crateres, 
      col: colorAsignado
    });
  }

  for (let i = obstaculos.length - 1; i >= 0; i--) {
    let o = obstaculos[i];
    push(); translate(o.x, o.y); noStroke();
    
    if (o.tipo === "METEORITO") {
      fill(o.col);
      // Efecto de brillo reactivo en meteoritos
      let brilloExtra = map(highEnergy, 0, 255, 0, 30);
      ellipse(0, 0, o.size);
      fill(0, 80); ellipse(o.size * 0.1, o.size * 0.1, o.size * 0.95);
    } 
    else if (o.tipo === "PLANETA_VOLCAN") {
      fill(o.col); 
      ellipse(0, 0, o.size);
      fill(0, 40); ellipse(o.size * 0.05, o.size * 0.05, o.size * 0.9);
      
      let pulsoCrater = map(midEnergy, 0, 255, 1, 1.1);
      for (let c of o.crateres) { 
        fill(30, 10, 10, 180); 
        ellipse(c.relX, c.relY, (o.size * c.relSize) * pulsoCrater); 
      }
    } 
    else {
      fill(140, 145, 150); ellipse(0, 0, o.size);
      fill(0, 60); ellipse(o.size * 0.1, o.size * 0.1, o.size * 0.95);
    }
    pop();
    o.y += velocidadEspacio;
    if (o.y > height + o.size + 200) obstaculos.splice(i, 1);
  }
}

function actualizarCoheteYDibujar(highRaw, lowSuave, midSuave, vibracion) {
  Body.setPosition(coheteBloque, { 
    x: (width / 2) + random(-vibracion, vibracion), 
    y: (height * 0.5) + random(-vibracion, vibracion) 
  });
  
  let pos = coheteBloque.position;
  if (mouseIsPressed) {
    currentHue = map(mouseX, 0, width, 0, 360);
    currentSat = map(mouseY, 0, height, 100, 0); 
  }

  push(); translate(pos.x, pos.y); textAlign(CENTER, CENTER); textSize(48); textFont('monospace'); textStyle(BOLD);
  fill(220, 220, map(highRaw, 0, 255, 180, 255));
  let paso = 300 / palabraOriginal.length;
  let inicioY = -150 + paso / 2;
  for (let i = 0; i < letrasRestantes.length; i++) { text(letrasRestantes[i], 0, inicioY + (i * paso)); }
  
  if (letrasRestantes.length > 0 && lowSuave > 20 && velocidadEspacio > 0.5) {
    drawFuegoPropulsion(lowSuave, midSuave, inicioY + (letrasRestantes.length - 1) * paso + 35);
  }
  pop();
}

function drawFuegoPropulsion(low, mid, offset) {
  push(); 
  colorMode(HSB, 360, 100, 100, 1);
  
  let highRaw = fft.getEnergy("treble");
  let ruidoMusica = map(highRaw, 0, 255, 0, 18); 
  let largoLlama = map(low, 20, 255, 10, 450, true) * (velocidadEspacio / 15);
  let anchoBase = map(mid, 0, 255, 20, 75, true);

  for (let i = 0; i < 15; i++) {
    let xNoise = random(-ruidoMusica, ruidoMusica);
    let brillo = map(i, 0, 15, 100, 30);
    let opacidad = map(i, 0, 15, 0.8, 0);
    
    fill(currentHue, currentSat, brillo, opacidad);
    
    let segmentH = (largoLlama / (i + 1.5)) * 2;
    let anchoSegmento = (anchoBase * map(i, 0, 15, 1, 0.2)) + random(-ruidoMusica, ruidoMusica);
    
    ellipse(xNoise, offset + (i * 12) + (segmentH * 0.5), anchoSegmento, segmentH);
    
    if (highRaw > 180 && random() > 0.7) {
      fill(currentHue, 20, 100, 0.6);
      ellipse(random(-25, 25), offset + random(0, largoLlama), random(3, 8));
    }
  }
  pop();
}

function dibujarLetrasCaidas() {
  push(); textFont('monospace'); textStyle(BOLD); textAlign(CENTER, CENTER);
  for (let i = letrasCaidas.length - 1; i >= 0; i--) {
    let l = letrasCaidas[i];
    push(); translate(l.x, l.y); rotate(l.rot);
    fill(255, l.alpha); textSize(l.size); text(l.letra, 0, 0);
    pop();
    l.velCaida = lerp(l.velCaida, velocidadEspacio * 0.4, 0.02); 
    l.y += l.velCaida; l.x += l.velX; l.rot += l.velRot;             
    l.alpha -= 0.8; l.size = lerp(l.size, 10, 0.005); 
    if (l.alpha <= 0) letrasCaidas.splice(i, 1);
  }
  pop();
}

function dibujarTierra() {
  if (tierra.y < height + tierra.size) {
    push(); noStroke();
    for (let i = 5; i > 0; i--) { fill(50, 150, 255, map(i, 0, 5, 0, 30)); ellipse(tierra.x, tierra.y, tierra.size + (i * 15)); }
    fill(20, 80, 220); ellipse(tierra.x, tierra.y, tierra.size);
    fill(34, 139, 34); ellipse(tierra.x - 200, tierra.y - 150, tierra.size * 0.4, tierra.size * 0.3);
    pop(); tierra.y += velocidadEspacio + 3; 
  }
}

function gestionarFondo() {
  noStroke(); fill(255);
  for (let e of estrellas) { ellipse(e.x, e.y, e.s); e.y += velocidadEspacio * 0.2; if (e.y > height) { e.y = 0; e.x = random(width); } }
  for (let p of polvoEspacial) { fill(p.col, 150); ellipse(p.x, p.y, p.s); p.y += velocidadEspacio * 0.6; if (p.y > height) { p.y = 0; p.x = random(width); } }
}

function keyPressed() {
  if (!started && keyCode === ENTER) {
    let fs = fullscreen();
    fullscreen(!fs);
    setTimeout(() => { resizeCanvas(windowWidth, windowHeight); reajustarTodo(); }, 250);
    if (song && !song.isPlaying()) song.play();
    started = true;
  }
  
  if (started && key === ' ' && letrasRestantes.length > 0) {
    let posC = coheteBloque.position;
    let yL = posC.y + (-150 + 25) + (letrasRestantes.length - 1) * (300/palabraOriginal.length);
    letrasCaidas.push({ 
      letra: letrasRestantes.charAt(letrasRestantes.length - 1), 
      x: posC.x, y: yL, alpha: 255, size: 48, rot: 0,
      velX: random(-0.8, 0.8), velRot: random(-0.03, 0.03), velCaida: 0 
    });
    letrasRestantes = letrasRestantes.substring(0, letrasRestantes.length - 1);
  }
  if (started && (key === 'b' || key === 'B')) estrellasFugaces.push(new EstrellaFugaz());
}

function windowResized() { resizeCanvas(windowWidth, windowHeight); reajustarTodo(); }
function reajustarTodo() { tierra.x = width / 2; luna.x = width / 2; initFisica(); initFondo(); }

class EstrellaFugaz {
  constructor() {
    let b = floor(random(3)); 
    if (b === 0) { this.x = random(width); this.y = -10; this.vx = random(-5, 5); this.vy = random(7, 12); }
    else if (b === 1) { this.x = width + 10; this.y = random(height * 0.5); this.vx = random(-10, -15); this.vy = random(4, 8); }
    else { this.x = -10; this.y = random(height * 0.5); this.vx = random(10, 15); this.vy = random(4, 8); }
    this.size = random(2, 5); this.alpha = 255; this.history = [];
    this.col = color(random(200, 255), random(220, 255), 255);
  }
  update() {
    this.history.push({ x: this.x, y: this.y }); if (this.history.length > 20) this.history.splice(0, 1);
    this.x += this.vx; this.y += this.vy; this.alpha -= 3.5;
  }
  display() {
    noStroke();
    for (let i = 0; i < this.history.length; i++) {
      fill(red(this.col), green(this.col), blue(this.col), map(i, 0, this.history.length, 0, this.alpha));
      ellipse(this.history[i].x, this.history[i].y, map(i, 0, this.history.length, 0, this.size));
    }
    fill(255, this.alpha); ellipse(this.x, this.y, this.size);
  }
  isDead() { return (this.alpha <= 0 || this.y > height + 100); }
}

```

**Enlace al sketch**

https://editor.p5js.org/natalieruizperez/sketches/YS6z_MQfl

**Capturas o registros de la pieza**

<img width="1756" height="1079" alt="image" src="https://github.com/user-attachments/assets/5b579240-5140-41a5-8e30-9ef3ed716759" />

<img width="1827" height="1074" alt="image" src="https://github.com/user-attachments/assets/5af449af-888a-405c-af17-3d8ded8b8844" />





## Bitácora de reflexión

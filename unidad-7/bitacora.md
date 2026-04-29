# Unidad 7

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 

**Palabra elegida**

Rocket

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

Despegue: El cohete arranca desde la Tierra para empezar el viaje.

Propulsión: Los graves controlan la velocidad. Si superan los 100, la velocidad del espacio sube proporcionalmente.

Obstáculos Espaciales: Se generan meteoritos cada 45 frames, pero solo si tiene cierta velocidad.

Fuego Inteligente: La llama no se corta en seco. Si la música para, la velocidad y el fuego bajan poco a poco con un fade out (usando lerp) para que se sienta más natural.

Aterrizaje: Cuando la canción termina y el volumen cae de 0.005, el código espera a que la llama se disipe casi por completo antes de activar el modoAterrizaje y mostrar la Luna.

Vibración: El "shaking" del cohete reacciona a los agudos para simular la turbulencia real de un motor.

Efecto de Esporas: Al aterrizar o frenar de golpe, salen partículas de humo desde la última letra de la palabra.

Desprendimiento: La letra que se desprende recibe un impulso lateral aleatorio y una rotación constante para simular la pérdida de inercia en el vacío.



**Mapa de interpretación**

Enter: Activa el audio, el modo pantalla completa y despega el cohete.

Espacio: Desprende la última letra del cohete, convirtiéndola en un objeto físico que cae y desaparece.


**Explicación de la relación entre audio y comportamiento**

Los graves mueven las estrellas y el fondo para que parezca que acelera. Los medios suavizan los movimientos, y los agudos sirven para crear ruido y vibración en el bloque del cohete. El silencio final es la instrucción para que el sistema busque la Luna y el cohete descanse.

**Evidencia del uso de IA**

Usé IA para meter Matter.js dentro de p5.js, sobre todo para que la gestión de memoria y para que el desprendimiento de las letras funcionara bien (que cada letra sea un cuerpo físico con rotación propia). También la usé para pulir el comportamiento orgánico de la velocidad, quería que el paso de "volar" a "frenar" tuviera una inercia creíble y que la llama se disipara suavemente en lugar de desaparecer como un interruptor. El concepto de la palabra desintegrándose y la visual y el planteamiento de la idea exacta son míos.


**Código fuente**

```js

const { Engine, World, Bodies, Body } = Matter;

let engine, world;
let coheteBloque;
let palabraOriginal = "ROCKET";
let letrasRestantes = "ROCKET"; 
let letrasCaidas = []; 
let tiempoInicioLetras; 

let song, fft, amplitude;
let started = false;
let estaListo = false; 
let songHasStartedPlaying = false; 

let estrellas = [];
let particulasEsporas = []; 
let obstaculos = []; 
let velocidadEspacio = 0;

let tierra, luna;
let modoAterrizaje = false; 
let aterrizado = false; 

let currentHue = 20;   
let currentSat = 90;   

let lowEnergySuave = 0;
let midEnergySuave = 0;
let yFinalCohete = 0; 

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
  luna = { x: width / 2, y: height + 2500, size: 2000 };
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
  for (let i = 0; i < 200; i++) {
    estrellas.push({ x: random(width), y: random(height), s: random(0.5, 2.5) });
  }
}

function draw() {
  if (!started) {
    background(2);
    return;
  }

  background(3, 3, 10); 
  fft.analyze();
  
  let lowRaw = fft.getEnergy("bass");
  let midRaw = fft.getEnergy("mid");
  let highRaw = fft.getEnergy("treble");
  let volActual = amplitude.getLevel();

  let tiempoTranscurrido = (millis() - tiempoInicioLetras) / 1000;
  if (tiempoTranscurrido >= palabraOriginal.length + 1.5) estaListo = true; 

  if (song.isPlaying() && volActual > 0.01) songHasStartedPlaying = true;

  let velocidadPrevia = velocidadEspacio;
  lowEnergySuave = lerp(lowEnergySuave, lowRaw, 0.15);
  midEnergySuave = lerp(midEnergySuave, midRaw, 0.15);

  Engine.update(engine);

  let propulsionDestino = (estaListo && !modoAterrizaje && song.isPlaying() && volActual > 0.001) 
    ? map(lowRaw, 100, 255, 1, 30, true) 
    : 0;
  
  velocidadEspacio = lerp(velocidadEspacio, propulsionDestino, 0.05); 

  gestionarFondo();
  dibujarTierra();
  if (modoAterrizaje) dibujarLuna();
  gestionarObjetosFisicos();

  if (estaListo && songHasStartedPlaying && (!song.isPlaying() || volActual < 0.005)) {
    if (velocidadEspacio < 0.5) modoAterrizaje = true; 
  }

  if (modoAterrizaje || (estaListo && velocidadPrevia > 5 && velocidadEspacio < 2)) generarEsporas();

  gestionarEsporas();
  dibujarLetrasCaidas();
  
  let vibracionEscala = estaListo ? (map(highRaw, 0, 255, 0, 2.5) * (velocidadEspacio / 10)) : 0;
  actualizarCoheteYDibujar(highRaw, lowEnergySuave, midEnergySuave, vibracionEscala, tiempoTranscurrido);
}

function drawFuegoPropulsion(low, mid, offset) {
  push(); colorMode(HSB, 360, 100, 100, 1);
  let largoLlama = map(low, 20, 255, 10, 450, true) * (velocidadEspacio / 15);
  let anchoBase = map(mid, 0, 255, 20, 75, true);
  for (let i = 0; i < 15; i++) {
    fill(currentHue, currentSat, map(i, 0, 15, 100, 30), map(i, 0, 15, 0.8, 0));
    let segmentH = (largoLlama / (i + 1.5)) * 2;
    ellipse(random(-5, 5), offset + (i * 12) + (segmentH * 0.5), anchoBase * map(i, 0, 15, 1, 0.2), segmentH);
  } pop();
}

function actualizarCoheteYDibujar(highRaw, lowSuave, midSuave, vibracion, tiempo) {
  Body.setPosition(coheteBloque, { 
    x: (width / 2) + random(-vibracion, vibracion), 
    y: (height * 0.5) + random(-vibracion, vibracion) 
  });
  
  let pos = coheteBloque.position;
  let pasoV = 300 / palabraOriginal.length;
  let ultimaY = -150 + (letrasRestantes.length - 1) * pasoV + pasoV / 2;

  push(); 
  translate(pos.x, pos.y); 

  if (estaListo && !modoAterrizaje && velocidadEspacio > 0.1 && letrasRestantes.length > 0) {
    drawFuegoPropulsion(lowSuave, midSuave, ultimaY + 35);
  }

  textAlign(CENTER, CENTER); textSize(48); textFont('monospace'); textStyle(BOLD);

  for (let i = 0; i < letrasRestantes.length; i++) {
    let tAnim = constrain((tiempo - (5 - i)) / 0.8, 0, 1);
    let suavizado = tAnim * tAnim * (3 - 2 * tAnim);
    let xActual = lerp((i - 5) * 45, 0, suavizado);
    let yActual = lerp(-150 + (5 * pasoV) + (pasoV / 2), -150 + (i * pasoV) + (pasoV / 2), suavizado);

    push();
    colorMode(HSB, 360, 100, 100);
    if (tiempo > palabraOriginal.length) {
        let factorFuego = map(i, 0, 5, 0.1, 1);
        let intensidadPropulsion = estaListo ? map(lowSuave, 0, 255, 0.2, 1) : 0.4;
        fill(lerp(0, currentHue, factorFuego), lerp(0, currentSat, factorFuego * intensidadPropulsion), lerp(90, 100, factorFuego));
    } else fill(0, 0, 100); 

    text(letrasRestantes[i], xActual, yActual);
    pop();
  }
  
  yFinalCohete = pos.y + ultimaY + 35;
  pop();
}

function dibujarLetrasCaidas() {
  push(); textAlign(CENTER, CENTER); textFont('monospace'); textStyle(BOLD);
  for (let i = letrasCaidas.length - 1; i >= 0; i--) {
    let l = letrasCaidas[i];
    push(); 
    translate(l.x, l.y); 
    rotate(l.rot); 
    fill(255, l.alpha); 
    textSize(l.size); 
    text(l.letra, 0, 0);
    pop();

    // Movimiento de deriva espacial
    l.x += l.vx; 
    l.y += l.vy; 
    l.rot += l.rotVel; 
    
    // Fricción sutil para estabilizar el movimiento
    l.vx *= 0.99; 
    l.vy *= 0.99; 
    l.alpha -= 1.0; 

    if (l.alpha <= 0) letrasCaidas.splice(i, 1);
  }
  pop();
}

function dibujarTierra() {
  if (tierra.y < height + tierra.size) {
    push(); noStroke();
    for (let i = 8; i > 0; i--) { fill(50, 150, 255, map(i, 0, 8, 0, 20)); ellipse(tierra.x, tierra.y, tierra.size + (i * 25)); }
    fill(20, 80, 220); ellipse(tierra.x, tierra.y, tierra.size); 
    fill(34, 139, 34); 
    ellipse(tierra.x - 200, tierra.y - 150, tierra.size * 0.4, tierra.size * 0.3);
    ellipse(tierra.x + 150, tierra.y - 50, tierra.size * 0.3, tierra.size * 0.4);
    pop();
    if (estaListo && !modoAterrizaje) tierra.y += velocidadEspacio + 3; 
  }
}

function dibujarLuna() {
  let puntoDetencion = height * 0.5 + 850; 
  if (luna.y > puntoDetencion) { luna.y = lerp(luna.y, puntoDetencion - 5, 0.08); } 
  else { luna.y = puntoDetencion; aterrizado = true; }
  push(); translate(luna.x, luna.y); noStroke();
  for (let i = 8; i > 0; i--) { fill(200, 220, 255, map(i, 0, 8, 0, 15)); ellipse(0, 0, luna.size + (i * 20)); }
  fill(180, 183, 185); ellipse(0, 0, luna.size);
  fill(150, 153, 155); ellipse(-200, -100, 150); ellipse(150, 50, 100);
  fill(0, 40); ellipse(0, -40, luna.size);
  pop();
}

function gestionarObjetosFisicos() {
  if (modoAterrizaje || !estaListo) return;
  if (tierra.y > height + 200 && frameCount % 45 === 0 && velocidadEspacio > 5) {
    obstaculos.push({ x: random(width), y: -100, size: random(20, 60), col: color(random(100, 180)) });
  }
  for (let i = obstaculos.length - 1; i >= 0; i--) {
    let o = obstaculos[i]; fill(o.col); noStroke(); ellipse(o.x, o.y, o.size);
    o.y += velocidadEspacio; if (o.y > height + 200) obstaculos.splice(i, 1);
  }
}

function gestionarFondo() {
  noStroke(); fill(255);
  for (let e of estrellas) { 
    ellipse(e.x, e.y, e.s); 
    if (estaListo && !modoAterrizaje) e.y += velocidadEspacio * 0.2; 
    if (e.y > height) e.y = 0;
  }
}

class Espora {
  constructor(x, y) {
    this.x = x; this.y = y;
    this.vx = random(-5, 5); this.vy = random(1, 4);
    this.alpha = 255; this.size = random(5, 15);
    this.decay = random(2, 5);
  }
  update() { this.x += this.vx; this.y += this.vy; this.vx *= 0.96; this.alpha -= this.decay; }
  display() { noStroke(); fill(200, 210, 220, this.alpha); ellipse(this.x, this.y, this.size); }
}

function generarEsporas() {
  if (!aterrizado) {
    for (let i = 0; i < 6; i++) {
      particulasEsporas.push(new Espora(width / 2, yFinalCohete));
    }
  }
}

function gestionarEsporas() {
  for (let i = particulasEsporas.length - 1; i >= 0; i--) {
    particulasEsporas[i].update(); 
    particulasEsporas[i].display();
    if (particulasEsporas[i].alpha <= 0) particulasEsporas.splice(i, 1);
  }
}

function keyPressed() {
  if (!started && keyCode === ENTER) {
    let fs = fullscreen();
    fullscreen(!fs);
    setTimeout(() => { resizeCanvas(windowWidth, windowHeight); reajustarTodo(); }, 250);
    tiempoInicioLetras = millis(); 
    if (song && !song.isPlaying()) song.play();
    started = true;
  }
  
  if (started && key === ' ' && letrasRestantes.length > 0) {
    let posC = coheteBloque.position;
    let pasoV = 300 / palabraOriginal.length;
    let yDesprendimiento = posC.y + (-150 + (letrasRestantes.length - 1) * pasoV + pasoV / 2);
    
    // Impulso espacial suave y elegante
    letrasCaidas.push({ 
      letra: letrasRestantes.charAt(letrasRestantes.length - 1), 
      x: posC.x, 
      y: yDesprendimiento, 
      vx: random(-2, 2),        
      vy: random(1, 2.5),         
      rotVel: random(-0.05, 0.05), 
      rot: 0, 
      alpha: 255, 
      size: 48 
    });
    letrasRestantes = letrasRestantes.substring(0, letrasRestantes.length - 1);
  }
}

function windowResized() { resizeCanvas(windowWidth, windowHeight); reajustarTodo(); }
function reajustarTodo() { tierra.x = width / 2; luna.x = width / 2; initFisica(); initFondo(); }
```

**Enlace al sketch**

https://editor.p5js.org/natalieruizperez/sketches/XFFqPN10t

**Capturas o registros de la pieza**

<img width="1595" height="898" alt="Screenshot 2026-04-29 132056" src="https://github.com/user-attachments/assets/55fc7b9c-5402-441f-93f3-fdf739f1c88c" />

<img width="1483" height="1079" alt="Screenshot 2026-04-29 132104" src="https://github.com/user-attachments/assets/b8673abf-1bee-4f85-b3b3-320a082c332f" />

<img width="1542" height="966" alt="Screenshot 2026-04-29 132123" src="https://github.com/user-attachments/assets/edc21eb8-482e-402d-84ed-67f507e7a39a" />

<img width="1735" height="1079" alt="Screenshot 2026-04-29 132152" src="https://github.com/user-attachments/assets/b2c955a9-538c-4083-8099-98ae2b3f9b92" />









## Bitácora de reflexión

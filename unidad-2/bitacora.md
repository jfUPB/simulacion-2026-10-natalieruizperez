# Unidad 2

## Bitácora de proceso de aprendizaje
### Actividad 01

<img width="780" height="557" alt="image" src="https://github.com/user-attachments/assets/8e18c18f-5191-4b5f-b220-bd1fe0a6a34f" />

<img width="640" height="180" alt="image" src="https://github.com/user-attachments/assets/3571009b-9995-4d20-ac9b-c873690248ee" />

<img width="833" height="646" alt="image" src="https://github.com/user-attachments/assets/be179af7-6bc6-4409-a45c-f7e598559b29" />

### Actividad 02
**1. ¿Cómo funciona la suma dos vectores en p5.js?**

La suma de vectores funciona componente a componente.

**2. ¿Por qué esta línea position = position + velocity; no funciona?**

No se puede hacer la suma de vectores porque ese operador no soporta la suma.

### Activdad 03

**1. ¿Qué tuviste que hacer para hacer la conversión propuesta?**

Agrupé la posición en un vector y tomé cada una de ellas según lo que necesitaba.

**Código**

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.position = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0);
    point(this.position);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.position.x++;
    } else if (choice == 1) {
      this.position.x--;
    } else if (choice == 2) {
      this.position.y++;
    } else {
      this.position.y--;
    }
  }
}
```

### Activdad 04

Los vectores se pasan por referencia y no es una copia de posición si no la dirección de memoria de un objeto (puntero).

**1. ¿Qué resultado esperas obtener en el programa anterior?**

Espero que se muestre en pantalla la posición del vector como texto.

**2. ¿Qué resultado obtuviste?**

Al final obtuve el vector 20,30 en vez del inicial.

**3. Recuerda los conceptos de paso por valor y paso por referencia en programación.**

Cuando es por valor copia ese valor pero cuando es la referencia al modificarlo, modifica también el porque la referencia porque le pasa el lugar de la memoria.

**4. ¿Qué tipo de paso se está realizando en el código?**

El código se pasa por referencia.

**5. ¿Qué aprendiste?**

Pude refrescar mis conocimientos respecto a que era el paso por memoria y por referencia, además entender que esto también se puede hacer con vectores.

### Actividad 05

**1. ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?**

El método mag() sirve para saber cuál es la magnitud y magSq() es para obtener la magnitud al cuadrado.

**2. ¿Para qué sirve el método normalize()?**

El método normalize sirve para volver unitario el vector.

**3. Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?**

Sirve para saber el ángulo entre los vectores, si son paralelos, perpendiculares y proyectar uno sobre otro.

**4. El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?**

Los dos hacen lo mismo pero se llaman de forma diferente.

**5. Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.**

Al hacer el producto cruz de dos vectores se obtiene un nuevo vector que es perpendicular a los vectores, tiene la misma magnitud que ellos y la dirección depende también de los demás.

**6. ¿Para que te puede servir el método dist()?**

Para la distancia entre puntos.

**7. ¿Para qué sirven los métodos normalize() y limit()?**

Limit es para limitar el vector y no sea más grande y normalize para que el vector sea unitario, entonces limit se puede usar para controlar la velocidad y normalize para preocuparse solo por la dirección.

### Actividad 07
**1. Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.**

El concepto de marco motion 101 consiste en tomar primero la velocidad y después se obtiene la posición. Se hace al contrario a lo intuitivo que sería realizar la integración de euler  se conoce como cuando se integra la posición para hallar velocidad y velocidad para hallar aceleración. Nosotros usamos a semi-implicit y se toma desde uno en lugar de 0. La de verlet se usa cuando las partículas están concectadas por constraints. El marco motion 101 se interpreta geométricamente a través de vectores que tienen la posición y la velocidad.

**2. ¿Cómo se aplica motion 101 en el ejemplo?**

En el ejemplo se aplica obteniendo la posición usmando la velocidad y la velocidad sumando la aceleración que es constante y se consigue creando un vector que está un poco inclinado.

### Actividad 08

**Qué observaste cuando usas cada una de las aceleraciones propuestas?**

En la aceleración constante inicialmente empezará lento pero a medida que pasa el tiempo como la velocidad se recalcula con la aceleración, a pesar de la aceleración ser constante esta aumenta. En cambio en la aceleración aleatoria no cambió unicamente la velocidad si no también la dirección. Finalmente en la aceleración hacia el mouse pensé que al acercarse a él aumentaría bastante la velocidad

## Bitácora de aplicación 

**Concepto**
Gotas de lluvia de colores que al apretar click son atraídas a la posición del mouse. Usé aceleración alatoria para generar esa sensación de las gotas cuando caen y también usé aceleración hacia el mouse para hacer que todas se concentraran en una misma parte, decidí eso porque quería hacer algo inesperado. Escogí las gotas de colores aleatorios para que se viera más mágico y fondo negro para que resaltaran.

**Código**

Mover
```js
class Mover {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = createVector();
    this.acceleration = createVector();
    this.topSpeed = 5; 
    this.size = 10;
    this.color = color(random(255), random(255), random(255), 200);
    this.followMouse = false;
  }

  update() {
    if (this.followMouse) {
      
      let mouseVec = createVector(mouseX, mouseY);
      let dir = p5.Vector.sub(mouseVec, this.position); 
      dir.normalize(); 
      dir.mult(0.2);   
      this.acceleration = dir;
    } else {
      
      this.acceleration = createVector(random(-0.05, 0.05), random(0.05, 0.2));
    }

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
  }

  show() {
    stroke(0);
    strokeWeight(1);
    fill(this.color);
    circle(this.position.x, this.position.y, this.size);
  }

  checkEdges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) {
      this.position.y = 0;
      this.position.x = random(width);
      this.velocity.mult(0);
    }
  }
}
```

Sketch
```js
let movers = [];
let total = 200;

function setup() {
  createCanvas(640, 240);
  for (let i = 0; i < total; i++) {
    movers.push(new Mover());
  }
}

function draw() {
  background(0); 

  for (let mover of movers) {
    mover.update();
    mover.checkEdges();
    mover.show();
  }
}

function mousePressed() {
  for (let mover of movers) {
    mover.followMouse = !mover.followMouse;
  }
}
```

**Enlace**
https://editor.p5js.org/natalieruizperez/sketches/x4qJCrbWh


**Capturas**

<img width="638" height="238" alt="image" src="https://github.com/user-attachments/assets/e06d4579-0368-4318-b790-9d99d02b2955" />

<img width="634" height="226" alt="image" src="https://github.com/user-attachments/assets/587ed4d3-1476-4df9-91e1-6dbed42086cd" />


## Bitácora de reflexión

**Concepto**
Círculos rojos y azules que caen y al apretar click los rojos aceleraran hacia el mouse y los azules le alejaran de los rojos. No quería modificar a gran escala el concepto que hice para la actividad anterior entonces conservé lo primordial que era la lluvia y la aceleración hacia el mousey para diferenciar mejor las partículas escogí dos colores rojo y azul.

**Código**

Mover
```js
class Mover {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = createVector();
    this.acceleration = createVector();
    this.topSpeed = 5; 
    this.size = 10;
    this.color = random([color(255, 0, 0, 200), color(0, 0, 255, 200)]);
    this.followMouse = false;
  }

  update(movers) {
    this.acceleration.mult(0); 

    
    if (this.followMouse) {
      let mouseVec = createVector(mouseX, mouseY);
      let dir = p5.Vector.sub(mouseVec, this.position); 
      dir.normalize(); 
      dir.mult(0.2);   
      this.acceleration = dir;
    } else {
      
      this.acceleration = createVector(random(-0.05, 0.05), random(0.05, 0.2));
    }

    
    if (repel && this.color.levels[0] === 0 && this.color.levels[1] === 0 && this.color.levels[2] === 255) {
      for (let other of movers) {
        if (other.color.levels[0] === 255) { 
          let diff = p5.Vector.sub(this.position, other.position);
          let d = diff.mag();
          if (d < 100 && d > 0) { 
            diff.normalize();
            diff.div(d); 
            diff.mult(1);
            this.acceleration.add(diff);
          }
        }
      }
    }

    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topSpeed);
    this.position.add(this.velocity);
  }

  show() {
    stroke(0);
    strokeWeight(1);
    fill(this.color);
    circle(this.position.x, this.position.y, this.size);
  }

  checkEdges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) {
      this.position.y = 0;
      this.position.x = random(width);
      this.velocity.mult(0);
    }
  }
}
```

Sketch
```js

let movers = [];
let total = 200;
let repel = false; 
function setup() {
  createCanvas(640, 240);
  for (let i = 0; i < total; i++) {
    movers.push(new Mover());
  }
}

function draw() {
  background(0); 

  for (let mover of movers) {
    mover.update(movers);
    mover.checkEdges();
    mover.show();
  }
}

function mousePressed() {
  repel = !repel; 
  for (let mover of movers) {
   
    if (mover.color.levels[0] === 255 && mover.color.levels[1] === 0 && mover.color.levels[2] === 0) {
      mover.followMouse = !mover.followMouse;
    }
  }
}
```

**Enlace**
https://editor.p5js.org/natalieruizperez/sketches/-JoYilG4Z

**Capturas**

<img width="637" height="241" alt="image" src="https://github.com/user-attachments/assets/484df725-b511-475e-96db-0472b5f2a715" />

<img width="620" height="234" alt="image" src="https://github.com/user-attachments/assets/b2390db2-0e0b-47d9-8730-a3190c3ffc18" />








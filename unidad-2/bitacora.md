# Unidad 2

## Bitácora de proceso de aprendizaje
### Actividad 01
**Busca inspiración**

### Actividad 02
**1. ¿Cómo funciona la suma dos vectores en p5.js?**

La suma de vectores funciona componente a componente.

**2. ¿Por qué esta línea position = position + velocity; no funciona?**

No se puede hacer la suma de vectores porque ese operador no soporta la suma.

### Activdad 03
**1. ¿Qué tuviste que hacer para hacer la conversión propuesta?**

**Código**

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


### Activdad 04
Los vectores se pasan por referencia y no es una copia de posición si no la dirección de memoria de un objeto (puntero).

**1. ¿Qué resultado esperas obtener en el programa anterior?**
Espero que se muestre en pantalla la posición del vector como texto.

**2. ¿Qué resultado obtuviste?**
**3. Recuerda los conceptos de paso por valor y paso por referencia en programación.**
**4. ¿Qué tipo de paso se está realizando en el código?**
**5. ¿Qué aprendiste?**

### Actividad 05
**1. ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?**
El método mag() sirve para saber cuál es la magnitud y magSq() es para

**2. ¿Para qué sirve el método normalize()?**
El método normalize sirve para 

**3. Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?**
Sirve para saber el ángulo entre los vectores, si son paralelos, perpendiculares y proyectar uno sobre otro.

**4. El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?**

**5. Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.**


**6. ¿Para que te puede servir el método dist()?**


**7. ¿Para qué sirven los métodos normalize() y limit()?**

### Actividad 06
**1. ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?**
**2. ¿Para qué sirve el método normalize()?**
**3. Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?**
**4. El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?**
**5. Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.**
**6. ¿Para que te puede servir el método dist()?**
**7. ¿Para qué sirven los métodos normalize() y limit()?**

### Actividad 07
**1. Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.**
**2. ¿Cómo se aplica motion 101 en el ejemplo?**

### Actividad 08
**Qué observaste cuando usas cada una de las aceleraciones propuestas?**

## Bitácora de aplicación 

Describe el concepto de tu obra generativa. Explica el concepto de tu obra generativa, qué regla aplicaste para la aceleración por qué si fue una decisión de diseño o qué te evoca si fue una exploración artística.
**Código**
**Enlace**
**Capturas**

## Bitácora de reflexión
**Describe el concepto de tu obra generativa. Explica el concepto de tu obra generativa.**
**Código**
**Enlace**
**Capturas**


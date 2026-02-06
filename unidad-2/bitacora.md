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

El concepto de marco motion 101 consiste en tomar primero la velocidad y después se obtiene la posición. Se hace al contrario a lo intuitivo que sería realizar la integración de euler  se conoce como cuando se integra la posición para hallar velocidad y velocidad para hallar aceleración. Nosotros usamos a semi-implicit y se toma desde uno en lugar de 0. La de verlet se usa cuando las partículas están concectadas por constraints. El marco motion 101 se interpreta geométricamente a través de vectores que tienen la posición y la velocidad 

**2. ¿Cómo se aplica motion 101 en el ejemplo?**

En el ejemplo se aplica obteniendo la posición usmando la velocidad y la velocidad sumando la aceleración que es constante y se consigue creando un vector que está un poco inclinado.

### Actividad 08

**Qué observaste cuando usas cada una de las aceleraciones propuestas?**

En la aceleración constante inicialmente empezará lento pero a medida que pasa el tiempo como la velocidad se recalcula con la aceleración, a pesar de la aceleración ser constante esta aumenta. En cambio en la aceleración aleatoria no cambió unicamente la velocidad si no también la dirección. Finalmente en la aceleración hacia el mouse pensé que al acercarse a él aumentaría bastante la velocidad

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




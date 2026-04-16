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

**Enlace al sketch.**

**Capturas o registros de momentos importantes de la pieza.**

### Actividad 07


Antes de ejecutar tu pieza debes explicar en 1 o 2 minutos:

**Cuál es el concepto de tu obra, cómo se relaciona con el tema musical**

**Qué tipo de comportamiento visual diseñaste y de qué manera se interpreta o ejecuta en vivo.**


## Bitácora de reflexión


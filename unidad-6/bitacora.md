# Evidencias de la unidad 6

## Set

### Actividad 1

<img width="1242" height="632" alt="image" src="https://github.com/user-attachments/assets/122d889c-8834-4731-99d4-d42b1055f036" />

Esta imagen me parece muy interesante ya que me recuerda algo que hace mucho no pensaba, y es que las experiencias no siemre tienen que ser un programa y se van o escuchen cosas de una pantalla, sino que tambien pueden ser cosas con incluso robotica.

<img width="1247" height="631" alt="image" src="https://github.com/user-attachments/assets/1ae99c79-c7d3-43fa-9f31-33cb7e39afd7" />

Este frame tambien me llamo mucho la atencion ya que el en su proceso creativo utiliza papel y colores para empezar a crear, y es algo que me gustaria empezar a hacer a futuro para proximos proyectos.

De su trabajo me inspira que el no tiene nunca planeado cual va a ser su punto final, solo como empezar pero no sabe exactamente a que va a llegar ni como.

## Seek 

### Actividad 2

Una steering force es una fuerza que corrige la velocidad de un agente para guiarlo hacia un objetivo o comportamiento deseado.
Se calcula como la diferencia entre la velocidad deseada y la velocidad actual, limitada por una fuerza máxima.

La diferencia es que las fuerzas fisicas provienen de la simulación de la naturaleza. Están definidas por leyes físicas, actúan de manera pasiva sobre el agente y no tienen “intención”. En cambio las steering forces son fuerzas artificiales de comportamiento. No vienen de la física, sino que se calculan para que un agente “decida” hacia dónde moverse (buscar, huir, alinearse). Representan intenciones, no leyes naturales.

La relacion es directa ya que el propuso la idea de que cuando se simularan bandadas de animales en lugar de usar solo fuerzas físicas clásicas, propuso que cada agente tuviera “reglas locales” (separación, alineación, cohesión), implementadas como steering forces que guían su velocidad.

### Actividad 3

2. Se utiliza un array 2D, Cada elemento es un p5.Vector que indica la dirección del flujo en esa celda del espacio, mediante Perlin Noise se calcula el angulo. Para cada celda (i, j) se calcula un ángulo theta = noise(xoff, yoff) * TWO_PI * 2.
Ese ángulo se convierte en un vector unitario con p5.Vector.fromAngle(theta).

3. El vehículo toma su posición actual y convierte esas coordenadas continuas en índices de celda de la cuadrícula del flow field. Primero escala ese vector deseado a la velocidad máxima y despues resta la velocidad actual del vehículo, esto produce un vector que apunta en la dirección en que debe corregir su movimiento.

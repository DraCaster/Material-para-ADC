## Interrupciones

_Este apunte (al igual que los otros) es un apunte personal preparado por mi para que se puedan entender un poco mejor el proceso de atención de interrupciones._

Dudas, consultas: lujanrojas.informatica@gmail.com

### La fuente u origen del pedido de interrupción puede provenir de:

- **Eventos internos**: (Excepciones)

Por ejemplo un error asociado a la ejecución de una instrucción: una instrucción que realiza una operación de división por cero por ejemplo.

Por un fallo en el hardware: perdida de energía por ejemplo.

- **Eventos externos**: (Generados por un periférico) 

por ejemplo una operacion de entrada/salida para indicar la finalización normal de una operación; Una transferencia sobre una impresora.

Estos eventos ya sean internos o externos requieren de la atención del cpu. Para atender la interrupción hay una funcionalidad que permite gestionar estas interrupciones.
Toda gestión de interrupción comienza detectando el pedido de interrupcion. ¿Pero que es lo que sucede? ¿Que tratamiento se le da a la interrupción?

- Se detecta el pedido de interrupción.

- Se detiene la tarea que se estaba ejecutando.

- Se salva el estado del proceso que va a ser interrumpido (Lo mínimo que se debe guardar es el CP o contador de programa).

- Se debe resolver lo que ocacionó el servicio de interrupción.

- Una vez resuelto, se debe reestablecer la tarea interrumpida.

- Se continua con la ejecución normal del programa interrumpido.

_En general el procesador maneja multiples pedidos de interrupciones._

### ¿Qué tipo de interrupciones hay?

- Interrupciones por hardware: son generadas por señales físicas o lógicas asociadas a eventos externos o internos. 

- Interrupciones por software: son instrucciones que tienen un efecto similar a una interrupción por hardware. Generalmente este tipo de interrupciones son atendidas/manejadas por el sistema operativo.

## ¿Que mecanismos existen para la detección del pedido de una interrupción?

- 1era opción: la CPU tiene múltiples líneas de interrupción. Pro: tiende a ser más rápido que otro mecanismo. Contra: es costoso a nivel hardware(más hardware)

- 2da opción: la CPU tiene una única línea de interrupción: Pro: menos hardware. Contra: tiende a ser más lenta respecto a la de múltiples líneas. 

- 3ra opción: interrupciones vectorizadas: la cpu tiene una única línea de interrupción, pero el procesador es capaz de 'leer' qué periférico fué quien solicitó el pedido de interrupción. 

## Interrupciones en el procesador 8086

- Utiliza como mecanismo de detección de interrupciones la tercera opción vista anteriormente. El PIC, que es básicamente un chip conectado al bus del sistema es el que se encarga de gestionar las interrupciones.
Para más información, ver: <a href="https://github.com/DraCaster/Material-para-ADC/blob/master/Interrupciones-EntradaySalida.md#pic-controlador-de-interrupciones-programable"> PIC - Programmable interrupt controller</a>

### Tipos de interrupciones en el procesador 8086

- Interrupciones por hardware: 

NMI (Interrupciones no enmascarables)

INTR (Interrupciones enmascarables)

- Interrupciones por software

Intrucción INT n
Intrucción IRET (Instrucción de retorno de subrutina que atiende las interrupciones)

### Proceso de atención de interrupción en 8086

- Llega una interrupción, se le avisa al PIC

- la cpu ANTES de atender la interrupcion, primero termina de ejecutar la instrucción que ya está cargada.

- Luego verifica si hay interrupciones pendientes y en ese caso atiende la de mayor prioridad.

- La ALU recibe el valor de un periferico recibido, lo multiplica por 4 y lo envia por el bus de direcciones hasta el vector de interrupciones.

- Al producirse la interrupción, se guardan los flags y la dirección de retorno. Esto lo hace el CPU automaticamente.

- Si se producen dos interrupciones a la vez, se atiende la de máxima prioridad(La interrupción de número mas pequeño es la que mayor prioridad  tiene).


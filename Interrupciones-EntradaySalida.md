# **Arquitectura de Computadoras - Material complementario.**
 
Dudas y consultas: lujanrojas.informatica@gmail.com

## **TIMER:** contiene dos registros de 8 bits cada uno.

|Registro  |Direccion |Descripción                                                                                   |
|----------|----------|----------------------------------------------------------------------------------------------|
|CONT      |10H       |Registro que contiene la cantidad de pulsos. Cuando su valor coincide con el valor del registro COMP, se produce una interrupción.                                                                                    |
|COMP      |11H       |Registro comparador(Cada cuanto se produce la interrupcion).                                    |

## **PIC:** controlador de interrupciones programable.

|Registro  |Direccion |Descripción                                                                                   |
|----------|----------|----------------------------------------------------------------------------------------------|
|EOI       |20H       |END OF INTERRUPTION. Al mover el valor 20H a este registro, se le avisa al PIC que ya termino de atender la interrupcion.|
|IMR       |21H       |Enmascara las entradas de interrupciones para habilitarlas o deshabilitarlas.   |
|IRR       |22H       |Registro de petición de interrupciones. Al ponerse en 1 una de las entradas de INT, indica que esa INT está pendiente.|
|ISR       |23H       |Registro de interrupcion en servicio. Al ponerse una entrada en 1 indica que esa interrupcion está siendo atendida.                              |
|INT0      |24H       |Tecla F10 ** |
|INT1      |25H       |Timer  **                             |
|INT2      |26H       |HandShake **  |
|INT3      |27H       |DMA  **    |
|INT4      |28H       |Sin uso  |
|INT5      |29H       |Sin uso  |
|INT6      |2AH       |Sin uso  |
|INT7      |2BH       |Sin uso  |

**Contiene el valor del vector de interrupciones. El valor que se almacene, multiplicado por 4 da como resultado un valor donde se encuentra alojado la direccion de memoria que apunta a la rutina que atiende a esta interrupcion.

## **PIO:** puerto de entrada/salida.

|Registro  |Direccion |Descripción      |
|----------|----------|-----------------|
|PA        |30H       |Registro de Datos  |
|PB        |31H       |Registro de Datos  |
|CA        |32H       |Registro de configuracion de PA |
|CB        |33H       |Registro de configuracion de PB |

## **HAND-SHAKE.**

|Registro  |Direccion |Descripción                                                                                   |
|----------|----------|----------------------------------------------------------------------------------------------|
|DATOS     |40H       |Una operacion de escritura sobre este registro permite sacar un dato a las lineas D0.. D7, mientras que una lectura del mismo nos da el ultimo dato sacado por las lineas D0..D7|
|ESTADO    |41H       |Registro de estado, su formato es el que se muestra debajo de este cuadro.|
---


|B7 |B6 |B5 |B4 |B3 |B2 |B1 |B0 |
|---|---|---|---|---|---|---|---|
|`INT`|`X`  |`X`  |`X`  |`X`  |`X`  |`STROBE` |`BUSY`  | 



***Operaciones de lectura:***

```mermaid
B0 = 0 --> LINEA BUSY DESACTIVADA
B0 = 1 --> LINEA BUSY ACTIVADA
```
```mermaid
B1 = 0 --> LINEA STROBE DESACTIVADA
B1 = 1 --> LINEA STROBE ACTIVADA
```
```mermaid
B2 .. B6 --> NO SE USAN
```
```mermaid
B7 = 0 --> LINEA DE INTERRUPCIONES DESACTIVADA
B7 = 1 --> LINEA DE INTERRUPCIONES ACTIVADA, LA INTERRUPCION SE GENERA CUANDO BUSY NO ESTÉ ACTIVA.
```

***Operaciones de escritura:***

```mermaid
B0 .. B6 --> NO SE USAN
B7 = 0 --> INHIBE LA ACTIVACION DE LA LINEA DE INTERRUPCIONES
B7 = 1 --> LA LINEA DE INTERRUPCIONES ESTÁ ACTIVADA
```

## **DMA.**

|Registro  |Direccion |Descripción      |
|----------|----------|-----------------|
|RFL       |50H       |Registro de direcciones fuente parte baja. **  |
|RFH       |51H       |Registro de direcciones fuente parte alta. ** |
|CONTL     |52H       |Registo contador parte baja. Indica la cantidad de bytes a transferir.  |
|CONTH     |53H       |Registo contador parte alta. Indica la cantidad de bytes a transferir.   |
|RDL       |54H       |Registro de direcciones destino parte baja. Solo utilizable en transferencias memoria a memoria.  |
|RDH       |55H       |Registro de direcciones destino parte alta. |
|CTRL      |56H       |Registro de control. ***  |
|ARRANQUE  |57H       |Registro de arranque. Para iniciar la transferencia de datos se fuerza los primeros 3 bits con el valor 1. |

** En transferencias memoria-periferico o a la inversa, se carga en el registro la direccion del bloque de memoria a transferir o recibir. En transferencias memoria-memoria funciona tal como su nombre lo indica.

*** Registro de control: El significado de sus bits varia en funcion de la operacion de lectura/escritura que se realice sobre el. Su formato es como se muestra a continuación: 

|B7 |B6 |B5 |B4 |B3 |B2 |B1 |B0 |
|---|---|---|---|---|---|---|---|
|`TC`|`X`  |`X`  |`X`  |`MT`  |`ST`  |`TT` |`STOP`  | 

***Operaciones de lectura:***

```mermaid
B0 (STOP) = 0 --> TRANSFERENCIA EN CURSO
B0 (STOP) = 1 --> TRANSFERENCIA DETENIDA POR EL CPU TEMPORALMENTE
```
```mermaid
B1 .. B6 --> NO SE USAN
```
```mermaid
B7 (TC/TERMINAL COUNT) = 0 --> TRANSFERENCIA NO FINALIZADA
B7 (TC/TERMINAL COUNT) = 1 --> TRANSFERENCIA FINALIZADA
```

***Operaciones de escritura:***

```mermaid
B0 (STOP) = 0  --> NO TIENE SENTIDO.
B0 (STOP) = 1 --> AL ESCRIBIRLO LA CPU DETIENE MOMENTANEAMENTE LA TRANSFERENCIA EN CURSO.
```
```mermaid
B1 (TIPO DE TRANSFERENCIA) = 0  --> MEMORIA - PERIFERICO, O A LA INVERSA.
B1 (TIPO DE TRANSFERENCIA) = 1 --> MEMORIA - MEMORIA.
```
```mermaid
ST SOLO TIENE SENTIDO SI EL TIPO DE TRANSFERENCIA ES DE MEMORIA - PERIFERICO O VICEVERSA.
B2 (SENTIDO DE TRANSFERENCIA) = 0  --> PERIFERICO - MEMORIA.
B2 (SENTIDO DE TRANSFERENCIA) = 1 --> MEMORIA - PERIFERICO.
```
```mermaid
B3 (MODO DE TRANSFERENCIA) = 0  --> POR DEMANDA.
B3 (MODO DE TRANSFERENCIA) = 1 --> POR BLOQUES.
```
```mermaid
B4 .. B7 --> NO SE USAN.
```

## **USART**

|Registro  |Direccion |Descripción      |
|----------|----------|-----------------|
|DIN       |60H       |Registro de datos de entrada. Donde la CPU leera el dato recibido por la linea serie.|
|DOUT      |61H       |Registro de datos de salida. Donde se ha de escribir el dato a enviar a la linea serie.|
|CTRL      |62H       |Registo de control. Dependiendo el tipo de acceso, actua como un registro de control o de estado.**|

** Dependiendo del tipo de operación que se realice sobre el registro de control, tiene los siguientes formatos:

***Registro CTRL en escritura***

|B7 |B6 |B5 |B4 |B3 |B2 |B1 |B0 |
|---|---|---|---|---|---|---|---|
|`Synch`|`ER`  |`RTS`  |`DTR`  |`RxEN`  |`TxEN`  |`VBoud` |`Sy/As`  | 

```mermaid
B0 (Tipo de comunicación) = 0  --> Sincronica.
B0 (Tipo de comunicación) = 1 --> Asincronica.
```
```mermaid
B1 (Velocidad de transmision) = 0  --> V1 (6 bouds) 
B1 (Velocidad de transmision) = 1 --> V2 (18 bouds)
```
```mermaid
B2 (TxRDY - Enable) = 0  --> No se activará.
B2 (TxRDY - Enable) = 1 --> Se activará.
```
```mermaid
B3 (RxRDY - Enable) = 0  --> No se activará.
B3 (RxRDY - Enable) = 1 --> Se activará.
```
```mermaid
B4 (Data terminal ready) = 0  --> Desactiva DTR.
B4 (Data terminal ready) = 1 --> Activa DTR.
```
```mermaid
B5 (Request to send) = 0  --> Desactiva RTS.
B5 (Request to send) = 1 --> Activa RTS.
```
```mermaid
B6 (Error reset) = 1 --> Resetea los flags de error.
```
```mermaid
B7 (Sync Char) = 1  --> Insercion y busqueda automatica de caracteres SYNC 16H.(Solo tiene sentido cuando B0 = 0 modo sincronico)
```

***Registro CTRL en lectura***

|B7 |B6 |B5 |B4 |B3 |B2 |B1 |B0 |
|---|---|---|---|---|---|---|---|
|`DSR`|`SYNDET`  |`CTS`  |`X`  |`FE`  |`OE`  |`RxRDY` |`TxRDY`  |

```mermaid
B0 (Transmitter ready) = 1 --> Activado indica que el registro de salida del controlador está vacío, no estando condicionado por CTS.
```
```mermaid
B1 (Receiver ready) = 1  --> Activado indica que el controlador ha recibido un dato por la linea RxD para ser leido por la CPU.
```
```mermaid
B2 (Overrun error) = 1 --> Su activacion indica que se ha recibido un nuevo dato antes de que la CPU haya podido leer el anterior, con lo cual aquel se pierde.

```
```mermaid
B3 (Framing error [Solo modo asincronico]) = 1 --> Se activa cuando el dato recibido no tiene el numero de bits de parada correcto.

```
```mermaid
B4 (x) --> No se usa.
```
```mermaid
B5 (Clear to send) = 0  --> Indica que el estado de la entrada CTS está desactivada.
B5 (Clear to send) = 1 --> Indica que el estado de la entrada CTS está activada.
```
```mermaid
B6 (Sync detect) = 1 --> Cuando se programa B7 = 1 y B0 = 0 , indica la recepcion del caracter SYNC.
```
```mermaid
B7 (Data set ready) = 1  --> La entrada DSR está activada.
B7 (Data set ready) = 0 --> La entrada DSR está desactivada.
```
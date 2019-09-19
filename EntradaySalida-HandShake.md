# Arquitectura de Computadoras - Material complementario.
 
Dudas y consultas: lujanrojas.informatica@gmail.com

## Entrada y salida.

***Acerca de la PIO***

* Es un puerto de E/S que contiene dos puertos paralelos de 8 bits cada uno: A y B. 
* Se puede programar cada bit por separado como Entrada/Salida.
* Se encuentra en la dirección 30H.
* Cuenta con cuatro registros de 8 bits:

|Registro  |Direccion |Descripción                                                                                   |
|----------|----------|----------------------------------------------------------------------------------------------|
|`PA`      |30H       |Registro de datos                                                                             |
|`PB`      |31H       |Registro de datos                                                                             |
|`CA`      |32H       |Registro de control, configura cada bit del PA como entrada(con un 1) o como salida (con un 0)|
|`CB`      |33H       |Registro de control, configura cada bit del PB como entrada(con un 1) o como salida (con un 0)|

*Depende la configuración, los registros de la PIO se pueden usar para controlar leds, el uso de impresora, o el uso de la impresora por Hand-Shake*

***Configuración 0: LEDS y Microconmutadores.***

* Los bits de PA se conectan con los microconmutadores. Se debe configurar como entrada por medio de CA.
* Los bits de PB se conectan con los leds. Se debe configurar como salida por medio de CB.

>**NOTA:** Recuerden que para leer o escribir en la PIO se utilizan las instrucciones IN y OUT. NO se debe usar MOV.


*Veamos un ejemplo sencillo:*

----

PA EQU 30H

PB EQU 31H

CA EQU 32H

CB EQU 33H

**ORG 2000H**

MOV AL,11111111b ---->*Los bits en 1 para configurar como entrada PA.*

OUT CA, AL    ---->*Configuro PA como entrada (Conmutadores)*

MOV AL, 0   ---->*Los bits en 0 para configurar como salida PB.*

OUT CB, AL   ---->*Configuro PB como salida (LEDS)*

POLL: IN AL, PA  ---->*Leo el valor que tienen los bits que representan los conmutadores*

OUT PB, AL ---->*Escribo el valor en PB que representa los LEDS*

JMP POLL

END

----

***Configuración 1: Uso de la impresora.***

Para esta configuracion, la PIO debe cumplir con las funciones de temporización que requiere la impresora para la comunicación, para eso:

* El registro PA se comportará como registro de estado. Recordemos que los registros de la PIO ocupan 8 bits cada uno. En este caso los usaremos asi:

 El bit 0 de PA se conecta a la línea de Busy.

 El bit 1 de PA se conecta a la línea de Strobe.


* El registro PB se comportará como registro de datos. Se conectará con la línea de datos de la impresora.

*¿Empezamos a configurar?*

>**NOTA:** Recuerden que para probar esta configuración es necesario que la interfaz del simulador esté en la configuración P1C1 y luego se debe presionar F5 para mostrar la salida en papel.

Lo primero que debemos hacer es inicializar la PIO para el uso de la impresora. 
Debemos configurar el registro PA, de los 8 bits solo vamos a configurar los primeros dos menos significativos:

* Bit 0 (Busy): inicializamos en uno.
* Bit 1 (Strobe): inicializamos en cero.

Luego debemos configurar el registro PB como salida: lo inicializamos en cero.

Listo! Ya tenemos configurada la PIO para el uso de la impresora. Ahora la implementación:

PIO EQU 30H

**ORG 1000H**

MSJ DB "HOLA"

FIN DB ?

**ORG 2000H**

MOV AL, 11111101b 

OUT PIO+2, AL  --->*Seteamos en CA: Busy en 1 y Strobe en 0. El resto de los bits no nos interesan*

MOV AL, 0

OUT PIO+3, AL --->*Seteamos en CB todo en cero para configurar PB como salida*

IN AL, PIO --->*Leemos el dato que el registro PA, recuerden que lo usamos como registro de estado*

AND AL, 11111101b --->*Forzamos el bit de strobe a 0*

OUT PIO, AL 

MOV BX, OFFSET MSJ 

MOV CL, OFFSET FIN-OFFSET MSJ

POLL: IN AL, PIO -->*Consulto el estado de la impresora.*

AND AL, 1 -->*Cuando Busy = 0, la impresora se desocupo, sino sigue esperando en el loop*

JNZ POLL 

MOV AL,[BX]

OUT PIO+1, AL --->*Mando el dato a imprimir*

IN AL, PIO 

OR AL, 02H -->*Para que se genere la impresion debemos mandarle una 'señal', conocida como pulso 'strobe'*

OUT PIO, AL --> *Seteamos el strobe en 1 para imprimir*

IN AL, PIO

AND AL, 11111101 

OUT PIO, AL --> *Volvemos a setear el strobe en 0, y el busy en 1 para avisar que la impresora está ocupada*

INC BX

DEC CL

JNZ POLL

INT 0

END

----

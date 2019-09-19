# Arquitectura de Computadoras - Material complementario.
 
Dudas y consultas: lujanrojas.informática@gmail.com

## Entrada y salida.

***Acerca del PIO***

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

*Depende la configuración, los registros del PIO se pueden usar para controlar leds, el uso de impresora, o el uso de la impresora por Hand-Shake*

***Configuración 0: LEDS y Microconmutadores.***

* Los bits de PA se conectan con los microconmutadores. Se debe configurar como entrada por medio de CA.
* Los bits de PB se conectan con los leds. Se debe configurar como salida por medio de CB.

>**NOTA:** Recuerden que para leer o escribir en el PIO se utilizan las instrucciones IN y OUT. NO se debe usar MOV.


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


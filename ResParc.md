# Compilado de ejercicios de parcial.

* Completar el siguiente fragmento de programa de manera que a la subrutina CALCULO se le pasen las variables DATO1 y DATO2 como parametros por valor y RESULT como parametro por referencia, todos a traves de la pila.

**ORG 1000H**

DATO1 DW 100

DATO2 DW 200

RESULT DW ?

**ORG 2000H**

MOV AX, DATO1

PUSH AX

MOV AX, DATO2

PUSH AX

MOV AX, OFFSET RESUL

PUSH AX

CALL CALCULO

* ¿Como haria para intercambiar los contenidos de los registros AX y BX sin utilizar otros registros? Por ejemplo si los contenidos iniciales fueran AX = 1, BX = 2, el resultado final deberia ser AX = 2, BX = 1.

**PUSH AX**

**PUSH BX**

**POP AX**

**POP BX**

* ¿Que indica el registro IMR del PIC en el siguiente caso?

**IMR = CBH**

Indica que las interrupciones INT2, INT4 e INT5 se encuentran habilitadas.

* Si los puertos de control y de datos de la impresora estan conectados a los puertos PA y PB del PIO, respectivamente, y la impresora NO esta ocupada, ¿ Que dos cosas deben hacerse para mandar a imprimir un caracter ?

Se debe mandar a PB el dato a imprimir, y luego generar el 'pulso strobe' para que lo imprima.



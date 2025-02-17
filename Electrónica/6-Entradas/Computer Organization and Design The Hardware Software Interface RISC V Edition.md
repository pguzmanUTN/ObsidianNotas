
Fecha de Creación: 14-02-2025 17:25

Estado: #expandible

Etiquetas: [[RISC-V]] [[Instruction Set]] [[Performance ]]

# Conceptos RISC-V Instruction Set

## Vocabulario

Word: Unidad natural de acceso en una computadora, usualmente un grupo de 32 bits

Doubleword: Otra unidad natural de acceso en una computadora, usualmente un grupo de 64 bits, corresponde al tamaño de un registro en la arquitectura RISC-V.

Instruction Set Architecture: Interfaz abstracta entre el hardware y el nivel mas bajo del software que abarca toda la información necesaria para escribir un programa en lenguaje maquina y que se ejecute correctamente

Data transfer function: Comando que sirve para la transferencia de datos entre memoria y registros

Address: Un valor utilizado para delinear la ubicación de un elemento de datos específico dentro de una matriz de memoria.

Opcode: Campo que denota la operación y formato de la instrucción.
## Principios de diseño de RISC-V

### Principio de diseño 1: La simplicidad favorece la regularidad

Esto se refiere a que al tener tres operandos, ni mas, ni menos, en las instrucciones de RISC-V Assembly favorece la simplicidad del hardware ante un numero variable de operandos.
### Principio de diseño 2: Cuanto más pequeño, más rápido

Se refiere al caso de tener un numero limitado de registros, típicamente 32 registros de 64bits en las computadores actuales. Un numero muy grande de registros puede aumentar el tiempo de ciclo de reloj (Clock Cycle time) porque las señales eléctricas tardan mas en viajar cuando lo hacen más lejos

Si bien este principio no es absoluto, ya que 31 registros tal vez no sean mas rápidos que 32, la verdad detrás de tales observaciones causa que los diseñadores de computadoras se lo tomen en serio, ya que el mismo diseñador debe encontrar el balance el deseo de los programas por tener mas registros con el deseo del diseñador por mantener el ciclo de reloj mas rápido

### Principio de diseño 3: Buenos diseños demandan buenos compromisos

Todas las instrucciones tienen la misma longitud en código maquina.


## Performance Equations

$$
\text{CPU execution time for a program} = \text{CPU clock cycles for a program} \times \text{Clock cycle time}
$$
$$
\text{CPU execution time for a program} = \frac{\text{CPU clock cycles for a program}}{\text{Clock rate}}
$$
$$
\text{CPU clock cycles} = \text{Instructions for a program} \times \text{Average clock cycles per instruction}

$$

Average clock cycles per instruction: CPI (Clock cycles per instruction)

$$
\text{CPU time} = \text{Instruction count} \times \text{CPI} \times \text{Clock cycle time}

$$
$$
\text{CPU time} = \frac{\text{Instruction count} \times \text{CPI}}{\text{Clock rate}}

$$
Otra medida de medir la velocidad de ejecución de un programa es el MIPS (million instructions per second)
$$
\text{MIPS} = \frac{\text{Instruction count}}{\text{Execution time} \times 10^6}

$$
$$
\text{MIPS} = \frac{\text{Instruction count}}{\frac{\text{Instruction count} \times \text{CPI}}{\text{Clock rate}} \times 10^6} = \frac{\text{Clock rate}}{\text{CPI} \times 10^6}


$$

## Operadores
### Operadores Aritméticos
Los operadores aritméticos contienen tres operandos, el registro donde se va a guardar el resultado, y los registros de los otros dos operandos que se pueden sumar o restar.

#### Ejemplo
Supongamos que queremos sumar dos números a = b + c. El comando a realizar seria el siguiente:
	add a, b, c.
Y también podemos verlo en forma de registros
	add x11, x12, x13
Donde el registro x11 ,x12 y x13 contienen a las variables a, b y c respectivamente.

Para restar se sigue el mismo procedimiento solo que la instruccion es sub.

#### Ejemplo 2
Para hacer una operación mas complicada, por ejemplo f= (a+b) - (c+d)
Hay que hacer un par de instrucciones mas y crear variables temporales

	add x20, x11, x12 //Suma a+b y lo guarda en un registro temporal
	add x21, x13, x14 //Suma c+d y lo guarda en un registro temporal
	sub x15, x20, x21 //resta los registros temporales y los guarda en f
	
Donde los registros x20 y x21 pertenecen a variables temporales y los registros x11, x12, ....,x15 pertenecen a las variables: a,b,c,d y f respectivamente.

####  Operador addi

Este operador funciona exactamente igual que el operador add, solo que el ultimo operando puede ser una constante.

	addi x22, x22, 4
### Operadores de memoria

Los mismos sirven para transferir datos de la memoria a los registros y viceversa
#### Comando ld

El comando ld permite cargar un registro especifico con el contenido de una dirección de memoria, el mismo cuenta con dos argumentos

	ld register_address memory_address
	
Donde register_address es la dirección del registro donde guardar el contenido de la memoria y memory_address es la dirección de memoria que queremos cargar en el registro

Hay un caso interesante es cuando queremos acceder a una array de datos, o a un elemento especifico de la misma, por ejemplo imagínate que tenemos un vector de 10 longitud llamado B y queremos acceder al dato B[6], en sabemos que este vector empieza en la dirección de memoria x19, entonces podemos guardar el dato contenido en esa posición del vector en un registro, por ejemplo, el x5, haciendo lo siguiente

	ld x5, 6(x19)

Donde x5 es donde cargamos el dato que queremos x19 es la base de la dirreccion del registro del vector y 6 es el offset,

#### Comando sd

Es la instrucción complementaria a ld, ya que copia el dato de un registro a la memoria.

	sd x5, 6(x19)

Este ejemplo copia los datos del registro x5 y los guarda en esa dirección de memoria	

#### Ejemplo integrador

Supongamos una variable h asociada con el registro x21 y la dirección base de una array A que empieza en x22. ¿Cual es el código de RISC-V Assembly para la siguiente asignación en C?

	A[12]=h + A[8]

Solución

	ld x9, 64(x22) //Guarda el dato de A[8] en un registro temporal, se pone de
					//offset 64 debido son palabras doubleword y el offset puede
					//acceder a bytes individuales, por eso mismo el offset se
					//calcula como 8 (que es el elemento del array) x 8
					// (que es cuantos bytes tiene un doubleword)
	add x9, x21 , x9 //Se guarda la suma en el registro x9
	sd x9, 96(x22) //Guarda la suma en memoria en A[12], 96=12x8
### Operadores Logicos

![[Pasted image 20250216223901.png]]

## Estructura registros

Si bien la estructura de los registros es doubleword, es decir, cada registro por separado tiene 64 bits, es decir 8 bytes, las arquitecturas de hoy en dia pueden acceder a bytes individuales de los registros, por eso mismo, la dirección de un doubleword coincide con la dirección de un byte individual contenido en el mismo y las direcciones de las doubleword difieren en 8 de cada una. 

![[Pasted image 20250215203841.png]]

## Formato de instrucción

El formato de instrucción es una representación mediante campos de números binarios de la instrucción que deberá cumplir la computadora, la longitud de todos los formatos de instrucción es la misma, ósea de 32 bits.

Los campos de instrucción tienen nombre cada uno, lo que nos facilita entender lo que hace cada uno.

**opcode:** Operación básica de la instrucción
opcode: Campo que denota la operación y formato de la instrucción.

**rd:** Registro de destino del operando, obtiene el resultado de la operación
**funct3:** Otro campo adicional de opcode
**rs1:** Primer registro fuente de operando
**rs2:** Segundo registro fuente de operando
**funct7:** Otro campo adicional de opcode

**immediate:** Valor constante interpretado como valor en complemento a 2, por lo tanto interpreta números de -2^11 hasta 2^11 -1. Pero cuando se usa para instrucciones load, el immediate representa el offset, entonces puede hacer referencia en una region de ±2^11 a cualquier double word en esa region con la dirección base


### Formato de instrucción (R-type)}

Es uno de los formatos de instrucción y se usa cuando todos los operandos son registros

![[Pasted image 20250216191225.png]]

#### Ejemplo

	add x9, x20, x21

![[Pasted image 20250216190613.png]]
![[Pasted image 20250216190633.png]]

El primer , cuarto y ultimo campo nos indica que la instrucción es una suma. El segundo campo nos da la dirección del segundo operando, el tercer campo nos da la del primer operando y por ultimo el quinto campo nos dice la dirección de registro donde guardar la suma.

### Formato de instrucción (I-type)

Es otro del los formatos de instrucción y es usado para operaciones aritméticas cuando uno de los operandos es una constante. Su formato es el siguiente:

![[Pasted image 20250216211026.png]]

#### Ejemplo

La instrucción 
	ld x9, 64(x22)
Ubica el 22 de x22 en el campo rs1, el 64 en el campo immediate y el 9 de x9 en el campo rd

### Formato de instrucción (S-type)

Es otro formato de instrucción que necesita dos fuentes de registro, para la dirección base y la store data. y un immediate para el offset de la dirección. Los campos S-type tienen la siguiente forma.

![[Pasted image 20250216212358.png]]

Los bits immediate en este caso están partidos en los 5 bits mas bajos y los 7 bits mas altos, se decidió mantener así porque los campos rs2 y rs1 se los mantiene en el mismo lugar que todos los formatos de instrucción. Manteniendo los formatos de instrucción de la forma mas similar posible reduce la complejidad del hardware. Similarmente, el opcode y funct3 tiene el mismo espacio en todas las localizaciones y siempre se encuentran en el mismo lugar.



### Opcode para distintas instrucciones y ejemplo de cada formato

ld: se pone el opcode y el funct3 en valor 3
add: se pone opcode en valor 51 y funct3 en 0
addi: se pone el opcode en valor 19 y funct3 en 0
sd se pone el opcode en valor 35 y el funct3 en valor 3

![[Pasted image 20250216222324.png]]
![[Pasted image 20250216222434.png]]
## Notas varias

### Nota 1

Algunos programas tienen mas variables que registros tiene una computadora, por eso mismo, los compiladores tratan de mantener las variables mas usadas en los registros y las menos usadas en memoria, ya que los registros son mas rápidos que la memoria. Usan comandos loads y store para ir moviendo variables entre registros y memoria. El proceso de poner las variables menos usadas en memoria se llama spilling registers.

### Nota 2
Si bien existen las instrucciones add y sub para sumar y restar datos de registros, y usamos la instrucción addi para sumar una constante al registro, no existe una instrucción subi, ya que el campo immediate esta representado en complemento a 2, por lo tanto con la instrucción addi podemos substraer constantes

### Nota 3
En el caso especial de los shift lógicos, tienen formato de instrucción I-type, pero solo los 6 bits mas a la izquierda son usados del immediate para indicar el shift 

![[Pasted image 20250216224258.png]]
# Referencias


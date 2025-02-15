
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
## Principios de diseño de RISC-V

### Principio de diseño 1: La simplicidad favorece la regularidad

Esto se refiere a que al tener tres operandos, ni mas, ni menos, en las instrucciones de RISC-V Assembly favorece la simplicidad del hardware ante un numero variable de operandos.
### Principio de diseño 2: Cuanto más pequeño, más rápido

Se refiere al caso de tener un numero limitado de registros, típicamente 32 registros de 64bits en las computadores actuales. Un numero muy grande de registros puede aumentar el tiempo de ciclo de reloj (Clock Cycle time) porque las señales eléctricas tardan mas en viajar cuando lo hacen más lejos

Si bien este principio no es absoluto, ya que 31 registros tal vez no sean mas rápidos que 32, la verdad detrás de tales observaciones causa que los diseñadores de computadoras se lo tomen en serio, ya que el mismo diseñador debe encontrar el balance el deseo de los programas por tener mas registros con el deseo del diseñador por mantener el ciclo de reloj mas rápido

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
Para hacer una operacion mas complicada, por ejemplo f= (a+b) - (c+d)
Hay que hacer un par de instrucciones mas y crear variables temporales
	add x20, x11, x12
	add x21, x13, x14
	sub x15, x20, x21
Donde los registros x20 y x21 pertenecen a variables temporales y los registros x11, x12, ....,x15 pertenecen a las variables: a,b,c,d y f respectivamente.
### Operadores de memoria
Los mismos sirven para transferir datos de la memoria a los registros y viceversa
# Referencias


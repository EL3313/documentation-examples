# Ejercicio 6

## Ejercicio 6: ALU

## 1. Abreviaturas

* FPGA: Field Programmable Gate Arrays
* ALU: Arithmetic Logic Unit


## 2. Referencias

[0] Harris, S. L., & Harris, D. (2021). Digital Design and Computer Architecture, RISC-V Edition. Morgan Kaufmann.

## 3. Desarrollo

### 3.1 Modulo ¨module_alu¨

El módulo de ALU es una parte principal de cualquier computador; además, de caracterizarse por realizar varias de las operaciones matemáticas y de bits utiles en un dispositivo. Así, los usuarios pueden contener archivos de texto, informacón de multimadia y muchi más.

El ALU, recibe las señales *operadorA*, *operadorB*, *ALUControl* y *ALUFlagIn* como entradas. lo que permite establecer la configuración entre los operador y la operación por realizar con las primeras tres señales; el ALUFlagIn es el encargado de definir algunas operaciones pero sobretodo de permitir un duplicado de la unida dy poder llevarlo a tamaños de bit mucho más grandes.

### 1. ENCABEZADO DEL MÓDULO

            module module_alu #(parameter N_BITS=4)(
                                          input logic [N_BITS-1:0] operatorA,
                                          input logic [N_BITS-1:0] operatorB,
                                          input logic [N_BITS-1:0] ALUControl,
                                          input logic ALUFlagIn,
                                          output logic [N_BITS-1:0] result,
                                          output logic Z,
                                          output logic C

    );


### 2. PARÁMETROS:

* N_BITS: es la cantidad de bits asignada a la trama, entrads, salidas  y buses de la unidad aritmética. Esto implica que tanto los operadores  y la señal de control, como el resultado de salida deben coincider con este tamaño.

### 2. ENTRADAS Y SALIDAS

 * operador A: es un valor de referencia para el ALU con tamaño N_BITS; en varias de las operaciones, es al único que se le realiza variaciones.
 * operador B: es un valor de referencia para el ALU con tamaño N_BITS; en varias de las operaciones, será el encargado de establecer limites en la cantidad de modificaciónes por realizar.
* ALUControl: es la señal de tamaño N_BITS que se utilaza como selector entre las operaciones matemáticas y lógicas contenidas en el ALU. Su variación va desde SUMA o RESTA, hasta CORRIMIENTO o AND, OR.
* ALUFlagIn: es la señal de union en caso de necesitar ALUs de gran tamaño; además, de definir valores entre 0 y 1 para operaciones lógicas.
* result: es la señal de salida de tamaño N_BITS, en este se mostrará o llevará el valor resultante de cualquier operación realizada por el ALU.
* Z: señal designada a  /cero/, esto significa que tendrá un valor de 1 si el resultado de cualquier operación ha sido cero.
* C: la señal de carry se delimita en la salida C. Sirve como medio de comunicación para los casos donde SUMAS,RESTAS u otras operaciones lógicas producen un bit de más al tamaño de N_BITS-1.


### 3. CRITERIOS DE DISEÑO

Una Unidad Aritmética Lógica al poseer varias operaciónes  en su gama de acciones, puede llegar a ser confuso su implementación. Por lo que se presentan los primeros dos niveles de diagramas de bloques para este dispositivo.

En él, se tine la abtracción de "caja negra", lo que conlleva a visualizar primeramente las entradas y salidas del sitema. Para luego, expandir un poco la visión con los bloque básicos de cada operación lógica o matemática que necesita ejecutarse. Se ilustran operaciones como AND,OR y XOR  que en síntesis son areglos de  compuertas existentes en el lenguaje de SYSTEM VERILOG y que se accederían a ellas mediante la activación de ese bloque. 

![TDD_Lab1-Página-4 drawio (1)](https://user-images.githubusercontent.com/76532945/221751471-a0c48c0f-02e2-4875-b405-c56dd936a4ae.png)
Diagrama de primer nivel y segundo nivel


La implementacón realizada contiene un código que depende de la señal ALIControl y la cual permite realizar en modo combinacional una estructura de /case/. Por lo que la elaboración de las operaciones se da en foma de bloques según la distribucion de los valores permitidos.


Un ejemplo de este es  el código para la suma con complemento a 2.

> 'h2: //sum
> 
>  if(ALUFlagIn !=2)
>  
>   begin
>   
>                complementB = operatorB[N_BITS-1]== 1? ~operatorB+1: operatorB;
>                
>                complementA = operatorA[N_BITS-1]== 1? ~operatorA+1: operatorA;
>                
>                prevResult = complementA + complementB + ALUFlagIn;
>                
>                C=prevResult[N_BITS];
>                
>                result= prevResult[N_BITS-1:0];
>                
>   end
            
En caso del cero, se realiza al final de toda combinación un chequeo del resultado.

> if(result == 0)
        Z=1;


La única operación no realizada es la del corrimieto a la derecha.

El esquemático se presenta a continuación.

![schematic_alu](https://user-images.githubusercontent.com/76532945/221446308-a6289484-6719-43fc-b8a5-9a948e77f50a.jpg)
 
 
### 4. TESTBENCH 
Al tratarse de un proyecto que se debe implementar con un chequeo automático del mismo, se realiza un testbench que varíe el ALUFlagIn y las opciones de operación aritmética. Con valores fijo de operandos en A= 0000 y B=1111. los resultados se muestran en consola y se adjunta una copia de esta.

 
>   run 30000 ms
>   
> ALUFLAG = 00000000000000000000000000000000
> 
> ------ AND RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ OR RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ SUM RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ INCREMENT RESULTS ------
> result  is OK
> C  is OK
> Z  is OK
> --------------------------------------------------------------------------
> ------ DECREMENT RESULTS ------
> result  is OK 
> Z  is OK
> C  is OK
> --------------------------------------------------------------------------
> ------ NOT RESULTS ------
> result is OK
> Z is OK
> C  is OK
> --------------------------------------------------------------------------
> ------ SUB RESULTS ------
> result is OK 
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ XOR RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ SHIFT LEFT RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> 
> ALUFLAG = 00000000000000000000000000000001
> 
> ------ AND RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ OR RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ SUM RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ INCREMENT RESULTS ------
> result is OK
> Z  is OK
> C  is OK
> --------------------------------------------------------------------------
> ------ DECREMENT RESULTS ------
> result  is OK
> Z  is OK
> C  is OK
> --------------------------------------------------------------------------
> ------ NOT RESULTS ------
> result  is OK
> Z is OK
> C  is OK
> --------------------------------------------------------------------------
> ------ SUB RESULTS ------
> result is OK 
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ XOR RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------
> ------ SHIFT LEFT RESULTS ------
> result is OK
> Z is OK
> C is OK
> --------------------------------------------------------------------------


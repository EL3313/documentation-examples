# Ejercicio 2
## Ejercicio 2: Switches & LEDs

## 1. Abreviaturas

- FPGA: Field Programmable Gate Arrays
- Switch: dsipositivo que enlaza o abre el circuito entre dos posiciones.
- LED: diodo emisor de luz

## 2. Referencias

[0] Harris, S. L., & Harris, D. (2021). Digital Design and Computer Architecture, RISC-V Edition. Morgan Kaufmann.


## 3. Desarrollo

### 3.1 Módulo "module_switches" 

El módulo recibirá toda la configuración de *switches* para dar la salida de los *LEDs* asociados, según los *buttons* presionados. Por lo que, apagará o dejará encendido las luces si así se realiza la combinación 

#### 1. ENCABEZADO DEL MÓDULO

    module module_switches(
    
        input logic [15:0] sw,
        input logic buttonU,
        input logic buttonR,
        input logic buttonD,
        input logic buttonL,
        output logic [15:0] led
        
    );


#### 2.ENTRADAS Y SALIDAS

Por lo que se presenta el conjunto de variables para entrada y salida del módulo completo

* sw : Se utiliza como la entrada completa de la configuración de los switches físicos.
* buttonU , buttonL, buttonD, buttonR: cada uno corresponde a la señal de entrada proveniente del los botones de push down designados respectivamente como UP, LEFT, DOWN y RIGHT. 
* led : se declara como variable de salida, para contener la salida de la sección física de las luces asociadas a los siwitches en el FPGA.


#### 3. CRITERIOS DE DISEÑO 

Como se mencionó en la etapa de diseño, para el ejercicio de switches y LEDs, se presenta la abtracción  de  un boton push con cuatro switches en la tabla siguiente. La sección resaltada es la que domina la salida visual en la los LEDs.

 La compuerta que ejecuta la comparación entre boton y switch; se da mediante un AND. Se considera que la salida en 1 es permitir el encendido de los LEDs y un 0 se define como el apagado; por lo que para este comportamiento y la utilización de esta compuerta, se niega la entrada del botón. Así cuando este sea presionado y se ejecute la comparación, cualquier switch activado se apagará o no se vea distorsionado los que están en posición de off.

La imagen siguiente a la tabla, ilustra las conexiones que deben realizarse para lograr que los cuatro botones push gobiernen sobre dieciséis switches y LEDs. En una división de cuatro grupos.

<img src="https://user-images.githubusercontent.com/76532945/219274697-8bada4c9-d65b-40bd-a1d0-ba57c5867ef6.jpg" width="250">
Tabla de verdad para 1 botón, 4 switches y 4 LEDs

<img src="https://user-images.githubusercontent.com/76532945/221427452-e5642608-038a-4cc6-83aa-dea7917b2313.png" width="350">
Diagrama desciptivo de conexión entre 1 botón, 4 switches y 4 LEDs

En cuanto a su implementación, se realiza mediante la concatenación de la salida lógica de un AND entre negación de los botones push y la trama de los switches.


> led = sw & { {4{~buttonU}} ,  {4{~buttonR}} , {4{~buttonD}} ,  {4{~buttonL}} };
    
    
 Además, se muestra el esquemático de la implementación. Este  muesta como en una "caja negra" se ingresa las variables de switch y botón respectivo. Para luego obtener la salidad del led y ser transcrita a la trama de 16 bits para estos últimos.
 
 
<img src ="https://user-images.githubusercontent.com/76532945/221430097-ecc518bb-c5e3-45b1-af7f-3b3322349427.jpg" width="350"> 
Diagrama de abtacción para 1 botón, 4 switches y 4 LEDs 
 
    
 Lo que permite que en una solo línea bajo el always_comb, realiza la estructura combinacional y se pueda ver el funcionamiento correcto en la programación de la FPGA; además de una simulación post síntesis que permite ver resultados binarios la salida.
 
 #### 4. TESTBENCH 
 
 Para este caso, se tiene un testbench o simulación que debe revisarse visualmente,por lo que se presenta un ejemplo donde se  contiene una configuración determianda en los switches y la activación de los botones U,R y L que deben apagar las posiciones 15:12, 11:8 y 3:0, respectivamente. En la señal led de salida, se percibe que los únicos valores en 1 o encendido corresponden a las luces 7:4. Los cuales son los asignados al botón D, en estado de no presionado.
 
 Ahora bien, se presenta un pequeño video donde se visualiza el funcionamiento e implementación físico en el FPGA de BASYS 3.
 


![simu_sw](https://user-images.githubusercontent.com/76532945/221446999-305f711c-cd52-4574-88d1-fc8a5b8e3438.jpg)
Simulación Switches & LEDs


![sw](https://user-images.githubusercontent.com/76532945/221429781-927ab260-68eb-4fc0-a780-7cb2476a217d.gif)

Video sobre implementación en FPGA.

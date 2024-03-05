# Ejercicio 5: Sumador y ruta crítica

# 1. Abreviaturas y definiciones
-> RCA: ripple carry adder
-> CLA: carry look-ahead adder

# 2. Desarrollo

El desarrollo de este ejercicio se divide en tres códigos y dos testbench.

### 2.1 modulo Full Adder 

Este primer modulo es la base para casi que todo el ejercicio en general.

#### 2.1.1 Encabezado del módulo:

<img width="254" alt="image" src="https://user-images.githubusercontent.com/125165283/221443093-60518f3b-8726-441d-8d53-c9f66fcbdeef.png">

#### 2.1.2 Parámetros:

Este módulo no posee parámetros.

#### 2.1.3 Entradas y salidas:

-> A: Primera entrada del sumador

-> B: Segunda entrada del sumador

-> C_in: Bit de acarrreo inicial

-> o_Sum: Salida que da como resultado la suma de A y B

-> C_out: Salida del bit de acarreo

#### 2.1.4 Criterios de diseño.

<img width="419" alt="image" src="https://user-images.githubusercontent.com/125165283/221443620-1d116902-1ab7-4a56-a50a-0858e9d8f0e8.png">

En esta imagen podemos ver la tabla de verdad con la que se hizo el código, con esto mismo podemos ver las salidas ya plasmada con compuertas lógicas, después de esto con la ayuda de compuertas primitivas logramos terminar el resto del código.

<img width="242" alt="image" src="https://user-images.githubusercontent.com/125165283/221443805-c075aa78-f1d2-4db3-b5ad-f51754cc4e33.png">


#### 2.1.5 Testbench.

En este módulo no se creó una testbench.

### 2.2 module RCA

Gracias al modulo anterior se pudo hacer este otro module, pues el anterior es básicamente la base para este.

#### 2.2.1 Encabezado del módule ripple_carry_adde:

<img width="312" alt="image" src="https://user-images.githubusercontent.com/125165283/221443973-ef66a028-5283-4e51-beba-ef2f6b3702fb.png">

#### 2.2.2 Parámetros:

N: Define la cantidad de etapas del mux, en este caso es de 4.

#### 2.2.3 Entradas y salidas:

En esta parte son basicamente las mismas entradas y salidas:

-> A: Primera entrada del sumador

-> B: Segunda entrada del sumador

-> C_in: Bit de acarrreo inicial

-> o_Sum: Salida que da como resultado la suma de A y B

-> C_out: Salida del bit de acarreo

Con la diferencia que estas están parametrizadas de [N - 1:0] esto porque son cuatro sumadores full adder los que conformar el Ripper carry adder


#### 2.2.4 Criterios de diseño

Para esta parte primero se partió de la definición del ripper carry adder, por lo que buscamos una imagen que nos ilustre el circuito como tal.

<img width="566" alt="image" src="https://user-images.githubusercontent.com/125165283/221444377-864758d2-bbbf-46fd-92ce-652a6f0d0047.png">

Ya con este, el codio en SystemVerilog tiene una mayor facilidad de hacerse, pues es simplemente la combinación de cuatro full adder. El módulo utiliza un bucle for para crear una serie de sumadores completos de un solo bit (full adders), donde cada bit es la suma de dos bits y el acarreo entrante (C_in). El bit menos significativo (LSB) se suma primero utilizando un full adder diferente, y el acarreo de salida del full adder anterior se utiliza como entrada del siguiente full adder. Los resultados de cada full adder se almacenan en un array llamado FA_Sum, y los acarreos de salida se almacenan en otro array llamado FA_C_out. Finalmente, el resultado de la suma se asigna a la salida Sum, y el último acarreo de salida se asigna a C_out.

<img width="543" alt="image" src="https://user-images.githubusercontent.com/125165283/221444553-d5f1eecb-61f8-403f-9829-7e6ca3f9a19c.png">

#### 2.2.5 Testbench

La testbench establece valores de entrada para A, B y C_in, y luego verifica si la salida o_Sum y C_out son iguales al resultado esperado de la suma y el acarreo correspondiente.

La prueba de suma utiliza dos bucles for para probar todas las posibles combinaciones de A y B con valores de 0 a 255. Para cada combinación, se establece C_in en 0 y se espera que la salida o_Sum sea la suma correcta de A y B, y que la salida C_out sea el acarreo correspondiente. Si o_Sum o C_out no son iguales al resultado esperado, se registra un error y se incrementa el contador de errores.

<img width="624" alt="image" src="https://user-images.githubusercontent.com/125165283/221444835-267fd819-9f9a-4a89-aeaa-597e63c1fb6b.png">

La prueba de acarreo establece A y B en 255 y C_in en 1, y espera que la salida o_Sum sea 0 y la salida C_out sea 1. Luego, se establece A y B en 0 y C_in en 0, y se espera que la salida o_Sum sea 0 y la salida C_out sea 0. Si la salida no coincide con el resultado esperado, se registra un error y se incrementa el contador de errores.

<img width="627" alt="image" src="https://user-images.githubusercontent.com/125165283/221444864-8075a50c-f92b-480f-9fc6-cfcf5edc2068.png">



### 2.3 module CLA
El CLA es un sumador muy utilizado en la electrónica digital ya que toma menos para determinar los bits de acarreo, la mayor diferencia con el RCA es que el CLA calcula uno o varios bits de acarreo antes de la suma.

#### 2.3.1 Encabezado
![imagen](https://user-images.githubusercontent.com/39966622/221758008-cbba2160-caae-4c73-95e2-22b90fdaad6c.png)

#### 2.3.2 Parámetros
WIDTH: define la cantidad de bits de los números a sumar

#### 2.3.3 Entradas y salidas
-> a: primera entrada del sumador

-> b: segunda entrada del sumador

-> sum: resultado de sumar a y b

-> generate_bit: bit de generación

-> propagete_bit: bit de propagación

-> carry_bit: bit de acarreo

#### 2.3.4 Criterios de diseño
Para cada bit, el CLA determinará si se genera un bit de acarreo o se propagará
Para generar un bit se multiplica el bit i de las entradas

![imagen](https://user-images.githubusercontent.com/39966622/221758950-56952e02-703e-4547-be58-46a813cefdab.png)

Para propagar se suman los bit i de las entradas

![imagen](https://user-images.githubusercontent.com/39966622/221759060-5b8cb1b7-21ee-41b7-a948-ddf3db6afd9e.png)

El bit de acarreo i se genera al multiplicar el bit de propagación i-1 con el acarreo i-1 y sumarle el bit de generación i-1, como se logra observar para i=1

![imagen](https://user-images.githubusercontent.com/39966622/221759879-b176213c-daef-4986-9fe5-9b4bd0fe8b86.png)

Teniendo en cuenta estas ecuaciones, se puede generalizar para varios bits:

![imagen](https://user-images.githubusercontent.com/39966622/221759156-edab1e63-356b-4a4d-aad0-f1fb260aba2b.png)

Si se sustituye el bit de acarreo i-1 en el i se obtiene la ecuación para cada bit:

![imagen](https://user-images.githubusercontent.com/39966622/221759306-1f60763e-1e60-43ba-b019-bd5f888e2a77.png)

Basado en estas ecuaciones, se describe el CLA en Vivado:

![imagen](https://user-images.githubusercontent.com/39966622/221759631-939ca69d-65b0-4164-b0e9-96809cfcc6cc.png)

#### 2.3.5 Testbench

![imagen](https://user-images.githubusercontent.com/39966622/221760792-d5285afc-46df-4859-9698-c87bc02ae17c.png)

Se define un ancho de 8 bits, un valor máximo igual a 255 (definido por un corrimiento a la izquierda introduciendo un 1 en el bit WIDHT + 1 y restándole 1) y 16 pruebas. Además, las entradas a y b, sum siendo el resultado del CLA y result en el cuál se sumará dentro del testbench a + b y se comparará con el resultado del CLA.

![imagen](https://user-images.githubusercontent.com/39966622/221761308-22602ed5-f7a1-4920-a64e-72485fe7c0f4.png)

El for loop se encarga de realizar la cantidad de pruebas, luego se definen a y b como números aleatorios en el rango de 0 al valor máximo, luego se suman estos números. El condicional if se encarga de comparar el resultado del CLA con result, si son diferentes indicará un error, de lo contrario no imprimirá nada.




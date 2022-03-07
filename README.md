# 📚 Proyecto Arquitectura de Computadores I.

# 💻 RVfpgaSoC

Algunos de los grandes contribuyentes actuales en el área de tecnología, se unieron para crear e impartir un curso para el aprendizaje y estudio de SoCs, _System-on-Chips_, entre ellas resaltan Diligent, RISC-V, Xilinx, Imagination entre otros. 

En este curso se muestra como construir un subconjunto de SweRVolfX SoC desde cero utilizando bloques de construcción como el núcleo SweRV, las memorias y los periféricos, mediante el software de Vivado.
El proyecto se lleva a cabo en dos partes esenciales, la creación de un bloque mediante Vivado, y luego la ejecución de este mediante simulación con Verilator.

## 📌  Primera parte -Creación del diagrma de bloques-
Inicialmente se busca crear un diagrama de bloques en el cuál se incluyen los siguientes bloques de vivado:

| **No.** 	| **Módulo**           	| **Vivado bloque**     	|
|---------	|----------------------	|-----------------------	|
| **1**   	| SweRV Core           	| swerv_wrapper_verilog 	|
| **1**   	| Interconnect Wrapper 	| intcon_wrapper_bd     	|
| **1**   	| Boot-ROM             	| bootrom_wrapper       	|
| **1**   	| GPIO Top Module      	| gpio_wrapper          	|
| **1**   	| System Controller    	| syscon_wrapper        	|
| **32**  	| Bidirec Gpio Module  	| bidirec               	|


Estos módulos se interconectan mediante un instructivo proporcionado por _Imaginaton_, donde se especifican las conecciones.

Inicialmente se conectan los bloques `swerv_wrapper_verilog` y `intcon_wrapper_bd`, en estos bloques hay tres tipos de sets de pines, los cuales son,  IFU (_Instruction Fetch Unit_), LSU (_Load Store Unit_) y SB (_Store Byte_). La conexión se realiza en el orden mencionado anteriormente, se continúa con el paso a paso de los otros bloques, al terminar las conexiones internas se inician las conexiones externas, estas son el reloj `clk`, el reset, `rst`, las memorias y finalmente los pines `bidir` de lso bloques bidirec, terminando con estos todas las conexiones del diagrama.

Luego de tener el diagrama de bloques completo se procede a generar el archivo de módulo Verilog configurando en el archivo de Vivado, al obtener el archivo `BD.v` se procede a generar el _bitstream_, donde se muestra si el proceso fue realizado correctamente o genera algún error.


![Alt text](https://i.imgur.com/ETXEuHu.png)

Al realizar la generación del _Bitstream_ se obtienen que la síntesis, implementación y generación han sido realizadas completamente. 

## 📌  Segunda parte -Simulación del SweRVolfX SoC-


Esta sección se puede realizar en simulación e implementandola en la fpga, sin embargo para la realización de esté se llevará a cabo sólo la parte de simulación, además será realizada mediante las herramientas instaladas en Windows.
* VSCode
* PlatformIO
* GTKWave
* Cygwin


Esta parte se desarrolla mediante una serie de pasos.

#### 1. Copiar el archivo `BD.v`

En la simulación se empleará una estructura cómo la que se muestra a continuación, teniendo como _top module_ el archivo `rvfpgasim`.

![Alt text](https://i.imgur.com/j6OcABD.png)

La simulación se trabajará mediante Verilator, Primero se debe buscar el archivo generado en la primera parte (Lab1), llamado `BD.v`, para anexarlo a la ruta `RVfpgaSoC/Labs/LabResources/Lab2/src/SweRVolfSoC` como se muestra en la siguiente figura.


![Alt text](https://i.imgur.com/RD0lpok.png)

#### 2. Verificación de los modulos.

Ahora teniendo este archivo se verifica que existan y esten exactamente los nombres de los siguientes modulos: 

* BD_bootrom_wrapper_0_0
* BD_gpio_wrapper_0_0
* BD_intcon_wrapper_bd_0_0
* BD_swerv_wrapper_verilog_0_0
* BD_syscon_wrapper_0_0

![Alt text](https://i.imgur.com/U6Dm4sa.png)


#### 3. Generar la simulación Binaria.

Para ello se ingresa a la dirección `RVfpgaSoC/Labs/LabResources/Lab2/verilatorSIM` deonde se encuentran los archivos `Makefile` y `swervolf_0.7.vc` los cuales brindan a Verilator información sobre donde encontrar las fuentes SoC. Luego se genera el archivo binario mediante estos comandos.

```sh
/cygdrive/c/Users/Lenovo/Downloads/RVfpgaSoC/RVfpgaSoC/Labs/LabResources/Lab2/verilatorSIM
```
```sh
make clean
```
```sh
clean
```

![Alt text](https://i.imgur.com/thVUUrt.png)

### Problemas presentados

Error parte2, paso 3

![Alt text](https://i.imgur.com/cptHnjR.png)
Se soluciona añadiendo librerías 
#include<limits>
#include<cstddef>
#include<iostream>

## 🔖 Conclusiones

*


## 📚 Referencias

* [Imagination University Programme](https://university.imgtec.com/) 
* [RVfpga – Introduction to RVfpgaSoC  V1.0](https://university.imgtec.com/resources/download/rvfpgasoc-v1-0/)
* [EH1 RISC-V SweRV CoreTM 1.9 from Western Digital](https://github.com/chipsalliance/Cores-SweRV.git) 
* [SweRVolf](https://github.com/chipsalliance/Cores-SweRVolf.git) 




#### Universidad Industrial de Santander
* 👩 Francy Jessenia Calderón Osorio - 2162491
* 👩 Andrea Paola Reyes Carreño - 2164095


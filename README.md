# 📚 Proyecto Arquitectura de Computadores I.

# 💻 RVfpgaSoC

Algunos de los grandes contribuyentes actuales en el área de tecnología, se unieron para crear e impartir un curso para el aprendizaje y estudio de SoCs, _System-on-Chips_, entre ellas resaltan Diligent, RISC-V, Xilinx, Imagination entre otros. 

En este curso se muestra como construir un subconjunto de SweRVolfX SoC desde cero utilizando bloques de construcción como el núcleo SweRV, las memorias y los periféricos, mediante el software de Vivado.
El proyecto se lleva a cabo en dos partes esenciales, la creación de un bloque mediante Vivado, y luego la ejecución de este mediante simulación con Verilator.

## 📌  Primera parte -Creación del diagrama de bloques-
Inicialmente se busca crear un diagrama de bloques en el cuál se incluyen los siguientes bloques de vivado:

| **No.** 	| **Módulo**           	| **Vivado bloque**     	|
|---------	|----------------------	|-----------------------	|
| **1**   	| SweRV Core           	| swerv_wrapper_verilog 	|
| **1**   	| Interconnect Wrapper 	| intcon_wrapper_bd     	|
| **1**   	| Boot-ROM             	| bootrom_wrapper       	|
| **1**   	| GPIO Top Module      	| gpio_wrapper          	|
| **1**   	| System Controller    	| syscon_wrapper        	|
| **32**  	| Bidirec Gpio Module  	| bidirec               	|


Estos módulos se interconectan mediante un instructivo proporcionado por _Imagination_, donde se especifican las conexiones.

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

### ✔️1. Copiar el archivo `BD.v`

En la simulación se empleará una estructura cómo la que se muestra a continuación, teniendo como _top module_ el archivo `rvfpgasim`.

![Alt text](https://i.imgur.com/j6OcABD.png)

La simulación se trabajará mediante Verilator, Primero se debe buscar el archivo generado en la primera parte (Lab1), llamado `BD.v`, para anexarlo a la ruta `RVfpgaSoC/Labs/LabResources/Lab2/src/SweRVolfSoC` como se muestra en la siguiente figura.


![Alt text](https://i.imgur.com/RD0lpok.png)

### ✔️2. Verificación de los modulos.

Ahora teniendo este archivo se verifica que existan y esten exactamente los nombres de los siguientes modulos: 

* BD_bootrom_wrapper_0_0
* BD_gpio_wrapper_0_0
* BD_intcon_wrapper_bd_0_0
* BD_swerv_wrapper_verilog_0_0
* BD_syscon_wrapper_0_0

![Alt text](https://i.imgur.com/U6Dm4sa.png)


### ✔️ 3. Generar la simulación Binaria.

Para ello se ingresa a la dirección `RVfpgaSoC/Labs/LabResources/Lab2/verilatorSIM` donde se encuentran los archivos `Makefile` y `swervolf_0.7.vc` los cuales brindan a Verilator información sobre donde encontrar las fuentes SoC. 

![Alt text](https://i.imgur.com/thVUUrt.png)

Luego se genera el archivo binario mediante estos comandos.

```sh
/cygdrive/c/Users/Lenovo/Downloads/RVfpgaSoC/RVfpgaSoC/Labs/LabResources/Lab2/verilatorSIM
```
```sh
make clean
```
```sh
clean
```
Con esto se genera un archivo RVfpgaSim, que posteriormente se utilizará para crear la traza de simulación del programa AL-Operations.

### ✔️ 4. Generar la traza de simulación desde PlatformIO.

Se abre desde _PlatformIO_ la carpeta `AL_Operations` en ella se encuentra el archivo `platformio.ini`, se abre para editar una línea donde se encuentra al ruta del archivo al cuál se le va a generar la traza, en este caso al final de la ruta establecida se agrega el archivo, `Vrvfpgasim.exe.` debido a que se está trabajando en Windows.

Luego de editar esta línea se procede correr la simulación y generar la traza en la opción `Generate Trace`, luego de ello si se obtiene un `SUCCESS` como resultado indica que está correcto.

![Alt text](https://i.imgur.com/1tkhRWw.png)

### ✔️ 5. Análisis de la simulación con GTKWave.


![Alt text]()

## ❌ Problemas presentados

* ### 🚩 Error en Vivado al generar el Bitstream.

Al terminar de crear el diagrama de bloques en vivado y realizar las debidas configuraciones, se procede a fenerar el bitstream sin embargo se generan una serie de errores plasmados a continuación.

![Alt text](https://i.imgur.com/gHgeoUj.png)

Para resolver estos errores se procede a revisar cada bloque empleado en el diagrama,  sin embargo no se encuentra ninguna falla en esto. Otro aspecto que se tuvo en cuenta es que la versión de Vivado empleada inicialmente era la 2018.3, se actualizó a la versión solicitada en el curso, la 2019.2 y se realizó nuevamente el proceso, logrando así la correcta generación del birstream.


* ### 🚩Error al generar la simulación Binaria.

Luego de correr los comandos mencionado en esta sección, se muestran estos errores, en el archivo `verilater.cpp`.

![Alt text](https://i.imgur.com/cptHnjR.png)

Se soluciona añadiendo algunas librerías faltantes en el mismo archivo, como se muestra en la imagen.

`#include<limits>`
`#include<cstddef>`
`#include<iostream>`

![Alt text](https://i.imgur.com/jYX7GSe.png)


![Alt text]()

## 🔖 Conclusiones

* El enfoque del curso ofrecido por Imagination, genera que el estudiante sienta curiosidad por investigar y abarcar más sobre estos temas, debido a que aunque no se tenga una fpga física, la herramienta de simulación es muy óptima para entender del tema.


## 📚 Referencias

* [Imagination University Programme](https://university.imgtec.com/) 
* [RVfpga – Introduction to RVfpgaSoC  V1.0](https://university.imgtec.com/resources/download/rvfpgasoc-v1-0/)
* [EH1 RISC-V SweRV CoreTM 1.9 from Western Digital](https://github.com/chipsalliance/Cores-SweRV.git) 
* [SweRVolf](https://github.com/chipsalliance/Cores-SweRVolf.git) 



#### Universidad Industrial de Santander
* 👩 Francy Jessenia Calderón Osorio - 2162491
* 👩 Andrea Paola Reyes Carreño - 2164095


# 馃摎 Proyecto Arquitectura de Computadores I.

# 馃捇 RVFPGASoC

Algunos de los grandes contribuyentes actuales en el 谩rea de tecnolog铆a, se unieron para crear e impartir un curso para el aprendizaje y estudio de SoCs, _System-on-Chips_, entre ellas resaltan Diligent, RISC-V, Xilinx, Imagination entre otros. 

En este curso se muestra como construir un subconjunto de SweRVolfX SoC desde cero utilizando bloques de construcci贸n como el n煤cleo SweRV, las memorias y los perif茅ricos, mediante el software de Vivado.
El proyecto se lleva a cabo en dos partes esenciales, la creaci贸n de un bloque mediante Vivado, y luego la ejecuci贸n de este mediante simulaci贸n con Verilator.

## 馃搶  Primera parte -Creaci贸n del diagrama de bloques-
Inicialmente se busca crear un diagrama de bloques en el cu谩l se incluyen los siguientes bloques de vivado:

| No. 	| **M贸dulo**             	| **Vivado bloque**     	|
|-----	|------------------------	|-----------------------	|
| 1   	| SweRV _Core_           	| swerv_wrapper_verilog 	|
| 1   	| _Interconnect Wrapper_ 	| intcon_wrapper_bd     	|
| 1   	| _Boot-ROM_             	| bootrom_wrapper       	|
| 1   	| _GPIO Top Module_      	| gpio_wrapper          	|
| 1   	| _System Controller_    	| syscon_wrapper        	|
| 32  	| _Bidirec Gpio Module_  	| bidirec               	|


Estos m贸dulos se interconectan mediante un instructivo proporcionado por _Imagination_, donde se especifican las conexiones.

Inicialmente se conectan los bloques `swerv_wrapper_verilog` y `intcon_wrapper_bd`, en estos bloques hay tres tipos de sets de pines, los cuales son,  IFU (_Instruction Fetch Unit_), LSU (_Load Store Unit_) y SB (_Store Byte_). La conexi贸n se realiza en el orden mencionado anteriormente, se contin煤a con el paso a paso de los otros bloques, al terminar las conexiones internas se inician las conexiones externas, estas son el reloj `clk`, el reset, `rst`, las memorias y finalmente los pines `bidir` de lso bloques bidirec, terminando con estos todas las conexiones del diagrama.

Luego de tener el diagrama de bloques completo se procede a generar el archivo de m贸dulo Verilog configurando en el archivo de Vivado, al obtener el archivo `BD.v` se procede a generar el _bitstream_, donde se muestra si el proceso fue realizado correctamente o genera alg煤n error.


![Alt text](https://i.imgur.com/ETXEuHu.png)

Al realizar la generaci贸n del _Bitstream_ se obtiene que la s铆ntesis, implementaci贸n y generaci贸n han sido realizadas completamente y mediante los resultados obtenidos en esta parte(Lab1), se procede a realizar la simulaci贸n del _SweRVolf SoC_.

## 馃搶  Segunda parte -Simulaci贸n del SweRVolfX SoC-


Esta secci贸n se puede realizar en simulaci贸n e implementandola en la FPGA, sin embargo para la realizaci贸n de est茅 se llevar谩 a cabo s贸lo la parte de simulaci贸n, adem谩s ser谩 realizada mediante las herramientas instaladas en Windows.

* VSCode
* PlatformIO
* GTKWave
* Cygwin


Esta parte se desarrolla mediante una serie de pasos.

### 鉁旓笍1. Copiar el archivo `BD.v`

En la simulaci贸n se emplear谩 una estructura c贸mo la que se muestra a continuaci贸n, teniendo como _top module_ el archivo `rvfpgasim`.

![Alt text](https://i.imgur.com/j6OcABD.png)

La simulaci贸n se trabajar谩 mediante Verilator, Primero se debe buscar el archivo generado en la primera parte (Lab1), llamado `BD.v`, para anexarlo a la ruta `RVfpgaSoC/Labs/LabResources/Lab2/src/SweRVolfSoC` como se muestra en la siguiente figura.


![Alt text](https://i.imgur.com/MuizMPG.png)

### 鉁旓笍2. Verificaci贸n de los modulos.

Ahora teniendo este archivo se verifica que existan y esten exactamente los nombres de los siguientes modulos: 

* _BD_bootrom_wrapper_0_0_
* _BD_gpio_wrapper_0_0_
* _BD_intcon_wrapper_bd_0_0_
* _BD_swerv_wrapper_verilog_0_0_
* _BD_syscon_wrapper_0_0_

![Alt text](https://i.imgur.com/U6Dm4sa.png)


### 鉁旓笍 3. Generar la simulaci贸n Binaria.

Para ello se ingresa a la direcci贸n `RVfpgaSoC/Labs/LabResources/Lab2/verilatorSIM` donde se encuentran los archivos `Makefile` y `swervolf_0.7.vc` los cuales brindan a Verilator informaci贸n sobre donde encontrar las fuentes SoC. 

![Alt text](https://i.imgur.com/thVUUrt.png)

Luego se genera el archivo binario mediante estos comandos.

```sh
/cygdrive/c/Users/Lenovo/Downloads/RVfpgaSoC/RVfpgaSoC/Labs/LabResources/Lab2/verilatorSIM
```
```sh
make clean
```
```sh
make
```
Con esto se genera un archivo RVFPGASim, que posteriormente se utilizar谩 para crear la traza de simulaci贸n del programa AL-Operations.

### 鉁旓笍 4. Generar la traza de simulaci贸n desde PlatformIO.

Se abre desde _PlatformIO_ la carpeta `AL_Operations` en ella se encuentra el archivo `platformio.ini`, se abre para editar una l铆nea donde se encuentra al ruta del archivo al cu谩l se le va a generar la traza, en este caso al final de la ruta establecida se agrega el archivo, `Vrvfpgasim.exe.` debido a que se est谩 trabajando en Windows.

Luego de editar esta l铆nea se procede correr la simulaci贸n y generar la traza en la opci贸n `Generate Trace`, luego de ello si se obtiene un `SUCCESS` como resultado indica que est谩 correcto.

![Alt text](https://i.imgur.com/1tkhRWw.png)

### 鉁旓笍 5. An谩lisis de la simulaci贸n con GTKWave.

Para visualizar el resultado final, mediante GTKWave se abre el archivo `Trace.vcd` y se busca el registro en el cual se muestra la salida final.
Sin embargo primero se buscan y visulizan las se帽ales que se ejecutan en cada sentido del n煤cleo RISC-V superescalar, ubicadas en el m贸dulo _ifu_ el cu谩l indica la unidad de obtenci贸n de instrucciones, la ruta a seguir una vez se est谩 en el GTKWave es `TOP/rvfpgasim/swervolf/swer_wrapper_verilog_0/swerv_eh1_2/swer/ifu` 

Imagen de la ruta seguida.

![Alt text](https://i.imgur.com/tMFm1G7.png)

A continuaci贸n se muestran dichas se帽ales llamadas `ifu_i0_instr[31:0]` y `ifu_i1_instr[31:0]` el _i0_ indica la forma superescalar 0 e _i1_ la forma superescalar 1 y  a su vez `instr[21:0]` hace referencia a la instrucci贸n de 32 bits.

![Alt text](https://i.imgur.com/OKtlsdn.png)

Despu茅s de visualizar las se帽ales anteriores se continua la b煤squeda del registro de salida para ello se ingresa a la siguiente ruta: `TOP/rvfpgasim/swervolf/swer_wrapper_verilog_0/swerv_eh1_2/swer/dec/arf/gpr_banks(0)/gpr(28)` .
El m贸dulo `gprff` contiene el valor del registro `t3` que en este caso es la salida requerida,  para visualizarlo se busca dentro de este m贸dulo a `gpr(28)` dentro de este se encuentra la se帽al `dout[31:0]` que muestra el contenido del registro `x28` empeleado en `AL_Operations.S`.

![Alt text](https://i.imgur.com/1p5xz5G.png)

En esta imagen se observa la forma en que se ejecutan las instrucciones mostrando el resultado mediante el registro mencionado anteriormente, donde se visualiza que se ejecuta una a una las instrucciones dadas a realizar, estas instrucciones est谩n implementadas mediante assembly.


## 鉂? Problemas presentados y solucionados.

* ### 馃毄 Error en Vivado al generar el _Bitstream_.

Al terminar de crear el diagrama de bloques en vivado y realizar las debidas configuraciones, se procede a fenerar el bitstream sin embargo se generan una serie de errores plasmados a continuaci贸n.

![Alt text](https://i.imgur.com/gHgeoUj.png)

Para resolver estos errores se procede a revisar cada bloque empleado en el diagrama,  sin embargo no se encuentra ninguna falla en esto. Otro aspecto que se tuvo en cuenta es que la versi贸n de Vivado empleada inicialmente era la 2018.3, se actualiz贸 a la versi贸n solicitada en el curso, la 2019.2 y se realiz贸 nuevamente el proceso, logrando as铆 la correcta generaci贸n del birstream.


* ### 馃毄Error al generar la simulaci贸n binaria.

Luego de correr los comandos mencionado en esta secci贸n, se muestran estos errores, en el archivo `verilater.cpp`.

![Alt text](https://i.imgur.com/cptHnjR.png)

Se soluciona a帽adiendo algunas librer铆as faltantes en el mismo archivo, como se muestra en la imagen.

* `#include<limits>`
* `#include<cstddef>`
* `#include<iostream>`

![Alt text](https://i.imgur.com/jYX7GSe.png)


## 馃敄 Conclusiones

* El enfoque del curso ofrecido por Imagination, genera que el estudiante sienta curiosidad por investigar y abarcar m谩s sobre estos temas, debido a que aunque no se tenga una FPGA f铆sica, la herramienta de simulaci贸n es muy 贸ptima para entender del tema.

* En cuanto al desarrollo de los  laboratorios muestran c贸mo crear un SoC a partir de un n煤cleo y otros componentes b谩sicos los cuales se trabajaron en el Laboratorio 1 y luego  c贸mo apuntarlo a un FPGA y ejecutar programas en el SoC reci茅n creado (Laboratorio 2).

* Usar en conjunto el amplio portafolio de software que permite a estudiantes y profesionales crear un SoC basado en RISC-V, implementandolo desde bloques, y obtener archivos `.v` y `.bit` los cu谩les son fundamentales para poder realizar la ejecuci贸n de programas escritos en **C** o **Assembly** de manera m谩s eficiente y realizar un debido an谩lisis de estos mediante la herramienta GTKWave.


## 馃摎 Referencias

* [Imagination University Programme](https://university.imgtec.com/) 
* [RVfpga 鈥? Introduction to RVfpgaSoC  V1.0](https://university.imgtec.com/resources/download/rvfpgasoc-v1-0/)
* [EH1 RISC-V SweRV CoreTM 1.9 from Western Digital](https://github.com/chipsalliance/Cores-SweRV.git) 
* [SweRVolf](https://github.com/chipsalliance/Cores-SweRVolf.git) 



#### Universidad Industrial de Santander
* 馃懇 Francy Jessenia Calder贸n Osorio - 2162491
* 馃懇 Andrea Paola Reyes Carre帽o - 2164095


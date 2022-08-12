# LAB 1: Deploy Program onto STM32
## Description
In LAB1, we deploy a pretrained sine regression model onto STM32H747. The input of the system is an x value, and the output is computed value of $\sin(x)$.

## Environment Setup
- mbed-cli==1.10.5
- python==3.9
- gcc-arm-none-eabi==9.2.1

1. Install [mbed-cli](https://os.mbed.com/docs/mbed-os/v6.15/build-tools/install-and-set-up.html) by pip :
```
$ python3 -m pip install mbed-cli
```

2. Install [GCC_ARM](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/downloads) by :
```
$ sudo apt-get install gcc-arm-none-eabi
```

### Git Pack Download
Downloaded file would be stored in folder named `Lab1`

```
$ mkdir Lab1 
$ cd Lab1
```
```
$ git clone https://github.com/marconi1964/tensorflow.git
```
```
$ cd tensorflow
```


## Build on PC
With this command, some necessary libraries and tools will be downloaded. Then test file along with all of its dependencies will be built. Makefile has instructed the C++ compiler to build the code and create a binary, which it will then run. The result should be like this:
    
```
$ make -f tensorflow/lite/micro/tools/make/Makefile test_hello_world_test
```

<!--
![](https://i.imgur.com/56qXKtP.png)
-->

## Deploy on STM32H747
```
$ make -f tensorflow/lite/micro/tools/make/Makefile TARGET=mbed OPTIMIZED_KERNEL_DIR=cmsis_nn generate_hello_world_mbed_project
```

Mbed requires source files to be structured in a certain way. The TensorFlow Lite for Microcontrollers Makefile knows how to do this for us, and can generate a directory suitable for Mbed. The building process should be like this:

<!--
![](https://i.imgur.com/tENEHNF.png)
-->
The directory below contains all of the example’s dependencies structured in the correct way for Mbed to be able to build it.

```
$ cd tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4_default/prj/hello_world/mbed
```



### Setting up Mbed
To get started, use the following command to specify to Mbed that the current directory is the root of an Mbed project:

```
$ mbed config root .
```
    
Next, instruct Mbed to download the dependencies and prepare to build:

```
$ mbed deploy
```

<!--
### Modify Mbed Configuration

By default, Mbed will build the project using C++ 98. However, TensorFlow Lite requires C++ 11. Run the following Python snippet to modify the Mbed configuration files so that it uses C++ 11. You should put `modify.py` in `tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4_default/prj/hello_world/mbed` and enter the command:

`$ python3 modify.py`
-->

### Modify header files

Replication arm_math.h and cmsis_gcc.h to correct folder.

```
$ cd ~/Lab1/tensorflow/
```

```
$ cp tensorflow/lite/micro/tools/make/downloads/cmsis/CMSIS/DSP/Include/arm_math.h  tensorflow/lite/micro/tools/make/gen/mbed_cortex m4_default/prj/hello_world/mbed/mbed-os/cmsis/TARGET_CORTEX_M/arm_math.h
```
```
$ cp tensorflow/lite/micro/tools/make/downloads/cmsis/CMSIS/Core/Include/cmsis_gcc.h  tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4_default/prj/hello_world/mbed/mbed-os/cmsis/TARGET_CORTEX_M/cmsis_gcc.h
```


### Compile 
```
$ cd tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4_default/prj/hello_world/mbed
```

1. To compile, run:

    ```
    $ mbed toolchain GCC_ARM
    ```
    ```
    $ mbed target DISCO_H747I
    ```
    ```
    $ mbed compile -c
    ```

2. This will produce a file named `mbed.bin` in `~/Lab1/tensorflow/tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4_default/prj/hello_world/mbed/BUILD/DISCO_H747I/GCC_ARM/`. To flash it to the board, copy the file to the volume mounted as a USB drive. For instance:\
```
$ cp /BUILD/DISCO_H747I/GCC_ARM/mbed.bin /media/<USER>/<BOARD_NAME>/
```
If the volume complains about being full, disconnect and reconnect the board and flash to it again.

## TODO: Cosine predictor
1. Find `train_hello_world_model.ipynb` in `~/Lab1/tensorflow/tensorflow/lite/micro/examples/hello_world/train`
2. modify code to cosine predictor, and replace model.cc
3. Replace `hello_world_test.cc`provide in our Lab1 Github: 
4. Run the following command to bulid:
```
$ make -f tensorflow/lite/micro/tools/make/Makefile test_hello_world_test
```

## (Optional) Control LCD on STM32H747I
1. Put `BSP` in
    `~/Lab1/tensorflow/tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4_default/prj/hello_world/mbed/`
    
2. Put `LCD_DISCO_F746NG.cpp` and `LCD_DISCO_F746NG.h`in
    `~/Lab1/tensorflow/tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4_default/prj/hello_world/mbed/tensorflow/lite/micro/examples/hello_world/`

3. Replace `output_handler.cc` in
     `~/Lab1/tensorflow/tensorflow/lite/micro/tools/make/gen/mbed_cortex-m4_default/prj/hello_world/mbed/tensorflow/lite/micro/examples/hello_world/output_handler.cc`




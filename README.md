# SRA BOARD 2020

Sra board is a development board based on esp32 with peripherals like motor driver, over current and reverse voltage protection circuit, programmable switches and leds, sensor ports.


![](/Documentation/assets/3d_sideview.png)

## Table of contents:
- [About this project](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#About-this-project)
- [What is development board](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#what-is-development-board-)
    - [Power supply unit](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#Power-supply-unit)
    - [Motor Driver](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#Motor-Driver)
    - [Sensor port](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#Sensor-port)
    - [Protection against reverse voltage and overcurrent](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#protection-against-reverse-voltage-and-overcurrent)
    - [Programmable switches and leds](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#Programmable-switches-and-leds)
    - [Power on/off switch](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#power-onoff-switch)
- [Images of the board](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#Images-of-the-board)
- [Major changes made in the new design](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#major-changes-made-in-the-new-design)
    - [7805 replaced by LM2596 buck](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#78055v-linear-regulator-replaced-by-lm2596-buck-circuit)
    - [ld33 replaced by AMS1117](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#ld3333v-replaced-by-ams1117)
    - [Reverse voltage protection diodes replaced by pmosfet](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#diodes-for-reverse-voltage-replaced-by-p-type-mosfet)
    - [L298N replaced by TB6612FNG](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#l298n-replaced-by-tb6612fng)
    - [4 motor channels and modes of motor driver](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#4-motor-channels-and-normal-parallel-mode-of-motor-driver)
    - [switch to bar graph led and more no. of switches](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#moving-back-to-vintage-bar-graph-led-and-more-no-of-switches)
- [3D model](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#3d-model)
- [Current Status](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#Current-Status)
- [Designer](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#Designer)
- [License](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/README.md#License)


## About this project:
- sra board is esp32 based development board used for Wall E and MARIO workshop conducted by  [SRA](https://github.com/SRA-VJTI) 
- Designed using eagle. Board and schematic files are [here](https://github.com/ombhilare999/SRA-BOARD-2020/tree/master/Eagle)  

## What is development board ?

<p align="center">
  <img width="460" height="300" src="/Documentation/assets/boards_compare.png">
</p>

In general every development board like sra board have following basic features:

- ### Power supply unit:
  - Microcontroller usually runs on 3.3V or 5V and input to the development board is normally 12V because of the motors.
  - So every development board has a power section which converts 12V to standard levels like 5V/ 3.3V for microcontroller and Sensors.
  - In previous year SRA board 12V to 5V was converted using LM7805 linear regulator. Using this 5V esp32 was powered.
  - Further this 5V was converted to 3.3V using LD33 linear voltage regulator. This 3.3V then given to the sensor port.

- ### Motor Driver:
    - Motors usually runs on 12V and microcontroller output is generally 5V/3.3V. So one need external motor driver circuitry to control motors according to the microcontroller input.
    - In sra board 2019 the motors was controlled using L298N motor driver which is a bjt based H Bridge motor driver.

- ### Sensor port:
    - According to the external sensor types usually development boards have on board sensor ports where sensor can be connected easily using FRC port.
    - Sra board 2019 contains two port for sensors. One port for LSA (line sensor array) and second port is for MPU6050.

- ### Protection against [reverse voltage](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjc8aaX1c3rAhXXXSsKHXphBgQQFjABegQICxAD&url=https%3A%2F%2Fwww.ti.com%2Flit%2Fpdf%2Fslva139&usg=AOvVaw0Qbub75JJ986MzLv6FYWKE) and overcurrent:
    - In sra board 2019 diodes are used in power line for reverse voltage protection.
    - And for the overcurrent protection of microcontroller and motor driver circuit fuses of 300mA and 3Amp are used respectively.

- ### Programmable switches and leds:
    - In general every development board should have some programmable switches and led for testing and control purpose.
    - So sra board 2019 consist of two programmable switches with two programmable leds.

- ### Power on/off switch:
    - Sra board 2019 has power switch for motor driver using which power supply to motor driver can be turned on or off.
    - Similarly there is a MCU switch for esp32 microcontroller using which microcontroller can be turned on or off.

## Images of the board:
- Images of SRA board 2020 are [here](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/Images/sra_board_images.md#images-of-sra-board-2020)

>Now that we covered basic of development board, let's talk about changes made in the new design.(for more detailed info of sra board 2019 checkout this [pdf](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/Documentation/assets/Sra-board-2019.pdf))

## Major changes made in the new design:
- ### **7805(5V linear regulator) replaced by [LM2596 buck](https://www.youtube.com/watch?v=m8rK9gU30v4) circuit:**
    - The reason of changing to buck circuit rather than using simple 7805 is the efficiency, output current and reliability of LM2596 is more than 7805.
    - The efficiency of LM2596 is upto 92% which is way better than 7805. The LM2596 can provide current upto 3 Amp so from now on servos in ros workshop can be run using on board regulator itself.
- ### **LD33(3.3V) replaced by [AMS1117](http://www.advanced-monolithic.com/pdf/ds1117.pdf)**:
    - Until last year we were using LD33 to convert 5V to 3.3V but after talking to our super senior we have decided to shift to more compact, reliable AMS1117(SOT-23) linear voltage regulator. (same voltage regulator i.e. AMS1117 is used on esp32 devkit c V4 module)

- ### **Diodes for reverse voltage replaced by P TYPE mosfet**:
    - Diodes in series of power line are too inefficient as compare to pmosfet and as we are using high rated motors and all it was too hard to maintain diode size. As current rating of diode increases it's size also increases.
    - so in this year board we have used pmosfet instead of diode. It is more efficient and it can handle more current than diodes used in earlier design.

- ### **L298N replaced by [TB6612FNG](https://dronebotworkshop.com/tb6612fng-h-bridge/)**:
    - L298N is a bjt based H bridge motor driver but it is less efficient as compare to the new mos based TB6612FNG.
    - Detailed comparison show below. As you can see the efficiency of TB6612FNG can reach up to 91-95% which is way higher than compare to the 40-70% efficiency of L298N.
    - The only drawback of TB6612FNG is less continuous current which is equal to 1.2 Amp. So for higher current capacity motors two TB6612FNG are given on the board. so they can be used in parallel mode to double the current capacity to 2.4 Amp.
    
    <p align="center">
        <img width="460" height="300" src="https://i1.wp.com/dronebotworkshop.com/wp-content/uploads/2019/12/TB6612-vs-L298N.jpeg?w=768&ssl=1">
    </p>

- ### **4 motor channels and normal, parallel mode of motor driver**:<br /><br />
    - In previous year sra board one L298N was used using which can only control two motors as l298n as two motor channels but in this year 2x TB6612FNG  motor driver is used so maximum 4 motors can be controlled using esp32.
    - Motor driver can be worked in two modes **Normal mode** and **parallel mode**:   
        1. **Normal Mode**:<br />
        <p align="center">
            <img width="460" height="300" src="/Documentation/assets/normal_mode.jpeg">
        </p>

        -  As discussed earlier in new design there are two motor drivers. Each TB6612FNG can control two motors. So therefore using two motor driver one can control 4 motors using 8 GPIO's of esp32.
        - For example: if 32 pin is HIGH(IN1 = HIHG) and pin 33 is low(IN2 = LOW) then motor 1 moves in the forward direction. 
        - So in normal mode 4 motors can be connected to the board and one thing to be noted is per channel/motor the current capacity is 1.2 Amp<br /><br />
        2. **Parallel Mode**:<br />
         <p align="center">
            <img width="460" height="300" src="/Documentation/assets/parallel_mode.jpeg">
        </p>

        -  Parallel mode is special feature, it is used for high rated motors. if suppose your motor needs more than 1.2 amp current then for that motor parallel mode should be used.
        -  In parallel mode the channel's directional pins and output pins are shorted. so only one motor is connected to one motor driver i.e. using two channels you can control one motor because of which current capacity is doubled to 2.4 amp.
        -  So in parallel mode two high rated motors can be controlled using esp32.
        -  One thing should be noted here that directional pin shorting is done by manual DPDT switch. If user turns on 'TB_A' switch then first motor driver goes into parallel mode and its directional pins are shorted. IN1 = IN3 = 25 and IN2 = IN4 = 26.If TB_A switch is off first motor driver goes into normal mode where gpio connections are IN1 = 32: IN2 = 33: IN3 = 25: IN4 = 26. This is all done automatically. 
        - In parallel mode the J1,J2,J3 and J4 junctions needs to be shorted.They shorts the output of the motor driver.
- ### **Moving back to vintage Bar Graph LED and more no. of switches**:<br /><br />
    - In previous year we were using 2 programmable switches and 2 programmable Leds but in this years design we have provided bar graph led which contains 10 Leds out of which two are reserved for 5V and 3.3V voltage indication purpose.
    -  So there are 8 programmable leds on the board. These leds multiplexed with directional pins of two motor driver to save pins.
    -  What are directional pins ? >> Every motor driver channel has two directional pins IN1, IN2. If IN1 is high and IN2 is low then motors move in clockwise direction and there are 4 channels on the board so 4*2 = 8 diretional pins are multiplexed with 8 programmable leds)
    -  With 8 leds in hand programmer can do crazy tricks for debugging. Some examples are as follows:
        1. According to the lsa data we can program 4 leds to turn on when line sensor detects white color and turn off for black color with this feature debugging will be easy.
        2. If motor is moving in forward direction according to that dedicated leds will be turn on indicating IN1 is high and IN2 is low because of this motion of the bot can be debugged.

## 3D model:

- 3D preview of *[SRA BOARD](https://a360.co/3c1Rjyv)*.

    1. Complete 3D model file of  [SRA_BOARD](https://github.com/ombhilare999/SRA-BOARD-2020/tree/master/3D%20models/sra_board_model)
    2. 3D models of motor driver, leds, esp32 etc. all are here :  [3d models of other components](https://github.com/ombhilare999/SRA-BOARD-2020/tree/master/3D%20models/Other_components_model)


## Current Status:

- The board is right now in testing phase.
- Thanks to [OSH](https://oshpark.com/) for providing us 100 dollar free coupon code for prototype boards.

## Designer:
- SRA BOARD 2020 was designed by [omkar bhilare](https://github.com/ombhilare999)

## License:
- [MIT License](https://github.com/ombhilare999/SRA-BOARD-2020/blob/master/LICENSE)




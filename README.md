
<!-- PROJECT LOGO -->
<br />

<p align="center">
  <a href="https://github.com/SRA-VJTI/sraboard-hardware-design">
    <img src="/documentation/assets/logo.png" alt="Logo" width="200" height="120">
  </a>

  <h3 align="center">SRA BOARD</h3>
  <p align="center">
    ESP32 based SRA board for robotics application
    <br />
    <br />
    <a href="https://github.com/SRA-VJTI/sraboard-hardware-design/tree/master/Eagle">Eagle</a>
    ·
    <a href="https://github.com/SRA-VJTI/sraboard-hardware-design/tree/master/Gerber%20files/CAMOutputs">Gerber</a>
    ·
    <a href="https://github.com/SRA-VJTI/sraboard-hardware-design/blob/master/Images/sra_board_images.md">Images</a>
    ·
    <a href="https://a360.co/3c1Rjyv">3D model</a>
  </p>
</p>

# SRA BOARD 2020

SRA board is a development board based on ESP32 with peripherals on board like programmable LEDs and switches, sensor ports, over current and reverse voltage protection circuit and finally the motor drivers.

![](/documentation/assets/3d_sideview.png)

## Table of contents:
- [About this project](#about-this-project)
- [what is a development board](#what-is-a-development-board)
    - [Power supply unit](#power-supply-unit)
    - [Motor Driver](#motor-driver)
    - [Sensor port](#sensor-port)
    - [Protection against reverse voltage and overcurrent](#protection-against-reverse-voltage-and-overcurrent)
    - [Programmable switches and LEDs](#programmable-switches-and-LEDs)
    - [Power on/off switch](#power-onoff-switch)
- [Images of the board](#images-of-the-board)
- [Major changes made in the new design](#major-changes-made-in-the-new-design)
    - [7805 replaced by LM2596 buck](#78055v-linear-regulator-replaced-by-lm2596-buck-circuit)
    - [ld33 replaced by AMS1117](#ld3333v-replaced-by-ams1117)
    - [Reverse voltage protection diodes replaced by P-MOSFET](#diodes-for-reverse-voltage-replaced-by-P-MOSFET)
    - [L298N replaced by TB6612FNG](#l298n-replaced-by-tb6612fng)
    - [Modes of the motor driver](#Modes-of-the-motor-driver)
    - [switch to bar graph led and more no. of switches](#moving-back-to-vintage-bar-graph-led-and-more-no-of-switches)
- [Problems and their solutions](#Problems-and-their-solutions)
    - [The simultaneous power supply issue](#The-simultaneous-power-supply-issue)
- [3D model](#3d-model)
- [Milestone](#Milestone)
- [Contributors](#Contributors)
- [Acknowledgements and Resources](#acknowledgements-and-resources)
- [License](#License)


## About this project:
- SRA board is ESP32 based development board used for Wall E and MARIO workshop conducted by  [SRA](https://github.com/SRA-VJTI) 
- Designed using eagle. Board and schematic files are [here](https://github.com/SRA-VJTI/sraboard-hardware-design/tree/master/Eagle)  

## What is a development board?

<p align="center">
  <img width="460" height="300" src="/documentation/assets/boards_compare.png">
</p>

In general, every development board like SRA board have the following basic features:

- ### Power supply unit:
  - Microcontroller usually runs on 3.3V or 5V and input to the development board is normally 12V because of the motors.
  - So every development board has a power section which converts 12V to standard levels like 5V/ 3.3V for microcontroller and Sensors.
  - In previous year SRA board 12V to 5V was converted using LM7805 linear regulator. Using this 5V ESP32 was powered.
  - Further, this 5V was converted to 3.3V using LD33 linear voltage regulator. This 3.3V then given to the sensor port.

- ### Motor Driver:
    - Motors usually run on 12V and microcontroller output is generally 5V/3.3V. So one needs external motor driver circuitry to control motors according to the microcontroller input.
    - In SRA board 2019 the motors were controlled using L298N motor driver which is a bjt based H Bridge motor driver.

- ### Sensor port:
    - According to the external sensor types usually, development boards have onboard sensor ports where the sensors can be connected easily using FRC port.
    - SRA board 2019 contains two port for sensors. One port for LSA (line sensor array) and the second port is for MPU6050.

- ### Protection against [reverse voltage](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjc8aaX1c3rAhXXXSsKHXphBgQQFjABegQICxAD&url=https%3A%2F%2Fwww.ti.com%2Flit%2Fpdf%2Fslva139&usg=AOvVaw0Qbub75JJ986MzLv6FYWKE) and overcurrent:
    - In SRA board 2019 diodes are used in power line for reverse voltage protection.
    - And for the overcurrent protection of microcontroller and motor driver circuit fuses of 300mA and 3Amp are used respectively.

- ### Programmable switches and LEDs:
    - In general, every development board should have some programmable switches and LEDs for testing and control purpose.
    - So SRA board 2019 consist of two programmable switches with two programmable LEDs.

- ### Power on/off switch:
    - SRA board 2019 has a power switch for the motor driver using which power supply to the motor driver can be turned on or off.
    - Similarly, there is a microcontroller switch for ESP32 microcontroller using which microcontroller can be turned on or off.

## Images of the board:
- Images of SRA board 2020 are [here](https://github.com/SRA-VJTI/sraboard-hardware-design/blob/master/Images/sra_board_images.md#images-of-sra-board-2020)

> Now that we covered basic of the development board, let's talk about changes made in the new design. (for more detailed info of SRA board 2019 checkout this [pdf](https://github.com/SRA-VJTI/sraboard-hardware-design/blob/master/documentation/assets/sra-board-2019.pdf)

## Major changes made in the new design:


| Topic  |  SRA board 2019  | SRA board 2020|
|:----:|:-------:| :-----: | 
| [12V to 5V](#78055v-linear-regulator-replaced-by-lm2596-buck-circuit)  | 7805 linear regulator | LM2596 buck |
|[5V to 3.3V](#ld3333v-replaced-by-ams1117)| LD33 | AMS1117 |
|[reverse voltage proteciton](#diodes-for-reverse-voltage-replaced-by-P-MOSFET) | Diodes | P-MOSFET |
|[Motor Driver](#l298n-replaced-by-tb6612fng)| L298N| TB6612FNG|
|[No. of motor channels](#Modes-of-the-motor-driver)|2|4|
|[No. of switches](#moving-back-to-vintage-bar-graph-led-and-more-no-of-switches)|2|4|
|[No. of LEDs](#moving-back-to-vintage-bar-graph-led-and-more-no-of-switches)|2|8|

- ### **7805(5V linear regulator) replaced by [LM2596 buck circuit](https://www.youtube.com/watch?v=m8rK9gU30v4):**
    - The reason for changing to buck circuit rather than using simple 7805 is the efficiency, output current and reliability of LM2596 is more than 7805.
    - The efficiency of LM2596 is up to 92% which is way better than 7805. The LM2596 can provide current up to 3 Amp so from now on servos in MARIO workshop can be run using onboard regulator itself.
- ### **LD33(3.3V) replaced by [AMS1117](http://www.advanced-monolithic.com/pdf/ds1117.pdf)**:
    - Until last year we were using LD33 to convert 5V to 3.3V but after talking to our super senior we have decided to shift to more compact, reliable AMS1117(SOT-23) linear voltage regulator. (same voltage regulator i.e. AMS1117 is used on ESP32 devkit c V4 module)

- ### **Diodes for reverse voltage replaced by P-MOSFET**:
    - Diodes in series of the power line are too inefficient as compare to P-MOSFET and as we are using high rated motors and all it was too hard to maintain diode size. As the current rating of diode increases, it's size also increases.
    - So in this year board, we have used P-MOSFET instead of a diode. It is more efficient and it can handle more current than diodes used in the earlier design.

- ### **L298N replaced by [TB6612FNG](https://dronebotworkshop.com/tb6612fng-h-bridge/)**:
    - L298N is a bjt based H bridge motor driver but it is less efficient as compared to the new mos based TB6612FNG.
    - Detailed comparison show below. As you can see the efficiency of TB6612FNG can reach up to 91-95% which is way higher than compared to the 40-70% efficiency of L298N.
    - The only drawback of TB6612FNG is the less continuous current which is equal to 1.2 Amp. So for higher current capacity motors, two TB6612FNG are given on the board. so they can be used in parallel mode to double the current capacity to 2.4 Amp.
    
    <p align="center">
        <img width="460" height="300" src="https://i1.wp.com/dronebotworkshop.com/wp-content/uploads/2019/12/TB6612-vs-L298N.jpeg?w=768&ssl=1">
    </p>

- ### **Modes of the motor driver**:
    - In previous year SRA board one L298N was used using which can only control two motors as l298n as two motor channels but in this year 2x TB6612FNG  motor driver is used so maximum 4 motors can be controlled using ESP32.
    - The motor driver can be worked in two modes **Normal mode** and **parallel mode**:   
        1. **Normal Mode**:<br />
        <p align="center">
            <img width="460" height="300" src="/documentation/assets/normal_mode.jpeg">
        </p>

        -  As discussed earlier in the new design there are two motor drivers. Each TB6612FNG can control two motors. Therefore, using two motor driver one can control 4 motors using 8 GPIO's of ESP32.
        - For example: if 32 pin is HIGH(IN1 = HIHG) and pin 33 is low(IN2 = LOW) then motor 1 moves in the forward direction. 
        - So in normal mode, 4 motors can be connected to the board and one thing to be noted is per channel/motor the current capacity is 1.2 Amp.<br /><br />
        2. **Parallel Mode**:<br />
         <p align="center">
            <img width="460" height="300" src="/documentation/assets/parallel_mode.jpeg">
        </p>

        -  Parallel mode is a special feature, it is used for high rated motors. if suppose your motor needs more than 1.2 amp current then for that motor parallel mode should be used.
        -  In parallel mode, the channel's directional pins and output pins are shorted. so only one motor is connected to one motor driver i.e. using two channels you can control one motor because of which current capacity is doubled to 2.4 amp.
        -  So in parallel mode, two high rated motors can be controlled using ESP32.
        -  One thing should be noted here that directional pin shorting is done by manual DPDT switch. If the user turns on 'TB_A' switch then first motor driver goes into the parallel mode and its directional pins are shorted. IN1 = IN3 = 25 and IN2 = IN4 = 26.If TB_A switch is off first motor driver goes into normal mode where GPIO connections are IN1 = 32: IN2 = 33: IN3 = 25: IN4 = 26. This is all done automatically.
        - In parallel mode, the J1, J2, J3 and J4 junctions need to be shorted. They short the output of the motor driver.
        
- ### **Moving back to vintage Bar Graph LED and more no. of switches**:<br /><br />
    - In the previous year, we were using 2 programmable switches and 2 programmable LEDs but in this year's design, we have provided bar graph led which contains 10 LEDs out of which two are reserved for 5V and 3.3V voltage indication purpose.
    -  So there are 8 programmable LEDs on the board. These LEDs multiplexed with directional pins of two motor driver to save pins.
    -  What are directional pins? >> Every motor driver channel has two directional pins IN1, IN2. If IN1 is high and IN2 is low then motors move in a clockwise direction and there are 4 channels on the board so 4*2 = 8 directional pins are multiplexed with 8 programmable LEDs)
    -  With 8 LEDs in hand, the programmer can do crazy tricks for debugging. Some examples are as follows:
        1. According to the LSA data, we can program 4 LEDs to turn on when the line sensor detects white colour and turn off for black colour with this feature debugging will be easy.
        2. If the motor is moving in a forward direction according to that dedicated LEDs will be turned on indicating IN1 is high and IN2 is low because of this motion of the bot can be debugged.


## Problems and their solutions:

- ### **The simultaneous power supply issue**:
    - ESP32 can be power using two ways. The first way is via the USB port given on the ESP32 and second way is powering ESP32 via providing a voltage on VIN pin.
    - In previous year SRA board if simultaneous power was given to the ESP32 then it won't work as there was no circuitry for such condition and it is also mentioned on ESP32 site.
    > *The power supply must be provided using one and only one of the options above, otherwise, the board and/or the power supply source can be damaged.*
    - So if we want both simultaneous power to work then we need some external circuit to handle this condition and accordingly pass only one signal to the ESP32 board.
    - #### Solution:
        - The solution for this issue was [diode auctioneering](http://doerry.org/norbert/papers/20190606-doerry-ashton-auctioneering-diodes.pdf)

        <p align="center">
            <img width="460" height="300" src="/documentation/assets/diode_vin.png">
        </p>

        - There is inbuilt BAT760 diode on the USB line on ESP32.
        - In short, if different voltages are applied at Vusb and Vin then the voltage with bigger magnitude will be given to LD1117 (LDO on ESP32), most of the time the voltage will be the same on Vin and Vusb i.e 5V.
        - But as we planned to use 1N5417 diode on Vin path which has more **Vf** (forward voltage) than bat760 so here voltage indifference will be created and in simultaneous power supply condition USB will be selected as its voltage will be more than Vin.

## 3D model:

- 3D preview of *[SRA BOARD](https://a360.co/3c1Rjyv)*.

    1. The complete 3D model file of  [SRA_BOARD](https://github.com/SRA-VJTI/sraboard-hardware-design/tree/master/3D%20models/sra_board_model)
    2. 3D models of motor driver, LEDs, ESP32 etc. all are here:  [ 3d models of other components](https://github.com/SRA-VJTI/sraboard-hardware-design/tree/master/3D%20models/Other_components_model)

<!-- Milestone -->
## Milestone:
- [x] Designing of the prototype board.
- [x] Modular testing of the circuit.
- [ ] Testing of prototype board.
- [ ] Final version.


<!-- CONTRIBUTORS -->
## Contributors:

- [Omkar Bhilare](https://github.com/ombhilare999): *Designer*
- [Saurabh Gupta](https://github.com/saurabh1002): *Mentor*
- [Udit Patadia](https://github.com/udit7395): *Mentor*
- [Laukik hase](https://github.com/laukik-hase): *Mentor*

<!-- ACKNOWLEDGEMENTS AND REFERENCES -->
## Acknowledgements and Resources
-   Thanks to [OSH](https://oshpark.com/) for providing us 100 dollars free coupon code for prototype boards.
-   Previous year design: [SRA board 2019](https://github.com/SRA-VJTI/PCB-Schematics-and-Layouts/tree/master/WallE-2.1%202018%20Dev%20Brd)
-   [README Template](https://github.com/roshanlam/ReadMeTemplate) by [roshanlam](https://github.com/roshanlam)

## License:
- Distributed under the  [MIT License](https://github.com/SRA-VJTI/sraboard-hardware-design/blob/master/LICENSE).





<!-- PROJECT LOGO -->
[![Stargazers][stars-shield]][stars-url]
[![Forks][forks-shield]][forks-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]


<br />

<p align="center">
  <a href="https://github.com/SRA-VJTI/sraboard-hardware-design">
    <img src="/documentation/assets/logo.png" alt="Logo" width="200" height="120">
  </a>

  <h3 align="center">SRA Development Board</h3>
  <p align="center">
    ESP32-based Development Board
    <br />
    <a href="https://github.com/SRA-VJTI/sraboard-hardware-design/tree/master/eagle">EAGLE</a>
    ·
    <a href="https://github.com/SRA-VJTI/sra-board-hardware-design/tree/master/gerber_files">Gerber</a>
    ·
    <a href="https://github.com/SRA-VJTI/sraboard-hardware-design/blob/master/documentation/images/sra_board_images.md#sra-board-2020-images">Images</a>
    ·
    <a href="https://a360.co/3c1Rjyv">3D Model</a>
  </p>

<p align="center">
<a href="https://oshpark.com/shared_projects/erADDDjN"><img src="https://oshpark.com/packs/media/images/badge-5f4e3bf4bf68f72ff88bd92e0089e9cf.png" alt="Order from OSH Park"></img></a>
</p>


# SRA Board 2022

The SRA board is a development board based on ESP32 with on-board peripherals like programmable LEDs and switches, sensor ports for Line Sensor Array and MPU-6050, protection circuit for over-current and reverse voltage and motor drivers.

![](/documentation/assets/3d_sideview.png)

## Table of Contents
- [SRA Board 2022](#sra-board-2022)
  - [Table of Contents](#table-of-contents)
  - [About the Project](#about-the-project)
  - [Getting Started with a Development Board](#getting-started-with-a-development-board)
  - [Major Changes for 2020](#major-changes-for-2020)
  - [Notable problems in the SRA Board 2019](#notable-problems-in-the-sra-board-2019)
  - [3D Models](#3d-models)
  - [Milestones](#milestones)
  - [Contributors](#contributors)
  - [Acknowledgements and Resources](#acknowledgements-and-resources)
  - [License](#license)


## About the Project
- This development board is used for the [Wall-E](https://github.com/SRA-VJTI/Wall-E_v2.1) and [MARIO](https://github.com/SRA-VJTI/ROS-Workshop-2.1) workshops conducted by [SRA](https://github.com/SRA-VJTI).
- Designed using EAGLE. The schematic and board files are [here](https://github.com/SRA-VJTI/sraboard-hardware-design/tree/master/eagle). 
- Resources for [previous work](https://github.com/SRA-VJTI/PCB-Schematics-and-Layouts/tree/master/WallE-2.1%202018%20Dev%20Brd).  For more details of the SRA board 2019, checkout this [link](https://github.com/SRA-VJTI/sraboard-hardware-design/blob/master/documentation/assets/sra-board-2019.pdf).
- The SRA board 2020 images can be found [here](https://github.com/SRA-VJTI/sraboard-hardware-design/blob/master/documentation/images/sra_board_images.md#sra-board-2020-images).

## Getting Started with a Development Board

<p align="center">
  <img width="460" height="300" src="/documentation/assets/boards_compare.png">
</p>

In general, every development board has the following basic features:

- ### Power Supply Unit
  - Microcontrollers (MCUs) usually run on 3.3V or 5V while input to a development board is normally 12V for motor control.
  - So, a *power* section which converts this 12V to standard levels like 5V/3.3V for MCU and sensors is present.
  - The previous edition of the SRA board (2019) used the LM7805 linear voltage regulator, for stepping down from 12V to 5V; this powered the ESP32.
  - Further, this 5V was converted to 3.3V using the LD33 linear voltage regulator, used by the sensor port.

- ### Motor Driver
    - Motors usually run on 12V and MCU output is generally 5V/3.3V. So, an external motor driver circuitry is required to control motors according to the MCU input.
    - The SRA Board 2019 used the L298N IC for motor-control, which is a BJT-based H-Bridge motor driver.

- ### Sensor Port
    - According to the external sensor types, usually development boards have onboard sensor ports where the sensors can be connected easily using FRC connector.
    - The SRA Board 2019 had two sensor ports - one for a LSA (line sensor array) and the second for the MPU6050.

- ### Protection against [Reverse Voltage](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjc8aaX1c3rAhXXXSsKHXphBgQQFjABegQICxAD&url=https%3A%2F%2Fwww.ti.com%2Flit%2Fpdf%2Fslva139&usg=AOvVaw0Qbub75JJ986MzLv6FYWKE) and Over Current
    - The SRA Board 2019 used diodes for reverse voltage protection in the power-line.
    - For the overcurrent protection of MCU and motor driver circuit, fuses of 300mA and 3A were used respectively.

- ### Programmable Switches and LEDs
    - Every development board should have some programmable switches and LEDs for testing, control and debugging purposes.
    - The previous edition had a pair of programmable switches and programmable LEDs each.

- ### Power Switch
    - The SRA Board 2019 had a power switch for the motor driver, using which power supply to the motor driver can be toggled. Similarly, there was a switch for the ESP32 MCU.


> Now that we covered basics of development boards, let us talk about the changes made in the new design. 

## Major Changes for 2020

| Feature  |  SRA Board 2019  | SRA Board 2020|
|:----:|:-------:| :-----: |
|[12V to 5V](#7805-5v-linear-regulator-to-lm2596-buck-convertor)  | LM7805 Linear Regulator | LM2596 Buck Convertor |
|[5V to 3.3V](#ld33-33v-to-ams1117)| LD33 | AMS1117 |
|[Reverse Voltage Protection](#reverse-voltage-protection-diodes-to-p-mosfet) | Diodes | P-MOSFET |
|[Motor Driver](#l298n-to-tb6612fng)| L298N| TB6612FNG|
|[No. of Motor Channels](#motor-driver-modes)|2|4|
|[No. of Switches](#moving-back-to-the-vintage-bar-graph-leds-and-more-switches)|2|4|
|[No. of LEDs](#moving-back-to-the-vintage-bar-graph-leds-and-more-switches)|2|8|

- ### **7805 (5V linear regulator) to [LM2596 Buck Convertor](https://www.youtube.com/watch?v=m8rK9gU30v4)**
    - The greater efficiency, output current and reliability of LM2596 were the reasons for this change.
    - The efficiency of LM2596 is up to 92% which is significantly better than 7805. The LM2596 can provide current up to 3A, so  the MARIO workshop manipulator can now be run using onboard regulator.
- ### **LD33 (3.3V) to [AMS1117](http://www.advanced-monolithic.com/pdf/ds1117.pdf)**:
    - The previous edition used the LD33 IC to step down from 5V to 3.3V; several discussions resulted in the shift to more compact, reliable AMS1117(SOT-23) linear voltage regulator. (_AMS1117 is used in the ESP32-DevKitC V4 module_)

- ### **Reverse voltage protection: Diodes to P-MOSFET**
    - Diodes in series to the power line are inefficient as compare to a P-MOSFET. Dut to the usage of high-rated motors, it was difficult to manage the diode size and the current rating. (_As the current rating of diode increases, its size also increases._)
    - So, the new edition uses the P-MOSFET instead of a diode, which is more efficient and can handle more current.

- ### **L298N to [TB6612FNG](https://dronebotworkshop.com/tb6612fng-h-bridge/)**
    - L298N is a BJT-based H-bridge motor driver but it is less efficient as compared to the new MOS-based TB6612FNG.
    - The detailed comparison is shown below. As you can see the efficiency of TB6612FNG can reach up to 91-95% which is significantly higher than the 40-70% efficiency of L298N.
    - The only drawback of TB6612FNG is the less continuous current which is equal to 1.2A. So, for higher current capacity motors, two TB6612FNG are given on the board, which can be used in parallel mode to double the current capacity to 2.4A.
    
    <p align="center">
        <img width="460" height="300" src="https://i1.wp.com/dronebotworkshop.com/wp-content/uploads/2019/12/TB6612-vs-L298N.jpeg?w=768&ssl=1">
    </p>

- ### **Motor Driver Modes**
    - The new edition has 2x TB6612FNG motor drivers which allow a maximum of 4 motors to be controlled. This motor driver is characterized by its operation in two modes - **Normal mode** and **Parallel mode**:   
        1. **Normal Mode**
        <br />
        <p align="center">
        <img width="460" height="300" src="/documentation/assets/normal_mode.jpeg">
        </p>

        -  As discussed earlier, the new design has two motor drivers. Each TB6612FNG can control two motors. Therefore, using two motor driver one can control 4 motors using 8 GPIO's of ESP32.
        - E.g.: If pin 32 is HIGH(IN1 = HIHG) and pin 33 is low(IN2 = LOW) then motor 1 moves in the forward direction. 
        - So in normal mode, 4 motors can be connected to the board, with a per channel/motor current capacity of 1.2A.
        <br/><br/>

        1. **Parallel Mode**
        <br />
        <p align="center">
        <img width="460" height="300" src="/documentation/assets/parallel_mode.jpeg">
        </p>

        -  The parallel mode is a special feature, used for high-rated motors, requiring more than the 1.2A current limit.
        -  In this mode, the channel's directional pins and output pins are shorted; only one motor is connected to a motor driver i.e. two channels, giving a current capacity of 2.4A. Thus, two high rated motors can be controlled using ESP32.
        -  Note: The directional pin shorting is done by a manual DPDT switch. If the user turns on TB_A switch then the first motor driver goes into the parallel mode and its directional pins are shorted, where GPIO connections are IN1 = IN3 = 25 and IN2 = IN4 = 26. If TB_A switch is off, then the first motor driver goes into normal mode where IN1 = 32: IN2 = 33: IN3 = 25: IN4 = 26. This is all done automatically. Also for parallel mode, the J1, J2, J3 and J4 junctions need to be shorted.
        <br/><br/>

- ### **Moving back to the vintage Bar-graph LEDs and more switches**
    - The previous edition used a pair of programmable switches and LEDs each but in this year, the provision for a bar graph LED has been made. It has 10 LEDs out of which two are reserved for 5V and 3.3V voltage indication.
    -  So there are 8 programmable LEDs on the board. These LEDs multiplexed with directional pins of the two motor drivers to save pins.
    -  Directional pins? >> Every motor driver channel has two directional pins IN1, IN2. If IN1 is high and IN2 is low then motors move in a clockwise direction and there are 4 channels on the board, so 4 * 2 = 8 directional pins are multiplexed with 8 programmable LEDs.
    -  With 8 LEDs in hand, debugging get easier. Some examples - 
        1. According to the line sensor array (LSA) data, one can program 4 LEDs to turn on when the line sensor detects white  and turn off for black- line following debugging.
        2. If a motor is moving in a forward direction, the dedicated LEDs will be indicating IN1 is high and IN2 is low - motor control debugging.


## Notable problems in the SRA Board 2019

- ### **The Simultaneous Power Supply Issue**
    - ESP32 can be power using two ways - one via the USB port given on the ESP32 and two via providing a voltage on VIN pin.
    - In the SRA Board 2019, if simultaneous power (on both the above sources) was provided to the ESP32 then it won't work as there was no circuitry for handling such a condition.
    - The following disclaimer from the official ESP32 documentation made it essential to design some external circuit to handle this condition and accordingly pass only one signal to the ESP32 board.
    > *The power supply must be provided using one and only one of the above options.Otherwise, the board and/or the power supply source can be damaged.*
    - #### Solution
        - The solution for this issue was [Diode Auctioneering](http://doerry.org/norbert/papers/20190606-doerry-ashton-auctioneering-diodes.pdf).

        <p align="center">
            <img width="460" height="300" src="/documentation/assets/diode_vin.png">
        </p>

        - There is an inbuilt BAT760 diode on the USB line on ESP32. If different voltages are applied at Vusb and Vin, then the voltage with bigger magnitude will be given to LD1117 (LDO on ESP32); often the voltage will be the same on Vin and Vusb i.e 5V.
        - But, the usage of the 1N5417 diode on the Vin path, which has more **Vf** (forward voltage) than the BAT760, will create a voltage indifference and in a simultaneous power supply condition, USB will be selected as its voltage will be more than Vin.

## 3D Models

- 3D preview of the *[SRA Board 2020](https://a360.co/3c1Rjyv)*

    1. The complete 3D model (.stl) file of [SRA Board 2020](https://github.com/SRA-VJTI/sraboard-hardware-design/tree/master/3d_models/sra_board_model)
    2. The 3D models of motor driver, LEDs, ESP32 etc.: [3d models of other components](https://github.com/SRA-VJTI/sraboard-hardware-design/tree/master/3d_models/other_components_model)

<!-- Milestone -->
## Milestones
- [x] Designing of the prototype board
- [x] Modular testing of the circuit
- [x] Testing of prototype board
- [ ] Final version

<!-- CONTRIBUTORS -->
## Contributors

- [Omkar Bhilare](https://github.com/ombhilare999): *Designer*
- [Dhiraj Patil](https://github.com/dhirajp15): *Mentor*
- [Saurabh Gupta](https://github.com/saurabh1002): *Mentor*
- [Udit Patadia](https://github.com/udit7395): *Mentor*
- [Laukik Hase](https://github.com/laukik-hase): *Mentor*

<!-- ACKNOWLEDGEMENTS AND REFERENCES -->
## Acknowledgements and Resources
-   Thanks to [OSH](https://oshpark.com/) for providing us with free coupons worth $100 for printing the prototype boards.
-   Previous Edition: [SRA Board 2019](https://github.com/SRA-VJTI/PCB-Schematics-and-Layouts/tree/master/WallE-2.1%202018%20Dev%20Brd)
-   [Eagle Tutorials](https://www.youtube.com/playlist?list=PL868B73617C6F6FAD) 
-   [README Template](https://github.com/roshanlam/ReadMeTemplate) by [roshanlam](https://github.com/roshanlam)

## License
- Distributed under the [MIT License](https://github.com/SRA-VJTI/sraboard-hardware-design/blob/master/LICENSE).


[forks-shield]:https://img.shields.io/github/forks/SRA-VJTI/sra-board-hardware-design
[forks-url]: https://github.com/SRA-VJTI/sra-board-hardware-design/network/members

[stars-shield]: https://img.shields.io/github/stars/SRA-VJTI/sra-board-hardware-design
[stars-url]: https://github.com/SRA-VJTI/sra-board-hardware-design/stargazers

[issues-shield]: https://img.shields.io/github/issues/SRA-VJTI/sra-board-hardware-design
[issues-url]: https://github.com/SRA-VJTI/sra-board-hardware-design/issues

[license-shield]: https://img.shields.io/github/license/SRA-VJTI/sra-board-hardware-design
[license-url]: https://github.com/SRA-VJTI/sra-board-hardware-design/blob/master/LICENSE



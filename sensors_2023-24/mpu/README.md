# TABLE OF CONTENTS
- [TABLE OF CONTENTS](#table-of-contents)
- [MPU6050](#mpu6050)
- [Connection with SRA Board](#connection-with-sra-board)
- [Inertial Measurement Unit](#inertial-measurement-unit)
- [Initialising MPU6050](#initialising-mpu6050)
  - [Gyroscope](#gyroscope)
  - [Reading Gyroscope Data](#reading-gyroscope-data)
  - [Accelerometer](#accelerometer)
  - [Reading Accelerometer Data](#reading-accelerometer-data)
  - [Accelerometer Euler Angles](#accelerometer-euler-angles)
  - [Complementary Filter](#complementary-filter)
  - [Digital Motion Processor](#digital-motion-processor-dmp)
  - [Reading Register Data](#reading-register-data)

# Connection with SRA Board
<img width="599" alt="PNG image" src="https://user-images.githubusercontent.com/72294682/152956585-ac4468b6-e78d-474f-aab1-e3f682476b97.png">


# MPU6050
- The MPU6050 is a **6-axis/6-DOF Motion Tracking Device**.
- It is a *Micro Electro-Mechanical System (MEMS)*.
> MEMS is a technology defined as miniaturized mechanical and electro-mechanical elements (i.e., devices and structures) that are made using the techniques of microfabrication.
> Their dimensions can range from one micron to several millimeters.
- The 6050 has both a 3-axis Accelerometer and 3-axis Gyroscope integrated on a single chip.
- It is a commonly used chip that combines a MEMS gyroscope and a MEMS accelerometer and uses a standard I2C bus for data transmission
- The system processor is an I2C master to the MPU6050 (default address is 0x68 but can be changed to 0x69)
- The MPU6050 can also act as an I2C master and directly accept inputs from an external compass
- This is 3.3V device and hence is safe to connect to ESP32 pins directly.

![MPU6050](https://cdn.shopify.com/s/files/1/0014/4313/5560/products/accelerometer-and-gyroscope-mpu6050_1280x.jpg?v=1577794144)

To give you some context:
> A Gyroscope is a device that measures the rate of change of angular position, aka the rotational velocity, over time in all 3 axes.

>An accelerometer is a tool that measures acceleration in its own instantaneous rest frame(e.g. an accelerometer at rest on the surface of the Earth will measure an acceleration due to Earth's gravity, straight upwards of g ≈ 9.81 m/s<sup>2</sup> and will measure zero acceleration when in freefall)

- When the 6050 is used, it returns 6 values; 3 from the accelerometer and the other 3 from the gyroscope.

# Inertial Measurement Unit

- These are small devices indicating changing orientation in smartphones, video game remotes, quadcopters, etc.
- The IMU has both a gyroscope which measures angular rate and an accelerometer which measures linear acceleration, and/or compasses (magnetometers)
- The number of sensor inputs in an IMU are referred to as ‘DoF’ (Degrees of Freedom), so a chip with a 3-axis gyroscope and a 3-axis accelerometer would be a 6-DOF IMU.

# Contents
- MPU6050 includes **Digital Motion Processor (DMP)**, which has property to solve complex calculations.
- It also consists of a 16-bit Analog to Digital Converter(ADC) hardware. This enables it to record 3-dimension motion at the same time.
- It has **I2C bus interface** to communicate with various microcontrollers.
- The main feature is that the MPU6050 is less expensive and that it can easily combine with accelerometer and gyroscope.

### Pins

![Pins](https://hackster.imgix.net/uploads/attachments/998991/mpu6050-pinout_yCBm0odT11.png?auto=compress%2Cformat&w=680&h=510&fit=max)

The MPU-6050 module has 8 pins,

<code>VCC</code>: Power supply pin. Connect this pin to +5V DC supply.

<code>GND</code>: Ground pin. Connect this pin to ground connection.

<code>SCL</code>: Serial Clock pin. Connect this pin to the microcontroller's SCL pin.

<code>SDA</code>: Serial Data pin. Connect this pin to the microcontroller's SDA pin.

<code>XDA</code>: Auxiliary Serial Data pin. This pin is used to connect other I2C interface enabled sensors' SDA pin to MPU-6050.

<code>XCL</code>: Auxiliary Serial Clock pin. This pin is used to connect other I2C interface enabled sensors' SCL pin to MPU-6050.

<code>AD0</code>: I2C Slave Address LSB pin. This is 0th bit in 7-bit slave address of device. If connected to VCC, then read as logic one and slave address changes.

<code>INT</code>: Interrupt digital output pin.

---------
# Working of the 6050

## Initialising MPU6050

- A sample wiring of the device to ESP32 looks as follows:

![](https://lh5.googleusercontent.com/OiTQl8mZl41Dwa_Nb6PD6yzMigvv6xV7KnY7bfW56dBSoYuK8shYet7VMev0rVWcFWhirvUIenbFn_B6jc_nmTulH4-3vInHrOZX3sW4f2lBvel_HcthTW_O89FAGwankOtBBJ8aLhY)

- When powered up, MPU6050 will start in SLEEP mode. To use it, we need to first disable this SLEEP mode 
- To use the I2C protocol, we send the value 0 to register PWR_MGMT_1 (0x6b)
- The procedure to do so is as follows :
   1. Send a start sequence
   2. Send the I2C address (0x68) of the MPU6050 with the R/W bit high
   3. Send the internal register number you want to write to (0x6b : PWR_MGMT_1)
   4. Send the data byte (0x00) - This will power on the device
   5. Send the stop sequence


![](https://lh5.googleusercontent.com/HsUgcO1ojRlHZgbtl1Pyk7aWiUjs1x7XnhI5oYsa8C2gCqAbfqOGpr9fnjk0TBxWHmoIfJEQLqHUqe4Kh3KsjMLKSHAD6jQMsiEwrNyA3KLSeHiNLC3tDjzjAKN_nENu8FMp0tYS60E)

 The data byte 0x00 sets all the bits of register 107 to 0, thus disabling SLEEP mode and CYCLE mode, and powering on the MPU-6050 

![](https://lh3.googleusercontent.com/4p2RF-ezjP6jtT6S7xBQGfls0bcj7JM27W_ZsgRj5pYGQKBnrHfP85MtPncMeuu5qmSEupHdCSu2qPf4TSSBbejcN5MMqU5gLvLMaUDsw9K-Zp7vwLNIiD42d8xdfBKu0A0qxFaS6jk)



There are various terms related with the Pulse Width Modulation:- 
- **Off-Time** :- Duration of time period when the signal is low.
- **ON-Time**  :- Duration of time period when the signal is high.
- **Duty cycle** :- It is the percentage of the time period when the signal remains ON during the period of the pulse width modulation signal.
- **Period** :- It is the sum of off-time and on-time of pulse width modulation signal.

### Gyroscope
![](https://www.electronicwings.com/public/images/user_images/images/Sensor%20%26%20Modules/MPU6050/2_Oreintation_Polarity_of_Rotation_MPU6050.PNG)
- When the gyros are rotated about any of the sense axes, the C*oriolis Effect* causes a vibration that is detected by a MEM inside MPU6050.

- The resulting signal is amplified, demodulated, and filtered to produce a voltage that is proportional to the angular rate.

-  This voltage is digitized using 16-bit ADC to sample each axis.

-  The full-scale range of output are **+/- 250, +/- 500, +/- 1000, +/- 2000.**

-  It measures the angular velocity along each axis in *degree per second* units.

### Reading Gyroscope Data

- The acceleration of the three axes, X,Y and Z, are stored in 3 pairs of registers on the MPU6050
- Each register on the MPU6050 is an 8-bit register
- In each of these pairs, one register stores the MSB (most significant bit) and the other stores the LSB (least significant bit)
- These 6 registers are [burst-read](#reading-register-data) and the 8-bit MSB and LSB bytes for each axis are combined to give a single 16-bit float
- The gyroscope has a sensitivity factor of about 131, i.e. 1 degree of rotation gives a reading of 131 units
- Therefore, the degrees of rotation are obtained by dividing the raw readings by 131 
- Euler angles are calculated by integrating raw readings of each axis over time

![](https://lh3.googleusercontent.com/dQ-SZL7kp5WEnFb-KXFH-FDvF7yv_2J28ycODkipFfFAAMN1lYAk0rPzKg7DaPcNDYk-BdUEJQSzApxxESam_7tnbsIOcLenjofFoUiCpAhsyuMtkWaFMcZPJrK5EMJyX9GCNHSNeeo)

### Accelerometer
![Tilt](https://www.electronicwings.com/public/images/user_images/images/Sensor%20%26%20Modules/MPU6050/3_Angle_of_Inclination.png)

![Accelerometer](https://camo.githubusercontent.com/10eb856f2ada00b1514210c1d2005d174bc4179e601a59aa4f6a83001da0e51d/68747470733a2f2f6c6173746d696e757465656e67696e656572732e636f6d2f77702d636f6e74656e742f75706c6f6164732f61726475696e6f2f4d454d532d416363656c65726f6d657465722d576f726b696e672e676966)
- The MPU6050 uses a MEMS accelerometer, which consists of stationary and movable electrodes acting as **capacitor plates**
- Acceleration along the axes deflects the movable mass. It produces change in g-forces on the moving device, which causes displacement of the movable capacitor plates.
-  This displacement of moving plate (mass) unbalances the differential capacitor, which causes a change in the capacitance, which is then detected internally by change in voltage. This in turn results in sensor output. Output amplitude is proportional to acceleration.
-  16-bit ADC is used to get digitized output.
-  The full-scale range of acceleration are **+/- 2g, +/- 4g, +/- 8g, +/- 16g.**
-  It is measured in *g (gravity force)* unit; for example when the device is placed on a flat surface, it will measure 0g on X and Y axes and +1g on Z axis.

### Reading Accelerometer Data

- The acceleration of the three axes, X,Y and Z, are stored in 3 pairs of registers on the MPU6050
- Each register on the MPU6050 is an 8-bit register
- In each of these pairs, one register stores the MSB (most significant bit) and the other stores the LSB (least significant bit)
- These 6 registers are [burst-read](#reading-register-data) and the 8-bit MSB and LSB bytes for each axis are combined to give a single 16-bit float

### Accelerometer Euler Angles 

- The roll and pitch angles are calculated from the accelerometer using the [formula](https://www.digikey.com/en/articles/using-an-accelerometer-for-inclination-sensing) below :
  
<img src="https://render.githubusercontent.com/render/math?math=\theta = \arctan(\frac{A_{X.OUT}}{A_{Y.OUT}})">

### Complementary Filter
* Since gyroscopic drift accumulates over time, it is necessary to reset the rate readings from the gyroscope at frequent intervals, so the drift can be minimised
* To do this, we combine both the gyroscope and accelerometer readings in a weighted average
* The value of alpha is determined experimentally
* [Formula](http://www.pieter-jan.com/images/equations/CompFilter_Eq.gif):

![](http://www.pieter-jan.com/images/equations/CompFilter_Eq.gif)
   

### Digital Motion Processor (DMP)
- Since the 6050 has multiple sensors, the input from these sensors can be fed into various complex algorithms that take raw data and convert it into extremely useful values(like an Inertial Navigation System as used in drones).
> Note that gyroscope and accelerometer sensor data of MPU6050 module consists of 16-bit raw data in 2’s complement form.
- These algorithms are known as Fusion algorithms since they take input from multiple sensor types. 
- The only problem is that these algorithms are:
    - hard to understand since they contain a lot of mathematics and tend to be complicated, and
    - demanding in RAM because of the amount of calculations that they require.
- This means that these algorithms are barely able to run on a relatively simple controller. 

This is where the DMP comes in.
- The MPU-6050 DMP is quite sophisticated. It supports **3D motion processing and gesture recognition algorithms**. It even reports temperature, and if you connect an external magnetometer, the DMP can also give you heading information.
- It is a motion sensing device that solve the problem mentioned above by running fusion algorithms internally.
- Hence, the SRA Board will be able to receive clean and reliable "fused" motion information from the module, instead of noisy motion data.

### Reading Register Data

* To read/receive data from a slave, the master has to first send a START condition addressing the MPU6050
* Then the master must WRITE the requested register from which the data is to be read
* After this, instead of sending a STOP condition to stop the WRITE operation and then sending a consecutive START condition to start the READ operation,
  the I2C bus provides the **repeated start** functionality, wherein instead of sending a STOP-START condition, we send only a START condition and read the
  specified number of bytes from the register.
* The procedure to do so is as follows :
  1. Send a start sequence
  2. Send the I2C address (0x68) of the MPU6050 with the R/W bit low - This writes to the device
  3. Send the Internal register address you want to read from
  4. Send a start sequence again (repeated start)
  5. Send the I2C address of the device with the R/W bit high - This reads from the device
  6. Read the number of the requested data bytes from the registers
  7. Send the stop sequence.
  6. All write and read operations (except the last read) are answered with a ACK if successful.

 ![](http://www.i2c-bus.org/static/i2c/repeatedstart.gif)

 ![](https://i.stack.imgur.com/FHPKk.png)

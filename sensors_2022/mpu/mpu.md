# The MPU 6050
### What is it?
- The MPU6050 is a **Motion Tracking Device**. It is one of the world's only **6-axis/6-DOF** tracking device.
- It is a *Micro Electro-Mechanical System (MEMS)*.
> MEMS is a technology defined as miniaturized mechanical and electro-mechanical elements (i.e., devices and structures) that are made using the techniques of microfabrication.
> Their dimensions can range from one micron to several millimeters.
- The 6050 has both a 3-axis Accelerometer and 3-axis Gyroscope integrated on a single chip.

![MPU6050](https://cdn.shopify.com/s/files/1/0014/4313/5560/products/accelerometer-and-gyroscope-mpu6050_1280x.jpg?v=1577794144)

To give you some context:
> A Gyroscope is a device that measures the rate of change of angular position, aka the rotational velocity, over time in all 3 axes.

>An accelerometer is a tool that measures acceleration in its own instantaneous rest frame(e.g. an accelerometer at rest on the surface of the Earth will measure an acceleration due to Earth's gravity, straight upwards of g ≈ 9.81 m/s<sup>2</sup> and will measure zero acceleration when in freefall)

- When the 6050 is used, it returns 6 values; 3 from the accelerometer and the other 3 from the gyroscope.

### Contents
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
## Working of the 6050
### Gyroscope
![](https://www.electronicwings.com/public/images/user_images/images/Sensor%20%26%20Modules/MPU6050/2_Oreintation_Polarity_of_Rotation_MPU6050.PNG)
- When the gyros are rotated about any of the sense axes, the C*oriolis Effect* causes a vibration that is detected by a MEM inside MPU6050.

- The resulting signal is amplified, demodulated, and filtered to produce a voltage that is proportional to the angular rate.

-  This voltage is digitized using 16-bit ADC to sample each axis.

-  The full-scale range of output are **+/- 250, +/- 500, +/- 1000, +/- 2000.**

-  It measures the angular velocity along each axis in *degree per second* units.

### Accelerometer
![Tilt](https://www.electronicwings.com/public/images/user_images/images/Sensor%20%26%20Modules/MPU6050/3_Angle_of_Inclination.png)

- Acceleration along the axes deflects the movable mass.
-  This displacement of moving plate (mass) unbalances the differential capacitor which results in sensor output. Output amplitude is proportional to acceleration.

-  16-bit ADC is used to get digitized output.

-  The full-scale range of acceleration are **+/- 2g, +/- 4g, +/- 8g, +/- 16g.**

-  It is measured in *g (gravity force)* unit; for example when the device is placed on a flat surface, it will measure 0g on X and Y axes and +1g on Z axis.

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


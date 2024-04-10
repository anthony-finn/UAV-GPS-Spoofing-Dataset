# UAV-GPS-Spoofing-Dataset
## Paper: Detecting Stealthy GPS Spoofing Attack Against UAVs Using Onboard Sensors
Paper reference goes here

## Introduction
This repository provides a summary and description of the UAV GPS Spoofing dataset. The dataset was collected using PX4-SITL Gazebo-Classic in 2023 using a **quadcopter** drone.

## Dataset Description
The dataset encompasses measurements from various onboard sensors, including the barometer, magnetometer, inertial measurement unit, and global positioning system. The **Attack Enabled** field classifies whether an attack was enabled at a point in time. The data was collected at a rate of **~250Hz**. The dataset can be resampled to other frequencies to emulate different sensor frequencies.

### GPS Attack
A GPS attack was constructed by modifying Gazebo's GPS sensor plugins and RPC messages. **The formulation of the GPS attack can be found within the paper.** The attack involves the idea of deviating the drone from its planned flight trajectory while preventing the drone from detecting the attack through an EKF or another method. Our attack achieves this by incrementally offsetting the drone’s reported GPS position by certain amount for each reported GPS step. If the increment is small enough, the GPS attack will go undetected and can lead to a large difference in groundtruth position and reported GPS position.

### Flight Paths
There are three flight plans used to generate the data: a straight flight, curved plan, and random plan. 

#### Straight
The most common flight scenario involves the UAV moving in a straight line towards a destination.

[![Straight Flight Plan Image](https://github.com/anthony-finn/UAV-GPS-Spoofing-Dataset/blob/main/Images/Plan1_Cropped.png)](https://github.com)

#### Curved
Another common scenario is the drone moving in a curved path.The drone may weave around objects or buildings during it's flight.

[![Curved Flight Plan Image](https://github.com/anthony-finn/UAV-GPS-Spoofing-Dataset/blob/main/Images/Plan2_Cropped.png)](https://github.com)

#### Random
These flight plans are a combination of the straight and curved flight plans. For example, the UAV may move in a straight line path, but transition into a curved flight plan.

[Image]

## Features

#### Barometer
| Feature Name | Data Range | Units | Description |
|  --------  |  -------  | ------- | -------  
| Absolute Pressure | [0, ∞) | Bar |
| Altitude | (∞, ∞) | m |
| Temperature | (∞, ∞) | °C |

#### Magnetometer
| Feature Name | Data Range | Units | Description |
|  --------  |  -------  | ------- | ------- 
| Magnetic Field X | (∞, ∞) | Tesla |
| Magnetic Field Y | (∞, ∞) | Tesla |
| Magnetic Field Z | (∞, ∞) | Tesla |

#### Inertial Measurement Unit (IMU)
| Feature Name | Data Range | Units | Description |
|  --------  |  -------  | -------  | -------
| Orientation X | [-1, 1] | None |
| Orientation Y | [-1, 1] | None |
| Orientation Z | [-1, 1] | None |
| Orientation W | [-1, 1] | None |
| Angular Velocity X | (∞, ∞) | °/s |
| Angular Velocity Y | (∞, ∞) | °/s |
| Angular Velocity Z | (∞, ∞) | °/s |
| Linear Acceleration X | (∞, ∞) | m/s<sup>2</sup> |
| Linear Acceleration Y | (∞, ∞) | m/s<sup>2</sup> |
| Linear Acceleration Z | (∞, ∞) | m/s<sup>2</sup> |

#### Global Positioning System (GPS)
| Feature Name | Data Range | Units | Description |
|  --------  |  -------  | ------- | ------- |
| Latitude | [-90, 90] | Degree | 
| Longitude | [-180, 180] | Degree |
| Altitude | (∞, ∞) | m |
| Velocity | (∞, ∞) | m/s |
| Velocity East | (∞, ∞) | m/s |
| Velocity North | (∞, ∞) | m/s |
| Velocity Up | (∞, ∞) | m/s |
| Attack Enabled | [0, 1] | Bit |

## Dataset
The link for the dataset can be found [here](https://drive.google.com/file/d/1jypbdKFP-4vN7HMwbnm4z4i-b0OhoPtS/view?usp=sharing).

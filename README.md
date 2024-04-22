
# UAV-GPS-Spoofing-Dataset
## Paper: Detecting Stealthy GPS Spoofing Attack Against UAVs Using Onboard Sensors
Paper reference goes here

## Introduction
This repository provides a summary and description of the UAV GPS Spoofing dataset. The dataset was collected using PX4-SITL Gazebo-Classic in 2023 using a **quadcopter** drone.

<Motivation>

## Description
The dataset encompasses measurements from various onboard sensors, including the barometer, magnetometer, inertial measurement unit, and global positioning system. The **Attack Enabled** field classifies whether an attack was enabled at a point in time. The data was collected at a rate of **~250Hz**. The dataset can be resampled to other frequencies to emulate different sensor frequencies.

There are three versions of the data: a raw, merged, and preprocessed data set. 

The raw dataset includes data for each UAV sensor (Barometer, GPS, IMU, Magnetometer). It also includes the groundtruth data and raw GPS data. The groundtruth data shows the real position of the drone within the simulated environment, while the raw GPS shows the signal data before attacking parameters and latitude or longitude deltas are applied. This dataset is not time synchronized enabling you perform your own preprocessing on the dataset. The merged dataset includes time synchronized data without the groundtruth or raw GPS data. This dataset can be used to resample the data frequency while keeping the data between each sensor synchronized. Several features have been removed from this set as there were not considered to be important. The preprocessed data set includes the data that was used in the paper as well as all the derived/calculated features. This data set can be used to mimic our work, and perform different testing with different machine learning techniques on our data.

Each dataset is provided at the **bottom** of this page.

### GPS Attack
A GPS attack was constructed by modifying Gazebo's GPS sensor plugins and RPC messages. **The formulation of the GPS attack can be found within the paper.** The attack involves the idea of deviating the drone from its planned flight trajectory while preventing the drone from detecting the attack through an EKF or another method. Our attack achieves this by incrementally offsetting the drone’s reported GPS position by certain amount for each reported GPS step. **If the increment is small enough, the GPS attack will go undetected and can lead to a large difference in groundtruth position and reported GPS position.** The GPS attacks were conducted at a random interval between 60-120 seconds after the start of a flight.

### Flight Paths
There are three flight plans used to generate the data: a straight flight, curved plan, and random plan. For each flight plan, a baseline flight and two attacked flights were simulated. Each of these flight plans represent common flight scenarios in which a UAV will encounter.

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
The following section depicts all features contained in the data, and what features can be derived/extracted. The applicable formulas for derivation have been provided in the **description** column.
#### Barometer
**Raw**
| Feature Name | Data Range | Units | Description |
| -------- | ------- | ------- | ------- | 
| Absolute Pressure | [0, ∞) | Bar | |
| Altitude | (∞, ∞) | m | |
| Temperature | (∞, ∞) | °C | |

**Derived**
None

#### Magnetometer
**Raw**
| Feature Name | Data Range | Units | Description |
| -------- |  ------- | ------- | ------- |
| Magnetic Field X | (∞, ∞) | Tesla | |
| Magnetic Field Y | (∞, ∞) | Tesla | |
| Magnetic Field Z | (∞, ∞) | Tesla | |

**Derived**
| Feature Name | Data Range | Units | Description |
| -------- | ------- | ------- | ------- |
| Yaw | [$-\pi$, $\pi$] | Radian | $atan2(m_y,m_x)$ |

#### Inertial Measurement Unit (IMU)
**Raw**
| Feature Name | Data Range | Units | Description |
| -------- | ------- | -------  | ------- |
| Orientation X | [-1, 1] | None | |
| Orientation Y | [-1, 1] | None | |
| Orientation Z | [-1, 1] | None | |
| Orientation W | [-1, 1] | None | |
| Angular Velocity X | (∞, ∞) | $\frac{^\circ}{s}$ |
| Angular Velocity Y | (∞, ∞) | $\frac{^\circ}{s}$ |
| Angular Velocity Z | (∞, ∞) | $\frac{^\circ}{s}$ |
| Linear Acceleration X | (∞, ∞) | $\frac{m}{s^2}$ |
| Linear Acceleration Y | (∞, ∞) | $\frac{m}{s^2}$ |
| Linear Acceleration Z | (∞, ∞) | $\frac{m}{s^2}$ |

**Derived**
| Feature Name | Data Range | Units | Description |
| -------- | -------  | ------- | ------- |
| Yaw | [$-\pi$, $\pi$] | Radian | $atan2(2(g_yg_w-g_xg_y),-1+2(g_w^2+g_x^2))$
| Pitch | [$\frac{-\pi}{2}$, $\frac{\pi}{2}$] | Radian | $asin(\frac{a_x}{g})$
| Roll| [$-\pi$, $\pi$] | Radian | $atan2(a_y,a_z)$

#### Global Positioning System (GPS)
**Raw**
| Feature Name | Data Range | Units | Description |
| -------- | ------- | ------- | ------- |
| Latitude | [-90, 90] | Degree | 
| Longitude | [-180, 180] | Degree |
| Altitude | (∞, ∞) | m |
| Velocity | (∞, ∞) | $\frac{m}{s}$ |
| Velocity East | (∞, ∞) | $\frac{m}{s}$ |
| Velocity North | (∞, ∞) | $\frac{m}{s}$ |
| Velocity Up | (∞, ∞) | $\frac{m}{s}$ |
| Attack Enabled | [0, 1] | Bit |

**Derived**
| Feature Name | Data Range | Units | Description |
| -------- | ------- | ------- | ------- |
| Velocity | (∞, ∞) | $\frac{m}{s}$ | WGS84 Distance Formula

## Dataset Structure
The following section displays the layout of the data, as well as how it is organized. This section aims to aid you in navigating the dataset.

### Directory Structure

The following diagram depicts the structure of the **raw and merged** dataset. The first directory contains what **type of data** is contained in its subdirectory (e.g. raw data). The next directory is what **type of flight plan** was used for the data. Then, whether there was an **attack present** on the data, or was the drone under **normal** operations.

The raw dataset provides each the log file with each sensor. The rows of the `.csv` are **not** guaranteed to be time synchronized, and must be preprocessed.
```
📦PX4_Simulation_Data.zip
 ┣ 📂Merged
 ┃ ┣ 📂Curved
 ┃ ┃ ┣ 📂Attacked
 ┃ ┃ ┃ ┣ 📜log_D_M_Y_HH_MM_SS.csv
 ┃ ┃ ┃ ┗ 📜...
 ┃ ┃ ┗ 📂Normal
 ┃ ┣ 📂Random
 ┃ ┗ 📂Straight
 ┣ 📂Raw
 ┃ ┣ 📂Curved
 ┃ ┃ ┣ 📂Attacked
 ┃ ┃ ┃ ┣ 📂log_D_M_Y_HH_MM_SS
 ┃ ┃ ┃ ┃ ┣ 📜barometer.csv
 ┃ ┃ ┃ ┃ ┣ 📜gps.csv
 ┃ ┃ ┃ ┃ ┗ 📜...
 ┃ ┃ ┃ ┗ 📂...
 ┃ ┃ ┗ 📂Normal
 ┃ ┣ 📂Random
 ┗ ┗ 📂Straight
```

### CSV Structure
**Merged**
`log_D_M_Y_HH_MM_SS.csv`
| Time | Pressure Altitude | Latitude | Longitude| Altitude| Velocity | Attack | Orientation | Angular Velocity | Linear Acceleration | Magnetic Field |
| -------- | ------- | ------- | ------- | ------- | ------- | -------  | -------  | ------- | ------- | ------- | ------- | ------- | ------- | 

**Raw**
`barometer.csv`
| Time | Temperature | Absolute Pressure | Pressure Altitude |
| -------- | ------- | ------- | ------- |

`gps.csv`
| Time | Latitude | Longitude | Altitude | eph | epv | Velocity | Attack |
| -------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- |

`groundtruth.csv`
| Time | Latitude | Longitude | Altitude | Velocity | Attitude |
| -------- | ------- | ------- | ------- | ------- | ------- | 
### Links 

The link for the raw dataset can be found [here](https://drive.google.com/file/d/1jypbdKFP-4vN7HMwbnm4z4i-b0OhoPtS/view?usp=sharing), while the link for the preprocessed dataset can be found [here]().

## Question and answers (Q&A)

- How would I get started?

- 

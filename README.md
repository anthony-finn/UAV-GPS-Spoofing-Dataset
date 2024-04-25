
# UAV-GPS-Spoofing-Dataset
## Paper: Detecting Stealthy GPS Spoofing Attack Against UAVs Using Onboard Sensors
Paper reference goes here

## Introduction
This repository provides a summary and description of the UAV GPS Spoofing dataset. The dataset was collected using PX4-SITL Gazebo-Classic in 2023 using a **quadcopter** drone.

Drones have a wide range of applications in military, commercial, and civilian sectors. These include real-time monitoring, search-and-rescue operations, expanding wireless coverage, remote sensing for data collection, delivery services, and security and surveillance tasks.

While Global Navigation Satellite Systems (GNSS) are widely used for drone navigation, with GPS being the most common system, a critical vulnerability exists.  These GPS-dependent drones require precise and reliable positioning data for safe operation. However, one of GPS' flaws is its lack of encryption and authentication. This makes them susceptible to manipulation by attackers who can either spoof the GPS signal or jam it entirely.

<What makes this dataset unique>
<Stealthy GPS Attack Not detected by EKF>

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

[![Straight Flight Plan Image](/main/Images/Plan1_Cropped.png)](https://github.com)

#### Curved
Another common scenario is the drone moving in a curved path.The drone may weave around objects or buildings during it's flight.

[![Curved Flight Plan Image](/Images/Plan2_Cropped.png)](https://github.com)

#### Random
These flight plans are a combination of the straight and curved flight plans. For example, the UAV may move in a straight line path, but transition into a curved flight plan.

## Features
The following section depicts all features contained in the data, and what features can be derived/extracted. The applicable formulas for derivation have been provided in the **description** column.

Here is an overview of all the features and derived features.\
[![Feature Overview](/Images/Feature_Overview.png)](https://github.com)
<Give annotation about features>
#### Barometer
**Raw**
| Feature Name | Data Range | Units | Description |
| -------- | ------- | ------- | ------- | 
| Absolute Pressure | [0, ∞) | Bar | |
| Altitude | (∞, ∞) | m | |
| Temperature | (∞, ∞) | °C | |

**Derived**\
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
| Latitude Delta | (∞, ∞) | Degree | Latitude Difference
| Longitude Delta | (∞, ∞) | Degree | Longitude Difference

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
The following table represents the file naming scheme and structure of the csv file. Each table column can have multiple values per field.

#### Raw
`barometer.csv`
| Time | Temperature | Absolute Pressure | Pressure Altitude |
| -------- | ------- | ------- | ------- |

`gps.csv` & `raw_gps.csv`
| Time | Latitude | Longitude | Altitude | eph | epv | Velocity | Attack |
| -------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- |



`groundtruth.csv`
| Time | Latitude | Longitude | Altitude | Velocity | Attitude |
| -------- | ------- | ------- | ------- | ------- | ------- | 

`imu.csv`
| Orientation | Orientation Covariance | Angular Velocity | Angular Velocity Covariance | Linear Acceleration | Linear Acceleration Covariance | Time |
| ------- | ------- | ------- | ------- | ------- | ------- | ------- |

`magnetometer.csv`
| Time | Magnetic Field | Magnetic Field Covariance |
| ------- | ------- | ------- 

#### Merged

`log_DD_MM_YYYY_HH_MM_SS.csv`
| Time | Pressure Altitude | Latitude | Longitude | Altitude| Velocity | Attack | Orientation | Angular Velocity | Linear Acceleration | Magnetic Field |
| -------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- |

### Data Links 

The link for the raw dataset can be found [here](https://drive.google.com/file/d/1jypbdKFP-4vN7HMwbnm4z4i-b0OhoPtS/view?usp=sharing), while the link for the preprocessed dataset can be found [here]().

## Question and answers (Q&A)

**Q: How would I get started?**\
**A:** The easiest way to get started with this data set is to download the preprocessed data set. This data will provide you with a good basis to begin formulating your own research and experiments using machine learning/artificial intelligence. The data can be loaded into a python notebook like this through the [pandas](https://pandas.pydata.org/) library:

```console
pip install pandas
```
```python
import pandas as pd
df = pd.read_csv("<FILE_NAME>")
```

The replace the **\<FileName\>** with the path to the preprocessed *.csv* file. After importing the file into a python notebook, you can begin working. There are many simple tutorials which can guide you into using various machine learning libraries. One popular machine learning platform is [Tensorflow](https://www.tensorflow.org/) and [scikit-learn](https://scikit-learn.org/). **This code sample can be applied to the merged data set as well!**

**Q: How can I use the raw data?**\
**A:** The raw data is difficult in that it is not synchronized. Each row is not guaranteed to be the same point in time between each file. Luckily, we store the UTC time information, which enables us to synchronized each file together. If you would like to synchronize the data, you can run the following python script:

```python
import glob
csv_files = glob.glob(f'<DIRECTORY_PATH>/*.csv')
dfs = []
for file in csv_files:
    df = pd.read_csv(file)
    dfs.append(df)

merged_df = dfs[0]
for i in range(1, len(dfs)):
    merged_df = pd.merge(merged_df, dfs[i], on='time_usec', how='outer')
    merged_df = merged_df.drop_duplicates(subset='time_usec', keep='first')
merged_df = merged_df.dropna()
merged_df.to_csv("<OUTPUT_PATH>", index=False)
```

This script will take a collection of the raw *.csv* data files and combine them into one *.csv* file which is synchronized by time. This accomplishes this by first joining each file on the *time_usec* field, then dropping the duplicates and null values. 

## More Information
For more detailed information on the formulation of the GPS attack, see [this]() repository. This repository includes the instructions on how to setup the virtual machine with PX4 and Classic Gazebo under the correct versions.

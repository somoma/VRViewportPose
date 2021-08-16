# VR Viewport Pose Dataset

## Outline

*  [Data Collection Setup](#1)
* [Download the Dataset](#2)
* [Extract the Orientation and Position Models](#3)


## 1. <span id="1"> Data Collection Setup </span> 
### 1.1 Stimuli

We conducted an IRB-approved data collection of the viewport pose for desktop, headset, and phone-based VRs, with open-source VR games from Unity store, containing 1 indoor (Office [1]) and 2 outdoor (Viking village [2], Lite [3]) scenarios. Main characteristics of these VR games with different scene complexities. In desktop VR, rotational and translational movements are made using the mouse and up arrow key. The poses in headset VR are collected with a standalone Oculus Quest 2, where rotational and translational movements are made by moving the head and by using the controller thumbstick. In the phone-based VR, in-lab experiment uses Pixel 2 XL with Android 9, and rotational and translational movements are made by moving the motion-sensor-equipped phone and by tapping on the screen using one finger.

<p align="center">
     <img src="https://github.com/VRViewportPose/VRViewportPose/blob/main/Stimuli.png" width = "800" height = "250" hspace="0"/>
</p>
<p align="center">
Figure 1: Open-source VR games used for the data collection: (a) Office; (b) Viking village; (c) Lite.
</p>

### 1.2 Procedure

In desktop VR, the participants are seated in front of a PC, and use the up arrow key and mouse to perform translational and rotational movement, respectively. In headset VR, each participant wears a standalone Oculus Quest 2 [4] headset, and makes rotational movement by moving her head, and makes translational movement using the  controller thumbstick. In phone-based VR, the participant makes rotational movement by moving the motion-sensor-equipped phone, and makes translational movement by tapping on the screen using one finger. Considering the device computation capability and the screen fresh rate, the timestamp and viewport pose of each participant are recorded at a target frame rate of 60 Hz, 72 Hz, and 60 Hz for desktop, headset, and phone-based VR, respectively. For each frame, we record the timestamp, the *x, y, z* positions and the roll, pitch, and yaw Euler orientation angles. For the Euler orientation angles <a href="https://www.codecogs.com/eqnedit.php?latex=(\beta,&space;\gamma,&space;\alpha)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?(\beta,&space;\gamma,&space;\alpha)" title="(\beta, \gamma, \alpha)" /></a>, the intrinsic rotation orders are adopted, i.e., the viewport pose is rotated <a href="https://www.codecogs.com/eqnedit.php?latex=\alpha" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\alpha" title="\alpha" /></a> degrees around the *z*-axis, <a href="https://www.codecogs.com/eqnedit.php?latex=\beta" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\beta" title="\beta" /></a> degrees around the *x*-axis, and <a href="https://www.codecogs.com/eqnedit.php?latex=\gamma" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\gamma" title="\gamma" /></a> degrees around the *y*-axis. We randomize the initial viewport position in VR games over the whole bounding area. We fix the initial polar angle of the viewport to be 90 degree, and uniformly randomize the initial azimuth angle on [-180,180) degree.

## 2. <span id="2">Download the Dataset</span>

The dataset samples can be download [**here**](https://github.com/VRViewportPose/VRViewportPose/blob/main/VR_Pose_Sample.zip). 

### 2.1 The structure of the complete dataset

The complete dataset follows a hierarchical file structure shown below:
```
VR_Pose
└───data_Desktop
│   │
│   └───Office_Desktop_1.txt
│   └───VikingVillage_Desktop_1.txt
│   └───Lite_Desktop_1.txt
│   └───Office_Desktop_2.txt
│   └───VikingVillage_Desktop_2.txt
│   └───Lite_Desktop_2.txt
│   ...
│
└───data_Oculus
│   │
│   └───Office_Oculus_1.txt
│   └───VikingVillage_Oculus_1.txt
│   └───Lite_Oculus_1.txt
│   └───Office_Oculus_2.txt
│   └───VikingVillage_Oculus_2.txt
│   └───Lite_Oculus_2.txt
|   ...
|
└───data_Phone
...
```
There are **3** sub-folders corresponding to the different VR interfaces. In the subfolder of data_Desktop, there are **60** TXT files, corresponding to **20** participants, each of them experiencing **3** VR games. There are **15** TXT files in both the data_Oculus and data_Phone subfolders, corresponding to **5** participants experiencing **3** VR games. In total, there are over **5.5 hours** of user data.

### 2.2 The structure of current dataset samples
The current dataset samples follow a hierarchical file structure shown below:
```
VR_Pose
└───data_Desktop
│   │
│   └───Office_Desktop_1.txt
│   └───VikingVillage_Desktop_1.txt
│   └───Lite_Desktop_1.txt
│
└───data_Oculus
│   │
│   └───Office_Oculus_1.txt
│   └───VikingVillage_Oculus_1.txt
│   └───Lite_Oculus_1.txt
|
└───data_Phone
│   │
│   └───Office_Oculus_1.txt
│   └───VikingVillage_Oculus_1.txt
│   └───Lite_Oculus_1.txt
```

There are also **3** sub-folders corresponding to the different VR interfaces. In each subfolder, there are only **3** TXT files, corresponding to **1** participant experienceing **3** VR games. ***The full dataset will be published upon the publication of my manuscript.***

## 3. <span id="3">Extract the Orientation and Position Models</span>

The `OrientationModel.py` and `PositionModel.py` are used to extract the orientation and position models for VR viewport pose, respectively. Before running the scripts in this repository, you need to download the repository and install the necessary tools and libraries on your computer, including `scipy`, `numpy`, `pandas`, `fitter`, and `matplotlib`.

### 3.1. Orientation model
#### Data processing

We convert the recorded Euler angles to polar angle <a href="https://www.codecogs.com/eqnedit.php?latex=\theta" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\theta" title="\theta" /></a> and azimuth angle <a href="https://www.codecogs.com/eqnedit.php?latex=\phi" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\phi" title="\phi" /></a>. After applying rotation matrix **R**, we have

<a href="https://www.codecogs.com/eqnedit.php?latex=\label{eqn:converteuler}&space;\begin{aligned}&space;{\bf{n}}&space;=&{\bf&space;R}\left[&space;\begin{array}{l}&space;0\\&space;0\\&space;1&space;\end{array}&space;\right]&space;=&space;\left[&space;{\begin{array}{*{20}{c}}&space;{\cos&space;\alpha&space;\cos&space;\gamma&space;-&space;\sin&space;\alpha&space;\sin&space;\beta&space;\sin&space;\gamma&space;}&{&space;-&space;\sin&space;\alpha&space;\cos&space;\beta&space;}&{\cos&space;\alpha&space;\sin&space;\gamma&space;&plus;&space;\sin&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma&space;}\\&space;{\sin&space;\alpha&space;\cos&space;\gamma&space;&plus;&space;\cos&space;\alpha&space;\sin&space;\beta&space;\sin&space;\gamma&space;}&{\cos&space;\alpha&space;\cos&space;\beta&space;}&{\sin&space;\alpha&space;\sin&space;\gamma&space;-&space;\cos&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma&space;}\\&space;{&space;-&space;\cos&space;\beta&space;\sin&space;\gamma&space;}&{\sin&space;\beta&space;}&{\cos&space;\beta&space;\cos&space;\gamma&space;}&space;\end{array}}&space;\right]\left[&space;\begin{array}{l}&space;0\\&space;0\\&space;1&space;\end{array}&space;\right]\\&space;=&&space;\left[&space;{\begin{array}{*{20}{c}}&space;{\cos&space;\alpha&space;\sin&space;\gamma&space;&plus;&space;\sin&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma&space;}\\&space;{\sin&space;\alpha&space;\sin&space;\gamma&space;-&space;\cos&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma&space;}\\&space;{\cos&space;\beta&space;\cos&space;\gamma&space;}&space;\end{array}}&space;\right].&space;\end{aligned}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\label{eqn:converteuler}&space;\begin{aligned}&space;{\bf{n}}&space;=&{\bf&space;R}\left[&space;\begin{array}{l}&space;0\\&space;0\\&space;1&space;\end{array}&space;\right]&space;=&space;\left[&space;{\begin{array}{*{20}{c}}&space;{\cos&space;\alpha&space;\cos&space;\gamma&space;-&space;\sin&space;\alpha&space;\sin&space;\beta&space;\sin&space;\gamma&space;}&{&space;-&space;\sin&space;\alpha&space;\cos&space;\beta&space;}&{\cos&space;\alpha&space;\sin&space;\gamma&space;&plus;&space;\sin&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma&space;}\\&space;{\sin&space;\alpha&space;\cos&space;\gamma&space;&plus;&space;\cos&space;\alpha&space;\sin&space;\beta&space;\sin&space;\gamma&space;}&{\cos&space;\alpha&space;\cos&space;\beta&space;}&{\sin&space;\alpha&space;\sin&space;\gamma&space;-&space;\cos&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma&space;}\\&space;{&space;-&space;\cos&space;\beta&space;\sin&space;\gamma&space;}&{\sin&space;\beta&space;}&{\cos&space;\beta&space;\cos&space;\gamma&space;}&space;\end{array}}&space;\right]\left[&space;\begin{array}{l}&space;0\\&space;0\\&space;1&space;\end{array}&space;\right]\\&space;=&&space;\left[&space;{\begin{array}{*{20}{c}}&space;{\cos&space;\alpha&space;\sin&space;\gamma&space;&plus;&space;\sin&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma&space;}\\&space;{\sin&space;\alpha&space;\sin&space;\gamma&space;-&space;\cos&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma&space;}\\&space;{\cos&space;\beta&space;\cos&space;\gamma&space;}&space;\end{array}}&space;\right].&space;\end{aligned}" title="\label{eqn:converteuler} \begin{aligned} {\bf{n}} =&{\bf R}\left[ \begin{array}{l} 0\\ 0\\ 1 \end{array} \right] = \left[ {\begin{array}{*{20}{c}} {\cos \alpha \cos \gamma - \sin \alpha \sin \beta \sin \gamma }&{ - \sin \alpha \cos \beta }&{\cos \alpha \sin \gamma + \sin \alpha \sin \beta \cos \gamma }\\ {\sin \alpha \cos \gamma + \cos \alpha \sin \beta \sin \gamma }&{\cos \alpha \cos \beta }&{\sin \alpha \sin \gamma - \cos \alpha \sin \beta \cos \gamma }\\ { - \cos \beta \sin \gamma }&{\sin \beta }&{\cos \beta \cos \gamma } \end{array}} \right]\left[ \begin{array}{l} 0\\ 0\\ 1 \end{array} \right]\\ =& \left[ {\begin{array}{*{20}{c}} {\cos \alpha \sin \gamma + \sin \alpha \sin \beta \cos \gamma }\\ {\sin \alpha \sin \gamma - \cos \alpha \sin \beta \cos \gamma }\\ {\cos \beta \cos \gamma } \end{array}} \right]. \end{aligned}" /></a>

From the above equation, <a href="https://www.codecogs.com/eqnedit.php?latex=\theta" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\theta" title="\theta" /></a> is calculated as  <a href="https://www.codecogs.com/eqnedit.php?latex=\theta=\sin&space;\alpha&space;\sin&space;\gamma&space;-&space;\cos&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\theta=\sin&space;\alpha&space;\sin&space;\gamma&space;-&space;\cos&space;\alpha&space;\sin&space;\beta&space;\cos&space;\gamma" title="\theta=\sin \alpha \sin \gamma - \cos \alpha \sin \beta \cos \gamma" /></a>, and <a href="https://www.codecogs.com/eqnedit.php?latex=\phi" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\phi" title="\phi" /></a> is given by 

<a href="https://www.codecogs.com/eqnedit.php?latex=\phi&space;=&space;\left\{&space;{\begin{array}{*{20}{l}}&space;{{\phi&space;^{'}},\cos&space;\beta&space;\cos&space;\gamma&space;\ge&space;0}\\&space;{\pi&space;&plus;&space;{\phi&space;^{'}},\cos&space;\beta&space;\cos&space;\gamma&space;<&space;0\;{\rm{and}}\;\cos&space;\gamma&space;\sin&space;\alpha&space;\sin&space;\beta&space;&plus;&space;\cos&space;\alpha&space;\sin&space;\gamma&space;>&space;0}\\&space;{&space;-&space;\pi&space;&plus;&space;{\phi&space;^{'}},\cos&space;\beta&space;\cos&space;\gamma&space;<&space;0\;{\rm{and}}\;\cos&space;\gamma&space;\sin&space;\alpha&space;\sin&space;\beta&space;&plus;&space;\cos&space;\alpha&space;\sin&space;\gamma&space;<&space;0}&space;\end{array}}&space;\right." target="_blank"><img src="https://latex.codecogs.com/gif.latex?\phi&space;=&space;\left\{&space;{\begin{array}{*{20}{l}}&space;{{\phi&space;^{'}},\cos&space;\beta&space;\cos&space;\gamma&space;\ge&space;0}\\&space;{\pi&space;&plus;&space;{\phi&space;^{'}},\cos&space;\beta&space;\cos&space;\gamma&space;<&space;0\;{\rm{and}}\;\cos&space;\gamma&space;\sin&space;\alpha&space;\sin&space;\beta&space;&plus;&space;\cos&space;\alpha&space;\sin&space;\gamma&space;>&space;0}\\&space;{&space;-&space;\pi&space;&plus;&space;{\phi&space;^{'}},\cos&space;\beta&space;\cos&space;\gamma&space;<&space;0\;{\rm{and}}\;\cos&space;\gamma&space;\sin&space;\alpha&space;\sin&space;\beta&space;&plus;&space;\cos&space;\alpha&space;\sin&space;\gamma&space;<&space;0}&space;\end{array}}&space;\right." title="\phi = \left\{ {\begin{array}{*{20}{l}} {{\phi ^{'}},\cos \beta \cos \gamma \ge 0}\\ {\pi + {\phi ^{'}},\cos \beta \cos \gamma < 0\;{\rm{and}}\;\cos \gamma \sin \alpha \sin \beta + \cos \alpha \sin \gamma > 0}\\ { - \pi + {\phi ^{'}},\cos \beta \cos \gamma < 0\;{\rm{and}}\;\cos \gamma \sin \alpha \sin \beta + \cos \alpha \sin \gamma < 0} \end{array}} \right." /></a>

where <a href="https://www.codecogs.com/eqnedit.php?latex={\phi&space;^{'}}&space;=&space;\arctan&space;\left(&space;{\frac{{\cos&space;\gamma&space;\sin&space;\alpha&space;\sin&space;\beta&space;&plus;&space;\cos&space;\alpha&space;\sin&space;\gamma&space;}}{{\cos&space;\beta&space;\cos&space;\gamma&space;}}}&space;\right)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\phi&space;^{'}}&space;=&space;\arctan&space;\left(&space;{\frac{{\cos&space;\gamma&space;\sin&space;\alpha&space;\sin&space;\beta&space;&plus;&space;\cos&space;\alpha&space;\sin&space;\gamma&space;}}{{\cos&space;\beta&space;\cos&space;\gamma&space;}}}&space;\right)" title="{\phi ^{'}} = \arctan \left( {\frac{{\cos \gamma \sin \alpha \sin \beta + \cos \alpha \sin \gamma }}{{\cos \beta \cos \gamma }}} \right)" /></a>.

After we obtain the polar and azimuth angles, we fit the polar angle, polar angle change, and azimuth angle change to a set of statistical models and mixed models (of two statistical models).

#### Orientation model script
The orientaion model script is provided via https://github.com/VRViewportPose/VRViewportPose/blob/main/OrientationModel.py. To obtain the orientation model, follow the procedure below:

1. Download and extract the VR viewport pose dataset.
2. Change the `filePath` variable in `OrientationModel.py` to the file location of the pose dataset.
3. You can directly run `OrientationModel.py` (`python .\OrientationModel.py`). It will automatically run the pipeline.
5. The generated EPS images named "*polar_fit_our_dataset.eps*", "*polar_change.eps*", "*azimuth_change.eps*", and "ACF_our_dataset.eps" will be saved in a folder. "*polar_fit_our_dataset.eps*", "*polar_change.eps*", and "*azimuth_change.eps*" show the distribution of the experimental data for polar angle, polar angle change, and azimuth angle change fitted by different statistical distributions, respectively. "*ACF_our_dataset.eps*" shows the autocorrelation function (ACF) of polar and azimuth angle samples that are <a href="https://www.codecogs.com/eqnedit.php?latex=\Delta&space;t" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\Delta&space;t" title="\Delta t" /></a> s apart.

### 3.2. Position model
#### Data processing

We apply the standard angle model proposed in [5] to extract flights from the trajectories. An example of the collected trajectory for one user in Lite and the extracted flights is shown below. It demonstrates that the real trajectory can be well approximated by a sequence of flights.

<img src="https://github.com/VRViewportPose/VRViewportPose/blob/main/FlightSample.png" width = "320" height = "220" hspace="200" align=center />

#### Position model script
The position model script is provided via https://github.com/VRViewportPose/VRViewportPose/blob/main/PositionModel.py. To obtain the position model, follow the procedure below:

1. Download and extract the VR viewport pose dataset.
2. Change the `filePath` variable in `PositionModel.py` to the file location of the pose dataset.
3. You can directly run `PositionModel.py` (`python .\PositionModel.py`). It will automatically run the pipeline.
5. The generated EPS images named "*flight_sample.eps*", "*flight.eps*", "*pausetime_distribution.eps*", and "*correlation.eps*" will be saved in a folder. "*flight_sample.eps*" shows an example of the collected trajectories and the corresponding flights. "*flight.eps*" and "*pausetime_distribution.eps*" show distributions of the flight time and the pause duration for collected samples, respectively. "*correlation.eps*" shows the correlation of the azimuth angle and the walking direction.

## References
[1] Unity Asset Store. (2020) Office. https://assetstore.unity.com/packages/3d/environments/snapsprototype-office-137490

[2] Unity Technologies. (2015) Viking village. https://assetstore.unity.com/packages/essentials/tutorialprojects/viking-village-29140

[3] Xiaolianhua Studio. (2017) Lite. https://assetstore.unity.com/packages/3d/environments/fantasy/makeyour-fantasy-game-lite-8312

[4]  Oculus.(2021) Oculus Quest 2. https://www.oculus.com/quest-2/

[5] I. Rhee, M. Shin, S. Hong, K. Lee, and S. Chong, “On the levy-walk nature of human mobility,” in *IEEE INFOCOM*, 2008.

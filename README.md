# VINS-Stereo：基于VINS-Mono的双目VIO系统开发

## Update on 19/11/2022
1. rewrite ```feature_manager.cpp``` and change ```trianglute()``` which use left and right to triangulate.
2. rewrite ```initial_sfm.cpp```:
   (1)add ```triangulateLeftAndRight()``` which use left and right to triangulate. 
   (2)use ```triangulateLeftAndRight()``` in ```construct()```.
   (3)rewrite window BA in ```construct()```.
   
In VIO with two cameras, the input information of vision doubled. So it is easier to recover 3D coordinates of features.
But it is more computational consuming in frontend tracking. 

## Earlier Info
这个系统是基于香港科技大学飞行机器人组的开源框架VINS-Mono开发的，原开源框架是针对单目SLAM。本双目SLAM系统是在原开源框架基础上的二次深度开发，外部接口与原框架一致。您可以依此对比原开源框架[VINS-Mono](https://github.com/HKUST-Aerial-Robotics/VINS-Mono)或[VINS-Fusion](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion)与本系统的区别。这个项目是我的研究课题项目，非商业用途，感谢HKUST的沈老师课题组提供的开源框架。


## 1.需要的基本配置
```
Ubuntu 16.04
ROS Kinetic
Ceres Solver
OpenCV 3.3.1
Eigen 3.3.3
```

## 2.在您的ROS上构造VINS-Dual
```
cd ~/catkin_ws/src
git clone https://github.com/Cai-RS/My_VINS-Stereo.git
cd ../
catkin_make
```

## 3. VINS-Dual的启动
请在[EuRoC MAV Dataset](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets)下载您需要测试的数据集。

打开1个控制台，输入：
```
roscore
```

进入devel目录，分别打开2个控制台，输入：
```
source setup.bash
```

再分别输入：
```
roslaunch vins_estimator euroc.launch 
roslaunch vins_estimator vins_rviz.launch
```

如果不使用回环检测，请输入：
```
roslaunch vins_estimator euroc_no_posegraph.launch 
```

再打开1个控制台，请输入：
```
rosbag play YOUR_PATH_TO_DATASET/MH_01_easy.bag 
```

## 4.参考文献
T.Qin, P.L.Li, S.J.Shen, VINS-Mono: A Robust and Versatile Monocular Visual-Inertial State Estimator.



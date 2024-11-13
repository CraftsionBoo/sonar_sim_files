## 简介

Previous sonar sensor plugins were based on image processing realms by translating each subpixel (point cloud) of the perceived image to resemble sonar sensors with or without sonar equations (Detailed Review for the previous image-based methods). Here, we have developed a `ray-based multibeam` sonar plugin to consider the `phase and reberveration physics of the acoustic signals providing raw sonar intensity-range data` using the point scattering model. Physical characteristics, including time and angle ambiguities and speckle noise, are considered. The time and angle ambiguity is a function of the point spread function of the coherent imaging system (i.e., side lobes due to matched filtering and beamforming). Speckle is the granular appearance of an image due to many interfering scatterers that are smaller than the resolution limit of the imaging system.

环境
- jetson agx xaiver 
- Ubuntu20.04 
- ROS noetic
- OpencV4.4.0

## 安装步骤

1.拉取开源项目
```bash
mkdir -p ~/uuv_ws/src
cd ~/uuv_ws/src
git clone https://github.com/Field-Robotics-Lab/dave.git
```

2.修改 dave/extras/repos/dave_sim.repos。删除dockwater，这一项并不需要。
```bash
### vcs工具拉取其他项目
sudo pip3 install -U vcstool
cd ~/uuv_ws/src
vcs import --skip-existing --input dave/extras/repos/dave_sim.repos
```

Could not determine ref type of version: git@github.com: Permission denied (publickey)的问题，需要绑定github账号根据以下链接进行修改
[<font color=blue>https://docs.github.com/en/authentication/troubleshooting-ssh/error-permission-denied-publickey</font>](https://docs.github.com/en/authentication/troubleshooting-ssh/error-permission-denied-publickey)

`PS:如果账号无法绑定，可以根据对应链接github clone即可`
```bash
cd ~/uuv_ws/src
# git clone -b main https://github.com/Field-Robotics-Lab/dockwater.git
git clone -b nps_dev https://github.com/Field-Robotics-Lab/ds_msgs.git
git clone -b nps_dev https://github.com/Field-Robotics-Lab/ds_sim.git
git clone -b master https://github.com/uuvsimulator/eca_a9.git
git clone -b master https://github.com/uuvsimulator/rexrov2.git
git clone -b master https://github.com/field-robotics-lab/uuv_manipulators.git
git clone -b master https://github.com/field-robotics-lab/uuv_simulator.git
git clone -b main https://github.com/field-robotics-lab/nps_uw_multibeam_sonar.git
git clone -b tags/v2.0.0 https://github.com/apl-ocean-engineering/marine_msgs.git
```

3.编译dave
```bash
# Install build tool catkin_tools
pip3 install -U catkin_tools
cd ~/uuv_ws
catkin build
```
`PS:如果报错可以一个个编译`
```bash
catkin build ${project_name}
```

4.运行
```bash
source devel/setup.bash
roslaunch nps_uw_multibeam_sonar local_search_blueview_p900_nps_multibeam_urdf_standalone_ray.launch
```
![](standalone_ray.png)

```bash
source devel/setup.bash
roslaunch nps_uw_multibeam_sonar local_search_blueview_p900_nps_multibeam_ray.launch
```
![](ray.png)

## FAQ
<font color=red>Q1:出现缺少libgazebo_ros_velodyne_gpu_laser.so</font>
```bash
## 这个库文件不在程序里面，我是安装了一个docker版本，将其移植进/opt/ros/noetic/lib/
sudo find / -name "libgazebo_ros_velodyne_gpu_laser.so"
```

也可以在以下repo中进行下载：https://github.com/CraftsionBoo/sonar_sim_files

<font color=red>Q2:Opencv版本问题导致image_view显示不了声纳图像</font>
```bash
## 安装opencv4.X版本(略)
install opencv4.4.0 [以4.4.0为例]

## 拉取ros官方视觉库，重新编译cv_bridge image_geometry
git clone https://github.com/ros-perception/vision_opencv.git
cd src/cv_bridge

## 修改CMakeLists.txt中的opencv版本 find_package(OpenCV 4.4.0 REQUIRED)
mkdir build && cd build 
cmake —DCMKAE_INSTALL_PREFIX=/opt/ros/noetic ..
make -j8
sudo make install 

## image_geometry一样步骤
```

## 致谢

[<font color =red>https://github.com/Field-Robotics-Lab/DAVE</font>](https://github.com/Field-Robotics-Lab/DAVE)

[<font color =red>https://field-robotics-lab.github.io/dave.doc/</font>](https://field-robotics-lab.github.io/dave.doc/)

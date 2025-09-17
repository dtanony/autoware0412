## Extended Autoware v0.41.2
This is a forked version of Autoware, extened from [Autoware v0.41.2](https://github.com/autowarefoundation/autoware/releases/tag/0.41.2) with the following changes:

- Add a new ROS 2 service interface that enables ROS clients to issue service requests to trigger AEB activation.
The modified code is in [src/universe/autoware.universe/control/autoware_autonomous_emergency_braking](src/universe/autoware.universe/control/autoware_autonomous_emergency_braking) package, in particular, [src/universe/autoware.universe/control/autoware_autonomous_emergency_braking/src/node.cpp](src/universe/autoware.universe/control/autoware_autonomous_emergency_braking/src/node.cpp) file.
Readers who curious about the changes made can compare with the original version from [Autoware Universe repo](https://github.com/autowarefoundation/autoware_universe/tree/main/control/autoware_autonomous_emergency_braking).

- Add [src/aw_monitor package](src/aw_monitor/) to support the implementation of AW-Runtime-Monitor. New msg and srv files are defined in the package to define our custom message formats.

- Modify default parameter configuration for AD in [src/launcher/autoware_launch/autoware_launch/config/planning/scenario_planning/common/common.param.yaml](src/launcher/autoware_launch/autoware_launch/config/planning/scenario_planning/common/common.param.yaml).
The default configuration ([here](https://github.com/autowarefoundation/autoware_launch/blob/main/autoware_launch/config/planning/scenario_planning/common/common.param.yaml)) for deceleration and jerk are 2.5 m/s2 and 1.5 m/s3, which are too small.
So increase these settings to 8.33 m/s2 (0.85 G) and 83.3 m/s3, following the JAMA standard.


### Installation
The hardware requirements is available here:
https://autowarefoundation.github.io/AWSIM-Labs/main/GettingStarted/QuickStartDemo/#pc-specs

Follow the procedure below, which is copied from https://autowarefoundation.github.io/AWSIM-Labs/main/GettingStarted/QuickStartDemo and https://autowarefoundation.github.io/autoware-documentation/main/installation/autoware/source-installation/,  to install and launch Autoware.

1. Download `map files (pcd, osm)` from [here](https://github.com/tier4/AWSIM/releases/download/v1.1.0/nishishinjuku_autoware_map.zip) and unzip the file.

2. Clone this repo.

```bash
cd ~
git clone https://github.com/dtanony/autoware0412.git autoware
cd autoware
```

3. Configure the environment. (Skip if Autoware environment has been configured before)
```
./setup-dev-env.sh
```

4. Install dependent ROS packages.
```
source /opt/ros/humble/setup.bash
rosdep update
rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
```

5. Build the workspace.
```
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```

6. Launch Autoware.
```
source install/setup.bash
ros2 launch autoware_launch e2e_simulator.launch.xml vehicle_model:=awsim_labs_vehicle sensor_model:=awsim_labs_sensor_kit map_path:=<absolute path of map folder> launch_vehicle_interface:=true

# Use the absolute path for the map folder, don't use the ~ operator.

# Example:
ros2 launch autoware_launch e2e_simulator.launch.xml vehicle_model:=awsim_labs_vehicle sensor_model:=awsim_labs_sensor_kit map_path:=/home/your_username/autoware_map/nishishinjuku_autoware_map launch_vehicle_interface:=true
```

If there is any build issue, refer to [Troubleshooting](https://autowarefoundation.github.io/autoware-documentation/main/support/troubleshooting/#build-issues).
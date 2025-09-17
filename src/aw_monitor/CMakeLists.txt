cmake_minimum_required(VERSION 3.8)
project(aw_monitor)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(builtin_interfaces REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(ament_lint_auto REQUIRED)

# Generate message
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/BoundingBox.msg"
  "msg/CustomPose.msg"
  "msg/PoseTwistAccel.msg"
  "msg/GroundtruthNPCVehicle.msg"
  "msg/GroundtruthNPCPedestrian.msg"
  "msg/GroundtruthKinematic.msg"
  "msg/VehicleSize.msg"
  "msg/GroundtruthSize.msg"
  "msg/ResponseStatus.msg"
  "srv/GroundtruthKinematic.srv"
  "srv/ExecutionState.srv"
  "srv/DynamicControl.srv"  
  "srv/MonitorRecordingState.srv"
  DEPENDENCIES std_msgs geometry_msgs builtin_interfaces
)

ament_package()

# optional testing block
if(BUILD_TESTING)
  set(ament_cmake_copyright_FOUND TRUE)
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

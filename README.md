# Simple_robotic_vehicle: ROS 2 Differential-Drive Robot with Camera

A differential-drive robot (box chassis, two drive wheels, caster) with an RGB camera, described in URDF/Xacro and simulated in **Gazebo** and **RViz**.

<!-- Add a screenshot or GIF of the robot in Gazebo / RViz here -->

## Features

- Xacro-based robot description with reusable inertia macros
- Differential drive and joint state publishing via Gazebo plugins
- Front-mounted camera (640×480, 80° FOV, 20 Hz) with Gaussian noise
- Custom Gazebo world with a fridge, bookshelf, dining table and wall
- One launch file starts Gazebo, RViz, the robot and the ROS–Gazebo bridge

## Requirements

- Ubuntu 24.04 (native or WSL2)
- ROS 2 Jazzy and Gazebo (`gz sim`)

```bash
sudo apt install ros-jazzy-ros-gz ros-jazzy-xacro \
  ros-jazzy-robot-state-publisher ros-jazzy-rviz2 \
  ros-jazzy-teleop-twist-keyboard
```

## Packages

| Package | Contents |
|---------|----------|
| `my_robot_description` | Xacro files (`robot.urdf.xacro`, base, camera, Gazebo plugins, common properties) and the RViz config |
| `my_robot_bringup` | Launch file, Gazebo world (`worlds/test_world.sdf`) and bridge config (`config/gazebo_bridge.yaml`) |

## Robot

| Part | Details |
|------|---------|
| Chassis | Box, 0.6 × 0.4 × 0.2 m, 5 kg |
| Drive wheels | Radius 0.1 m, 1 kg each, wheel separation 0.45 m |
| Caster | Sphere, radius 0.05 m, low friction |
| Camera | Front of chassis, topics `camera/image_raw` and `camera/camera_info` |

Frames: `base_footprint` → `base_link` → wheels, caster, `camera_link` → `camera_link_optical`

## Build

```bash
mkdir -p ~/ros2_ws/src && cd ~/ros2_ws/src
git clone https://github.com/<your-username>/<your-repo>.git
cd ~/ros2_ws
colcon build
source install/setup.bash
```

## Run

```bash
ros2 launch my_robot_bringup <launch_file>.launch.xml
```

This starts:
1. `robot_state_publisher` with the robot description
2. Gazebo with `test_world.sdf`
3. The robot spawned in Gazebo
4. `ros_gz_bridge`, configured by `gazebo_bridge.yaml`
5. RViz with the saved config

Drive the robot:

```bash
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

## Troubleshooting

- **RViz sliders lag on WSL:** `sudo systemctl disable --now systemd-timesyncd`
- **Gazebo slow on WSL:** check that GPU drivers and WSLg are working.

## License

MIT. See `LICENSE`.

## Author

**Niranjan Bapurao Kadam**

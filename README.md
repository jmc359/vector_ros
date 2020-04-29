# vector_ros
This repository contains an *unofficial* ROS package for [Anki Vector](https://www.anki.com/en-us/vector). This package is essentially a wrapping of core Vector functions from [Vector Python SDK](https://github.com/anki/vector-python-sdk) as ROS topics, services and actions(full list below). THe original repository is that we forked from is [here](https://github.com/betab0t/vector_ros/) 

# Setup

## Requirements
- ROS Melodic with Python 3.7.7 or higher installed
- [Vector Python SDK](https://github.com/anki/vector-python-sdk)
- [diff_drive](https://github.com/merose/diff_drive) package

## Virtual Environment
It's highly recommended to use a virtual environment in order to run Python 3 properly on ROS. Follow the instructions:
1. Install virtualenv 
```sh
# Get virtualenv package
sudo apt update
sudo apt install python3-dev python3-pip
sudo pip3 install -U virtualenv

# Verify installation
python3 --version
pip3 --version
virtualenv --version

# Make python3 virtual environment
virtualenv --system-site-packages -p python3 ./venv
source ./venv/bin/activate  # sh, bash, ksh, or zsh
pip install --upgrade pip
pip list  # show packages installed within the virtual environment
deactivate  # don't exit until you're done running code
```

2. Install Vector SDK
Follow the instructions listed [here](https://developer.anki.com/vector/docs/install-linux.html). *Make sure to use your [Anki Developer](https://developer.anki.com/) username and password*

3. Clone this repository
```sh
git clone https://github.com/betab0t/vector_ros
```

# Topics
* `/vector/camera`  *(sensor_msgs/Image)*

Vector camera feed.

* `/lwheel_ticks` *(std_msgs/Int32)*

Cumulative encoder ticks of the left wheel. used by [diff_drive](https://github.com/merose/diff_drive) package.

* `/rwheel_ticks`  *(std_msgs/Int32)*

Cumulative encoder ticks of the right wheel. used by [diff_drive](https://github.com/merose/diff_drive) package.

* `/lwheel_rate`  *(std_msgs/Int32)*

Left wheel rotation rate. used by [diff_drive](https://github.com/merose/diff_drive) package.

* `/rwheel_rate`  *(std_msgs/Int32)*

Right wheel rotation rate. used by [diff_drive](https://github.com/merose/diff_drive) package.

# Services

* `/vector/battery_state`

* `/vector/set_head_angle`

* `/vector/set_lift_height`

* `/vector/anim_list`

* `/vector/say_text`

# Actions

* `/vector/play_animation`

Play animation by name.

# Examples
## View single image from camera
```sh
beta_b0t@home:~$ rosrun image_view image_saver image:=/vector/camera
[ INFO] [1550425113.646567813]: Saved image left0000.jpg
[ INFO] [1550425113.752592532]: Saved image left0001.jpg
[ INFO] [1550425113.848999553]: Saved image left0002.jpg
...
(Ctrl+C)
...
beta_b0t@home:~$ eog left0000.jpg
```

## Set head angle
```sh
beta_b0t@home:~$ rosservice call /vector/set_head_angle "deg: 45.0"
```

## Say text
```sh
beta_b0t@home:~$ rosservice call /vector/say_text "text: 'hello world'"
```

## Play animation 
```sh
beta_b0t@home:~$ rostopic pub /vector/play_animation/goal vector_ros/PlayAnimationActionGoal "header:
  seq: 0
  stamp:
    secs: 0
    nsecs: 0
  frame_id: ''
goal_id:
  stamp:
    secs: 0
    nsecs: 0
  id: ''
goal:
  anim: 'anim_turn_left_01'"
```

# FAQ
- **[How do i find Vector's IP address?](https://developer.anki.com/vector/docs/troubleshooting.html#can-t-find-vector-s-ip-address)**

- **[How do i find Vector's name?](https://developer.anki.com/vector/docs/troubleshooting.html#can-t-find-robot-name)**

- **[How do i find Vector's serial number?](https://developer.anki.com/vector/docs/troubleshooting.html#can-t-find-serial-number)**

- **Why isn't this XX from Vector SDK supported?** Well, I didn't wrap all the functions from the SDK - only the main ones as i see it. Yet, if you found a missing function that you need/would like to see as part of vector_ros, please consider opening a [new issue](https://github.com/betab0t/vector_ros/issues/new) with your proposal.

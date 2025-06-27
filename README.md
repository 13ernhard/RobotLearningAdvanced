# This is the repository of group 5 for arl25

Watch it in action:
![Speed 2x & Duplicate Frames removed](FullAnimationSpeedupLQ.gif)

# Docker Instructions

```bash ./build.sh ``` builds the Docker image.
```bash ./start.sh``` starts the Docker container.
```bash ./connect.sh``` connects to the running container.

## Hints:

First spin of Docker container serves Ollama to pull models ```bash nomic-embed-text``` & ```bash gemma3:4b``` via ```bash entrypoint.sh```.
Remove ```bash -rm``` flag in ```bash start.sh``` for container to persist upon exit.

# Tower of Hanoi Robot Simulation – Execution Instructions

This guide explains how to run the complete Tower of Hanoi simulation with the integrated robot learning pipeline. The simulation requires five terminal windows and the use of a conda-based virtual environment.

## python scripts

The setup file: for the tower of hanoi start scene should be placed in the following order

```bash
toh_setup.launch
```

for the tower of hanoi start scene should be placed in the following directory:

```bash
catkin_ws/src/om_position_controller/launch/
```

The python script:

```bash
toh_solver_group_5_prototype.py
```

that solves the tower of hanoi problem in the simulation, should be placed in the following directory:

```bash
catkin_ws/src/my_scripts/assignment_3/Docker_volume/
```

## Environment Setup (All Terminals)

Before running any processes, ensure the required virtual environment is activated in **each terminal window** by executing the following commands:

```bash
export PATH="/opt/conda/bin:$PATH"
echo 'export PATH="/opt/conda/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
source /opt/conda/etc/profile.d/conda.sh
conda activate llm_env
source devel/setup.bash
```

## Starting the Simulation

In four terminal windows out of the five you should execute the following commands separately

### ROS Core

```bash
roscore
```

### Launch gazebo Simulation

```bash
roslaunch om_position_controller toh_setup.launch sim:=true
```

### Start Cube Detection

```bash
python src/om_position_controller/scripts/4_rs_detect_sim.py
```

### Start Ollama

```bash
ollama serve
```

### Execute Robot Motions

After pressing the Play button in the simulation interface (e.g., Gazebo), run the following commands in the fifth terminal window in order of:

```bash
python src/my_scripts/assignment_3/Docker_volume/move_to_home.py

python src/my_scripts/assignment_3/Docker_volume/toh_solver_group_5_prototype.py
```

```

```

## Troubleshooting:

If gazebo_ros_link_attacher plugin is not responsive, manually attach links like so:
```bash
rosservice call /link_attacher_node/attach "model_name_1: 'unit_box_1'
link_name_1: 'link'
model_name_2: 'unit_sphere_1'
link_name_2: 'link'"
```
and detach like so:
```bash
rosservice call /link_attacher_node/detach "model_name_1: 'unit_box_1'
link_name_1: 'link'
model_name_2: 'unit_sphere_1'
link_name_2: 'link'"
```

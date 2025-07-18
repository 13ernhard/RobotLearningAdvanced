# This is the repository of group 5 for arl25

Watch it in action:
![Speed 2x & Duplicate Frames removed](FullAnimationSpeedupLQ.gif)

# Docker Instructions

Build the Docker image
```bash
./build.sh
```
Start the Docker container
```bash
./start.sh
```
Connect to running container
```bash
./connect.sh
```

## Hints:

First container spin serves Ollama to pull models `nomic-embed-text` & `gemma3:4b` via `entrypoint.sh`. \
Remove `-rm` flag in `start.sh` for container to persist upon exit. \

The file `entrypoint.sh` already sources the environent. \
Proceed with `Starting the Simulation` from `Tower of Hanoi Robot Simulation - Execution Instructions`.


# Tower of Hanoi Robot Simulation – Execution Instructions

This guide explains how to run the complete Tower of Hanoi simulation with the integrated robot learning pipeline. \
The simulation requires five terminal windows and the use of a conda-based virtual environment.

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

### Docker image not building
The created Docker image will be about 30GB in size. \
If your build stays at `[35/39] RUN conda env create -f /tmp/llm_env.ymal && conda clean -afy` for >1 hour, abort & rebuild.

If you plan to modify the Docker image, watch out for version compatibility of `protobuf` & `chromadb`.

### Failed to contact link_attacher_node

If gazebo_ros_link_attacher plugin is not responsive, manually attach links like so (e.g. `blue cube`):
```bash
rosservice call /link_attacher_node/attach "model_name_1: 'blue_cube'
link_name_1: 'link'
model_name_2: 'gripper_link_sub'
link_name_2: 'link'"
```
and detach like so:
```bash
rosservice call /link_attacher_node/detach "model_name_1: 'blue_cube'
link_name_1: 'link'
model_name_2: 'gripper_link_sub'
link_name_2: 'link'"
```

Alternatively, update `toh_solver_group_5_prototype.py` to check whether `link_attacher_node` is running using `rosservice.get_service_list()`.


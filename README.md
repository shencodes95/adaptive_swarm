# Adaptive Swarm

<img src="https://github.com/RuslanAgishev/adaptive_swarm/blob/master/figures/passage_3_drones.gif" width="450"/> <img src="https://github.com/RuslanAgishev/adaptive_swarm/blob/master/figures/passage_3_drones_payload.gif" width="450"/>

## Package description

This project is a layered path planner algorithm to solve multiple agents navigation problem in a cluttered environment.

The general path planning problem is divided into approximate global trajectory construction, which is further smoothed by a local path planning method.
The proposed approach provides a solution based on a leader-followers architecture with a prescribed formation geometry that adapts dynamically to the environment and avoids collisions.

<img src="https://github.com/RuslanAgishev/adaptive_swarm/blob/master/figures/layered_planner/rr_path.png" width="400"/> <img src="https://github.com/RuslanAgishev/adaptive_swarm/blob/master/figures/layered_planner/navigation.png" width="400"/>

![potential_surface](https://github.com/RuslanAgishev/adaptive_swarm/blob/master/figures/layered_planner/surface_potential_trajs.png)

The path generated by the global planner based on rapidly-exploring random tree (RRT) algorithm is corrected with the artificial potential fields (APF) method that ensures robots trajectories to be collision-free, reshaping the geometry of the formation when required by environmental conditions. Take a look at my another path planning [repository](https://github.com/RuslanAgishev/motion_planning) for more examples.


## Getting started

Execute the following command in order to see how the planner algorithm works in simulation:
```bash
python scripts/layered_planner/layered_planner_sim.py 
```

## Real flight

Here I would like to describe how to use the package for autonomous path planning of a group of nano-quadrotors
[Crazyflies](https://www.bitcraze.io/products/old-products/crazyflie-2-0/).

### Dependencies
* [ROS](https://www.ros.org/)
* [crazyflie_ros](https://github.com/whoenig/crazyflie_ros)
* [vicon_bridge](https://github.com/ethz-asl/vicon_bridge)

Install ROS (the package is tested with [kinetic](http://wiki.ros.org/kinetic/Installation/Ubuntu)
version and Ubuntu 16.04),
setup a workspace and build the packages:

```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
git clone --recursive https://github.com/whoenig/crazyflie_ros
git clone https://github.com/ethz-asl/vicon_bridge
git clone https://github.com/RuslanAgishev/adaptive_swarm.git
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

The path planning algrithm is built with a known map assumption.
You can define obstacles location of your environment in
[`layered_planner.py`](https://github.com/RuslanAgishev/adaptive_swarm/blob/master/scripts/layered_planner/layered_planner.py#L92).

- Laucnh external position estimator (Vicon motion capture system), and connect to drones:
```bash
roslaunch adaptive_swarm connect123.launch
```
- Command the drones to fly in a formation through a map of obstacles:
```bash
rosrun adaptive_swarm layered_planner.py 
```
<img src="https://github.com/RuslanAgishev/adaptive_swarm/blob/master/figures/layered_planner/narrow_passage/real1.png" width="240"/> <img src="https://github.com/RuslanAgishev/adaptive_swarm/blob/master/figures/layered_planner/narrow_passage/real2.png" width="240"/> <img src="https://github.com/RuslanAgishev/adaptive_swarm/blob/master/figures/layered_planner/narrow_passage/real3.png" width="240"/>

## Citation
Feel free to cite my [Master thesis](https://github.com/RuslanAgishev/adaptive_swarm/blob/master/MSc_Thesis_Skoltech.pdf), if you find the package useful for your research.
```
@MastersThesis{Agishev:Thesis:2019,
    author     =     {Ruslan Agishev},
    title     =     {{Adaptive Control of Swarm of Drones for
Obstacle Avoidance}},
    school     =     {Skolkovo Institute of Science and Technology},
    address     =     {Moscow, Russia},
    year     =     {2019},
}
```


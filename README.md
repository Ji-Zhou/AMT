# AMT

Note: The project is just for internal discussion currently.  
Contact me: Ji.Zhou@monash.edu or zhouji1212@gmail.com  
  
# Overview method

High-Level(Central CPU): Improved Hybrid A*  
  
Conflict-Solver(Central CPU): Priority Based Search  
  
Low-Levl(Truck CPU): Quintic Planner  
  
# Basic Settings

N mining trucks (6m * 3m) move in specific area (100m * 100m) with own initial and final states. Max speed, acceleration and steer are constricted.  
  
The truck status includes specific positions and yaw angles, and the reason for this consideration is that trucks typically need to meet specific postures to cooperate with forklift.  
  
The algorithm is dynamic and time step is set as 0.1s. In case, the planned trajectory of each truck is updated every 0.1 seconds, with a planning period of 4.5 seconds to 8.5 seconds.  
  
High-Level method provide reference path for each truck and truck is moving following with max offset Path_Width. While no feasible trajectory can be obtained by Low-Level method, truck is braked, 4.5s-8.5s is sufficient for stopping.  
  
If there is no truck movement within 10 meters of the stopped truck, it is considered that it can be restarted.  
  
Once stopped, maintain the stop state for a minimum of TIME_! seconds.  
  
When the stop time exceeds TIME_2 seconds, it is considered as a deadlock and the higher management will replan the reference path.  
  
Remote takeover is allowed during truck stoppage.
  
# Baseline

[1]-[3] have both open-source their code, where we use the 2D mode of [3]. [1]-[3] do not simultaneously satisfy both kinematics and dynamic characteristics, but we use them  as a comparison to further validate the effectiveness of the proposed method

[1] Y. Ouyang, B. Li, Y. Zhang, T. Acarman, Y. Guo and T. Zhang, "Fast and Optimal Trajectory Planning for Multiple Vehicles in a Nonconvex and Cluttered Environment: Benchmarks, Methodology, and Experiments," 2022 International Conference on Robotics and Automation (ICRA)  
  
[2]J. Li, M. Ran and L. Xie, "Efficient Trajectory Planning for Multiple Non-Holonomic Mobile Robots via Prioritized Trajectory Optimization," in IEEE Robotics and Automation Letters, vol. 6, no. 2, pp. 405-412, April 2021  

[3]J. Park, D. Kim, G. C. Kim, D. Oh and H. J. Kim, "Online Distributed Trajectory Planning for Quadrotor Swarm With Feasibility Guarantee Using Linear Safe Corridor," in IEEE Robotics and Automation Letters, vol. 7, no. 2, pp. 4869-4876, April 2022
  
# Datasets

Based on the test data used in [2] and [3], we also selected three different sized fleets for testing, including 4, 8, and 16 trucks, evenly distributed in the square area. The line connecting the two opposing vehicles passes through the center of the square.  

The line connecting the starting and ending points of all trucks passes through the center of a square, which is considered an extreme case to qualitatively evaluate the efficiency of the proposed method.  
  
In addition, currently the maximum number of trucks put into use in a single mining area does not exceed 30, and testing the algorithm's ability in conflict areas (loading and unloading areas, interactions) with 16 vehicles is sufficient.  
  
# Test 1

Trucks are not allowed to move laterally along the reference path. In the event of a deadlock, the central CPU will re plan the reference path.  

![image]([https://github.com/ZhiliangMa/MPU6500-HMC5983-AK8975-BMP280-MS5611-10DOF-IMU-PCB/blob/main/img/IMU-V5-TOP.jpg](https://github.com/Ji-Zhou/AMT/blob/main/git/4_1.gif))


# AMT

Note: The project is just for internal discussion currently.  
  
For the replay of GIF, please refresh this interface. This project currently does not support loop playback of GIF.  
  
Contact me: Ji.Zhou@monash.edu or zhouji1212@gmail.com or ZhouJ@bupt.cn  
  
# Overview method

High-Level: Improved Hybrid A*  
  
Conflict-Solver: Priority Based Search  
  
Low-Levl: Quintic Planner  
  
# Overview

N mining trucks (6m × 3m) operate within a designated 100m × 100m area, each starting and ending at specific states. The trucks' maximum speed, acceleration, and steering are constrained to meet realistic operational requirements.  
  
The state of each truck includes precise positions and yaw angles, as trucks often need to align their posture accurately for cooperation with forklifts.  
  
The algorithm is dynamic, with a time step of 0.1 seconds. Accordingly, the planned trajectory of each truck is updated every 0.1 seconds, and the planning period ranges from 4.5 to 8.5 seconds.  
  
The high-level method provides a reference path for each truck, and the truck follows it with a maximum allowable lateral offset. If the low-level method fails to generate a feasible trajectory, the truck will brake, and the 4.5–8.5-second planning window is sufficient to bring the truck to a complete stop.  
  
If no other trucks come within 10 meters of a stopped truck, it is considered eligible to restart. However, a stopped truck must remain stationary for at least TIME_1 seconds. If the stop duration exceeds TIME_2 seconds, it is deemed a deadlock, prompting the higher-level control to replan the reference path.  
  
Remote takeover is allowed during truck stoppage.
  
# Baseline

References [1]-[3] have made their codes open source, and we utilize them for comparison(2D mode of [3]). While [1]-[3] do not simultaneously account for both kinematic and dynamic constraints, they are included as baselines to validate the effectiveness of the proposed method.  
  
[1] Y. Ouyang, B. Li, Y. Zhang, T. Acarman, Y. Guo and T. Zhang, "Fast and Optimal Trajectory Planning for Multiple Vehicles in a Nonconvex and Cluttered Environment: Benchmarks, Methodology, and Experiments," 2022 International Conference on Robotics and Automation (ICRA)  
  
[2]J. Li, M. Ran and L. Xie, "Efficient Trajectory Planning for Multiple Non-Holonomic Mobile Robots via Prioritized Trajectory Optimization," in IEEE Robotics and Automation Letters, vol. 6, no. 2, pp. 405-412, April 2021  

[3]J. Park, D. Kim, G. C. Kim, D. Oh and H. J. Kim, "Online Distributed Trajectory Planning for Quadrotor Swarm With Feasibility Guarantee Using Linear Safe Corridor," in IEEE Robotics and Automation Letters, vol. 7, no. 2, pp. 4869-4876, April 2022
  
# Datasets

Using the test data from [2] and [3], we evaluated our approach on fleets of varying sizes (4, 8, and 16 trucks), evenly distributed within the square area.  
  
For all configurations, the line connecting the start and end points of the trucks passes through the center of the square, representing a challenging case designed to qualitatively assess the efficiency of the proposed method.  

Currently, the maximum number of trucks deployed in a single mining area does not exceed 30. Testing with 16 trucks is sufficient to evaluate the algorithm's performance in high-conflict areas, such as loading and unloading areas or intersections.
  
  
# Test 1

Trucks are not allowed to move laterally along the reference path. In the event of a deadlock, the reference path will be replanned.  

![img](https://github.com/Ji-Zhou/AMT/blob/main/git/4_1.gif)
  
# Test 2

Trucks are allowed to move laterally along the reference path.  

![img](https://github.com/Ji-Zhou/AMT/blob/main/git/4_2.gif)
  
# Test 3

The above two situations are only for testing the abilities of proposed method. In fact, for extreme cases where the reference paths of two trucks coincide, we choose to consider the midpoint of the overlapping paths as an obstacle and replan the reference path.  

![img](https://github.com/Ji-Zhou/AMT/blob/main/git/4_3.gif)
  
# Test 4

In the case of 8 trucks, tasks can be also finished.  

![img](https://github.com/Ji-Zhou/AMT/blob/main/git/8_1.gif)
  
# Test 5

In the case of 16 trucks, more braking, reversing, and replanning are carried out, but trucks are still able to reach the finish point.    

![img](https://github.com/Ji-Zhou/AMT/blob/main/git/16_1.gif)
  
# Test 6

Considering low density obstacles (5 circular obstacles with a radius of 1).    

![img](https://github.com/Ji-Zhou/AMT/blob/main/git/16_Low.gif)
  
# Test 7

Considering high density obstacles (9 circular obstacles with a radius of 1).    

![img](https://github.com/Ji-Zhou/AMT/blob/main/git/16_High.gif)
  
# Experiments

The tests above serve qualitative evaluation purposes. For further validation, we compared the results of our method with those in [1]-[3]. Note that [1] and [2] are not dynamic methods, and while [2] and [3] do not account for truck kinematics, their agents are unconstrained by steering and other factors. Thus, the experiments data is provided for reference only due to the algorithms considering different factors and being designed for different scenarios.   
  
In a word, compared with [2] and [3], our path and velocity will definitely be smoother. Compared with [1], the advantages in these indicators may decrease, but we can address more complex and dynamic scenarios.   
  
The dynamic method has a time step setting, while the static method records as none.  RMSD (Root Mean Square Deviation) and CV (Coefficient of Variation) are used to measure smoothness of curve and velocity.  
  
Due to the privacy of the data in such a open link, the data in the table has not been completed yet, but you can already see some trends. 

I will complete these data once I receive your approval if it's no problem completing the experiments like this.  
![img](https://github.com/Ji-Zhou/AMT/blob/main/git/Figure2.png)

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
The algorithm is dynamic and time step is set as 0.1s. In case, the planned trajectory of each truck is updated every 0.1 seconds, with a planning period of 4.5 seconds to 8.5 seconds
s. High-Level method provide reference path for each truck and truck is moving following with specific offset Path_Width. While no feasible trajectory can be obtained by Low-Level method, truck is braked, while 4.5s-8.5s is sufficient for stopping. 

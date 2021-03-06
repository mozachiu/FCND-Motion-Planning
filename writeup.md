## Project: 3D Motion Planning
![Quad Image](./misc/enroute.png)

---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
The "backyard_flyer.py" use fixed flight path after take off. The "motion_planning.py" uses a similar base as the "backyard_flyer". To calculate path before take off, it added PLANNING state and use the  method "plan_path" performs path planning, than get waypoints to fly to the goal.
- motion_planning.py
` 
The plan_path() method read obstacle data from 'colliders.csv', get grid representation of a 2D configuration space based on given obstacle data, drone altitude and safety distance with the method "create_grid()" in "planning_utils.py". 
` 
- planning_utils.py
` 
The class "Action" valuate the cost and return the allowed action. The method "valid_actions" returns a list of valid actions given a grid and current node.
Then find a path from start to goal using A* with the method "a_star()". A* is a search algorithm calculate the sum cost of all plan with the lowest expected cost.
Finally generate the waypoints  and sent to the simulator using the method "send_waypoints".
` 

### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
Here students should read the first line of the csv file, extract lat0 and lon0 as floating point values and use the self.set_home_position() method to set global home. Explain briefly how you accomplished this in your code.
- Read the first line of the csv file, get the lat0 and lon0 values using with the code 'plan_path' method in `motion_planning.py`. Feed the lat0 and lon0 value to the self.set_home_position() method as global home.

#### 2. Set your current local position
Here as long as you successfully determine your local position relative to global home you'll be all set. Explain briefly how you accomplished this in your code.
- Retreived the current position from self.global_position.
- Convert the current global position to local position using the method "global_to_local" with global home position get from last step from self.global_home.

#### 3. Set grid start position from local position
This is another step in adding flexibility to the start location. As long as it works you're good to go!
- Get the grid map and the offset of the north and east coordinates using the method "create_grid", then we know the safety range to obstacles.
- Set start point(grid_start) from current location.

#### 4. Set grid goal position from geodetic coords
This step is to add flexibility to the desired goal location. Should be able to choose any (lat, lon) within the map and have it rendered to a goal location on the grid.
- Use the manual mode go to the desired place I like and get the goal position. For example [-122.400566, 37.796384].
- Convert the global goal position to the local position using  the method "global_to_local".
- Set the local position to grid goal position.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
Minimal requirement here is to modify the code in planning_utils() to update the A* implementation to include diagonal motions on the grid that have a cost of sqrt(2), but more creative solutions are welcome. Explain the code you used to accomplish this step.
- For better movement to the goal, I add SOUTH_EAST, NORTH_EAST, SOUTH_WEST and NORTH_WEST directions to the Action class with cost of sqrt(2). Also the valid_actions method make sure actions is valid.

#### 6. Cull waypoints 
For this step you can use a collinearity test or ray tracing method like Bresenham. The idea is simply to prune your path of unnecessary waypoints. Explain the code you used to accomplish this step.
- Using collinearity_prune function provided by the lectures. The trajectories looks better.

### Execute the flight
#### 1. Does it work?
It works!

### Double check that you've met specifications for each of the [rubric](https://review.udacity.com/#!/rubrics/1534/view) points.
  
# Extra Challenges: Real World Planning

For an extra challenge, consider implementing some of the techniques described in the "Real World Planning" lesson. You could try implementing a vehicle model to take dynamic constraints into account, or implement a replanning method to invoke if you get off course or encounter unexpected obstacles.



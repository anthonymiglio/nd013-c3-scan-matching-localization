# SDCND: Scan Matching and Localization

## Requirement: 
In this final project, your goal will be to localize a car driving in simulation for at least 170m from the starting position and never exceeding a distance pose error of 1.2m. The simulator provides the simulation car with a Lidar at regular intervals of Lidar scans. There is also a point cloud file "map.pcd" available, and by using point registration matching between the map and scans to accomplish localization for the car, this point cloud map is from the CARLA simulator (https://carla.org/).


To run this project in the workspace folder: 

At c3-main.cpp at code line 310 Select between: Iterative Closest Point (ICP) or Normal Distributions Transform (NDT)
```
// define the ICP and NDT method for the comparison by uncommenting one of the next two lines:
// transform = ICP(mapCloud, cloudFiltered, pose, iterations);
transform = NDT(mapCloud, cloudFiltered, pose, iterations);
```

Compile
```
cd /home/workspace/c3-project
cmake .
make
```

Run on Terminal 1
```
su - student // Ignore Permission Denied, if you see student@ you are good
cd /home/workspace/c3-project
./run_carla.sh
```

Run on Terminal 2
```
su - student // Ignore Permission Denied, if you see student@ you are good
cd /home/workspace/c3-project
./cloud_loc // Might have core dump on start up, just rerun if so. Crash doesn't happen more than a couple of times
```


## Task:
Localize a self-driving car within a point cloud from the CARLA simulator with ICP and NDT Scan Matching localization algorithms by traveling 170m within a maximum pose error of 1.2m.

**Step 1:** Filter scan using voxel filter
The first step is fairly short - make use of cloudFiltered and scanCloud to filter the point cloud using a voxel grid.

**Step 2:** Find pose transform by using ICP or NDT matching
The second step is the main portion of the exercise. You can choose to use either ICP or NDT matching in order to find the pose transformation. It is suggested to keep the code for this fairly short within the main() function, and instead call out to a separate function you create either elsewhere within c3-main.cpp or in a separate .cpp file. Note also that you can make use of the getPose() function in helper.cpp if you output a 4D transform matrix from your ICP or NDT function in order to get the Pose object.

**Step 3:** Transform the scan so it aligns with ego's actual pose and render that scan
In the final major step, you should be able to transform the filtered scan using your calculated transform into a new point cloud using pcl.

Note that you will also want to update what is fed into renderPointCloud to change scanCloud into your newly transformed point cloud, or else your transform will not be appropriately rendered.


<img src="/img/project-overview.png"/>
The display is all rendered using the PCL viewer. Car as red box, 'map.pcd' showing as blue point cloud and current lidar scan as red point cloud.

## Controlling the car
You will be manually controlling the simulation car. To do this PCL has a listener in its viewer window. First click on the view window to activate the keyboard listener and then the following keys can be used to actuate the car. Note in order not to overwhelm the listener refrain from holding down keys and instead just tap keys.

*Throttle* ( **UP** Key )
Each tap will increase the throttle power. Three presses is a good amount of throttle power.

*Reverse/Stop* ( **Down** Key )
A single tap will stop the car and reset the throttle to zero if it is moving. If the car is not moving it will apply throttle in the reverse direction.

*Steer* ( **Left/Right** Keys )
Tap these keys to incrementally change the steer angle. The green line extruding out in front of the red box in the image above represents the steer value and its end point will move left/right depending on the current value.

*Center Camera* ( **a** )
Press this key to recenter the camera with a top down view of the car.

*Rotate Camera* ( mouse **left click** and **drag** )
*Pan Camera* ( mouse **middle button** and **drag**)
*Zoom* (mouse **scroll**)

## Iterative Closest Point (ICP) Outcome
The ICP algorithm has a more direct approach, is more straightforward to comprehend, and is faster and less accurate than the NDT algorithm.
After some tries adjusting (iterations = 30, filter resolution = 1.0, scan rate = 5000), the requirement of 170m travel with maximum pose error of 1.2 was reached.
<img src="/img/ICP.png"/>

## Normal Distributions Transform (NDT) Outcome
Because of the more accurate measurements, for a fair comparison, with the same adjustments, it took much longer for the NDT algorithm to accomplish the task than the ICP algorithm.
<img src="/img/NDT.png"/>


# Fiducial-Based Localization


This is one of the ways in which a robot's position in the `world` (localization) can
be determined.  For a discussion of other approaches, see the
[Navigation and Localization Overview](../overview/overview.md).

This document assumes that you are running the software on a Ubiquity
Robotics robot base, using the supplied Raspberry Pi camera.

## Basic Concept

If you're eager to start localizing your robot, and don't wish to read about
how fiducial localization works, you can skip reading this sub-section.

The Ubiquity Robotics localization system uses a number of fiducial markers
of known size.  Detection of the markers
is done by using the rogobot's camera.  The characteristics of the images of the fiducials enable the robot to compute its location.


![Fiducial coordinate system](fiducial.png)

*T<sub>map_fid2</sub> = T<sub>map_fid1</sub> * T<sub>cam_fid2</sub> * T<sub>fid1_cam</sub>*


![Fiducial coordinate system](two_fiducials.png)

## Print Some Fiducials

A PDF file containing a range of fiducial markers can be generated with a
command such as:
```rosrun aruco_detect create_markers.py 100 112 fiducials.pdf```

The *100* and *112* specify the starting and ending numerical IDs of the
markers.  It is not important what these are (we recommend starting at 100),
but it is important that each marker has a unique ID.  This PDF file can
be printed to produce the fiducials.  
## Mount the fiducials
Affix the fiducials (in any order) to any convenient
surface, such as a ceiling, where they will be viewed by the robot's camera.
They need to be at a sufficient spacing so that more than one is visible at a time, if the robot is at least 6 feet away from the wall or ceiling holding the fiducials..

## Running the Software

The following command launches the detection and SLAM nodes on the robot:

```roslaunch magni_nav aruco.launch```

It should be run for the first time with at least one marker visible.
A map (this is a file of fiducial poses) is created such that the current
position of the robot is the origin.

## Using rviz to Monitor Map Creation

The following command can be used on a laptop or desktop to run the
[robot visualization tool](http://wiki.ros.org/rviz), rviz.

[Link here to the tutorial where we explain ROS_MASTER_URI]???


```roslaunch fiducial_slam fiducial_rviz.launch```

This will produce a display as shown below.  The bottom left pane shows the
current camera view.  This is useful for determining if the fiducial density
is sufficient.  The right-hand pane shows the map of fiducials as it is being
built. Red cubes represent fiducials that are currently in view of the camera.
Green cubes represent fiducials that are in the map, but not currently
in the view of the camera. The blue lines show connected pairs of fiducials
that have been observed in the camera view at the same time.  The robustness
of the map is increased by having a high degree of connectivity between the
fiducials.

![Visualizing with rviz](fiducial_rviz.png)
# Artag
Package ROS pour détecter la position et l'angle des ArTags avec noetic (package opérationnel avec noetic)
See [ROS wiki](http://wiki.ros.org/ar_track_alvar) for the users document.

# Generating AR tags

Two pdf files are in the markers directory containing tags 0-8 and 9-17, respectively. The markers are 4.5 cm (although when printed and measured, came out to 4.4 cm for me). If you want to generate your own markers with different ID numbers, border widths, or sizes, run:
```
rosrun ar_track_alvar createMarker
```
Instructions will appear describing the various options.



# Detecting individual tags

The first use case for this package is to identify and track the poses of (possibly) multiple AR tags that are each considered individually. The node individualMarkers takes the following command line arguments:

- *Marker size *(double) -- The width in centimeters of one side of the black square marker borderMax new marker error (double) -- A threshold determining when new markers can be detected under uncertainty
- *Max track error *(double) -- A threshold determining how much tracking error can be observed before an tag is considered to have disappeared
- *Camera image topic* (string) -- The name of the topic that provides camera frames for detecting the AR tags. This can be mono or color, but should be an UNrectified image, since rectification takes place in this package
- *Camera info topic (*string) -- The name of the topic that provides the camera calibration parameters so that the image can be rectified
-* Output frame *(string) -- The name of the frame that the published Cartesian locations of the AR tags will be relative to 
```bash
    <arg name="marker_size"          default="5.0" />
    <arg name="max_new_marker_error" default="0.05" />
    <arg name="max_track_error"      default="0.05" />

    <arg name="cam_image_topic" default="/rgb/image_raw" />
    <arg name="cam_info_topic" default="/rgb/camera_info" />
    <arg name="output_frame"         default="/camera_base" />
```
To run the detection:
```bash
roslaunch ar_track_alvar usb_cam_ar_track.launch
```

# **ROS API**

## Subscribed topic (common among usecases)

> (e.g.) /kinect_head/rgb/camera_info (sensor_msgs/CameraInfo)

Camera needs calibrated before this topic to become available. 

> (any topic in an image format) (sensor_msgs/Image)

Image to be analyzed for markers. 


## Published Topics
> visualization_marker (visualization_msgs/Marker)

This is an rviz message that when subscribed to (as a Marker in rviz), will display a colored square block at the location of each identified AR tag, and will also overlay these blocks in a camera image. Currently, it is set to display a unique color for markers 0-5 and a uniform color for all others. 

> ar_pose_marker (ar_track_alvar/AlvarMarkers)

This is a list of the poses of all the observed AR tags, with respect to the output frame 

## Provided tf Transforms
> Camera frame (from Camera info topic param) → AR tag frame

Provides a transform from the camera frame to each AR tag frame, named ar_marker_x, where x is the ID number of the tag. 




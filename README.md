# artag
Package ROS pour détecter la position et l'angle des ArTags avec noetic (package opérationnel avec noetic)
See [ROS wiki](http://wiki.ros.org/ar_track_alvar) for the users document.

Using:
- Modify the camera topics in the files: artag/ar_track_alvar/launch/usb_cam_ar_track.launch
```bash
    <arg name="marker_size"          default="5.0" />
    <arg name="max_new_marker_error" default="0.05" />
    <arg name="max_track_error"      default="0.05" />

    <arg name="cam_image_topic" default="/rgb/image_raw" />
    <arg name="cam_info_topic" default="/rgb/camera_info" />
    <arg name="output_frame"         default="/camera_base" />
```
run
```bash
roslaunch ar_track_alvar usb_cam_ar_track.launch
```

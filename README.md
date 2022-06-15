# Reach RTK ROS Node


This is a very simple ROS node that allows for publishing of NMEA messages onto the ROS framework.
This package aims to support the [Reach RTK GNSS](https://emlid.com/shop/reach-rtk-kit/) module by Emlid.
Right now this supports all NMEA messages from the package, while some are not used to publish anything onto ROS.

Edit 06/15/2022: Modified by Srijal Shekhar Poojari to include receiving NMEA fixes over a serial port. Tested on the Reach M+ module.

## Quickstart Guide

1. Clone this package into your ROS workspace and build it
2. Turn on your Reach RTK module
3. Ensure that you are connected to the same network as it, open the Reachview 3 App or the IP address of device on a web browser.

For getting fixes over TCP:
4. Edit the "Position output"
   * Be in TCP mode
   * Role: Server
   * Address: localhost
   * Port: any free port
   * Format: NMEA
5. Using the IP of the Reach RTK and the specified port launch this package
6. `rosrun reach_ros_node nmea_tcp_driver _host:=128.4.89.123 _port:=2234`

Alternatively, if you connect the Reach M+ module via a USB cable:
4. Set position streaming to "Serial" with
   * Port: USB to PC
   * Baud rate: 38400
   * Format: NMEA
5. The node automatically finds the device serial port using pyudev (install v0.21.0 `pip3 install -Iv pyudev==0.21.0` if not installed), and starts it. 
6. Run using `rosrun reach_ros_node nmea_serial_driver`

## Driver Details

* Publishes GPS fix, velocity, and time reference
  * `/fix` - NavSatFixed
  * `/vel` - TwistedStamped
  * `/time` - TimeReference
* Can specify the following launch parameters
  * `~frame_timeref` - Frame of the time reference
  * `~frame_gps` - Frame of the fix and velocity
  * `~use_rostime` - If set to true, ROS time is used instead of the GPS time
  * `~use_rmc` - Use compressed RMC message (note: this does not have the covariance for the fix)



## Credit

Original starting point of the driver was the ROS driver [nmea_navsat_driver](https://github.com/ros-drivers/nmea_navsat_driver) which was then expanded by [CearLab](https://github.com/CearLab/nmea_tcp_driver) to work with the Reach RTK.
This package is more complete, and aims to allow for use of the Reach RTK in actual robotic systems, please open a issue if you run into any issues.
Be sure to checkout this other driver by [enwaytech](https://github.com/enwaytech/reach_rs_ros_driver) for the Reach RS.







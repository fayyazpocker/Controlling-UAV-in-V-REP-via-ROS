# Controlling-drone-in-V-REP-via-ROS
A drone model in V-REP can be controlled using ROS commands in terms of pitch, roll, throttle and yaw.

Follow the steps to control drone in V-REP using ROS
1. Run roscore by typing the following command in your terminal:
roscore
2. Launch the simulator by typing ./vrep.sh in the V-REP directory and check if RosInterface is loaded properly
3. Drag and drop uav.ttm in the scene
4. On running simulation, you can find the following topics 
/drone_command /drone_yaw

“/drone_command” is a topic subscribed by the uav model. It commands the drone’s motion in terms of roll, pitch, yaw and throttle.
“/drone_yaw” is a topic published by the uav model. It indicates the drone’s orientation about the z-axis with respect to the V-REP world.
5. Check the structure of the message of /drone_command using the following command:
rostopic type /drone_command | rosmsg show
6. Check the structure of the message of /drone_yaw using the following command:
rostopic type /drone_yaw | rosmsg show


## Arming the drone
The condition to arm the drone is rcThrottle = 1000 (minimum value) and rcAUX4 ≥ 1300. To test arming the drone model, publish the following message to the topic “/drone_command” by typing the command:
≫ rostopic pub /drone_command plutodrone/PlutoMsg "{rcRoll: 1500,
rcPitch: 1500, rcYaw: 1500, rcThrottle: 1000, rcAUX1: 0, rcAUX2:
0, rcAUX3: 0, rcAUX4: 1500}"

This should now arm the drone. A message should pop on the V-REP window which says “ARMED” and the
propellers should start rotating


## Flight (Take-Off)
The condition for the drone to take-off is rcThrottle ≥ 1500, after arming. To test the drone’s take-off, publish the following message to increase the throttle:
≫ rostopic pub /drone_command plutodrone/PlutoMsg "{rcRoll: 1500,
rcPitch: 1500, rcYaw: 1500, rcThrottle: 1500, rcAUX1: 0, rcAUX2:
0, rcAUX3: 0, rcAUX4: 1500}"

The drone should now steadily rise until a new command is given.

## Disarming the drone
A disarmed drone means the drone is in a mode that will not take any commands from a user or software to
fly.
The condition to disarm the drone is rcAUX4 ≤ 1200. To test disarming the drone model, publish the following
message to the topic “/drone_command” by typing the command:
≫ rostopic pub /drone_command plutodrone/PlutoMsg "{rcRoll: 1500,
rcPitch: 1500, rcYaw: 1500, rcThrottle: 1000, rcAUX1: 0, rcAUX2:
0, rcAUX3: 0, rcAUX4: 1200}"
The drone should now be disarmed. A message should pop on the V-REP window which says “DISARMED”
and the propellers should stop rotating.

## Heading of the drone
Blue dummy indicates the pitch direction of the drone
**[Running Your Code](Running-Your-Code)**
* **[Overview](Running-Your-Code#overview)**
* **[Turning the Robot On](Running-Your-Code#turning-the-robot-on)**
* **[Connecting to the Robot](Running-Your-Code#connecting-to-the-robot)**
* **[Checking the Ports](Running-Your-Code#checking-the-ports)**
* **[Launch the Code on the Robot](Running-Your-Code#launch-the-code-on-the-robot)**
* **[Enable the Robot](Running-Your-Code#enable-the-robot)**
* **[Congrats](Running-Your-Code#congrats)**


##Overview
This guide details how to run completed code on the robot. If you have any trouble navigating Eclipse, please reference the [Eclipse Guide](Eclipse-Guide) for details.

##Turning the Robot On

[[https://github.com/CMUFeiyue/Guides/blob/master/images/running/switch00.png]]

Make sure the battery is connected to the robot, and then flip the switch to turn the robot on.  

##Connecting to the Robot
Open your internet connection options and make sure you connect to the robot. For testing purposes connect to 3504 Testboard 2.

##Checking the Ports
`RobotMap.java` contains values for the ports of motors, joysticks and limit switches. All these ports must be verified with the hardware. Basically, if you're going to refer to a motor connected to port 1, you need to check that the robot is wired with that motor actually in port 1. Refer to [Determining Port Numbers](https://github.com/CMUFeiyue/Guides/wiki/Determining-Port-Numbers) for more information about this.

##Launch the Code on the Robot 

[[https://github.com/CMUFeiyue/Guides/blob/master/images/running/launch00.png]]

Go to Eclipse and open the ```Robot.java``` class. Launch your code as described in the [Eclipse Guide](Eclipse-Guide) and as seen below.

##Enable the Robot

[[https://github.com/CMUFeiyue/Guides/blob/master/images/running/driver01.png]]

Open the Driver Station. Make sure that the status of Communications and Robot Code is green.
Then enable the correct mode (TeleOperated or Autonomous). If you are running the code from [Programming your Robot](Programming-the-Robot), please enable Teleoperated mode. **If you need to stop the robot from executing, simply click disable.** If you encounter problems or need more help, please reference the [Driver Station Guide](Driver-Station-Guide).

##Congrats!
If everything went right, the robot should be executing your code. In the case of the Programming the Robot guide, this means the motor should start spinning forward when you press A and backwards when you press B on the joystick. Once you’re done observing the robot, switch it back into disabled mode to stop it.
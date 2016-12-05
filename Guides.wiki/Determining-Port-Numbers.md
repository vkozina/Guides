**[Determining Port Numbers](https://github.com/CMUFeiyue/Guides/wiki/Determining-Port-Numbers)**
* **[Overview](https://github.com/CMUFeiyue/Guides/wiki/Determining-Port-Numbers#overview)**
* **[RoboRIO Configuration](https://github.com/CMUFeiyue/Guides/wiki/Determining-Port-Numbers#roborio-configuration)**
* **[Joysticks](https://github.com/CMUFeiyue/Guides/wiki/Determining-Port-Numbers#joysticks)**

##Overview
Before running your code on the robot, you must make sure that all the port values in `RobotMap` match up with the actual wiring of components.

##RoboRIO Configuration

The RoboRIO has an online configuration tool that allows you to view all the motor ports. While connected to the roboRIO, open **Internet Explorer** and go to [http://roboRIO-9999-FRC.local](http://roboRIO-9999-FRC.local). You need to use Internet Explorer because the tool requires Silverlight, which is no longer available on certain other browsers, including Chrome.
[[https://github.com/CMUFeiyue/Guides/blob/master/images/ports/onlineconfig00.PNG]]

Once there, you can click on the "Talon SRX" icons on the left to view more information about a specific motor. To see determine the corresponding motor on the robot, click the check box next to **Light Device LED** and select **Save**. This will temporarily flash the talon orange at a quicker pace than usual, allowing you to see which motor it is. The device ID is the port number of the motor. Make sure that this matches the code in `RobotMap.java`, so you're spinning the appropriate motor.
[[https://github.com/CMUFeiyue/Guides/blob/master/images/ports/onlineconfig01.PNG]]

## Joysticks
If your code uses a joystick (like the [Programming your Robot](Programming-the-Robot) code), connect it to your computer. Open the driver station and go to the USB tab to make sure your joystick is connected. Make sure that the port also matches the value of the joystick port in ```RobotMap.java```. In the case shown the port of the joystick should be 0.

```java
public static final int JOYSTICK_PORT = 0;
```

[[https://github.com/CMUFeiyue/Guides/blob/master/images/ports/joystickports00.PNG]]
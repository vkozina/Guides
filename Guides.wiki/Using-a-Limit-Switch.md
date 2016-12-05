**[Using a Limit Switch](Using-a-Limit-Switch)**
* **[Overview](Using-a-Limit-Switch#overview)**
* **[RobotMap.java](Using-a-Limit-Switch#robotmapjava)**
* **[Subsystem](Using-a-Limit-Switch#subsystem)**
* **[Robot.java](Using-a-Limit-Switch#robotjava)**
* **[Command](Using-a-Limit-Switch#command)**
* **[OI.java](Using-a-Limit-Switch#oijava)**

##Overview
A limit switch is a physical mechanism attached to the robot that can be used to trigger commands. Limit switches can either be "normally open" or "normally closed", which allows the trigger to be either "the switch opened" or "the switch closed". An example of a use case is bounding the movement of an arm in code to avoid overextending it, since doing so may cause mechanical failure.

In this example, we will modify the `FirstCBRProjectExample` project to use a limit switch.

##Robotmap.java
Declare a variable to store the port of the limit switch.
```java
public static final int LIMITSWITCH_PORT = 0; 
```

##Subsystem
Declare a `DigitalInput` object in whatever subsystem you want to control with it. In this example we are modifying the `FirstCBRProjectExample` project, so the subsystem of interest is `DriveMotor`.

```java
private DigitalInput limitSwitch;
```

Add code to instantiate the limit switch in the `DriveMotor` constructor. Use the constant for the port created in `RobotMap.java`.
```java
limitSwitch = new DigitalInput(RobotMap.LIMITSWITCH_PORT);
```

Add a method that returns whether or not the limit switch got closed.
```java
public boolean isBumped() {
	return !limitSwitch.get();
}
```

##Robot.java
This class does not need to change.

##Command
Modify any commands so that their `isFinished` functions return true if the switch deviates from its "normal" state. In this case, the switch is normally open, and the command should stop if the switch is ever bumped (or closed). You might want to rename the command to indicate this new behavior. For example, when this was added to the `DriveForward` command, the name was changed to `DriveUntilLimitSwitchIsPressed`. Any number of commands can react to the same limit switch.

```java
protected boolean isFinished() {
   	 return Robot.driveMotor.isBumped();
}
```

##OI.java
Make sure to call your new command (if you changed the name) instead of the old one.

```java
button1.whenPressed(new DriveUntilLimitSwitchIsPressed());
```
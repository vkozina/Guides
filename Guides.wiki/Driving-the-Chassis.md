**[Driving the Chassis](Driving-the-Chassis)**
* **[Overview](Driving-the-Chassis#overview)**
* **[RobotMap.java](Driving-the-Chassis#robotmapjava)**
* **[Subsystem](Driving-the-Chassis#subsystem)**
  * **[Fields](https://github.com/CMUFeiyue/Guides/wiki/Driving-the-Chassis#fields)**
  * **[Constructor](https://github.com/CMUFeiyue/Guides/wiki/Driving-the-Chassis#constructor)**
  * **[driveByJoystick](https://github.com/CMUFeiyue/Guides/wiki/Driving-the-Chassis#drivebyjoystick)**
  * **[stop](https://github.com/CMUFeiyue/Guides/wiki/Driving-the-Chassis#stop)**
* **[Robot.java](Driving-the-Chassis#robotjava)**
* **[Command](Driving-the-Chassis#command)**
* **[OI.java](Driving-the-Chassis#oijava)**

##Overview
This guide goes over how to drive a full chassis with a joystick. It assumes a single gear box of two motors on each side.

##RobotMap.java
Instead of one motor, we now have a gear box on each side. A gear box has 2 motors, one that receives all the commands and another that simply follows the first. This relationship is called master/slave and it allows you to easily keep two motors in sync.

Create variable in `RobotMap.java` for the master and slave on either side. Make sure not to cross the motor ports.
```java
public static final int MASTER_LEFT = 1;
public static final int SLAVE_LEFT = 2;
	
public static final int MASTER_RIGHT = 11;
public static final int SLAVE_RIGHT = 12;
```

You can also declare other values to aid in future calculations, although this is fully optional. The following example calculates the distance for every pulse of the encoder for a particular wheel. This will later be used to find the distance traveled. Make sure to update these values depending on what kind of wheels and gear ratios you are using.
```java
// Encoder-to-distance constants
// How many ticks are there on the encoder wheel?
private static final double pulsePerRevolution = 360;

// How far to we travel when the encoder turns one full revolution?
// Gear ratio is turns of the wheel per turns of the encoder
private static final double wheelSize = 8.0;
private static final double gearRatio = (1/27.21);
	
private static final double distPerRevolution = 
		wheelSize * Math.PI * gearRatio; //(9.07)
	
// Given our set of wheels and gear box, how many inches do we travel per pulse?
public static final double DIST_PER_PULSE = 
		distPerRevolution / pulsePerRevolution;
```

##Subsystem

###Fields
Declare the variables for each talon motor. Additionally declare a `RobotDrive` object, which will later be used in joystick driving. `encOffsetValueRight` and `encOffsetValueLeft` will store a starting position for the encoder in order to calculate "distance since last encoder reset" rather than just "total distance".
```java
private CANTalon masterLeft;
private CANTalon slaveLeft;
    
private CANTalon masterRight;
private CANTalon slaveRight;
    
private RobotDrive robotDrive;
    
private double encOffsetValueRight = 0;
private double encOffsetValueLeft = 0;
```

###Constructor
Inside the constructor, begin by instantiating all the motors. Use the constants declared in `RobotMap.java` to help readibility.

```java
masterLeft = new CANTalon(RobotMap.MASTER_LEFT);
slaveLeft = new CANTalon(RobotMap.SLAVE_LEFT);
		
masterRight = new CANTalon(RobotMap.MASTER_RIGHT);
slaveRight = new CANTalon(RobotMap.SLAVE_RIGHT);
```

Next, `enableBrakeMode` for every motor so that they don't coast when stopping.
```java
masterLeft.enableBrakeMode(true);
slaveLeft.enableBrakeMode(true);
		
masterRight.enableBrakeMode(true);
slaveRight.enableBrakeMode(true);
```

For both slaves, set their `ControlMode` to be a follower. Then `set` them to the master on their side. This means that they will be synced with the other motor in the gear box, and will behave in the exact same way.
```java
slaveLeft.changeControlMode(CANTalon.TalonControlMode.Follower);
slaveLeft.set(masterLeft.getDeviceID());
	
slaveRight.changeControlMode(CANTalon.TalonControlMode.Follower);
slaveRight.set(masterRight.getDeviceID());
```

Instantiate the `robotDrive` object, using the left and right masters.
```java
robotDrive = new RobotDrive(masterLeft, masterRight);
```

Set some safety controls for the `robotDrive`.
```java
robotDrive.setSafetyEnabled(true);
robotDrive.setExpiration(0.1);
robotDrive.setSensitivity(0.5);
robotDrive.setMaxOutput(1.0);
```

###driveByJoystick
The `robotDrive` object has a method called `arcadeDrive`, that takes in the amount the robot needs to move and how much it needs to rotate.
```java
public void driveByJoystick(double move, double rotate){
	SmartDashboard.putString("driveByJoystick?", move + "," + rotate);
	robotDrive.arcadeDrive(move, rotate);
}
```

###Encoder Info
The following methods work with the encoders to display information about distance traveled. These do not directly affect the driving capabilities of the robot, but rather provide details that may be useful for driving or testing.
```java
public void printEncoderValues() {
	getEncoderDistance();
}

public double getEncoderRight() {
	return -masterRight.getEncPosition();
}

public double getEncoderLeft() {
	return masterLeft.getEncPosition();
}

public double getEncoderDistance() {
	double numPulseLeft = getEncoderRight() - encOffsetValueLeft;
	double numPulseRight = getEncoderRight() - encOffsetValueRight;
		
	SmartDashboard.putNumber("Chassis Distance Right", (numPulseRight * RobotMap.DIST_PER_PULSE));
	SmartDashboard.putNumber("Chassis Distance Left", (numPulseLeft * RobotMap.DIST_PER_PULSE));
	
	return (numPulseRight) * RobotMap.DIST_PER_PULSE;
}

public void resetEncoderDistance() {
	encOffsetValueRight = getEncoderRight();
	encOffsetValueLeft = getEncoderLeft();
	getEncoderDistance();
}
```

###stop
Make sure to include a `stop` method. The robot will stop if the joystick is exactly center, but this is important to include if you ever want to terminate the drive by joystick functionality.
```java
public void stop() {
	robotDrive.drive(0, 0);
}
```
##Robot.java
Make sure to declare the chassis as a field of `Robot.java`
```java
public static Chassis chassis;
```
and instantiate it in the constructor.
```java
chassis = new Chassis();
```

##Command
The `DriveByJoystick` command requires the `chassis`.
```java
public DriveByJoystick() {
        requires(Robot.chassis);
}
```

The `execute` method gets the position of the joystick using the `OI` and passes it into the `driveByJoystick` method in the `chassis` subsystem.
```java
protected void execute() {
    	Robot.chassis.driveByJoystick
    		(Robot.oi.getDrivingJoystickY(), Robot.oi.getDrivingJoystickX());
}
```

The `end` method calls stop, making sure the joystick no longer controls the robot.
```java
protected void end() {
   	Robot.chassis.stop();
    	SmartDashboard.putBoolean("Drive by Joystick", false);
}
```

##OI.java
The `OI` declares and instantiates a joystick, using the port determined in `robotMap`.
```java
Joystick drivingStick = new Joystick(RobotMap.JOYSTICK_PORT);
```

The two methods `getDrivingJoystickY` and `getDrivingJoystickX` get the X and Y displacement of the joystick relative to the center. This is what determines that you've moved the joystick up or to the left. These values are fed through the `driveByJoystick` command into the `arcadeDrive` method of `robotDrive` in the `chassis` subsystem.

```java
public double getDrivingJoystickY() {
	return drivingStick.getY();
}

public double getDrivingJoystickX() {
	return drivingStick.getX();
}
```

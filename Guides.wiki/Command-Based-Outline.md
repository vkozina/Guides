**[Command Based Outline](https://github.com/CMUFeiyue/Guides/wiki/Command-Based-Outline)**
* **[Overview](Command-Based-Outline#overview)**
* **[Command Based Project Outline](Command-Based-Outline#command-based-project-outline)**
  * **[RobotMap](Command-Based-Outline#robotmap)**
  * **[Subsystems](Command-Based-Outline#subsystems)**
  * **[Robot](Command-Based-Outline#robot)**
  * **[Commands](Command-Based-Outline#commands)**
  * **[OI(Operator Interface)](Command-Based-Outline#oi-operator-interface)**
* **[Analogy](Command-Based-Outline#analogy)**
* **[The Scheduler](Command-Based-Outline#the-scheduler)**

##Overview
This guide goes over the general structure of a Command Based robot. It discusses the purposes of each portion of the robot and how it interacts with the rest of the system.

##Command Based Outline
Below is a brief overview of the 5 components to the Command based model which we will be using. We’ll go over each component in more detail (and with more code examples) later.

###RobotMap
- Specify which motors, sensors, and encoders the robot uses, and in which configuration they are plugged in. 
- Defines variables so the remainder of the code can use meaningful names instead of just port numbers. 
- For example, including `public static int leftMotor = 1` inside this class would allow you to reference the motor plugged into port 1 as `leftMotor`.

###Subsystems
- Most complicated component of the Command Based Robot
- Independent operating systems of the robot
- Different moving parts of the robot (eg. Chassis, elevator belt, pneumatic arms, etc.)
- Multiple Subsystems per program
- Extends Subsystem

###Robot
- Initializes all the subsystems for use by the commands.
- Initializing and Terminating the Autonomous and Teleoperated modes

###Commands
- The actions the robot will perform
- Calls on the subsystems necessary to complete the action
- Eg. (the command `driveForward` requires the subsystem `chassis`) 
- Multiple commands per program
- Extends Command class

###OI (Operator Interface)
- Declare and instantiate cause and effect of operator actions on the robot.

##Analogy
To better understand the purpose of each component, image you are a robot we're programming. So what now? 

- Well, a good example of a **subsystem** might be your arm. Each joint is like a motor – your shoulder, elbow and wrist. Your arm is what is responsible for those joints, and knows how to make them move in a particular way – raise the entire arm up, rotate your wrist, etc.
- The **robot**, or you, need to be aware that you have an arm so that you can use it. You also need to know where your arm is and how to communicate with it, which is what **robotMap** does.
- What if someone tosses you a ball? You think, I should move my arm in this way to catch the ball, and your brain sends that message to your arm to execute. You need a **command** (maybe "move arm"), telling the arm to raise the entire arm up and then rotate your wrist.
- Before they throw the ball, they might say, "catch!". This is the signal for your brain to start your "move arm" command to get your arm in place. This is kind of like the **OI** reacting to a button press.

##The Scheduler
The [FRC Control System Website](https://wpilib.screenstepslive.com/s/4485) has a [scheduling commands](https://wpilib.screenstepslive.com/s/4485/m/13809/l/599745-scheduling-commands) article that details the `Scheduler` construction and what gets run when your robot is enabled. The robot has four modes &ndash; autonomous enabled, teleoperated enabled, test enabled and disabled. During any of the enabled modes, the appropriate `init` and `periodic` methods are called from `Robot.java`. For example, when enabling autonomous mode, the robot will first run `autonomousInit` once, and then proceed to call `autonomousPeriodic` at regular time intervals. The call to `Scheduler.getInstance().run()` cycles through all the current commands, runs their `execute` method, and then checks `isFinished`. If `isFinished` returns true, the command gets removed from the list of current commands. If no currently running command requires a particular subsystem (for example nothing is using the robot arm), the `default` command for that subsystem (declared in `initDefaultCommand` within the subsystem) is added to the scheduler.
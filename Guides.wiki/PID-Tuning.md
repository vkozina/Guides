##Overview
This guide walks through the use of PID tuning for beginners. It covers general concepts of PID control, as well how to write custom subsystems to allow for easy tuning. There are two projects outlined, both of which can be found in the [Feiyue_2016](https://github.com/CMUFeiyue/Feiyue_2016) repository. [PIDGearBox](https://github.com/CMUFeiyue/Feiyue_2016/tree/master/PIDGearBox) is an example subsystem integrating PID control, and [PIDTuner](https://github.com/CMUFeiyue/Feiyue_2016/tree/master/PIDTuner) allows for the graphing of responses.

##Background
PID control can be used to maintain consistency and predictability of motor movements. This is especially useful during autonomous mode when there is no driver to correct for drifting in the drive train. There are many resources available to gain a better understanding of PID loops and their purpose. The following youtube videos do a great job of explaining both the theoretical and mathematical sides of PID control.
- [Controlling Self Driving Cars](https://www.youtube.com/watch?v=4Y7zG48uHRo): practical example of a PID loop and its use in self-driving cars.
- [PID Control - A brief introduction](https://www.youtube.com/watch?v=UR0hOmjaHp0): a more detailed description of all the terms and their mathematical meanings.
- [Understanding PID Loops](https://www.youtube.com/watch?v=l03SioQ9ySg): a good demo and intuitive discussion of the purpose of each term.
- [East Bay Guide to PID Loops, Part 2: The math and the coding](https://www.youtube.com/watch?v=sDd4VOpOnnA): a more in-depth explanation of the math behind the error calculations, as well as example code for implementation.

##Dependencies
There are a few external libraries that are necessary for these projects. Before moving forward, download [JFreeChart](http://www.jfree.org/jfreechart/download.html) and a jar of the [NetworkTables-desktop](http://first.wpi.edu/FRC/roborio/maven/release/edu/wpi/first/wpilib/networktables/java/NetworkTables/3.0.0-SNAPSHOT/NetworkTables-3.0.0-20160211.210246-4-desktop.jar). At the time of writing, the links were to the most recent versions of both libraries, however it may be necessary to explore the [full page of libraries] (http://first.wpi.edu/FRC/roborio/maven/release/edu/wpi/first/wpilib/networktables/java/NetworkTables/3.0.0-SNAPSHOT/) or the [Maven Artifacts screensteps article](https://wpilib.screenstepslive.com/s/4485/m/wpilib_source/l/480976-maven-artifacts) for more up-to-date resources. Any version of the NetworkTables-desktop jar should be sufficient.

Make sure to update the build path of the PID Tuning project before continuing.

##PIDGearBox
###RobotMap.java
package org.usfirst.frc.team3504.robot;

public class RobotMap {
	public static final int DRIVE_MASTER = 1;
	public static final int DRIVE_SLAVE = 2;
	
	public static final int JOYSTICK_PORT = 0;
}


###Subsystems
####PIDGearBox

package org.usfirst.frc.team3504.robot.subsystems;

import org.usfirst.frc.team3504.robot.RobotMap;
import edu.wpi.first.wpilibj.CANTalon;
import edu.wpi.first.wpilibj.livewindow.LiveWindow;

public class PIDGearBox extends TunablePIDSubsystem {

	private CANTalon driveMaster;
	private CANTalon driveSlave;
	
	public PIDGearBox(String name, double kP, double kI, double kD, double f) {
		super(name, kP, kI, kD, f);
	}
	
	public PIDGearBox() {
		this("GearBox", 0.1, 1, 0, 0);
    	
    	driveMaster = new CANTalon(RobotMap.DRIVE_MASTER);
		driveSlave = new CANTalon(RobotMap.DRIVE_SLAVE);
		
		driveSlave.changeControlMode(CANTalon.TalonControlMode.Follower);
		driveSlave.set(driveMaster.getDeviceID());
		
     	setInputRange(-1, 1);
    	setPercentTolerance(10);
    	getPIDController().setContinuous(false);
    	
    	LiveWindow.addActuator(this.toString(), "PID Controller", getPIDController());
    }
    
    public void initDefaultCommand() {
    
    }
    
    protected double returnPIDInput() {
    	return driveMaster.getSpeed();
    }
    
    protected void usePIDOutput(double output) {
    	driveMaster.pidWrite(output);
    }
    
    public void stop() {
    	driveMaster.set(0);
    }
}

####TunablePIDSubsystem.java

package org.usfirst.frc.team3504.robot.subsystems;

import org.usfirst.frc.team3504.robot.RobotMap;
import edu.wpi.first.wpilibj.CANTalon;
import edu.wpi.first.wpilibj.livewindow.LiveWindow;

public class PIDGearBox extends TunablePIDSubsystem {

	private CANTalon driveMaster;
	private CANTalon driveSlave;
	
	public PIDGearBox(String name, double kP, double kI, double kD, double f) {
		super(name, kP, kI, kD, f);
	}
	
	public PIDGearBox() {
		this("GearBox", 0.1, 1, 0, 0);
    	
    	driveMaster = new CANTalon(RobotMap.DRIVE_MASTER);
		driveSlave = new CANTalon(RobotMap.DRIVE_SLAVE);
		
		driveSlave.changeControlMode(CANTalon.TalonControlMode.Follower);
		driveSlave.set(driveMaster.getDeviceID());
		
     	setInputRange(-1, 1);
    	setPercentTolerance(10);
    	getPIDController().setContinuous(false);
    	
    	LiveWindow.addActuator(this.toString(), "PID Controller", getPIDController());
    }
    
    public void initDefaultCommand() {
    
    }
    
    protected double returnPIDInput() {
    	return driveMaster.getSpeed();
    }
    
    protected void usePIDOutput(double output) {
    	driveMaster.pidWrite(output);
    }
    
    public void stop() {
    	driveMaster.set(0);
    }
}

###Robot.java
There are a few necessary changes to `robot.java`, which are detailed below. The remaining components can remain the default.

`robotInit` needs to create an instance of the PIDSubsystem that needs to be tuned, initializes the `NetworkTable` so that it writes to a location that matches the one in PIDTuner and sets the `startCommand` value to start at `false`.
```java
driveTrain = new PIDGearBox();
table = NetworkTable.getTable("PID");
table.putBoolean("startCommand", false);
```

`teleopPeriodic` checks the current value of `startCommand` from the `NetworkTable`. If it is currently true, that means that the PIDTuner would like to run the test with the inputed set of values. To do this, a new `TunePID` command is created, with the same type as the subsystem initialized previously and that subsytem as input. `startCommand` is immediately switched back to false to ensure that only one command gets created when the signal is delivered. The command is then started and the scheduler run.
```java
boolean startCommand = table.getBoolean("startCommand", false);
    	
if(startCommand) {
	pidCommand = new TunePID<PIDGearBox>(Robot.driveTrain);
    	table.putBoolean("startCommand", false);
    	pidCommand.start();
}
    	
Scheduler.getInstance().run();
```

###Commands

The class declaration specifies that the `TunePID` class must be created with a specified type, in this case the type must extend `TunablePIDSubsystem`. Because of this, the `TunePID` command allows any `TunablePIDSubsystem` to be tuned with no additional effort. Basically, there are certain proceedures that are necessary for a subsystem to have in order to properly interface with the `PIDTuner`. All of these proceedures are included as required functions for a `TunablePIDSubsystem`, therefore ensuring that nothing about `TunePID` must be altered to use a different subsystem. 
```java
public class TunePID<T extends TunablePIDSubsystem> extends Command {
```

The `PIDTuner` stores the array of read-off times and values, as well as the `PIDSubsytem` that was given as the argument when the command was first created. This is necessary to be able to interface with the proper subsytem.
```java
private ArrayList<Double> values;
private ArrayList<Double> times;
private T PIDSubsystem;
private NetworkTable table = NetworkTable.getTable("PID");
	
public final static Logger log = Logger.getLogger(TunePID.class.getName());	


public TunePID (T PIDSubsystem) {
    	this.requires(PIDSubsystem);
	log.setLevel(Level.ALL);
	this.PIDSubsystem = PIDSubsystem;
}
```

When the command first gets created, the PID values and timeout get read from the `NetworkTable` and updated. This means that the command will only run for the amount of time, with the new PID constants, specified in `PIDTuner`.
```java
protected void initialize() {
	log.info("init");
    	
    	values = new ArrayList<Double>();
    	times = new ArrayList<Double>();
    	
    	PIDSubsystem.updatePIDValues(table);

    	double timeout = table.getNumber("timeout", 0);

    	this.setTimeout(timeout);
		log.info("running");
}
```

A new setpoint is created. Note that the `PIDGearBox` `PIDSubsystem` tries to tune the speed of the motor, so in this case the value should be in terms of desired motor speed. Although this is the most general use-case, this value may need to get changed for further testing or other applications. It may be worthwhile to include this as another editable field in `PIDTuner`. As the command executes, it logs the times and error values read off from the subsystem.
```java
protected void execute() {
    	PIDSubsystem.setSetpoint(50);
    	PIDSubsystem.enable();
    	
    	double value = PIDSubsystem.getPIDController().getError();
    	values.add(value);
    	
    	double time = timeSinceInitialized();
    	times.add(time);
    	
    	PIDSubsystem.printInfo();
}

This ensures that the command stops when the specified amount of time has gone by.
```java
protected boolean isFinished() {
	return isTimedOut();
}
```

When the command times out, the PID control is disabled and the motion stopped. The times and errors read during execution are all sent to the `NetworkTable` to be read by the `PIDTuner`. The boolean value `doneCommand` is marked as true, to indicate to the `PIDTuner` that the test terminated and the data is ready to be plotted. 
```java
protected void end() {
	log.info("done");
    	PIDSubsystem.disable();
    	PIDSubsystem.stop();
    	
    	Double[] valuesArray = values.toArray(new Double[values.size()]);
    	Robot.table.putNumberArray("values", valuesArray);
    	
    	Double[] timesArray = times.toArray(new Double[times.size()]);
    	Robot.table.putNumberArray("times", timesArray);
		
    	PIDSubsystem.printPIDValues(table);
    	
    	table.putBoolean("doneCommand", true);
	log.info("" + PIDSubsystem.onTarget());
}    	
```

Interruptions are ignored.
```java
protected void interrupted() {

}
```

###IO.java
Since no joystick control is used, this file can be left with its default components.

##PIDTuner
package tuner;

import java.awt.*;
import javax.swing.*;

import org.jfree.chart.ChartFactory;
import org.jfree.chart.ChartPanel;
import org.jfree.chart.ChartUtilities;
import org.jfree.chart.JFreeChart;
import org.jfree.chart.plot.PlotOrientation;
import org.jfree.data.category.DefaultCategoryDataset;

import edu.wpi.first.wpilibj.networktables.NetworkTable;

import java.awt.event.*;
import java.io.File;
import java.io.IOException;
import java.util.HashMap;

public class PIDTuner extends JFrame
{
	//Size of the ui window
	private static final int WINDOW_WIDTH = 1000;
	private static final int WINDOW_HEIGHT = 1000;
	
	//Determines how this program connects to a roborio
	private static final int TEAM_NUMBER = 9999;
	
	//Extra time to wait before aborting runMotor and assuming error (in seconds)
	private static final int TIME_BUFFER = 1;
	
	private static final Font font = new Font("Verdana", Font.BOLD, 20);

	private static NetworkTable table;
	
	//UI components
	private HashMap<String, JTextField> values;
	private JPanel buttonPane;
	private Container contentPane;
	private ChartPanel graphPane;
	
	private JFreeChart graph;
	
	public PIDTuner() {
		
		setTitle("PID Tuner");
		
		/*
		 * Create the button pane to display all the selections
		 */
		buttonPane = new JPanel();
		buttonPane.setLayout(new BoxLayout(buttonPane, BoxLayout.LINE_AXIS));
		buttonPane.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
    	buttonPane.add(Box.createHorizontalGlue());
    	buttonPane.add(Box.createRigidArea(new Dimension(10, 0)));
		
		addButton("Run Motor", new MotorButtonHandler());
		addButton("Save", new SaveButtonHandler());
		addButton("Exit", new ExitButtonHandler());

		
		/*
		 * Create the graph pane to contain the plot of data
		 */
		graph = createGraph();
		graphPane = new ChartPanel(graph);
        
		
		/*
		 * Create panel to receive user input for variables such as kP, kI and kD
		 * These variables match the ones a TunablePIDSubsystem will use as input
		 */
		JPanel valuePane = new JPanel();
		valuePane.setLayout(new BoxLayout(valuePane, BoxLayout.LINE_AXIS));
		valuePane.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20));

		values = new HashMap<String, JTextField>();		
		String [] valueNames = {"kP", "kI", "kD", "f", "timeout"};
		JLabel tempLable;
		JTextField tempField;
		
		for(String name : valueNames) {
			tempLable = new JLabel(name + ": ", SwingConstants.RIGHT);
			tempField = new JTextField(10);
			values.put(name, tempField);
			valuePane.add(tempLable);
			valuePane.add(tempField);
		}
			

		/*
		 * Combine all the separate panels into a single window
		 */
		contentPane = getContentPane();
		contentPane.add(valuePane, BorderLayout.PAGE_START);
		contentPane.add(graphPane, BorderLayout.CENTER);
		contentPane.add(buttonPane, BorderLayout.PAGE_END);
		
		// Set the font of all components to the desired one, otherwise text is tiny
		setPanelFont(contentPane.getComponents(), font);
		
        contentPane.validate();
        
        // Display the window
		setSize(WINDOW_WIDTH, WINDOW_HEIGHT );
		setVisible(true);
		setDefaultCloseOperation(EXIT_ON_CLOSE);
	}
	
	/*
	 * Returns a graph based on the data from createDataset();
	 */
	private JFreeChart createGraph() {
		String graphTitle = "Error vs Time";
		String xLable = "Time";
		String yLable = "Error";
		
		return ChartFactory.createLineChart(
				graphTitle, xLable, yLable,
				createDataset(), 
				PlotOrientation.VERTICAL,
				true,true,false);
	}
	
	/*
	 * Facilitates adding buttons to the bottom of the window
	 * Requires there be a handler for the button that will perform the desired action
	 */
	private void addButton(String name, ActionListener handler) {
		JButton button = new JButton(name);
		button.addActionListener(handler);
		buttonPane.add(button);
	}
	
	/*
	 * Handles a click of the "Run Motor" button
	 * Reads off values of controllable variables and pass them on to the roborio
	 * Reads off resulting measurements taken by the roborio after the motor is run
	 * Regraphs the data in the center of the window and stores the graph
	 */
	private class MotorButtonHandler implements ActionListener {
		public void actionPerformed(ActionEvent e) {
			//Replace original graph with a loading screen to prevent confusion
			contentPane.remove(graphPane);
			JLabel loading = new JLabel("Loading...");
			contentPane.add(loading, BorderLayout.CENTER);
			
			runMotor();
			
			double startTime = System.currentTimeMillis();
			double runTime = (table.getNumber("timeout", 0) + TIME_BUFFER) * 1000;
			double endTime = startTime + runTime;
			
			//While the robot is running, with a max time to prevent infinite loops
			while(!table.getBoolean("doneCommand", false) && 
					System.currentTimeMillis() < endTime) {
				//do nothing
			}
			
			//If loop ended because the robot actually finished, graph new data
			if(table.getBoolean("doneCommand", false)) {	
				//Make a new graph based on the data collected from roborio
				graph = createGraph();
				ChartPanel graphPane = new ChartPanel(graph);
				contentPane.add(graphPane, BorderLayout.CENTER);
		        contentPane.validate();
		        
		        //Reset the value of doneCommand for next time
		        table.putBoolean("doneCommand", false);
			}
		}
		
		/* 
		 * Sets the flag in the table that indicates to the motor it needs
		 * to create a new TunePID command.
		 */
		public void runMotor() {
			for(String name : values.keySet()) {
				String textBox = values.get(name).getText();
				double value;
				
				if(! textBox.equals("")) {
					//If the value has been changed in the text box, update it
					value = Double.parseDouble(textBox);
				} else {
					//Otherwise take the old value from the roborio table
					value = table.getNumber(name, 0);
				}
				
				table.putNumber(name, value);
			}
			
			table.putBoolean("startCommand", true);
		}
	}
	
	/*
	 * Opens a filechooser, letting the user select where to save the file
	 * as well as what to call it. Remembers previous location if this is not
	 * the first file saved.
	 * Saves the stored graph at that location
	 */
	public class SaveButtonHandler implements ActionListener {
		private File chartPath = new File("");
		public void actionPerformed(ActionEvent e) {
			JFileChooser fileChooser = new JFileChooser(chartPath);
			fileChooser.setPreferredSize(new Dimension(1000, 500));
			setPanelFont(fileChooser.getComponents(), font);	    

			int returnVal = fileChooser.showSaveDialog(fileChooser);
			
			String path = "";
			if(returnVal == JFileChooser.APPROVE_OPTION) {
				path = fileChooser.getSelectedFile().getAbsolutePath();
			}

			int width = 640; 
			int height = 480;
			chartPath = new File(path + ".jpeg"); 
			
			try {
				ChartUtilities.saveChartAsJPEG(chartPath, graph, width ,height);
			} catch (IOException e1) {
				e1.printStackTrace();
			}
		}
	}
	
	/*
	 * Exits the program
	 */
	public class ExitButtonHandler implements ActionListener {
		public void actionPerformed(ActionEvent e) {
			System.exit(0);
		}
	}
	
	/* 
	 * Sets up the connection to the robot
	 * Starts the window and button listeners
	 */
	public static void main(String[] args) {
		NetworkTable.setClientMode();
		NetworkTable.setIPAddress("roborio-" + TEAM_NUMBER + "-frc.local");
		table = NetworkTable.getTable("PID");
		PIDTuner rectObj = new PIDTuner();
	}
	
	/*
	 * Reads the data from the roborio and creates a graphable dataset from it
	 */
	public static DefaultCategoryDataset createDataset() {	
		double[] tempArray = {0};
		double[] values = table.getNumberArray("values", tempArray);
		double[] times = table.getNumberArray("times", tempArray);
		
		double temp = 0;
		double kP = table.getNumber("kP", temp);
		double kD = table.getNumber("kD", temp);
		double kI = table.getNumber("kI", temp);
		double f = table.getNumber("f", temp);
		String PIDInfo = ("kP: " + kP + ", kD: " + kD + ", kI: " + kI + ", f: " + f);
		

		DefaultCategoryDataset dataset = new DefaultCategoryDataset( );
		for(int i = 0; i < times.length; i++ ) {
			double time = times[i];
			double value = values[i];
			dataset.addValue( value , PIDInfo , time + "" );
		}
		
		return dataset;
	}
	
	/*
	 * Recursively updates the font of each component to be the desired one
	 */
	public static void setPanelFont(Component[] comp, Font font) {
		for(int x = 0; x < comp.length; x++) {
			if(comp[x] instanceof Container) {
				setPanelFont(((Container)comp[x]).getComponents(), font);
			}
			
			try {
				comp[x].setFont(font);
			}
			catch(Exception e){ }
		}
	}
}

[[https://github.com/CMUFeiyue/Guides/blob/master/images/pidtuner/ui_example.PNG]]


**[The Camera Project](The-Camera-Project)**
* **[Overview](The-Camera-Project#overview)**
  * **[Subsystems](The-Camera-Project#subsystems)**
  * **[Commands](The-Camera-Project#commands)**
* **[RobotMap.java](The-Camera-Project#robotmapjava)**
* **[CameraManager](The-Camera-Project#cameramanager)**
  * **[Constructor](The-Camera-Project#constructor)**
  * **[openCamera](The-Camera-Project#opencamera)**
  * **[getImage](The-Camera-Project#getimage)**
* **[Vision Processor](The-Camera-Project#visionprocessor)**
  * **[Helper Classes](The-Camera-Project#helper-classes)**
    * **[CameraType Enum](The-Camera-Project#cameratype-enum)**
    * **[Target Class](The-Camera-Project#target-class)**
    * **[Particle Class](The-Camera-Project#particle-class)**
  * **[VisionProcessor Init](The-Camera-Project#visionprocessor-init)**
  * **[Vision Pipeline](The-Camera-Project#vision-pipeline)**
    * **[resizeImage](The-Camera-Project#1-resizeimage)**
    * **[thresholdImage](The-Camera-Project#2-thresholdimage)**
    * **[identifyParticles](The-Camera-Project#3-identifyparticles)**
    * **[drawParticleBox](The-Camera-Project#4-drawparticlebox)**
* **[UpdateCam](The-Camera-Project#updatecam)**


##Overview
This guide walks through the [Camera](https://github.com/CMUFeiyue/Feiyue_2016/tree/master/Camera) project. It explains all the code, as well as how it could be extended if necessary.

###Subsystems
There are two subsystems – the `CameraManager` and `VisionProcessor`. 

The `CameraManager` is responsible for managing the feeds from all the cameras on the robot. This means it opens all the feeds, and gets frames when necessary. The `CameraManager` does not care what happens to the frame once it is grabbed, and can pass it to any number of locations.

The `VisionProcessor` is responsible for all the operations performed on a raw image, such as resizing, thresholding and blurring. The `VisionProcessor` does not care where the image it needs to process comes from, and can perform these operations on any number of images.

###Commands
`UpdateCam` is the command that ties together `CameraManager` and `VisionProcessor` by taking the images returned from `CameraManager` and forwarding them onto the necessary functions in `VisionProcessor`.

##RobotMap.java
First, the logger logs that it will log the activity in the `RobotMap`. Then, within the constructor, the logger is informed to log all levels of events. 

```java
public final static Logger log = Logger.getLogger(RobotMap.class.getName());
	
public RobotMap() {
	log.setLevel(Level.ALL);
}
```

Next, the camera is set to `cam0`, which is how the roborio will view the camera. However, within the code, we can call cam0 by the name `Camera.MY_CAMERA``.

```java
public enum Camera {
	MY_CAMERA(“cam0");
```

An invalid camera is given the value -1, and the `string systemName` and `int streamDescriptor` are declared.
```java
public static final int INVALID_CAM = -1;
protected final String systemName;
protected int streamDescriptor;
```

Then, the camera is set as `systemName`, and the descriptor is set as `INVALID_CAM`.

```java
Camera(String systemName) {	
    	this.systemName = systemName;
    	this.streamDescriptor = INVALID_CAM;
}
```

The `systemName` function returns the system name of the camera

```java
public String systemName()	{
 	return systemName;	
}
```

The `setStreamID` function takes in a `steamNum` and sets the cameras `streamDescriptor` to it. You can use this function to open cameras. `getStreamID` returns the current value of the cameras streamDescriptor.

```java
public void setStreamID(int streamNum) {
    	this.streamDescriptor = streamNum;
}
    	
public int getStreamID() {
	return streamDescriptor;
}
```

##CameraManager

###Constructor
Within the `CameraManager` constructor, written like this:

```java
    public CameraManager()
```

the logger is activated to log anything that happens with any level of severity, including the initialization of the camera, within the following lines:

```java
    log.setLevel(Level.ALL);
    log.info("Camera init”);
```

Then, for every value of the `Camera` enum, the robot tries to open the camera.
```java
for(RobotMap.Camera camera : RobotMap.Camera.values()) {
	try {
		openCamera(camera);
	}	
}
```

However, if the camera is not given a value, an exception is thrown and the logger logs that the camera was unable to open, along with which camera it tried to open. 

```java
catch (Exception ex) {
	log.severe("Failed to open the camera " + camera.toString());
}
```

###openCamera

To open a camera, the camera’s name is returned and set to the `streamID`, along with the camera control mode controller. Then, the opening of the camera itself is logged. Next, the robot takes the actual camera stream and begins acquiring and gathering information from the stream. The logger then logs that the camera has been configured.

```java
public void openCamera(RobotMap.Camera camera) {
	int streamID;
	streamID = NIVision.IMAQdxOpenCamera(camera.systemName(), 
		NIVision.IMAQdxCameraControlMode.CameraControlModeController);
	camera.setStreamID(streamID);
	log.info(camera.toString() + " opened");
				
	NIVision.IMAQdxConfigureGrab(streamID);				
	NIVision.IMAQdxStartAcquisition(streamID);
	log.info(camera.toString() + " configured");
}
```

###getImage
`getImage` takes in a `RobotMap.Camera`
```java
public Image getImage(RobotMap.Camera camName)
```

If the `streamID` of the given camera is not `INVALID_CAM` (meaning the camera was successfully opened), a frame is grabbed from the camera and returned.
```java
int streamID = camName.getStreamID();

//If the camera does not have a valid stream ID, try again to open it
if(streamID != RobotMap.Camera.INVALID_CAM) {
	openCamera(camName);	
}
		
//Make sure the robot does not crash if something goes wrong with acquiring image
try {
	NIVision.IMAQdxGrab(streamID, frame, 1);
	return frame;
}
```

Otherwise an error is logged and `NULL` is returned.
```java
catch(Exception e) {
	log.warning(camName.toString() + " is not open");
	return null;
}
```

##VisionProcessor
The `VisionProcessor` runs a variety of NIVision functions on a given images, and returns the results. You can find the API for all the functions [here](TODO: fix this link).

###Helper Classes
`VisionProcessor` contains a number of helper classes to store information in a more intuitive way. They are included at the bottom of the code for reference, but are explained here first to better introduce the structure of code.

####CameraType Enum
The `CameraType` enum allows for quickly switching between models of cameras, without editing a lot of code. If any camera-specific attributes are found, they can be added as another field of the enum. For example, when calculating the real-world distance to an object in an image, the focal length of the camera is required. This will probably be different for each camera. Adding a `protected final double focalLength` field to the enum would let you keep track of all the values. More importantly, you can then reference `CameraType.AXIS_M10011.focalLength` instead of leaving "magic" numbers in your code. Enums work very similar to classes, and you can modify the new fields scope or add methods to interact with them. For example you could add a `public double getFocalLength()` function to allow outside classes use of the `focalLength` field.

```java
public enum CameraType {
	AXIS_M10011 (49.4), 
	AXIS_M1013 (64), 
	AXIS_206 (51.7), 
	HD3000_SQR (52), 
	HD3000_640X480 (60);
    	
    	//View angle of camera, changes depending on camera used.
    	protected final double viewAngle;
    	CameraType(double viewAngle) {	this.viewAngle = viewAngle;		}
    	private double viewAngle()	{	return viewAngle;	}
}
```


####Target Class
`Target` is a helper class that stores attributes of objects of interest. 

Each target contains an `id` that is unique to it, and a `targetName` that makes sense to users. The ids are determined by the program by keeping a counter of all targets created and assigning the current number to a new target. This value should not be set anywhere other than the constructor. The `minPercentArea` dictates what percentage of the image area a particle must take up in order to qualify to be the target. If the target is large or you only want to recognize it when you are close by, set the value higher and it will ignore all particles that are "too small". A value of 0.5 means that a particle must take up at least half the image to be the target. The `minScore` addresses how close a match the found particle must be to be considered. The `hueRange`, `satRange`, and `valRange` fields store the pixel values that count as part of the target. If you're looking for something green, the `hueRange` might be around 100-140, since the hue for pure green is 120. A dull green would have a value closer to 50, whereas a bright green would be 100. These values must be determined experimentally to properly discern pixels of the target.
```java
int id;
String targetName;
double minPercentArea;
double minScore;
NIVision.Range hueRange, satRange, valRange;
```


The constructor simply takes in all the relevant values and stores them in the proper fields. It creates a `hueRange` of `hueMin - hueMax` and so on.
```java
public Target(String targetName, double minPercentArea, double minScore, 
				int hueMin, int hueMax, int satMin, int satMax, int valMin, int valMax)
```


There is also a default constructor for calibration purposes.
```java
public Target() {
	this("Default", 0.5, 0.5, 0, 255, 0, 255, 0, 255);
}
```


The descriptor of a target is the combination of it's unique ID and the more descriptive name.
```java
public String getDescriptor() {
	return (this.id + ". " + this.targetName + " ");
}
```


Equality is determined by descriptors and no targets are the same unless their ids are the same.
```java
public boolean equals(Target other) {
	return this.getDescriptor().equals(other.getDescriptor());
}
```


`displayRanges` and `calibrateRanges` interface with the SmartDashboard by either outputing all the information about the stored ranges, or modifying the ranges based on user input to the dashboard.
```java
public void displayRanges() {
	SmartDashboard.putNumber(this.getDescriptor() + " hue min", hueRange.minValue);
	SmartDashboard.putNumber(this.getDescriptor() + " hue max", hueRange.maxValue);
	SmartDashboard.putNumber(this.getDescriptor() + " sat min", satRange.minValue);
	SmartDashboard.putNumber(this.getDescriptor() + " sat max", satRange.maxValue);
	SmartDashboard.putNumber(this.getDescriptor() + " val min", valRange.minValue);
	SmartDashboard.putNumber(this.getDescriptor() + " val max", valRange.maxValue);		
}

public void calibrateRanges() {
	hueRange.minValue = (int)SmartDashboard.getNumber(this.getDescriptor() + " hue min", hueRange.minValue);
	hueRange.maxValue = (int)SmartDashboard.getNumber(this.getDescriptor() + " hue max", hueRange.maxValue);
	satRange.minValue = (int)SmartDashboard.getNumber(this.getDescriptor() + " sat min", satRange.minValue);
	satRange.maxValue = (int)SmartDashboard.getNumber(this.getDescriptor() + " sat max", satRange.maxValue);
	valRange.minValue = (int)SmartDashboard.getNumber(this.getDescriptor() + " val min", valRange.minValue);
	valRange.maxValue = (int)SmartDashboard.getNumber(this.getDescriptor() + " val max", valRange.maxValue);
}
```

####Particle Class
The `Particle` class stores information about a group of pixels found during processing. It contains a `percentArea`, which is the percent of the image it takes up. An `area` stores the actual number of pixels, without any relation to the image size. The values of `x_coord` and `y_coord` describe a point (x, y) that is the upper left corner of the bounding box of the particle. (0, 0) is considered the upper left corner of the image. `width` and `height` are the dimensions of the bounding box, extending right and down from the (x, y) point.

```java
double percentArea;
double area;
int x_coord;
int y_coord;
int width;
int height;
```

A constructor exists to initialize all the values.
```java
public Particle(double percentArea, double area, int x, int y, int width, int height)
```

Another form of the constructor takes in an array of all the attributes in the appropriate order and calls the first constructor with the according values.
```java
public Particle(double[] properties) {
			this(properties[0], 
				properties[1], 
				(int) properties[2], 
				(int) properties[3], 
				(int) properties[4], 
				(int) properties[5]);
}
```

Comparing particles depends on their area, and a larger particle is considered greater than a smaller one.
```java
public int compareTo(Particle r) {
	return (int)(r.area - this.area);
}

public int compare(Particle r1, Particle r2) {
	return (int)(r1.area - r2.area);
}
```

###VisionProcessor Init

The VisionProcessor stores a list of all targets, as well as a count of how many there are. The count is used to assign unique IDs to new targets. It also creates a logger for the class.
```java
static int targetCount = 0;
ArrayList<Target> targets = new ArrayList<Target>();
public final static Logger log = Logger.getLogger(VisionProcessor.class.getName());
```

The constuctor selects which camera is being used from the enum. From here on out you can use camType to refer to camera-specific attributes, making switching cameras a one-line operation.
```java
CameraType camType = CameraType.AXIS_M10011;
```

It also sets up logging.
```java	
log.setLevel(Level.ALL);
```

The constructor is also where you can define new targets. Two examples exist below. All new targets get added to `targets` so they can be referenced later.
```java
Target redBox = new Target("Red Box", 0.5, 0, 40, 255, 0, 80, 0, 80);
Target yellowCard = new Target("Yellow Card", 0.5, 40, 80, 100, 0, 80, 0, 80);
targets.add(redBox);
targets.add(yellowCard);
}
```

###Vision Pipeline
The example pipeline included in the code has several steps.

1. [`resizeImage`](The-Camera-Project#1-resizeimage): Scales the image down
2. [`thresholdImage`](The-Camera-Project#2-thresholdimage): Thresholds the image based on the attributes of a provided target
3. [`identifyParticles`](The-Camera-Project#3-identifyparticles): Finds all the relevant particles in the image
4. [`drawParticleBox`](The-Camera-Project#4-drawparticlebox): Draws a bounding box around the largest particle found

The `findTarget` method runs this pipeline with a given image and target. The individual steps are explained below.
```java
public Image findTarget(Image inputImg, Target target) {
	Image scaledImg = resizeImage(inputImg, 2, 2);
	Image thresholdedImg = thresholdImage(scaledImg, target);
	ArrayList<Particle> particles = identifyParticles(thresholdedImg, target);
	Image boxImg = drawParticleBox(inputImg, particles);
	return boxImg;
}
```

####1. resizeImage
`resizeImage` takes in an image, a scale for the x dimension and a scale for the y dimension.
```java
public Image resizeImage(Image inputImg, int xscale, int yscale)
```

It first creates a new `NIVision Image` in which to store the result and calls it `scaledImg`. Note that the `ImageType` of this image must be RGB.

```java
Image scaledImg = NIVision.imaqCreateImage(ImageType.IMAGE_RGB, 0);
```

It finds the size of the `inputImg` using `NIVision.imaqGetImageSize`.
```java
NIVision.GetImageSizeResult inputImgSize = NIVision.imaqGetImageSize(inputImg);
```

It uses the `inputImgSize` in order to create a new `NIVision Rect`, `imageRect` that will establish which portion of the original image to resize. If the `imageRect` is created with dimensions smaller than the original image, the image will be cropped prior to resizing. For example `NIVision.Rect imageRect = new NIVision.Rect(0, 0, 50, 100)` would result in only the top 50 and leftmost 100 pixels to be used from here on out.

```java
NIVision.Rect imageRect = 
		new NIVision.Rect(0, 0, inputImgSize.height, inputImgSize.width);
```

It then calls `imaqScale` with the appropriate parameters. Note that `NIVision.ScalingMode.SCALE_SMALLER` is what dictates that the image should be scaled **down** by the `xscale` and `yscale` provided. Using `NIVision.ScalingMode.SCALE_LARGER` would result in the original dimensions being multiplied by the scales, rather than divided.

```java
NIVision.imaqScale(scaledImg, inputImg, xscale, yscale, 
		NIVision.ScalingMode.SCALE_SMALLER, imageRect);
```

Lastly, it returns the resulting image.
```java
return scaledImg;
```

####2. thresholdImage
`thresholdImage` takes in an image and a target of interest.
```java
public Image thresholdImage(Image inputImg, Target target)
```

It first creates a new `NIVision Image` in which to store the result and calls it `thresholdedImg`. Note that the `ImageType` of this is `U8`, which means it is a binary (black and white) image.

```java
Image thresholdedImg = NIVision.imaqCreateImage(ImageType.IMAGE_U8, 0);
```

Then, `imaqColorThreshold` gets called with the HSV ranges of the target. The third parameter (255) is the `replaceValue`, in this case black. This means that any pixel that doesn't fit into the ranges provided should get set to black. This value can be anything, but is typically left at black to end up with a binary (black and white) image. `NIVision.ColorMode.HSV` dictates that HSV ranges are being used. It is also possible to use `NIVision.ColorMode.RGB`, however HSV tends to be more precise when working with computer vision and is prefered.

```java
NIVision.imaqColorThreshold(thresholdedImg, inputImg, 255, 
		NIVision.ColorMode.HSV, target.hueRange, target.satRange, target.valRange);
```

Lastly, it returns the resulting image.
```java
return thresholdedImg;
```

####3. identifyParticles
`identifyParticles` takes in an image and a target of interest.
```java
public ArrayList<Particle> identifyParticles(Image inputImg, Target target) 
```

It first creates a new `NIVision Image` in which to store the result and calls it `filteredImg`. Note that the `ImageType` of this is `U8`, which means it is a binary (black and white) image.

```java
Image filteredImg = NIVision.imaqCreateImage(ImageType.IMAGE_U8, 0);
```

It creates a new array of filter criteria with only one element.
```java
NIVision.ParticleFilterCriteria2 filterCriteria[] = new NIVision.ParticleFilterCriteria2[1];
```

It creates the only element in the `filterCriteria` array. It dictates that the parameter being used is the percent area of the particle with respect to the whole image using `NIVision.MeasurementType.MT_AREA_BY_IMAGE_AREA`. It also passes in the targets `minPercentArea` as the lower bound to the range of accepted area percentages. It uses 100 as an upper bound, so particles cannot be "too large". 
The last two paraterers, both zero, mean that he measurement is in uncalibrated pixels (instead of real-world values) and that particles between the range should be **included**. If the last parameter was a 1, that would mean that all values between the range should be excluded and only particles smaller than `target.minPercentArea` or larger than 100% of the image area should be condisidered.
```java
filterCriteria[0] = new NIVision.ParticleFilterCriteria2
	(NIVision.MeasurementType.MT_AREA_BY_IMAGE_AREA, target.minPercentArea, 100.0, 0, 0);
```

It then creates the `filterOptions`. All the parameters are values of 0 or 1, effectively acting like a boolean true or false. In order, the parameters mean 
1. **keep** only the particles that fit the criteria (a value of 1 would remove only those that do)
2. do not reject particles at the border
3. fill holes within the particles
4. use 8-connectivity, where pixels on the diagonal are considered neighbors (a value of 0 would indicate the use of 4-connectivity in which neighbors are only those pixels directly up, down, left and right from a pixel)

```java
NIVision.ParticleFilterOptions2 filterOptions = new NIVision.ParticleFilterOptions2(0,0,1,1);
```

It then calls `imaqParticleFilter4` with the options and criteria just created and stores the returned error.
```java
int imaqError = 
	NIVision.imaqParticleFilter4(filteredImg, inputImg, filterCriteria, filterOptions, null);
```

Once the particles are identified, `imaqCountParticles` is called to see how many were found.
```java
int numParticles = NIVision.imaqCountParticles(filteredImg, 1);
```

A new `ArrayList` is declared to store the attributes of all the particles found.
```java
ArrayList<Particle> particles = new ArrayList<Particle>();
```

Measurements of interest are established so that the values can be determined for every particle.
```java
NIVision.MeasurementType[] imaqProperties = 
	{ 	NIVision.MeasurementType.MT_AREA_BY_IMAGE_AREA,
		NIVision.MeasurementType.MT_AREA,
		NIVision.MeasurementType.MT_FIRST_PIXEL_X,
		NIVision.MeasurementType.MT_FIRST_PIXEL_X,
		NIVision.MeasurementType.MT_BOUNDING_RECT_HEIGHT,
		NIVision.MeasurementType.MT_BOUNDING_RECT_WIDTH		};		
```

Then, for each particle found, each of the measurements of interest are taken and stored inside an instance of the `Particle` class discussed earlier. The instance is then added to the `ArrayList` of particles.
```java
//the properties of a particle in order (areaPercent, area, x, y, width and height)
double[] parProperties = new double[6];

for(int particleIndex = 0; particleIndex < numParticles; particleIndex++){
	for(int property = 0; property < imaqProperties.length; property++) {
		parProperties[property] = 
				NIVision.imaqMeasureParticle
					(filteredImg, particleIndex, 0, imaqProperties[property]);
	}
	Particle par = new Particle(parProperties);
	particles.add(par);
}
```

The list of particles is returned.
```java
return particles;
```

####4. drawParticleBox
`drawParticleBox` takes in an image and a list of particles.
```java
public Image drawParticleBox(Image inputImg, ArrayList<Particle> particles) 
```

If there were particles found, `drawParticleBox` creates a new `NIVision Image` in which to store the result and calls it `boundedImg`. Note that the `ImageType` of this image must be RGB.

```java
if(particles.size() != 0) {
	Image boundedImg = NIVision.imaqCreateImage(ImageType.IMAGE_RGB, 0);
```

It sorts all the particles, so that the largest one is at index 0, and selects that particle.
```java
particles.sort(null);
Particle particle = particles.get(0);
```

It logs the measurements of the particle for reference.
```java
log.info("particle x: " + particle.x);
log.info("particle y: " + particle.y);
log.info("particle height: " + particle.height);
log.info("particle width: " + particle.width);
```

Then it creates a new `NIVision.Rect` with the parameters of the particles bounding box.
```java
NIVision.Rect rect = 
	new NIVision.Rect(particle.y, particle.x, particle.height, particle.width);
```

It draws this bounding box on top of the image it is provided. This image should be the resized RGB frame.
```java
NIVision.imaqDrawShapeOnImage(boundedImg, inputImg, rect, 
				DrawMode.DRAW_VALUE, ShapeMode.SHAPE_RECT, 0.0f);		
```

Lastly, it returns the resulting image.
```java
return boundedImg;
```

If no particles were found, it logs an error and returns the image it was given without drawing anything.
```java
else {
	log.warning("no particles found");
	return inputImg;
}
```

##UpdateCam
The `UpdateCam` file is a command that repetitively updates the feed from the camera, giving a live video feed. 
First, the logger is activated to begin logging information about what the camera does. It logs that it is logging the `UpdateCam` command. 

```java
public final static Logger log = Logger.getLogger(UpdateCam.class.getName());
```

Then, the constructor sets the requirements for the command, so it knows what subsystems will need to be used in order for the command to be executed. This command requires the use of the `CameraManager` subsystem, as well as the `VisionProcessor` subsystem.

```java
public UpdateCam() {
	requires(Robot.camera);
	requires(Robot.vision);
}
```

Next, the command is initialized. This allows the command to actually begin functioning and leads it to the next step, which tells it to execute.

```java
protected void initialize()
```

When the command begins to execute, it logs the camera and then grabs an image from a specified camera. 

```java
protected void execute() {
	log.info("Grabbing image");
	Image img = Robot.camera.getImage(RobotMap.Camera.MY_CAMERA);
```


It then creates a condition, saying that as long as the image grabbed is not null, the logger should log that it’s processing an image and then display the processed image. Here is where you can modify which `VisionProcessor` functions are being called on which camera images, and what you would like to display on the SmartDashboard.

```java
if(img != null) {
    	log.info("Processing image");
    	Image toDisplay = Robot.vision.findTest(img);
    	CameraServer.getInstance().setImage(toDisplay);
}
```

However, if the image grabbed is null, the logger logs that no image was grabbed at the warning level.

```java
else {
 	log.warning("No image was grabbed");
}
```

Then, since the command repeats, `isFinished()` returns a false state, and `end()` and `interrupted()` return nothing since they are void. 

```java
protected boolean isFinished() {
 	return false;
}

protected void end() {
}

protected void interrupted() {
}
```
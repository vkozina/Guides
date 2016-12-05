**[Installation Guide](Installation-Guide)**
* **[Overview](Installation-Guide#overview)**
* **[Tools to Install](Installation-Guide#tools-to-install)**
* **[Downloading Java](Installation-Guide#downloading-java)**
* **[Download Eclipse](Installation-Guide#download-eclipse)**
* **[Setting up the JDK in Eclipse](Installation-Guide#setting-up-the-jdk-in-eclipse)**
  * **[Adding the JDK](Installation-Guide#adding-the-jdk)**
* **[Configuring Eclipse](Installation-Guide#configuring-eclipse)**
  * **[Save automatically before build](Installation-Guide#save-automatically-before-build)**
  * **[Automatic updates](Installation-Guide#automatic-updates)**
* **[Online Installing the Development Plugins](Installation-Guide#online-installing-the-development-plugins)**
  * **[Selecting the correct plugins](Installation-Guide#selecting-the-correct-plugins)**
* **[Github Desktop](Installation-Guide#github-desktop)**
* **[Setting Up Java with Github](Installation-Guide#setting-up-java-with-github)**
* **[Driver Station](Installation-Guide#driver-station)**
  * **[Downloading](Installation-Guide#downloading)**
  * **[Activating](Installation-Guide#activating)**
  * **[Microsoft .NET Framework](Installation-Guide#microsoft-net-framework)**
    * **[Windows Features (.NET Framework 3.5 not on)](Installation-Guide#windows-features-net-framework-35-not-on)**
    * **[Windows Features (.NET Framework 3.5 already on)](Installation-Guide#windows-features-net-framework-35-already-on)**
* **[Disabling the Windows Firewall](Installation-Guide#disabling-the-windows-firewall)**
* **[Final notes](Installation-Guide#final-notes)**

##Overview
This guide outlines the computer setup (or environment) necessary for working with the robot.

It includes the installation instructions for a 64-bit Windows computer (such as a Windows Surface). If you are setting up other operating systems or require more information, consult the [FRC Java Programming](https://wpilib.screenstepslive.com/s/4485/m/13809/c/88899) page, as many of the steps are adapted from there.

Please refer to the later guides for instructions on how to best utilize the tools.

##Tools to Install
By the end of this guide you should have the following tools installed.
- **Java** – Object-oriented programming language used to write commands and procedures 
- **Eclipse** – Integrated Development Environment (IDE). Basically a fancy text editor that lets you code in Java (and other languages).
- **Github Desktop** – Desktop applet for github.com, a version of source control that lets you collaborate with other members of the team and save past versions of code.
- **Driver Station** – FRC provided application to interface with the robot 

##Downloading Java
Navigate to the [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/java00.png]]

Select "JDK Download", then scroll down the page to "Java SE Development Kit <version>". Accept the license agreement and download the Java SDK for Windows x64.

Java 8 is installed on the RoboRIO and to take advantage of all the features it offers, it is suggested that you use Java 8 on your development system.

##Download Eclipse
You can get eclipse Mars (4.2) from the [Java web site](https://eclipse.org/downloads/packages/release/Mars/2).

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/eclipse00.png]]

Select the Windows 64 bit version of Eclipse IDE for Java Developers and click the link to download. Choose a location such as the downloads folder for the zip file.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/eclipse01.png]]

Extract the contents of the zip file by right-clicking on the .zip file in a windows explorer window and selecting "Extract All..." and taking the default for the location to extract it.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/eclipse02.png]]

Move the extracted folder to Program Files. Within the eclipse folder you'll see the executable file "eclipse". You can right-click on "eclipse.exe" and select "Pin to task bar” to make it easier to run eclipse without having to find the installation location.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/eclipse03.png]]

Start Eclipse. The first time Eclipse starts it will ask you for the location of your workspace. A workspace is the location on disk where projects and files are stored by default. You can have multiple workspaces but for now, leave the default workspace and hit ok.

##Setting up the JDK in Eclipse

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/jdk00.png]]

Select Windows from the menu bar, then Preferences. Choose Java in the list on the left of the Preferences window, then Installed JREs. Be sure that the installed JDK is selected as shown  (make sure the "Location" field includes jdk 8 or 1.8, the name field may be the same in either location). This will enable eclipse to build Java programs for the RoboRIO. Without this setting you will see error messages about the JRE path not being set correctly.

If you do not see any option with the appropriate location, see the next step "Adding the JDK".

###Adding the JDK
**Only** if the JDK is not shown in the step above, complete the steps below.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/jdk01.png]]

1. Click Add
2. Select Standard VM
3. Click Next
4. Click Directory and browse to the folder for the JDK (usually C:\Program Files\Java\* or C:\Program Files (x86)\Java\*). The image shows jdk1.7.0_51, you will likely have a jdk1.8.* version.
5. Click OK. Pick a name for the JRE such as JDK8.
6. Click Finish
7.Make sure the box for the newly added JDK entry is checked.
8.Click Apply and then OK

##Configuring Eclipse
There are a huge number of configuration options for eclipse to set up the environment for your preferences. Most of these are highly recommended but not strictly required.

###Save automatically before build

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/configure00.png]]

One suggested setting to note is: "Save automatically before build." This setting will cause all of your workspace changes to be saved when you build the project. If you don't set this, remember to save changes before building, otherwise the rebuilt program won't reflect your newest updates.
To set this, go to Window -> Preferences -> General -> Workspace -> Check Save automatically before build -> OK

###Automatic updates

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/configure01.png]]

With Automatic Updates enabled, Eclipse will check for updated versions of the plugins each time it starts and inform you if an update is available. This will help insure you are notified of new versions of the WPILib plugins.
To enable Automatic Updates, select Install/Update then Automatic Updates. Check the box at the top to Automatically find new updates and notify me. Select the radio button to Look for updates each time Eclipse is started. Then click Apply and OK.

##Online Installing the Development Plugins

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/configure02.png]]

It is recommended to install the plugins using this method, which requires an active internet connection and fetches the plugins directly from the WPILib site. This will allow you to check for updates to the plugins using Eclipse.

Eclipse extensions are based on user-installed plugins. To get the WPILib development tools you will need to install the correct plugin for your language.

When Eclipse starts:
1. Select "Help"
2. Click "Install new software".
3. From here you need to add a software update site, the location where the plugins will be downloaded. Push the "Add..." button then fill in the "Add Repository" dialog with:
4. Name: FRC Plugins
5. Location: http://first.wpi.edu/FRC/roborio/release/eclipse/
6. Click "OK".

###Selecting the correct plugins

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/configure03.png]]

1. Click the arrows if necessary to expand the WPILib Robot Development menu.
2. Select the WPILib Robot Development plugin for Java
3. Click Next, Next on the next page, then click the radio button to accept the license agreement and click Finish
4. If you receive a Security Warning prompt, click OK to continue.
5. When prompted, restart Eclipse. After Eclipse restarts and you select your Workspace (if prompted) you will see a dialog that says Installing Java. This details the installation progress of the plugins, wait for the install to complete before proceeding. This dialog should only appear when the plugins are first installed or updated.

##Github Desktop
Download the Github Desktop by going to the [Github website](https://desktop.github.com/) and simply hitting download. Navigate to the Downloads folder and double click on “GitHubSetup.exe” to install the application. Launch it from the Start Menu.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/github00.png]]

Enter the appropriate username and password and click Log in. Continue through the Configure step, leaving in the default values. 

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/github01.png]]

Click on the plus in the upper left corner, select Clone → <Name of desired repository> → Clone. Browse to select a destination for this repository. 

##Setting Up Java with Github

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/github02.png]]

Once you have Github installed, re-open Java and hit browse to change the workspace to be the directory created by Github. Next, create a temporary robot project. For a currently unknown reason, importing Github projects into a blank workspace does not link to the WPI libraries correctly. Go to File – New – Project – Robot Java Project – Next. Name the Project “Test” and select “Command-Based Robot”. Then click finish. You can delete this project as soon as you finish this section.

Now to import the already existing projects on Github into the workspace. Navigate to File → Import…

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/github03.png]]

Select Git → Project from Git and hit next. Select Existing local repository and click next.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/github04.png]]

Go to Add… and Browse for the folder just created by Github. Click Ok and then make sure there is a check mark next to the folder just imported before selecting Finish. 

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/github05.png]]

Click Next again, select “Import existing Eclipse projects” and then click finish. This should populate the Package Explorer on the left with all the projects from the Github repository.

##Driver Station

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver00.png]]

The Driver Station is part of the (FRC Update Suite)[http://ni.com/download/cds/view/p/id/5773/lang/en]

###Downloading

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver01.png]]

If installing on Windows 8 or 10 and the above error appears, jump down to the Addendum on Windows 8 installation before returning here to re-start the installation.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver02.png]]

Right click on the downloaded zip file and select Extract All. Choose a convenient location to extract to like the Desktop. Open the extracted folder and any subfolders until you reach the folder containing "setup" (may say "setup.exe" on some machines). Double click on the setup icon to launch the installer. Click "Yes" if a Windows Security prompt appears. Click "Next" on the splash screen that appears.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver03.png]]

Click "Next"

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver04.png]]

Un-check the box, then Click "Next"

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver05.png]]

Enter full name and organization and the serial number from your kit of parts then click Next
{delete this after Feiyue} Serial Number: M81X07988

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver06.png]]

Select "I accept..." then click "Next"

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver07.png]]

Select "I accept..." then click "Next"

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver08.png]]
[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver09.png]]
[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver10.png]]

Make sure the box is checked to Run License Manager... then click Next

###Activating

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver11.png]]

Select your desired activation method (Internet activation recommended), then click Next

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver12.png]]

Make sure the serial number in the box matches the one from your kit, then click Next.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver13.png]]

Log in or create an NI Profile. One profile may be used for multiple installations.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver14.png]]

Click Next

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver15.png]]

Click Next

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver16.png]]

After the product is activated, Click Finish. If prompted to Reboot, click Yes

###Microsoft .NET Framework

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver17.png]]

If installing on Windows 8 or 10, the Microsoft .NET Framework 3.5 may need to be installed for the driver station. **Only if you see the dialog shown above, click "Cancel" and perform the steps shown below**. An internet connection is required to complete these steps.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver18.png]]

Open the "Programs and Features" window from the control panel and click on "Turn Windows features on or off"

####Windows Features (.NET Framework 3.5 not on)

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver19.png]]

Select ".NET Framework 3.5 (includes .NET 2.0 and 3.0)" to enable it (a black dot, not a check box will appear) and then click "OK". When installation finishes restart installation of FRC 2016 Update Suite.

####Windows Features (.NET Framework 3.5 already on)

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/driver20.png]]

If a black dot is shown next to ".NET Framework 3.5" the feature is already on. Click "Cancel" and restart installation of FRC 2016 Update Suite.

##Disabling the Windows Firewall

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/firewall00.png]]

The driver station does not connect properly if this step is not taken. Open the start menu and search for “Windows Firewall”. Switch the Windows Firewall State to “Off” for both private and public networks. 

##Final notes
Congratulation! You’re all done setting up your working environment. Now you are ready to write some code. Check out the individual guides for more help on using the tools mentioned here. 
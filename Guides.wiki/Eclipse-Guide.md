**[Eclipse Guide](Eclipse-Guide)**
* **[Overview](Eclipse-Guide#overview)**
* **[Opening Eclipse](Eclipse-Guide#opening-eclipse)**
* **[Workbench Layout](Eclipse-Guide#workbench-layout)**
  * **[1) Package Explorer](Eclipse-Guide#1-package-explorer)**
  * **[2) Editor Window](Eclipse-Guide#2-editor-window)**
  * **[3) Console](Eclipse-Guide#3-console)**
  * **[4) Riolog](Eclipse-Guide#4-riolog)**
* **[A few key buttons](Eclipse-Guide#a-few-key-buttons)**
* **[Creating a New (Command Based) Project](Eclipse-Guide#creating-a-new-command-based-project)**
* **[Creating a New Class](Eclipse-Guide#creating-a-new-class)**
* **[Editing Your Code](Eclipse-Guide#editing-your-code)**

##Overview
This guide goes over the basics of using Eclipse to edit code. It details operations such as creating new classes, running code and viewing output. It focuses on working on a command based robot so the instructions may vary for other types of projects.

##Opening Eclipse

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/opening00.png]]

If, when you open Eclipse this window pops up, simply select which workspace you wish to use and click ok. You can have as many workspaces as you want. They are essentially separate folders that allow you to organize projects and other work. For example, all the code written for Feiyue 2016 should be kept in the workspace at C:\Users\Girls of Steel\Feiyue\Feiyue_2016 so it doesn’t get mixed in with the Girls of Steel code base.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/opening01.png]]

The very first time you open a workspace, this welcome window should pop up. Simply click on the Workbench arrow to proceed.

##Workbench Layout

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/layout00.png]]

This is the default layout of your workspace. It is separated into several different sections.

###1) Package Explorer

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/layout01.png]]

The package explorer allows you to see all the projects contained in this workspace.

<ol type="A">
  <li>Click on the **arrows** to expand a project or package</li>
  <li>Code in Java is organized in a hierarchy of containers. A **Project** is top level container that holds all the code for any particular task. It contains **Packages** which in turn contain **Classes**. </li>
  <li>A **Class** is what contains your actual code. Double click on any class to open it.</li>
</ol>

For more information on this structure, please refer to the Intro to Java guide.

###2) Editor Window

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/editor00.png]]

Classes you open will show up as tabs in this window. You can then edit the code and switch between tabs as necessary. It features helpful tools such as line numbers and error warnings.

###3) Console

The **Console** is the default destination standard programs will print to.

###4) Riolog

The **Riolog** is the default destination robot programs will print to.

##A few key buttons

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/running00.png]]

A. The **Run** button lets you compile and execute (run) your code. 

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/running01.png]]

For Robot projects you’ll want to Run As → WPILib Java Deploy.
	
B. Clicking the **Stop** button will terminate a running program. This is very useful if something goes wrong.

##Creating a New (Command Based) Project  

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/project00.png]]
[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/project01.png]]

To create a new Command Based Project, go to File – New – Project – Robot Java Project – Next

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/project02.png]]

Enter the desired name for the project under Project Name and select “Command-Based Robot”. Then click finish. 

Once the project is created, expand org.us.first.frc.team3504.command - Right Click on ExampleCommand.java – Delete. Expand org.us.first.frc.team3504.subsystem - Right Click on ExampleSubsystem.java – Delete as well. Ignore any errors this causes  -- the Programming the Robot guide will address these later.

##Creating a New Class

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/class00.png]]
[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/class01.png]]

To create a class go to File – New – Other and expand the “WPILib Robot Java Development” folder. 

[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/class02.png]]
[[https://github.com/CMUFeiyue/Guides/blob/master/images/eclipse/class03.png]]

Select **“Command”** or **“Subsystem”** depending on what kind of class you want to create and click next. Select the project you would like to put this class in using the dropdown menu. Name your class and hit finish.

##Editing Your Code

You can visit Help → Key Assist for helpful keyboard shortcuts. Make sure to save your changes often.

TODO: Make this section actually helpful

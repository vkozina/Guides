#Welcome to the Guides

These are here to help you understand the existing code base, learn how contribute your own code, and keep all your work tidy.

## Prologue:
- If you are working on a personal machine and want to get all the required tools, follow the [Installation Guide](https://github.com/CMUFeiyue/Guides/wiki/Installation-Guide). It'll walk you through getting set up.

## Getting Started:
- Read through the [Github](https://github.com/CMUFeiyue/Guides/wiki/Github-Guide) and [Eclipse](https://github.com/CMUFeiyue/Guides/wiki/Eclipse-Guide) guides
- Create a Command Based Robot project and name it `FirstCBRProject<your name>` (do not include special characters or spaces in the file name. For example FirstCBRProjectVal). You don't have to make the commands or subsystems just yet.
- Push your project to the Feiyue_2016 github repository
- Read through the [Intro to Java](https://github.com/CMUFeiyue/Guides/wiki/Intro-to-Java) and [Command Based Outline](https://github.com/CMUFeiyue/Guides/wiki/Command-Based-Outline) guides and absorb as much of them as you can.
- Work through the [Programming the Robot](https://github.com/CMUFeiyue/Guides/wiki/Programming-the-Robot) guide. At the end of this, you should have code in your CBRProject that spins a motor on a joystick button press.
- The Feiyue_2016 github has a project called [FirstCBRProjectExample](https://github.com/CMUFeiyue/Feiyue_2016/tree/master/FirstCBRProjectExample). This is what your code should look like when you're done.
- Look at the [Running your Code](https://github.com/CMUFeiyue/Guides/wiki/Running-Your-Code) guide for details on how to test your code.
- Once you're done with this section, fill out this [form](https://docs.google.com/a/andrew.cmu.edu/forms/d/e/1FAIpQLSdvTJOHPErDEC9gIdl0XX1WBhrQnw7SAYOkuDyaoNhF-b5eFg/viewform) so we can get a feel for how much you understand.

## Some More Advanced Stuff:
- [Using a Limit Switch](https://github.com/CMUFeiyue/Guides/wiki/Using-a-Limit-Switch) explains how to go about modifying subsystems and commands to react to a limit switch.
- [Driving the Chassis](https://github.com/CMUFeiyue/Guides/wiki/Driving-the-Chassis) details how to extend your first project into one that drives the entire chassis. This means more motors and more fun.
- [The Camera Project](https://github.com/CMUFeiyue/Guides/wiki/The-Camera-Project) goes over the entire camera and computer vision project line by line. If you want to get into using computer vision to assist your robot, this is a good place to start.
- The [ReferenceRobot](https://github.com/CMUFeiyue/Feiyue_2016/tree/master/ReferenceRobot) in the Feiyue_2016 github combines all the components in a project that drives a chassis with a joystick until a limit switch is pressed while running computer vision. You can reference this for information on how to grow your robot project.
- The [PIDGearBox](https://github.com/CMUFeiyue/Feiyue_2016/tree/master/PIDGearBox) project in the Feiyue_2016 github contains some code to make adding tunable PID control easier. For some background information on what PID control is, take a look at the youtube videos [Controlling Self Driving Cars](https://www.youtube.com/watch?v=4Y7zG48uHRo) and [PID Control - A brief introduction](https://www.youtube.com/watch?v=UR0hOmjaHp0)

## Extras:
- If you ever encounter problems, take a look at the [Common Problems](https://github.com/CMUFeiyue/Guides/wiki/Common-Problems) guide.
- APIs are incredibly helpful when you're using outside libraries. To find out more about the WPILib API and how to read it, take a look at [Reading an API](https://github.com/CMUFeiyue/Guides/wiki/Reading-an-API)
- For more information about proper documenting and logging please see [Documenting Your Work](https://github.com/CMUFeiyue/Guides/wiki/Documenting-your-work)
- Before running your code, you should verify that all the port numbers in your code match with the physical system. You can use [Determining Port Numbers](https://github.com/CMUFeiyue/Guides/wiki/Determining-Port-Numbers) as a reference for how to do that.
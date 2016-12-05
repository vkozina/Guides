##Overview
This is a list of things that tend to go wrong. It includes problems from every stage of writing code, including installation.

##Installation
Below are some troubleshooting steps for commonly encountered issues during installation.

###Unable to read repository
"Unable to read repository at http://first.wpi.edu/FRC/roborio/release/eclipse/content.xml” occurs if Eclipse cannot contact the server to download the plugins.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/installation/troubleshoot00.png]]

There are a couple possible causes of this issue:
1. Your computer is not connected to the Internet. Verify your network connection and try again.
2. Your firewall is blocking Eclipse. Try adding an exception for Eclipse or disabling your Firewall.
3. Your proxy settings were read improperly by Eclipse. In Eclipse Select Window->Preferences->General->Network Connections. If you don't use a proxy or don't know, set the Active Provider to Direct. If you use a proxy set the Active Provider to Manual and configure the proxy information by selecting the protocol and clicking Edit.

###Need Java 1.7 or newer
If you get an error message when attempting to run Eclipse that says you "need Java 1.7 or newer", you have mismatched versions of Java and Eclipse installed. The easiest fix is to go back and download the other version of Eclipse (32 bit if you had 64, 64 if you had 32).
**[Documenting Your Work](https://github.com/CMUFeiyue/Guides/wiki/Documenting-your-work)**
* **[Overview](https://github.com/CMUFeiyue/Guides/wiki/Documenting-your-work#overview)**
* **[Logger](https://github.com/CMUFeiyue/Guides/wiki/Documenting-your-work#logger)**
* **[Github](https://github.com/CMUFeiyue/Guides/wiki/Documenting-your-work#github)**
  * **[Issues](https://github.com/CMUFeiyue/Guides/wiki/Documenting-your-work#issues)**
  * **[Commit Messages](https://github.com/CMUFeiyue/Guides/wiki/Documenting-your-work#commit-messages)**
* **[Comments](https://github.com/CMUFeiyue/Guides/wiki/Documenting-your-work#comments)**


## Overview
This guide outlines some methods for proper logging and documenting, to make your code easier to debug, understand and edit.

## Logger
Java comes with a build in logger class that helps with testing and debugging. Using the logger is a great way to manage what kind of information gets printed at which times. It makes it extremely easy to turn print statements on and off when necessary.

Each class has it's own logger, which lets you adjust what kind of details each class provides.. To include a logger, delcare and instantiate one Logger within the class.
```java
public final static Logger log = Logger.getLogger(ExampleClass.class.getName());
```

You can set the logging level to any of:
- `OFF`
- `SEVERE`
- `WARNING`.
- `INFO`
- `CONFIG`
- `FINE`
- `FINER`
- `FINEST`
- `ALL`

by calling
```java
log.setLevel(Level.ALL);
```
Setting the level of the logger indicates that all messages of that level or above should be displayed. Setting the level to `ALL` means that messages of all levels will be printed, whereas setting it to `WARNING` means only messages with level `WARNING` or `SEVERE` will be shown.

Then use
```java
log.<log level>("Some message");
```
to add a new message of a particular level.

For example
```java
log.info("Some message");
```

You can find more information about the java logger in the [API](http://docs.oracle.com/javase/6/docs/api/java/util/logging/package-summary.html)

## Github
### Issues
Github issues are a great way to keep track of what still needs to be done (or fixed). You can see an example issue in the [Issues](https://github.com/CMUFeiyue/Guides/issues) tab, under **closed**. Make sure to close issues when the problems detailed are resolved.

### Commit Messages
Commit messages allow other people to see what changes have been made since the last time they worked with the repository. Try to keep these as detailed as possible. "Added code", while probably true, is not a helpful description of the commit. "Modified Camera code to resize image before thresholding" on the other hand, can tell people at a glance what changed.

##Comments
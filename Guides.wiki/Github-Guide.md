**[Github Guide](Github-Guide)**
* **[Overview](Github-Guide#overview)**
* **[Local / Remote Repositories](Github-Guide#local--remote-repositories)**
* **[History](Github-Guide#history)**
* **[Committing Changes](Github-Guide#committing-changes)**
* **[Pushing Changes](Github-Guide#pushing-changes)**
* **[Pulling Changes](Github-Guide#pulling-changes)**
* **[Branching and Merging](Github-Guide#branching-and-merging)**
* **[Github Online](Github-Guide#github-online)**

##Overview
Github is a tool that allows people to collaborate on projects. More importantly, it provides something called source control. Basically, it will store all previous iterations of the code so if something goes wrong, it is possible to recover an older working version.

##Local / Remote Repositories
A repository (sometimes called a “repo”) is essentially just a fancier folder. It can store all the necessary files for a project. A local repository is the copy that is stored on your computer. No one else can see any changes you make to this copy unless they use the same computer. The remote repository is the one stored online that can be accessed by anyone with the proper permission. 

##History
One of the most important features of Github is its ability to store all the versions of a project. With Github it is possible to backup the current state of a folder at any point in the editing process. However, instead of constantly backing up (like Google Docs for example), Github requires that you specify the points at which the state of the project should be backed up. This allows users to revert back to previous working iterations if anything goes wrong. Basically it’s like being able to set a point while editing and then having the ability to easily undo back to that point.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/github/history00.png]]

The bar at the top of the desktop client lets you see the history of the repository. Clicking any prior dots will let you see the previous iterations of the repository, why this iteration was stored and the things that were new when it was saved. 

##Committing Changes

[[https://github.com/CMUFeiyue/Guides/blob/master/images/github/committing00.png]]

**Committing** a change is basically like saving an iteration of the repository and setting a new point in history. You can make as many changes as you want before committing them, which is what makes Github so powerful. When you commit to Github it will locally create the new save point.

Whenever Github detects that you’ve made changes within the repository (like adding, deleting, moving or modifying files inside it), it will automatically **stage** those changes. You can **unstage** changes by simply clicking the boxes next to their description. Changes that are not staged will not be included in the committed version.

After selecting the changes you wish to include, provide a summary of the changes you made (also called a **commit message**). Then hit “commit to master” to commit them. It is also recommended you provide a more detailed description of the new version in the second box, but this is not required.

##Pushing Changes
After committing changes locally, you need to add the changes to the remote repository. Remember, any changes you make on your computer are only seen by you until you add them to the online copy of the repository. This is called **pushing**. Simply hit the “sync” button and it will update the remote repository to contain all your changes.

##Pulling Changes
Often times there will be multiple people working on a single project. Github allows for collaboration by letting you **pull** changes. This will update your local repository to match the remote one. If anyone else pushed changes to the project while you were away, you can press the “sync” button to pull the changes and make sure you have the most current version of the project to work with.

##Branching and Merging
Another powerful feature of Github is the ability to create a new branch. This is basically another line in history that originates at the current point. With branches, you can start with the same code and make completely different edits to the individual branches. This allows you to have multiple versions of the project that all start with the same code, and each have their own line in history. Every Github repository contains a **master** branch, which is the main branch for any project.

Merging is the process of combining branches. If there are overlapping changes (for example both branches changed a particular file), merging allows you to resolve conflicts by selecting which changes to keep. This can quickly get complicated, but is an invaluable tool when there are many collaborators.

Because branching and merging  is intricately complex, this guide recommends that beginners get the feel for the simpler tasks (like pulling, committing and pushing) before moving on. Please refer to the [Github Tutorials](https://guides.github.com/activities/hello-world/#intro or https://guides.github.com/introduction/flow/) if you want to learn more about these concepts.

##Github Online

[[https://github.com/CMUFeiyue/Guides/blob/master/images/github/online00.png]]

Github also has a website, called [github.com](github.com). You can access remote repositories from there, even if the computer you are working on does not have Github Desktop installed. It is also helpful when checking that local changes were properly pushed to the remote repository.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/github/online01.png]]

Once you login you can see all your repositories. Click on a repository to see the files it contains.

[[https://github.com/CMUFeiyue/Guides/blob/master/images/github/online02.png]]

From here you can navigate the directory like any other file system. It is also possible to add, upload and edit files directly online. If you do so make sure to pull the changes the next time you work locally.
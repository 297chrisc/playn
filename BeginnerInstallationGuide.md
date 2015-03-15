<font color='red'>
<hr />
<h1>NOTE: THIS WIKI IS (PARTIALLY) OUT OF DATE</h1>
<ul><li>See the GettingStarted wiki for the most up to date instructions<br>
</li><li>This wiki may still have useful information though<br>
</li><li>Ignore references to the <code>1.1-SNAPSHOT</code> version in this wik; use the most current release version instead<br>
<hr />
</font></li></ul>

_Originally posted on Mike Fleischauer's [Game From Scratch](http://www.gamefromscratch.com/post/2011/10/13/Getting-started-with-PlayN.aspx) blog._


Before you can use PlayN, you need to download and configure it, which is not a completely trivial task.



First of all, you are going to need GIT.  Git is a version control system and if you are working on Windows there are a few ways to go about it.  First is to install Cygwin which is a Linux like environment for Windows.  The other ( and easier ) option is to download it here. ( I used this one ).  Run through the setup, the only option to be aware of is this one:


**EDIT: It was pointed out other Git options such as the EGit plugin for Eclipse exist.**



![http://playn.googlecode.com/files/11-3-2011_image.png](http://playn.googlecode.com/files/11-3-2011_image.png)



This is the option I used and will assume you will use going forward.  Also I chose windows style line breaks on checkout. Now that Git is installed, its now a matter of downloading the most recent source code from Google.  I assume you have an internet connection, now open a command prompt and change to the directory you want to install PlayN in.  Git will automatically create a “playn” folder, so in my case I used c:\ as the install path.



Now type “git clone https://github.com/threerings/playn.git” and it should look like this:



![http://playn.googlecode.com/files/11-3-2011_image-1.png](http://playn.googlecode.com/files/11-3-2011_image-1.png)



And after running, it should create a directory called playn whose contents look just like this:




![http://playn.googlecode.com/files/11-3-2011_image-2.png](http://playn.googlecode.com/files/11-3-2011_image-2.png)




If you haven’t already got a Java JDK installed, go get one now.  I started this tutorial off using 7, but truth is Java 6 seems to work better.  So if you don’t already have it, go get it now.



Now you need some kind of an IDE.  In this example I am going to use Eclipse, as it seems to be the one that Google most supports.  If you don’t already have Eclipse installed, head on over to Eclipse.org's download section and pick out a version.  In my case I went with Eclipse 3.7.1 Classic x64.  Simple open up the zip file and extract the eclipse folder wherever you want your install to run from.  Run Eclipse.exe and select where ever you want your project files to exist.



Now you need to add a couple plugins.



First we need to install the M2E ( Maven ) plugin.  You need to do this inside Eclipse.  In Eclipse, select the Help menu, then Install New Software…



In the dialog that pops up pull down the “Works with” box and select “http://download.eclipse.org/releases/indigo/201109230900”.  In a few seconds it will populate a list of options, you want to expand Collaboration and select “m2e – Maven Integration for Eclipse” then click Next, like this:


![http://playn.googlecode.com/files/11-3-2011_image-3.png](http://playn.googlecode.com/files/11-3-2011_image-3.png)




Click Next again then agree to the terms and conditions and it will go out, download and configure the Maven integration.  After it is complete it will ask you to restart Eclipse, allow it to.



Now we need to add the two Google specific libraries, the Google Eclipse Plugin and the Android Eclipse Plugin.  Both are installed in a very similar manner, just using a different repository.  In Eclipse, select Help->Install New Software.  This time click the “Add…” button and in the “Add Repository” dialog, enter dl.google.com/eclipse/plugin/3.7 like this:




![http://playn.googlecode.com/files/11-3-2011_image-4.png](http://playn.googlecode.com/files/11-3-2011_image-4.png)


Now click OK and it will again populate the list of selections available.  We need to add another repository for the Android Plugin as well, this repository is located at http://dl-ssl.google.com/android/eclipse/ like such:



![http://playn.googlecode.com/files/11-3-2011_image-5.png](http://playn.googlecode.com/files/11-3-2011_image-5.png)



Alright, now that those are both added, back in the Install New Software screen, in Works With select our newly created Android repository and select either Android Developer Tools or optionally just select Developer Tools to install everything, which is what I am going to do.  So your screen should look like this:




![http://playn.googlecode.com/files/11-3-2011_image-6.png](http://playn.googlecode.com/files/11-3-2011_image-6.png)


Finally click Next, Next again, agree to license terms, then Finish.  It will proceed installing the Android tools, you may receive the following warning:



![http://playn.googlecode.com/files/11-3-2011_image-7.png](http://playn.googlecode.com/files/11-3-2011_image-7.png)



Simply click OK.  Once completed it will ask you to Restart again, choose “Restart Now”.  Now all we need to do is configure the Google Plugin.  Follow the exact same process, Help->Install New Software… and this time in Work With select the Google repository.  I am not specifically sure exactly which plugins and SDKs are required by PlayN, so I simply clicked Select All… disk space is cheap these days right?


![http://playn.googlecode.com/files/11-3-2011_image-8.png](http://playn.googlecode.com/files/11-3-2011_image-8.png)




Click Next, Next, accept the licensing terms and click Finish.  Now Eclipse is going to do it’s thing… taking a fair bit longer than before.  You may once again receive an “unsigned content” warning, simply click OK again.  And, I bet you saw this one coming… one last request to restart Eclipse.  The last time, I swear!



Now that all the tools are created, we need to configure our project.  This is where the m2e plugin we installed earlier comes in.  In Eclipse, in the File Menu select Import.  You should see a list like the following, if the Maven option isn’t there, your m2e install didn’t work properly and try again.  ( Or possibly you didn’t reboot Eclipse earlier? )



![http://playn.googlecode.com/files/11-3-2011_image-9.png](http://playn.googlecode.com/files/11-3-2011_image-9.png)



You want to select Maven->Existing Maven Projects.  Now for Root Directory you want to navigate to the place Git installed PlayN, in my case c:\playn.  Stupidly you can’t just type the path, you have to hit the Browse button or it won’t well… do whatever it does. Speaking of which, whatever exactly it is doing takes a few minutes.  Many moons later it should look like this:


![http://playn.googlecode.com/files/11-3-2011_image-10.png](http://playn.googlecode.com/files/11-3-2011_image-10.png)




Click Next, then Finish.  It will in a rage inspiring way that only Java seems capable of, it will appear to reinstall all of the things we just installed then make me a bit of a liar as it asks you to restart Eclipse one more time.  ARGH.  Alright, last time I double promise.



Now you have to run the Maven install for playn-archetype.  To do so, switch to the Java Perspective using the Window Menu->Open Perspective->Java Browsing.  On the left hand side should be your Package Explorer with a list of maven projects.  Right click playn-archetype and select Run As –> Maven Install, like this:




![http://playn.googlecode.com/files/11-3-2011_image-11.png](http://playn.googlecode.com/files/11-3-2011_image-11.png)




One the bright side, Eclipse is now fully configured and ready to go.  Now we just create our project!



In Eclipse, select File->New->Other...

Now scroll down and select Maven->Maven Project, like this:



![http://playn.googlecode.com/files/11-3-2011_image-12.png](http://playn.googlecode.com/files/11-3-2011_image-12.png)





Click Next, choose your location, I clicked Browse and navigated to my default workspace under my user profile and clicked Next again. In the next screen, check the “Include snapshot archetypes” box, under Filter type “play” then click “com.googlecode.playn”  like such:




![http://playn.googlecode.com/files/11-3-2011_image-13.png](http://playn.googlecode.com/files/11-3-2011_image-13.png)






Now click Next.



This next screen is again typical Java of making things look way more complex than they really are.  Now you need to provide a groupID and a artifactID, which are bizarro over engineered ways of saying your domain name and game name.  For group id put your domain name ( even a made up one is fine ) in reverse order, for example com.gamefromscratch is what I am using.  Then in ArtifactID put your games name, for example playndemo.  Finally in the properties section gameName will have automatically been created, in the value enter your game name again, like this:




![http://playn.googlecode.com/files/11-3-2011_image-14.png](http://playn.googlecode.com/files/11-3-2011_image-14.png)




Obviously you will want to use different values.  You will see that the packagename is auto-generated based on the values you chose.  Now click Finish.  Not there is a little glitch in Eclipse in that after filling in the value for gameName, you need to click somewhere else on the dialog window to get the Finish button to be enabled.





Now your Project is created.  If you look in Package Explorer you will now see it ( all 5 of them ).  Here is mine:





![http://playn.googlecode.com/files/11-3-2011_image-15.png](http://playn.googlecode.com/files/11-3-2011_image-15.png)





Now it’s a matter of getting things to actually run.  Since we are working on a desktop, lets start with the core ( Java Desktop ) application.  Right click on playndemo-core ( obviously your name will be different ) and choose Run As->Java Application.




![http://playn.googlecode.com/files/11-3-2011_image-16.png](http://playn.googlecode.com/files/11-3-2011_image-16.png)




After selecting Java Application a window will pop up asking you to Select your Java Application.  Scroll down till you find your project it will be named [yourprojname](yourprojname.md)Java.java like such:






![http://playn.googlecode.com/files/11-3-2011_image-17.png](http://playn.googlecode.com/files/11-3-2011_image-17.png)






Click it and press OK, and finally the fruit of all our labours!  Our game.




![http://playn.googlecode.com/files/11-3-2011_image-18.png](http://playn.googlecode.com/files/11-3-2011_image-18.png)



___
<a name="eclipse_setup"></a>
#Eclipse Setup
![](images/eclipse_logo.jpeg)  
The Eclipse Platform is our chosen building integrated development environments (IDE). It is a versatile platform to create applications as diverse as web sites, embedded Java programs, C/C++ programs, etc. For more information see also wikipedia’s page and [eclipse’s official site](http://www.eclipse.org/).


##Dependencies
* Install **Oracle JDK** from the [official site](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

##Download
There are a few versions of the Eclipse IDE, each equipped with a different set of plugins. You should download the **Eclipse IDE for Java EE Development** from [Eclipse download site](http://www.eclipse.org/downloads/index.php).

##Installation
1. Unpack the contents of the zip file into the `$ECLIPSE` directory
2. Make sure you don’t finish up with `$ECLIPSE/eclipse` directory. Check that `$ECLIPSE` has the subdirectories plugins, features and configuration
3. Create a shortcut (if needed) to the `$ECLIPSE/eclipse.exe` file and place it anywere you find useful
4. Run Eclipse (either by running `$ECLIPSE/eclipse.exe` or double-clicking the shortcut created above)

##Configuring Eclipse
* Set the Workspace directory: When prompted for the workspace directory enter the value of `$HOME/workspace`.

Eclipse provides an incremental compiler that runs in background (check Project → Build automatically). Some compiler’s checks are usefull for detecting potential coding problems and should be enabled.

###Configure the Background Compiler (Window → Preferences):
1. On the left list select Java and then Compiler
2. Open Errors/Warnings
3. Expand Code Style
4. Set Undocumented empty block to Warning
5. Expand Potential coding problems
6. Set Possible accidental boolean assignement to Warning
7. Set Enum type constant not covered by switch to Warning
8. Press OK. You might receive a message saying that Eclipse needs to rebuild, just press Yes.

###Configure the Code formater
The code formater is an Eclipse component that formats (or indents) the code automatically when CTRL+SHIFT+F is pressed. Some adjustments to the original configuration are needed so that the code formatted is not rejected by Checkstyle plugin and to increase the code readability.

1. Go to Window → Preferences
2. Open Java → Code style → Formater
3. Click Import and use the eclipse-java-formater-config.xml file in the same directory as this instructions

###Enable the Javadoc Checker
Configure Eclipse to warn about malformed JavaDoc comments. It is important that as you develop code you get the javadoc comments are well written otherwise you’ll probably end up with errors when you generate the javadoc.

1. Go to Window → Preferences
2. Open Java → Compiler → Javadoc
3. Check the Process Javadoc Comments option
4. On Malfomed Javadoc Comments set the warning type to warning

###Attach the source code of JDK
1. Sometimes the documentation of the Java API is not comprehensive enough forcing you to look at the actual code of the library. We can actually learn a lot by looking at the Java library code. If you want to seamlessly jump to an API item as an hyperlink you should have the source code of the JDK attached to your JDK installation on Eclipse.
2. Go to Window → Preferences
3. Open Java → Installed JRE
4. Click on the appropriate JRE (e.g. jre6)and press Edit…
5. Select all item in the System Library window
6. Click on Source Attachment…
7. Click on External File…
8. Locate the the src.zip file on your JDK
9. Press OK and then press Finish
10. To test the configuration:
11. Create a simple java file like:
	
        public class Teste {
				void testme() {
					System.out.println();
				}
			}
	
12. Press CTRL and click on System class identifier. An editor window should open with the code of the java.lang.System.

##Install additional Eclipse Plugins
Your Eclipse installation comprises a number of plugins. However some plugins that are important for your productivity are not part of the base installation.

* Eclipse GitHub plugin
* Eclipse Window Builder plugin
* Eclipse CheckStyle plugin
* Eclipse Autodoc plugin
* Eclipse FindBugs plugin
* Eclipse ObjectAid UML plugin
* Eclipse JAD plugin
* Eclipse JavaCC plugin
	
##Troubleshooting
* Launch fails with the message `java.lang.NoClassDefFoundError: examples/MyClass. Exception in thread “main”`.
This happens because the build process is failing. This type of failure can be caused by multiple reasons, yet the the most common is because no *.class* is being generated. Go to *Window → Show view → Problems* and try correct the problems listed. Sometimes the problem is just a missing dependency among projects.

* Eclipse doesn’t start and complains with Workspace is in use choose another one
This happens when Eclipse crashes. Kill the java processes of Eclipse and try again. If the problem presists, go to the .metadata directory within the workspace directory and delete the .lock file.

* A freshly installed Eclipse doesn’t launch an RCP application java.lang.RuntimeException: No application id has been found. This happens because some setting in the launch configuration have to be adjusted. See section 5 of this document.
____
<a name="checkstyle"></a>
#Eclipse Checkstyle Plugin
Checkstyle is a Java style checker and duplicate code detector. It can be run as a stand alone application (for example, to be incorporated in a build process) or as an Eclipse plugin. We strongly recommend to be always active so that coding mistakes are caught early.

##Dependencies
* [Install Eclipse IDE](#eclipse_setup)
	
##Installation
1. Go to **Help → Install New Software…**
2. Click **Add…**
3. Type in the update site name: **Checkstyle**
4. Type in the update address [http://eclipse-cs.sf.net/update/](http://eclipse-cs.sf.net/update/)  
*Note: It may take a while to respond, you’ll probably see the message ‘pending…’*  
5. Check Checkstyle and press **Next**
6. Accept the license agreement and when prompted restart  
7. If you receive a warning telling that the software contains unsigned content, don’t worry and click **OK**.

##Configuration
By default, only a subset of the checks are enabled. Thus, Checkstyle must be properly configured to enable important checks that can be missing. Furthermore some adjustments have to be made to match the style of Code Formater of Eclipse. This adjustments are made automatically importing the configuration files as explain below. Alternatively but can be configured manually following the instructions about [Manually Configuring the Checkstyle Plugin](#checkstyle_manual)

###Showing the Checkstyle Views
1. In Eclipse, go to **Window → Show view → Other…**
2. Select **Checkstyle** and, holding Ctrl key, select **Checkstyle violations**, **Duplicated Code**
	
###Installing Checkstyle Configuration Files
1. Go to **Window → Preferences → Checkstyle**
2. Click **New...**  
3. In Type select **External Configuration File** and browse for the *eclipse-checkstyle-config-src.xml* and name it **Eclipse Checkstyle for Sources**  
4. Repeat steps 2 and 3 to add the *eclipse-checkstyle-config-test.xml* and name it **Eclipse Checkstyle for Tests**  
5. For each project, open the project properties and select **Checkstyle**
6. Set the **Checkstyle active for this project**
7. Uncheck the **Use simple configuration option**
8. Remove any already existing checkstyle configuration from list (**Sun Checks** is probably there) and click **Add...**
9. This configuration will be applied to a specific file set. On the window that just opened, set the **File Set Name** to **Source Checks**
10. Select the "Eclipse Checkstyle for Sources" in the Check Configuration option
11. On the **Regular Expression Patterns** list click **New..** and add this regular expression: `src/.*\.java$` (assuming *src/* is your source folder)
12. Repeat the steps 9 through 11, naming the File Set Name to **Test Checks**, selecting **Eclipse Checkstyle for Tests** and adding the regular expression `test/.*\.java$`

###Run Checkstyle on Demand on a Specific File/Folder

1. In Eclipse, right-click with your mouse over the file or folder you want to scan
2. Go to Checkstyle and select Check code with Checkstyle
3. Later you can disable Checkstyle warnings by selecting the bellow option Clear Checkstyle violations
4. Checkstyle reports the violations found with a small icon at the respective lines. You can also see a detailed list of all violations at Checkstyle violations view.

###Customise the Checkstyle Plugin

1. Go to Window → Preferences and select Checkstyle on left menu list
2. Add the checkstyle files for sources and tests in the same directory as this file
	
As a principle, recurring strings should be declared as constants. However, there are cases where this make no sense like in:

        return x + ", " + y + ", "+ z;

Unfortunately Checkstyle has no way to distinguish between short and long strings and complains that the string " ," is used multiple times.

The option *Disable Return count* is active in this configuration file. While it is true that many return points can be indication that code is attempting to do too much or may be difficult to understand. Most methods with this problem are small (<10 lines). Since we cannot disable checking this rule on small methods we disable it. Lengthy methods will be indicated by another rule.

This configuration file doesn't have the *java.lang.Throwable* class name in the *Illegal throws* list. Otherwise, perfectly good code like overriding finalize will be marked with an error:

        @Override
    	protected void finalize() throws Throwable {
    			super.finalize();
        		this.close();
    		}
	
The *Disable Final Parameters* option is enabled. **Rationale:** Final parameters are a means to prevent assignments to parameters, since this is a very poor programming practice. However, enabling the Parameter Assignment check on the Coding Problems section performs this check without forcing the programmer clutter the code and write the final keyword for every parameter.

The *Enable Suppression Comment Filter* option is active in this configuration file. **Rationale:** In some situations it may be legitimate to create a block of code that is not means to be checked. This should a rare exception that will have to be documented appropriately.

##Testing the installation
1. Open Window → Preferences and the select Checkstyle
2. The Checkstyle configuration panel should appear.

Test that the plugin is working:

1. Create a new Java project
2. Activate Checkstyle for the project
3. Create a new Java class
4. Create a constructor with a parameter that is not final, e.g.:
        
        public class Xyz {
    		public Xyz(int i) {
        		i = 10;
    		}
 	    }  
	   
You should see a warning informing you that 10 is a magic number.

##Using Checkstyle
How to enable or disable Checkstyle from verifying the code on a given code region?
	
* Surround the code region that you need not to be checked with the special comments `//CHECKSTYLE:OFF`  and  `//CHECKSTYLE:ON`

How to quickly fix a file with many Checkstyle warnings?
This can be done step-by-step as follows:
	
1. Firstly, format the complete file according to the format that is being used. This will eliminate extra TAB characters and other formatting problems
2. Have the Eclipse Code Formatter configured appropriately
3. Press CTRL + A (to select the entire file) and then CTRL + SHIFT + F (to format)
3. Then apply the Checkstyle automatic fixes. This will correct missing brackets and put all change member declarations to have the qualifiers in the correct order
4. Right click on the buffer and select Apply Checkstyle fixes….
5. Finally refactor the code, selecting the code you want to refactor and pressing ALT + SHIFT + T
___
___
<a name="findbugs"></a>	
#Eclipse Findbugs Plugin
FindBugs is an open source program which looks for bugs in Java code. It uses static analysis to identify hundreds of different potential types of errors in Java programs. FindBugs operates on Java bytecode, rather than source code. The software is distributed as a stand-alone GUI application.


##Dependencies
* Install Eclipse IDE

##Installation
1. Go to Help → Install New Software…
2. Click Add…
3. Type in the update site name: FindBugs
4. Type in the update address http://findbugs.cs.umd.edu/eclipse/  (Note: may take a while…)
5. Check FindBugs and press Next
6. Accept the license agreement and when prompted restart
7. If you receive a warning telling that the software contains unsigned content, don’t worry and click OK.

##Configuration
1. In Eclipse, go to Window → Show view → Other…
2. Select Findbugs → Bug explorer

**Activate FindBugs for a project:**

1. In Eclipse, right-click with your mouse over the project’s root folder and select Properties
2. Go to Findbugs and check Run automatically

You can *enable project specific settings* by checking the respective option.

**Run FindBugs on demand on a specific file/folder:**  
1. In Eclipse, right-click with your mouse over the file or folder you want to scan  
2. Go to Find Bugs and select Find Bugs

FindBugs reports the errors found with a small icon at the respective lines. You can also see a list of all the errors at FindBugs’ view.

##Testing the installation
1. Create a new Java project
2. Activate FindBugs for the project
3. Create a new Java class named **Test**
        
        public class Test {
			Integer test;

   			public Test(int i) {
       			test = new Integer(i);
    	   }
	   }
4. Save

You should see a warning of FindBugs informing you that the parameter new Integer(i) is an inefficient way of creating an Integer Object.

___
___
<a name="jautodoc"></a>
#Eclipse JAutodoc Plugin (admin)
JAutodoc is a plugin that creates Javadoc comments automatically based on templates. The advantage of using this plugin is that it cuts a great deal of the time needed for creating Javadoc comments. Apart from adding the template for Javadoc style comments for the class/method & attributes it is smart enough to add the description also based on the signatures.


##Dependencies
* Install Eclipse IDE

##Installation
1. Go to Help → Install New Software…
2. Click Add…
3. Type in the update site name: JAutodoc
4. Type in the update address http://jautodoc.sourceforge.net/update/
5. Note: It may take a while to respond, you’ll probably see the message ‘pending…’
6. Accept the license agreement and say ‘Yes‘ when prompted restart
7. If you receive a warning telling that the software contains unsigned content, don’t worry and click OK

##Configuration
1. Go to Window → Preferences → Java → JAutodoc
2. Uncheck the Single line comments option
3. Using JAutodoc
4. Auto-generate Javadoc for an entire file:
5. Right-click over the java file you want the javadoc to be generated
6. Go to JAutodoc and select Add Javadoc
7. Auto-generate Javadoc for a specific method:
8. Select any line of the method you want the javadoc to be generated
9. Press CTRL + ALT + J or right-click the line, go to JAutodoc and select Add Javadoc

##See also
The plugin’s home page

___
___
<a name="objectaid"></a>
#Eclipse ObjectAid Plugin Setup
ObjectAid is a simple and diagram editor plugin for Eclipse.This plugin has some important features, such as, diagrams are saved in text format and thus can be added to a version repository and are kept coherent with the code automatically.

##Dependencies
* Install Eclipse IDE
	
##Installation
1. Open Help → Install New Software…
2. Add the ObjectAid update site http://www.objectaid.net/update
3. Install the plugin and restart Eclipse
	
##Configuration
1. Open Window → Preferences → ObjectAid Class Diagram:
2. On Classifiers, leave only the Show Icons option checked
3. On Relationships, leave Add Generalizations, Add Associations, Show Association Multiplicity, Show Association Labels options checked, all remaining options should be unchecked
4. On Attributes uncheck all options and click Apply
	
##Testing the installation
1. On a Java project in Eclipse, go to File → New → Other…
2. Under ObjectAid UML Diagram…, select Class Diagram
3. Enter the name of the diagram
4. Drag some classes into the class editor canvas
5. You should see the classes rendered in UML notation.

##Troubleshooting
Deleted associations reappear when a project is reloaded. This happens because the project was created with the Always add associations option checked. Recreate the project with this option unchecked.

___
___
<a name="jadclipse"></a>
#JadClipse Plugin
JAD is the best Java Decompiler available. This plugin integrates it into Eclipse. Then, when we follow a link of class that is in a .jar or .class file and the source is not available is decompiles the class and shows the code automatically.

Having this feature is very important for debugging and code understanding.


##Dependencies
* Eclipse Setup
	
##Installation
1. Download the JADClipse plugin. Be carefull to match your version of Eclipse
2. Unzip the zip file
3. Copy the jadclipse.jar to the $ECLIPSE/plugins folder
4. Restart Eclipse

##Configuration
1. Open Window → Preferences → Java → JadClipse
2. Set Path to decompiler to the value to the jad jad executable, e.g., $PROGFILES/jad/jad
3. Set the directory for temporary files to $CACHE/jadclipse
4. Select Formating
5. Check the Output fields before methods option
6. Check the Print default initializers for fields option
7. Check the Output space between keyword and expression option
8. Click Apply

##Testing the installation
Open a .class file of some jar that is included in your project
The source should appear automatically in the editor window.

___
___
<a name="javacc"></a>
#Eclipse JavaCC Plugin
Do you need to write parsers for markup documents that do not subscribe to standard formats such as HTML or XML? JavaCC allows you to do all of that in Java. JavaCC plugin integrates it into Eclipse with grammar syntax highlight and auto-completion and on-the-fly error checking. It re-generates the Java file automatically when the grammar file is altered and saved.


##Dependencies
* Install Eclipse IDE

##Installation
1. Go to Help → Install New Software… → Click Add…
2. Type in the update site name: JavaCC Plugin
3. Type in the update address: http://eclipse-javacc.sourceforge.net/
4. Accept the license agreement and say ‘Yes‘ when prompted restart
5. Testing the installation
6. Open Window → Preferences → JavaCC
7. The color options panel should appear

##See also
Official JavaCC for Eclipse home page

___
___
<a name="checkstyle_manual"></a>
#Manually Configuring the Checkstyle Plugin
Checkstyle is a Java style checker and duplicate code detector. It can be run as a stand alone application (for example, to be incorporated in a build process) or as an Eclipse plugin. We strongly recommend to be always active so that coding mistakes are caught early.

By default, only a subset of the checks are enabled. Thus, Checkstyle must be properly configured to enable important checks that can be missing. Furthermore some adjustments have to be made to match the style of Code Formater of Eclipse.

##Show Checkstyle view

1. In Eclipse, go to **Window → Show view → Other…**
2. Select **Checkstyle → **(and pressing Ctrl key) **Checkstyle violations**, **Duplicated Code**
3. Place the new views wherever you want
4. Activate Checkstyle on your project by right-clicking over the project’s root folder and select **Properties**
5. Go to **Checkstyle** and check **Checkstyle active for this project**. This way all the files of the project (except the ones excluded at Checkstyle‘s configuration) will be continuously checked

You may want to exclude files outside the “src” directory, like “tests” for instance. This is because test files tend to generate many unnecessary warnings. You can do it by checking the option *files from non source directories*, under *Exclude from checking…*

##Run Checkstyle on demand on a specific file/folder

1. In Eclipse, right-click with your mouse over the file or folder you want to scan
2. Go to **Checkstyle** and select **Check code with Checkstyle**
3. Later you can disable Checkstyle warnings by selecting the bellow option Clear Checkstyle violations
4. Checkstyle reports the violations found with a small icon at the respective lines. You can also see a detailed list of all violations at Checkstyle violations view.

##Customize the Checkstyle Plugin

1. Go to **Window → Preferences** and select **Checkstyle** on left menu list
2. Select Sun Checks and press Copy…
3. Enter the name StreetJava Checks and press Ok
4. Select StreetJava Checks and press Configure…
5. Open module Regexp and disable RegExpSingleLine expression that check for lines with trailing spaces
6. Open module Whitespace and disable Do whitespace before and disable Typecast Parent Pad
7. Open module Blocks and, On Left Curly Brace placement, set the option to eol
8. Disable Avoid nested blocks
9. Open module Size Violations and enable Anonymous inner class lengths. For that, press Add… You should see a new window with the module name “Annonymous inner class lengths”. If not press Cancel until it does. Set max to 75 and press OK. Press Cancel for the next modules’ windows
10. Disable Maximum Line Length
11. Open module Coding problems and enable the following coding problems (besides those already enabled). So press Add… and for each module press OK to Enable it or Cancel to Disable it
12. Enable Covariant Equals
13. Enable Default Comes Last
14. Enable Declaration Order Check
15. Enable Explicit initialization
16. Enable Fall through
17. Enable Final Local variable
18. Enable Illegal Catch
19. Enable Illegal throws
20. Enable JUnit test case
21. Enable Missing constructor
22. Enable Modified Control Variable
23. Enable Multiple Variable Declaration
24. Enable Nested if Depth and set max to 3
25. Enable Nested try Depth and set max to 2
26. Enable No Clone
Java clone() is mostly broken. We should resort copy constructors as endorsed by Joshua Bloch.

27. Enable Package Declaration
28. Enable Parameter Assignment
29. Enable Return Count and set max 3
30. Enable String Literals Equality
31. Enable Super Clone
32. Enable Super Finalize
33. Disable Trailing Array Coma
34. Disable Multiple String Literals
35. As a principle, recurring strings should be declared as constants. However, there are cases where this doesn't make sense, like this:

        return x + ", " + y + ", "+ z;
Unfortunately Checkstyle has no way to distinguish between short and long strings and complains that the string " ," is used multiple times.

36. Disable Return count
While it is true that many return points can be indication that code is attempting to do too much or may be difficult to understand. Most methods with this problem are small (<10 lines). Since we cannot disable checking this rule on small methods we disable it. Lengthy methods will be indicated by another rule.

37. Find and double-click Illegal throws and delete the java.lang.Throwable class name from the list. Otherwise, perfectly good code like overriding finalize will be marked with an error :

        @Override
	    protected void finalize() throws Throwable {
	        super.finalize();
	        this.close();
			}
	
38. Find and double-click Redundant Throws
39. Enable allowUnchecked and allowSubClasses
40. Open module Miscellaneous
41. Disable Final Parameters  
***Rationale:*** *Final parameters are a means to prevent assignments to parameters, since this is a very poor programming practice. However, enabling the Paramenter Assignmentcheck on the Coding Problems section performs this check without forcing the programmer clutter the code and write the final keyword for every parameter.*

42. Open module Duplicates and enable Strict duplicate code using the Add… button
43. Open module Class design and select Visibility Modifier and enable protected allowed
44. Select Design for Extension and set the severity to Info. To be more precise only the classes that implement the decorator pattern should have this setting.
45. Open module Javadoc Commnents, select Method JavaDoc and check allowUndeclaredRTE and allowThowsTagsForSubclasses
46. Select the module Filters
47. Enable Suppression Comment Filter  
***Rationale:*** *In some situations it may be legitimate to create a block of code that is not means to be checked. This should a rare exception that will have to be documented appropriately.*



##Testing the installation
1. Open Window → Preferences and the select Checkstyle
2. The Checkstyle configuration panel should appear.

Test that the plugin is working:

1. Create a new Java project
2. Activate Checkstyle for the project
3. Create a new Java class  
4. Create a constructor with a parameter that is not final, e.g.:  
        
        public class Xyz {
    		public Xyz(int i) {
        		i = 10;
    		}
 	   }
	   
You should see a warning informing you that 10 is a magic number.

##Using Checkstyle
How to enable or disable Checkstyle from verifying the code on a given code region?
	
* Surround the code region that you need not to be checked with the special comments `//CHECKSTYLE:OFF`  and  `//CHECKSTYLE:ON`

How to quickly fix a file with many Checkstyle warnings?
This can be done step-by-step as follows:

1. Firstly, format the complete file according to the format that is being used. This will eliminate extra TAB characters and other formatting problems
2. Have the Eclipse Code Formatter configured appropriately
3. Press CTRL + A (to select the entire file) and then CTRL + SHIFT + F (to format)
3. Then apply the Checkstyle automatic fixes. This will correct missing brackets and put all change member declarations to have the qualifiers in the correct order
4. Right click on the buffer and select Apply Checkstyle fixes….
5. Finally refactor the code, selecting the code you want to refactor and pressing ALT + SHIFT + T

___
<a name="codeformater_manual"></a>
#Manually Configuring the Code Formater
The code formater is an Eclipse component that formats (or indents) the code automatically when CTRL+SHIFT+F is pressed. Some adjustments to the original configuration are needed so that the code formatted is not rejected by Checkstyle plugin and to increase the code readability.

1. Go to Window → Preferences
2. Open Java → Code style → Code templates
3. Configuring the code templates:
	* Expand Comments → Types	
	* Remove the ${user}  
***Rationale:*** *According to Agile Practices, we must aim at promoting a shared ownership of the code. Hence, the code is not owned by a single person, it has no single author.*

4. Configuring the code formater:
	* Expand Java → Code style → Code formater
	* Choose Java Conventions and click Edit…
	* On the Indentation tab
	* Set the tab policy to Tabs only  
	* On the Blank lines tab set the number of blank lines to preserve to 10  
***Rationale:*** *More that 10 blank lines will be automatically compacted*
	* On the New Lines tab: Enable at end of file
	* On the Line Wrapping tab, set the maximum line with to 100  
***Rationale:*** *There is a lot of debate on this question but 80 columns is considered the standard for consfortably fitting printex text. We allow 100 because it is also possible to print in 132 columns. If our code needs to go beyond that limit that is a good indicator that the code is needing refactoring. Using more than that makes the code more difficult to follow and to print for people with smaller screens.*

5. Select Class Declarations :
	* Set Line wrapping policy to Wrap only when necessary
	* Check Force split for both for extends and implements clauses
6. Expand Function Calls and select Arguments
7. On Indentation Policy, select Indent on Column
8. On the Comments tab → On General Settings:
	* Disable Block comments
	* Enable Header comment formatting
9. On the Comments tab → Javadoc Settings:
	* Enable Remove blank lines in comments  
***Rationale:*** *Extra blank lines are not needed. Javadoc comment blanks lines, if needed, should be introduced using HTML (e.g using <br> tag).*

10. Disable New line after @param tags and click Ok  
***Rationale:*** *This makes comment blocks more compact, following SUN’s standard.*

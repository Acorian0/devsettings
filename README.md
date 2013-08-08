devsettings
===========

Contains the configuration files required to properly setup a development environment.

eclipse:
--------
The eclipse folder contains the java formatting style to be used and the checkstyle files.
The checkstyle files check that good Java programming practices are followed and have two sets of rules, one for the src folder and another for tests. The explanation on how to configure your eclipse to used these files can be found at smartbuildings.ist.utl.pt/blog/


maven:
------
The maven folder contains a exclusions/suppression's file to be used by the checkstyle maven plugin. Because the checkstyle plugin (for maven) doesn't support two sets of rules for different file sets (as of 08/08/2013), we use a suppression's file that will ignore certain checks for certain files. This suppression's file must be manually kept in sync the differences between the eclipse files.

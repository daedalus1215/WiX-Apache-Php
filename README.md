# WiX-Apache-Php
Apache and Php installer with WiX.

Simple Wix installer that is intended to install Apache and Php, to start the Apache service and set the appropriate environment variables for Apache and Php.

1. Stores both Apache and Php under ````C:\Programming\````	
2. Creates a system environment variable and links it to the ````PATH```` system environment variable. It is called: ````PROGRAMMING````
3. Creates values for ````PROGRAMMING```` system environment variable with the paths to ````C:\Programming\Php\bin```` and ````C:\Programming\Apache\bin````.
4. It pulls in the Apache files and directories
5. It pulls in the Php files and directories


There are a few things to note to get it to work.
1. You first need to create GUIDs for everything, for the ````UpgradeCode````, ````Component````.
2. Will need to download the Apache source files and Php source files, then paste them into the wix project's directory. 
3.0. Then when we have the Apache/Php source files we must heat them up to create 2 ````.wxs```` files that will be ````referenced```` in the WiX project. 
3.1. In order to bake Apache with the appropriate references to installation directories and component IDs for feature references I ran:
````
	heat.exe dir "Apache" -dr ApplicationApacheDirectory -cg ApacheFilesGroup -gg -g1 -sf -srd -out ".\ApacheFile.wxs" -sreg suppress registry harvesting
````
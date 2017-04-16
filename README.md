# WiX-Apache-Php
Apache and Php installer with WiX.

Simple Wix installer that is intended to install Apache and Php, to start the Apache service and set the appropriate environment variables for Apache and Php.

1. Stores both Apache and Php under ````C:\Programming\````	
2. Creates a system environment variable and links it to the ````PATH```` system environment variable. It is called: ````PROGRAMMING````
3. Creates values for ````PROGRAMMING```` system environment variable with the paths to ````C:\Programming\Php\bin```` and ````C:\Programming\Apache\bin````.
4. It pulls in the Apache files and directories
5. It pulls in the Php files and directories


There are a few things to note to get it to work.
1. You first need to create GUIDs for everything, for the ````UpgradeCode````, ````Component````. I just use Visual Studio's Tools > Create GUID feature.
2. Will need to download the Apache source files and Php source files, then paste them into the wix project's directory. I always place them under a directory called ````requisites````, which is on the same level as the wix project's ````Product.wxs```` file.
3. Then when we have the Apache/Php source files we must heat them up to create 2 ````.wxs```` files that will be referenced in the WiX project. This process is handled seperately in #4 and #5 below. 
4. In order to bake Apache with the appropriate references to installation directories and component IDs for feature references I run:
````
	heat.exe dir "Apache" -dr ApplicationApacheDirectory -cg ApacheFilesGroup -gg -g1 -sf -srd -out ".\ApacheFile.wxs" -sreg suppress registry harvesting
````
5. In order to bake Php with the appropriate references to installation directories and component IDs for feature references I run:
````
	heat.exe dir "php" -dr ApplicationPhpDirectory -cg PhpFilesGroup -gg -g1 -sf -srd -out ".\PhpFile.wxs" -sreg suppress registry harvesting
````
6. Open up ````ApacheFile.wxs```` and replace all references to ````SourceDir```` with ````requisites\apache````. That way we are linking to all the files and directories we need during the build of the ````.msi````.
7. Open up ````PhpFile.wxs```` and replace all references to ````SourceDir```` with ````requisites\php````. That way we are linking to all the files and directories we need during the build of the ````.msi````.
8. Copy over your newly adjusted .wxs files (#6, #7) and move them onto the same level as ````Product.wxs````. 
9. Within Visual Studio right click on the project, in Solution Explorer, and add existing file to project and add the two aforementioned files. 
10. If we want the two fragments, that install and start http service, then make sure we grab the directory ID of Apache's bin directory in the ````ApacheFile.wxs````, and paste it into the two fragments that install and start the http service respectively.

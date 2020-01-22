# How to create a new project

The easiest way to create a new goGPS project is by using the provided GUI or copying another existing project.

Open the goGPS interface by executing `goGPS` in the MATLAB Command Window.

![Menu Project New](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/menu_new_prj.png?raw=true)

* From the `Menu` choose `Project` -> `New`

goGPS will open this window:

![Create New Project GUI](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/new_prj.png?raw=true)

Fill all the fields.

1. **Project Time** - Select the type of project that you want to create, depending on this choice the settings of the new project will be adjusted to a suggested configuration. The possible choices are four:

    1. PPP Precise Point Positioning
    2. NET (short baselines) no iono - no tropo
    3. NET (medium baselines) no iono
    4. NET (long baselines) iono - free network

2. **Where to create** - select the directory that will contain the new project folder
3. **Project Name** - Provide a name for the new project (this will be also the name of the project folder)
4. **Observations folder** - select the directory in which the observations are stored, a subfolder structure can be present (e.g. `RINEX/YEAR/DOY/RINEX_FILE_NAME.YYO`) and the operation to perform:

    1. **Copy** the observations folder into the new project
    2. **Move** the observations folder into the new project
    3. **Keep** observations in the current folder
    4. Do not add any receiver now
5. Press "Create New Project". If no errors will occur, the project will be created and the goGPS interface for editing settings will be refreshed with the new parameters.

At this point, the new project can be customized and executed.

## **Useful links**

 * [Wiki settings page](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Settings) - for the meaning of all the parameters
 * [Wiki Command language page](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Command-language) - for a description of all the available commands

## Tip
A goGPS project is usually stored in a folder with a fixed structure, although all the directories could be changed in the settings file, we suggest to keep the original structure (see the section [Directories of goGPS](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Installation/_edit#directories-of-gogps)). Ephemerides and external resources can be stored within the project folder, but since the same resource could be used in different projects we keep them in folders common among all the projects.

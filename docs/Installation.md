# Requirements
goGPS have been developed to work in MATLAB environment, it specifically avoids to use any non-free toolboxes and mex-functions to avoid compilation and additional requirements.
A full license of MATLAB is needed to run the software from code, although in future compiled versions of goGPS will be released so that they will just require the free MATLAB runtime libraries.

### List of minimum software requirements:
 - **MATLAB 2016a** (needed for the extended usage of newer functions such as 'movmedian', 'movstd', ...)
 - **[GUI Layout Toolbox](https://www.mathworks.com/matlabcentral/fileexchange/47982-gui-layout-toolbox)** (free toolbox - needed for the graphical user interface)

### Additional requirements:
 - **[LAMBDA v3](http://saegnss2.curtin.edu.au/gnssweb/index.php?request=getlambda)** (unfortunately the redistribution of this useful set of algorithms to fix integer ambiguities cannot be re-distributed but a copy can be requested on [curtin webpage](http://saegnss2.curtin.edu.au/gnssweb/index.php?request=getlambda))
 - **[aria2](https://aria2.github.io)** (needed for speedup automatic downloads, a compiled version for windows is included in the goGPS package)
 - **Google Maps API** - to use Google Maps the user API key must be copied into a .mat file `<install_dir>/goGPS/utility/thirdParty/plotGoogleMap/api_key.mat`, this is a MATLAB file containing a single string variable called `apiKey` containing the string of your API KEY. (If you don't have an API key [request it here](https://developers.google.com/maps/documentation/javascript/get-api-key))

### Third-party functions included in goGPS:

A full set of additional third-party functions can be found in the folder goGPS/utility/thirdParty
they are redistributed with this software together with their license.

Many thanks to all the developers sharing their work for the entire community!

# Installation

The best way to try goGPS at the moment is through the GitHub repository that is continuously updated.
When any bug is found and reported on GitHub we try to fix it as fast as we can.

1. Clone the repo in the folder you prefer, either by the standard git clone command `git clone https://github.com/goGPS-Project/goGPS_MATLAB.git goGPS_MATLAB`, or by using your preferred Git client (e.g. Github Desktop, GitKraken, SourceTree, ...)
2. Install [GUI Layout Toolbox](https://www.mathworks.com/matlabcentral/fileexchange/47982-gui-layout-toolbox). It will be automatically prompted at the first execution of goGPS GUI
3. Request LAMBDA v3 to have its support for Integer Ambiguity Fixing
    * copy the LAMBDA folder into `<install_dir>/goGPS/positioning/lambda/lambda_v3`
4. Download and install [aria2](https://aria2.github.io) for faster downloads
    * **Windows**: nothing to do (a version of aria2 is already included in goGPS)
    * **Linux** (Debian based): `sudo apt-get install aria2`
    * **Mac OS** (Using [brew](https://brew.sh)): `brew install aria2`

# Directories of goGPS
When a clone of the repository is downloaded from GitHub a set of folders will appear in the installation dir.

* **docs** - contains a collection of some files useful for the development of goGPS
* **icons** - contains the set of icons to be used by the logging window
* **goGPS** - is the main folder containing the goGPS code
* **data** - is the folder where projects and resources are usually stored

A more detailed description of the dir structure of goGPS can be read in this nfo file: [dir_structure.nfo](https://github.com/goGPS-Project/goGPS_MATLAB/blob/goGPS_1.0_beta/docs/DIR_STRUCTURE.nfo)

```
goGPS_MATLAB
    │
    ├─ DOCS                            // Collection of some files useful for the development of goGPS
    │
    ├─ goGPS                           // Main folder containing the goGPS code
    │   ├─ last_settings.ini           // Store the latest setting in use
    │   ├  ...
    │   └  ...
    │
    ├─ ICONS                           // Icon set for the log window
    │
    └─ DATA
        │
        ├─ Antenna
        │   └─ ATX                     // ANTEX file
        │
        ├─ Satellite
        │   ├─ CLK                     // clk files
        │   ├─ EPH                     // Ephemeris files (e.g. igs SP3 files)
        │   ├─ CRX                     // CRX files storing Satellite problems
        │   ├─ ATM                     // Atmospheric error grids / models
        │   ├─ SBAS                    // Downloaded SBAS data
        │   │   └─ EMS                 // EMS files from the EGNOS Message Server, via Internet
        │   └─ DCB                     // Differential code biases
        │
        ├─ Reference
        │   ├─ Geoid                   // Geoids for orthometric correction
        │   └─ DTM                     // DTM for reference altitude
        │
        ├─ Station
        │   ├─ station_info.ini        // Info of the station
        │   ├─ network_info.ini        // Info of the network (FTP info)
        │   ├─ < MET >                 // Meteorological data files
        │   ├─ < CRD >                 // Permanent station positions
        │   ├─ < SNX >                 // Sinex files, global solutions
        │   └─ < Ocean >               // Computed ocean loading
        │
        └─ Project
            └─ TestName                // Storing a project
                ├─ Config
                │   └─ config.ini      // config file of the project
                │                      // (storing ALL the goGPS parameters)
                │
                ├─ RINEX               // All the RINEX files of the project
                │   └─ < Year >        // Year corresponding to GPS time
                │       └─ DOY         // DOY corresponding to GPS time
                │
                ├─ < Ref_path >        // <opt> reference paths
                │
                ├─ < Antenna >         // <opt> use the global dir if not present
                │   └─ < ATX >         // <opt> use the global dir if not present
                │       └─ < custom >  // <opt> put custom .ATX here
                │
                ├─ < Stations >        // <opt> use the global dir if not present
                │   ├─ < CRD >         // <opt> use the global dir if not present
                │   ├─ < MET >         // <opt> Meteorological data files
                │   └─ < Ocean >       // <opt> use the global dir if not present
                │
                └ Out
```

# Execution
To use go GPS you just need to move into goGPS folder and execute goGPS

1. Move into goGPS folder `cd <install_dir>/goGPS`
2. Execute `goGPS`

If everything works a GUI like this should appear on your screen:
![goGPS Main Window](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_MainWindow.png?raw=true)

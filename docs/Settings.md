# goGPS Settings

The settings for running a project in goGPS are completely stored in a unique text file (.ini).
This is fully mapped to an object instance of a class Main_Settings and always automatically exported with all the comments necessary to understand the meaning of the parameters.
The .ini file can be edited using a normal text editor and passed to goGPS to run automatically.
You can also modify 99% of the settings using the goGPS Edit GUI that will be automatically loaded on goGPS start, the remaining 1% of the settings can be found together with the plain text ini file in the Advanced TAB of the interface.

![goGPS Advanced Tab](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_Advanced.png?raw=true)

In the following sections, it is possible to find a description of all the elements present in the current goGPS Edit Settings Interface and the corresponding parameter present in the setting file.

## Index of contents

The goGPS Edit interface is structured with a Menu, a Main SideBar, a list of Tabs and a bar in the bottom:

1. [Menu](#menu)
2. [Sidebar](#sidebar)
3. [Tabs](#tabs)
   1. [Tab Advanced](#tab-advanced)
   2. [Tab Resources](#tab-resources)
   3. [Tab Commands](#tab-commands)
   4. [Tab Data Sources](#tab-data-sources)
   5. [Tab Receiver Info](#tab-receiver-info)
   6. [Tab Pre-Pprocessing](#tab-pre-processing)
   7. [Tab Processing](#tab-processing)
   8. [Tab Regularization](#tab-regularization)
   9. [Tab Atmosphere](#tab-atmosphere)
3. [Bottom bar](#bottom_bar)

Note: In the main window flags on the left of file or directory paths indicate if the file/directory exists or if it is missing

## Menu

The goGPS menu allows performing simple operations and opening tools or other windows. It is still under development and will be expanded in the future. 

At the moment it allows to do the following operations:
* **goGPS**
   - **About** open the about window
* **Options** contains a set of combo modifier of all the settings to perform basic processing
   - **Set for PPP troposphere estimation**
   - **Set for NET solution (short baselines - ignore ionosphere - ignore troposphere)**
   - **Set for NET solution (medium baselines < 20km - ignore ionosphere)**
   - **Set for NET solution (long baselines - iono-free)**
* **Project**
   - **New** open a window to create a new project. The automatic procedure will guide the user to generate a project folder with its main structure and a new generic settings file.
   - **Load** load a .ini config file
   - **Save** save the current configuration on the current .ini file, the current file is displayed in the bar at the bottom of the interface.
   - **Save As** save the current configuration at the given location

[Go back to the Index of contents](#index-of-contents)

## Sidebar

On the left side of goGPS Edit Interface, there is a side-bar containing basic information about the data that will be available for processing. 

A section dedicated to the processing *sessions* is located at the top, it contains:
* a **Check** button to update the Session limits in the case they are not automatically updated
* **First** and **Last epoch** to be processed
* **Duration** (length) of each single session 
* **Buffer** dimension in seconds of the data intervals to be used in addition to one of the sessions. (e.g. with a session of 86400 seconds - one day - and buffers of 10800 seconds - 3 hours - a total of 30 hours of data will be processed for each session, each session will overlap with the next, thus allowing a smoother estimation of tropospheric parameters and a better arc ambiguity estimation)

A section dedicated to the **receiver list** is at the bottom of the sidebar. There you can find 3 buttons:

1. **Check** to refresh the list of receivers when is not automatically updated
2. **Plot** to generate a plot showing the period covered by each RINEX file that will be used for the project
3. **Trackings** to generate a table displaying all the code observation types tracked (works best on RINEX 3 files, this tool is still experimental)

Below there is a list of all the stations of the project. For each station, two columns are reporting how many requested files are present or missing (e.g. in the processing of 1 month of daily files some of the RINEX could be missing)

[Go back to the Index of contents](#index-of-contents)

## Tabs
At the right of the sidebar, some tabs are containing the various global settings that can be defined

### Tab Advanced
This tab is used to change all the settings including the hidden ones.
![goGPS Advanced Tab](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_Advanced.png?raw=true)
In the **editor** a copy of the current ini config file is present and it can be modified directly. Two **buttons** are used to refresh the UI when the ini file is changed.

**Communication dir** is the directory used by goGPS parallelism to communicate with the slave processors. This must be seen by all the Slave processes and the Master (a.k.a. the MATLAB process running the full goGPS)

[Go back to the Index of contents](#index-of-contents)

### Tab Resources
The Resource tab contains all the settings defining the kind of orbits that need to be used, and the location of all the downloadable resources. **Note:** the Antenna file, the Geoid, and the Geomagnetic reference field are provided together with the code of goGPS, and updated constantly on the GitHub repository, while all the other resources are **automatically downloaded** and saved into the specified folders by goGPS (if the download flag is checked).
![goGPS Resources](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_Resources.png?raw=true)
In this tab many commands and fields are present, the first information that can be seen here is the location of the remote_resources.ini file that can be found at the top of the tab. This **resource file** contains all the information needed to find and download a goGPS resource (e.g. orbits, clock errors, Vienna Mapping Functions), together with their names. Generally, this file must never be changed, a more detailed guide on about file will be added to this wiki.
To use orbits or other resources from a center not available in the list of goGPS centers the remote_resource.ini file must be modified manually (if you have a center to suggest and want it to be added to the remote_resource file open a ticket in GitHub and we will add it as soon as we can).

List of commands/fields:
* **"Allow automatic download [...]"** - this command is a flag, if enabled it allows goGPS to search for the resources it needs online (always recommended)
* **Center** pop-up list -  this control allow the select the kind of resources/provider to be used, the main centers are already available in goGPS: IGS, EMR, ESOC, JAXA, CODE, ... at the left of the command, the list of supported constellations is reported for the active choice. E.g. selecting JAXA the supported constellations are "GRJ", each letter represents a constellation:
   - G GPS (US)
   - R GLONASS (Russia)
   - E Galileo (EU)
   - J QZSS (Japan)
   - C Beidou (China)
   - I IRNSS (India)
* **"Center orbit type preference"** - contains a set of flags to specify the kind of orbit to be used. If more choices are selected the leftmost resource is the primary choice, when a type of resource (e.g. final orbits) is not present (even on a remote server) the next best solution is the one that will be used (e.g. rapid orbits)
* **"Center iono type preference"** - similarly to the orbits different types of ionospheric models are available with descending quality from left to right
* **"Resource tree inspector"** - this field cannot be modified, it just shows the remote location of all the resources that are required. All the information it displays are built on run-time from the remote_resource.ini file
* **"Reset all resources path"** - this button comes in handy when importing a project from another computer, in the settings file the location of the resources may be expressed as a full path and it might be no longer existing in the importing computer. Pressing this button all the resources paths will be reset to their default values. The green/red flags shown in this tab are useful to understand if a file/folder is missing.

Below the button follows the list of all the local paths of the resources.
* Special time keywords can be used in the paths, they will automatically take all the values they can assume in the sessions span:
  - ${WWWW}    _4 char GPS week_
  - ${WWWWD}   _4+1 char GPS week + day of the week_
  - ${D}       _1 char day of the week_
  - ${3H}      _2 char GPS hour (00, 03, 06, 09, 12, 15, 18, 21)_
  - ${6H}      _2 char GPS hour (00, 06, 12, 18)_
  - ${HH}      _2 char GPS hour_
  - ${QQ}      _2 char GPS quarter of hour (00, 15, 30, 45)_
  - ${5M}      _2 char GPS five minutes (05, 10, ... , 55)_
  - ${YYDOY}   _2+3 char GPS year + day of year_
  - ${YYYY}    _4 char GPS year_
  - ${YY}      _2 char GPS year_
  - ${MM}      _2 char GPS month_
  - ${DD}      _2 char GPS day_
  - ${DOY}     _3 char GPS day of the year_
  - ${S}       _1 char session_

* **"Antenna (ATX) filename"** - here the user has to set the path that points to the antenna file (usually the last IGS file available). This file contains information about Phase Center Offsets (PCOs) and Phase Center Variations (PCVs) of the antennas of the satellites and the receivers. It is a mandatory file, without it, the error in computing a solution can be extremely high. When the antenna for a certain receiver is not found with the proper radome the "NONE" radome is used instead. If no information about a frequency is found the data corresponding to the closest frequency will be used. If no information about a constellation is found the data corresponding to GPS with the closest frequency will be used. **Note:** additional antenna files (in ANTEX format) with custom values can be passed to goGPS, to load them they must be saved into a folder contained in the home of the project you are running, this folder is `<prj_home>/station/ATX/custom/`, the file can have any name but its extension must be `.ATX`.
* **"Geoid local path"** - in this line the user has to set the location of the geoid grid file (in MATLAB .mat format) used to compute orthometric heights, note that passing a grid of a quasigeoid in here will lead to use normal heights (look at [Vanicek et al 2012](https://www2.unb.ca/gge/Personnel/Vanicek/Vanicek-et-al_CGG-42-1_PROOF.pdf) for more info). For 99.9% of the processing, the included goGPS grid of EGM2008 is more than enough; when a point does not fall on the exact knot of the grid a bilinear (or cubic) interpolation is performed.
* **"CRX path"** - this is the path to the folder containing CRX files (satellite problem file), these are automatically downloaded from the CODE FTP server, they contain useful information on the status of the satellites (maneuvers, outage, and other unusual behaviors).
* **"Eph local dir"** - orbits ephemerides are loaded from their standard exchange format SP3, this is the location of storage of them, the name of the orbit is automatically generated depending on the selected "center orbit type". The orbits are automatically downloaded in the right location by goGPS but it is possible to put them manually in their local location.
* **"Clk local dir"** - similar to the ephemeris path this is the path of the corresponding clock file of the orbits. Some orbits have different clock files (e.g. @30 seconds, @5 seconds) they are chosen with the priority sequence listed into the remote_resource file. Generally, also this path must not be changed, goGPS will try to download the resources it needs by itself.
* **"ERP local dir"** - this is the folder containing the Earth Rotation Parameter files to be used together with their corresponding orbits.
* **"IONO local path"** - this points to the local folder where ionospheric models will be downloaded by goGPS. They can be used to reduce the ionospheric delay to allow a better single-frequency positioning or to just help convergence and improve interpolation techniques.
* **"IGRF local path"** - this is the folder pointing to the International Geomagnetic Reference Field Schmidt semi-normalized spherical harmonic coefficients, usually the user does not need to change this files or folder cause they are provided together with the goGPS code.
* **"DCB local path"** - this points to the local folder where the Differential Code Biases will be downloaded by goGPS. Differential Code Biases are automatically used by goGPS when available.
* **"VMF local path"** - this points to the local folder where the Vienna Mapping Function files are automatically downloaded by goGPS. 
* **"ATM local path"** points to the local folder where the Atmospheric Loading files are automatically downloaded by goGPS. 

[Go back to the Index of contents](#index-of-contents)

### Tab Commands
This is the main tab where the sequence of commands that goGPS will perform during his execution are defined.
![goGPS Commands](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_MainWindow.png?raw=true)
The components that can be seen here are just 3:
* On the right there is an editor containing some command list *Execution examples* for various kinds of processing. The user cannot interact with this, it can only copy parts of the code present here.
* On the left there is another editor where the user can type to commands to be executed by goGPS. The commands must be written according to the [goGPS command language](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Command-language#gogps-command-language)
* At the bottom of the editor, there is a button that will open a window that displays a simple command language help with the list of all the commands available and their corresponding parameters.
![goGPS Language Help](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_LanguageHelp.png?raw=true)

[Go back to the Index of contents](#index-of-contents)

### Tab Data Sources
In this tab, the user defines the location of the observation files, the period of processing, and the size of the sessions.
![goGPS Data Sources](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_DataSource.png?raw=true)

* **"Project home directory"** - this field points to the <prj_home>. The project home is the folder containing all the folders of the project. It is very important to notice that all the relative paths that can be used in the configuration start from this folder, this allows having a portable project as long as into the .ini file all the other resources paths are written as relative paths and everything is moved together. The GUI automatically translates full paths to relative paths.

**Sessions**
In the sessions panel is possible to define the limits of epoch to process, the time keywords used for the definition of the paths will be substituted accordingly to these.
As long as the resources and observations files are defined with time keywords goGPS allows the definition of flexible limits (e.g. it is possible to process daily sessions even if the observations are stored into hourly RINEX files, and vice-versa)
* **"Start"** - marks the first epoch to process.
* **"Stop"** - marks the last epoch to process.
* **"Session duration"** - this is useful to limit the number of epochs to batch process, or to get solutions at higher rates.
    1. e.g. 86400 seconds - will process 24 hours of data all together in a unique LS adjustment;
    2. e.g. 3600 seconds - will generate independent (if no buffer is selected) solutions every hour of data to have a series of hourly positions.
* **"Buffers"** - buffers are an almost unique feature of goGPS, they can be very useful to limit border effects. The epoch in the buffer will concur to the joint LS solution but they will not affect the dimension of the sessions. Only the parameters estimated in the session will be exported in the output receiver object (buffers are never considered).
    1. e.g. [14400, 14400] seconds - means 3 hours for each buffer, if the session is of 86400 seconds this will make goGPS process a total of 30 hours of data in a unique solution, each session will overlap the next one for a total of 6 hours
* **"Smooth troposphere at boundaries"** - this is a flag to be used only when buffers are set different from 0. When set, the tropospheric parameters estimated in the overlap with another session will be merged thus allowing a smoother solution at the change of session.
* **"Separate coordinates at boundaries"** - when set, the buffer will be used for the estimation of the tropospheric parameters and the phase ambiguities, but not for the positions. A separate set of coordinates will be estimated for each one of the two buffers, this improves the ambiguity estimation and limits the border effect but should not influence (just minimally) the estimated position for the defined session.
* **"RINEX based session"** - this flag forces the dimension of the session to be exactly the one of the RINEX observation files (this dimension should be homogeneous among all the files).
* **"Session character list"** - RINEX2 standard naming convention uses a single character to define the session, in these fields it is possible to define the characters to be used. Usually, these are just ["0", "0", "0"] for daily files and ["abcdefghijklmnopqrstuvwx", "a", "x"] for hourly files.

**Stations**
* **"Observation directory"** - this is the root folder containing all the observation files, relative paths in the next field start from here. When selecting a folder containing observation files goGPS try to add all the station present in there to the processing.
* **"Observation files"** - contains the names and relative paths of the files of the stations (receivers) to be processed. Note that goGPS accept RINEX2 and RINEX3 observations files only.
* **"Recursive get marker names"** - using this button is possible to add stations in a recursive way starting from a directory.
 
[Go back to the Index of contents](#index-of-contents)

### Tab Receiver Info
This is a tab reserved for receiver specific parameters. In here in the future, it will be possible to set individual parameters for each station to override the generic ones (e.g. trackings to use, cut-off, etc...) but at the moment only coordinates can be provided. At the bottom of the tab, the user can set the path to the directory containing the multipath maps files and the location of the ocean loading file.

![goGPS Receiver Info](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_RecInfo.png?raw=true)

* **"CRD filename"** - points to the coordinate file of all the stations. This is the only field in this tab to have a corresponding matching in the `.ini` setting file, goGPS import the coordinate file before starting the computations, for this reason, any **unsaved modification** to the coordinates will be **ignored** during the processing.
* **"Coordinate table"** - this is a table containing information loaded from the coordinate files, to modify them the user can directly manipulate the file with a text editor or through this interface. At the moment the data stored into the coordinate file are:
   - Earth Centered Earth Fixed (ECEF) XYZ coordinates
   - a flag explaining how to consider these coordinates
       - **"0"** use the coordinates as a rough a-priori position (>100 meters of error) 
       - **"1"** use the coordinates as a good a-priori position
       - **"2"** consider the coordinates as fixed (they will not be estimated by goGPS) 
       - **"3"** consider the coordinates as fixed only by the pre-processing (they will not be estimated by goGPS in the pre-processing step unless detected to be not good enough), this allows a faster and more homogeneous pre-processing and outlier detection.
   - Temporal validity limits of the coordinates
   - Speed of the coordinates as a linear trend. The reference epoch for computing coordinates at a different time is the "start" epoch.
* **"Clear All"** - clear the coordinates table.
* **"Add a line"** - adds an empty line to the coordinates table.
* **"Remove selected"** - remove lines with selected cells from the coordinates table.
* **"Import from RINEX"** - will try to fill the table of coordinates reading the XYZ values from the header of the RINEX files that will be read by goGPS during its execution.
* **"Save"** - will save the current status  of the coordinates as visualized in the table to the current coordinate file 
* **"Save as"** - will save the current status  of the coordinates as visualized in the table asking for the path of the file
* **"Save (Default)"** - will save the current status of the coordinates as visualized in the table to a file named `stations.crd` in the folder `<prj_home>/station/CRD/`
* **"Inspect Trackings"** - generates a table displaying all the code observation types tracked (works best on RINEX 3 files, this tool is still experimental)
* **"ShowMap"** - will display a map of the stations at the coordinates present into the table overlaying Google satellite images. This tool can be useful for a fast overview of the stations.

* **"Multipath mitigation dir"** - this is the directory containing receiver specific multipath maps.
* **"Ocean loading filename"** - this is a resource that cannot be downloaded automatically and is strictly related to the stations. The resources tab requires little modifications while this field is typically checked every time a station is added, for this reason, it is positioned just after the definition of the observation files. Ocean loading are stored into a file with `.blq` extension and they have to be generated by [Chalmers website](http://holt.oso.chalmers.se/loading/) using the tide model FES2004. To help the user requests this file with the missing tides the button **"Get missing BLQ"** can be pressed, it will open a window containing the code to copy and paste in the Chalmers website. When they send the ocean tide coefficients via e-mail the user should manually create or append them to the file pointed in this field.
![goGPS Ocean Loading Helper](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_OceanLoading.png?raw=true)

[Go back to the Index of contents](#index-of-contents)

### Tab Pre-Processing
In this tab, the user can select the data to be processed, set some thresholds for the pre-processing and activate some input modifiers.
![goGPS Pre-Processing](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_PrePro.png?raw=true)
* **"Constellation Selection"** - this panel allows to activate and de-activate some satellite systems / frequencies. **Note:** 
   1. the user needs to select the proper orbits for the enabled constellations (e.g. use MGEX orbits when various systems are selected);
   2. when processing with the combined engine only the best frequencies are used for creating an iono-free combination;
   3. be aware that due to the inter-frequency clock bias with L5 not automatically handled by goGPS, selecting GPS L5 may produce bad results;
   4. to use more than two frequency at a time the uncombined engine must be used for the processing;
   5. if a frequency/constellation is not present in a receiver goGPS should be able with the remaining observables.
* **"Min satellite per epoch"** - under this threshold the epoch is discarded from the processing (default value is 2).
* **"Data cut-off angle"** - during the preprocessing, after the estimation of a good position of the receiver, all the observations of a satellite below this elevation are deleted from the workspace and not considered in the rest of the processing.
* **"SNR absolute threshold"** - Signal to Noise Ratio provides a good insight on the quality of the observations registered by the receiver, in particular, it is severely affected by multipath. When this index goes below a certain value it indicates that the observations (especially the pseudo-ranges) might be degraded, setting a threshold allows goGPS to discard these bad data. For more information see the function _remUnderSnrThr_ in _Receiver_Work_Space.m_.
* **"SNR scaled threshold"** - different trackings (e.g. GPS C1C, C1W or GLONASS C1P and C1C) and different frequencies have different SNR scaling. Setting a threshold on 28dB might be good for one tracking, but the same value for another could delete too many values. goGPS try to rescale all the SNR from different systems/frequencies/trackings to the one of C1C (or comparable) to be able to set up a unique SNR limit valid for all the observables instead of setting a different one for each observable. A value between 25-32 dB is generally good. To scale the SNR a rough estimation of the pseudo-ranges noise is performed reducing the data with their synthetic version, biases are removed and the standard deviation of the residuals is computed on a moving window arc by arc. 
Pseudo-ranges below the threshold are deleted from the receiver before the last estimation of the code-only solution in pre-processing. For more information see the function _remUnderSnrThr_ in _Receiver_Work_Space.m_. In the following figure, the level of noise of a WTZZ (Wetzel) station for GLONASS C1 is compared, C1P has lower noise but also lower SNR, its SNR is scaled to have similar values for the same level of noise of C1C.

![goGPS clock re-alignment](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/SNR_Scaling.png?raw=true)

* **"Min arc length"** - goGPS will delete any arc smaller than this value, the number is expressed in epochs and not seconds. Remember that the ambiguity of a small arc is generally badly estimated, a reasonable value for this parameter in a 30 seconds processing is 10-12 epochs

* **"Max code positioning error"** - if the pre-processing of a station returns a sigma0 (from the LS adjustment) larger than this value, the station is considered having problems, pre-processing fails and PPP or network adjustment can not be computed.
* **"Max code observation error"** - all the pseudo-range observations having a residual from the last step of pre-processing larger than this value are deleted and a new LS adjustment is performed without them.

* **"Trackings combination"** - when this flag is selected carrier-phase observations from multiple trackings of the same frequency are combined using their approximate noise levels as weights, tracking biases and cycle-slips not present on all the trackings are estimated and corrected (experimental - default "on"). 
For more details see the function _combinePhTrackings_ in _Receiver_Work_Space.m_. To estimate the noise level of each carrier-phase (_ph_) and then the weights the following algorithm is applied:
   1. `using the position computed at the end of pre-processing (code positioning) compute the synthesized pseudo-ranges (_spr_) for the satellites seen by the receiver`
   2. `compute the residuals: _ph_ - _spr_`
   3. `remove relative tracking biases`
   4. `compute the moving variance (5 epoch window) for each tracking of the same observable (e.g. L1C L1W)`
   5. `compute a cubic interpolation of the variance with respect to the elevation`
   6. `compute the weight as 1 / this interpolated index`

* **"Clock re-alignment"** - the satellite clocks estimated by many providers have severe jumps at the end of the day. These are spanning from some centimeters to a few meters; selecting this flag goGPS will try to compensate and correct this jumps (experimental - default "off"). This effect is probably due to the independent estimation of the clock errors among different days. For more details see the function _addClk_ in _Core_Sky.m_. Here follows an example of this jump, seen as a spike in the clock drift of a satellite (expressed in meters):


![goGPS clock re-alignment](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_clock_re_alignment.png?raw=true)


The effect of this flag influences the solution near midnight GMT when two sets of old and new ephemeris connect. Removing this jump improves the smoothness of the orbits and thus the outlier detection phase that otherwise may find false-positive outliers. In the following example using different orbit providers and processing the ZIMM (Zimmerwald) station with the PPP uncombined goGPS engine we can see how the orbit of GFZ and CNES produce larger deviation in the estimated ZTD with respect to the other solutions:


![goGPS clock re-alignment off](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_u2_clock_re_alignment_off.png?raw=true)


when the re-alignment flag is selected the solutions change showing the following behaviors:


![goGPS clock re-alignment on](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_u2_clock_re_alignment_on.png?raw=true)


solutions computed with GFZ and CNES orbits improve, while some of the others are now worse. Use this flag carefully checking case-by-case if this adjustment improves the solution.

The correction algorithm at the moment is very simple and works as follows:
1. `Take 200 epochs at the left of the file change epoch, and 200 at the right`
2. `compute std of the drift of the two subsets of data`
3. `if the jump is greater than the standard deviation compute a correction`
4. `(correction) perform a linear interpolation of the two subsets predicting one epoch of the clock from the two sides and offset the right side to minimize the jump`
An offset in a satellite clock in a network solution causes no problems, while in PPP might influence the ambiguity estimation of an arc, this correction is still experimental and needs additional investigations to be applied safely. 

**PPP corrections** - This panel allows applying all the corrections needed for a Precise Point Positioning. All the corrections are applied at the end of the pre-processing phase, using fixed coordinates. Although all these corrections are not necessary for a network adjustment, applying them allows a better outlier detection. Among all the available corrections "Atmospheric Loading" and "High Order Ionosphere" are the ones with a lesser impact and can be disabled safely. 
The list of corrections that can be applied to the GNSS observations are:
 * **"Receiver PCO/PCV"** - phase center offsets and variations, the magnitude of this effect depends on the antenna type
 * **"Solid Earth Tide"** - tides due to Earth's crust movements (tens of centimeter).
 * **"Pole Earth Tide"** - tides due to the wobbling of the Earth rotation axis (up to 2.5 cm vertical - 0.7 cm horizontal).
 * **"Phase Wind Up"** - affect only phases due to the electromagnetic nature of circularly polarised waves, it depends on the relative orientation of satellite and receiver antennas (some centimeters).
 * **"Shapiro Delay"** - relativistic effect (less than 2 cm).
 * **"Ocean Loading"** - due to ocean tides deformations of the seafloor and a surface displacement of adjacent land (few centimeters).
 * **"Atmospheric Loading"** - this effect is due to the variation of the atmospheric pressure.
 * **"High Order Ionosphere"** - effect due to the second, and third-order due to the signal passing through the ionosphere.
 * **"Use a-priori Iono Model"** - effect of ionosphere computed with the model selected in the [Athmosphere tab](#tab-atmosphere), this is very useful in case of ionospheric interpolation.
 * **"Multipath mitigation"** - with this pop-up menu the user can choose to try to mitigate multipath using previously generated multipath maps. It is possible to choose between a map based on a Zernike polynomials expansion (smoother) or the version with restored the higher frequencies of staking the residuals, see the command section **[Multipath management](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Command-language#multipath-management)** for further details.

![MP final maps](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/MP_Final.png?raw=true)
_On the left the smoother solution, on the right the highest details of the staking are preserved_

[Go back to the Index of contents](#index-of-contents)

### Tab Processing
In the processing tab, the user can manipulate the options (PPP and Network adjustment) of the processing engines and select the output to be kept in the output container object.

![goGPS Processing](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_Processing.png?raw=true)

**Coordinate estimation**
 * **"Separate antenna center for each GNSS"** - when selected goGPS internally computes different phase antenna centers for each constellation
 * **"Additional coordinates rates"** - this function at the moment is used mostly for debugging, it allows to compute up to 3 separate coordinates sets at a different rate. To view their computed values you need to access directly to the object `GNSS_Station`, at the end of execution they will be in `rec(rec_number).work.add_coo` (for the last computed work-space) and `rec(rec_number).out.add_coo` (for the complete series of outputs).

**Generic Options**
 * **"Observation weighting"** - Select the type of observation weighting function. These are the available options:
   - uniform
   - satellite elevation 
 * **"Max code observation err"** - the pseudo-range observations having residuals above this threshold are excluded from the LS adjustment (this threshold works only for the uncombined engine).
 * **"Max code observation err"** - the carrier phase observations having residuals above this threshold are excluded from the LS adjustment.

**Tropospheric options**
 * **Estimate ZTD** - when this flag is selected the Zenit Total Delay is estimated during the step of processing, it may be useful to disable this option executing a network adjustment of very short baselines.
 * **Estimate ZTD gradients** - this option enables the computation of north and east tropospheric gradients.
 * **Absolute tropo in-network** - activating this flag when performing a network adjustment with long base-lines the full tropospheric delay is estimated for each station.

**Ionosphere Options**
 * **Ionosphere Management** - here it is possible to select the kind of management for the ionospheric delay, this option is valid only when using the combined engine
   - **1 - Iono-free** - when selected goGPS uses the iono-free combination to process dual-frequency data.
   - **2 - Smooth GF** - when selected goGPS will use the geometry-free combination pf L1 and L2 (only these two) to compute a smooth ionospheric delay that will be removed from the observations. This is similar to using an iono-free approach but with a slightly smoother final result.
   - **3 - External-model** - when this option is selected (or only one frequency is logged by the receiver) no combinations are used, an a-priori external model should be applied (see tab pre-processing) to have reasonable results. The model to be used is set in the [Atmosphere tab](#tab-atmosphere).

**PPP only Options** 
These options are dedicated to the Precise Point Positioning adjustment.
 * **"PPP Snooping / Reweight"** - This pop-up menu allow the user to chose different types of outlier rejection / data re-wight. The list of methods currently available is:
   - **1 - none** perform neither snooping nor reweight
   - **2 - re-weight Huber** see Table2 in [Gökalp et al, 2008 - _Evaluation of Different Outlier Detection Methods for GPS Networks_](https://www.mdpi.com/1424-8220/8/11/7344/pdf) is applied on all the observations having residuals greater than a threshold level, set to be 2 * std(residuals)
   - **3 - re-weight Huber (no threshold)** Huber M-estimator applied on all the residuals
   - **4 - re-weight Danish**  see Table2 in [Gökalp et al, 2008 - _Evaluation of Different Outlier Detection Methods for GPS Networks_](https://www.mdpi.com/1424-8220/8/11/7344/pdf)  is applied on all the observations having residuals greater than a threshold level, set to be 2 * std(residuals)
   - **5 - re-weight DanishWM** Modified Danish M-estimator,  is applied on all the observations having residuals greater than a threshold level, set to be 2 * std(residuals)
   - **6 - re-weight Tukey** see Table2 in [Gökalp et al, 2008 - _Evaluation of Different Outlier Detection Methods for GPS Networks_](https://www.mdpi.com/1424-8220/8/11/7344/pdf)  is applied on all the observations having residuals greater than a threshold level, set to be 2 * std(residuals)
   - **7 - simple snooping** all the observations having residuals greater than 2.5 * std(residuals) are removed from the adjustment
   - **8 - smart snooping** it's a method based on two threshold levels, the first one triggers the removal of the observations until its residuals re-enter below the second threshold (e.g. detection on 6 sigma0, removal of data till their residuals are below 2.5 sigma0).
   - **9 - smart snooping + arc trim** like the previous but the second threshold is lower (2 * std(residuals)), furthermore, all the arc edges having residuals greater than 2 * std(residuals) are removed
 * **"PPP Try to fix Ambiguity (Experimental)"** - when this flag is enabled the PPP engine will try to fix ambiguities, note that this option at the moment works only with CNES orbits (they provide BIASes), receivers with precise code, and the combined engine.
 * **"Enable PPP for receivers containing only a single frequency"** - by default goGPS does not compute any PPP solutions for single frequency receivers due to the high ionospheric errors that might be introduced. In some cases, the ionospheric delay is removed from the observations by interpolation or models and the user might want to re-enable the PPP processing.

**Network only Options** 
These options are dedicated to NETWORK adjustment.
 * **"NET Snooping / Reweight"** - Network snooping and reweight have not been fully implemented. A different strategy is applied here: the uncombined engine just disregard all the observations set above the limit for maximum phase and code residuals in the generic options (see `Network.adjustNew` for more details); the combined approach instead have three options:
   - **1 - none** perform neither snooping nor reweight
   - **2 - simple 4 loops** - this procedure removes outliers iteratively on residuals. At first, try to remove the worse data till the solution (in maximum 4 iterations) converges to a final adjustment (see `Network.adjust` for more details)
   - **3 - 4 loops + remove bad satellites** In addition to the previous approach, goGPS remove "bad" satellites, i.e. all the observations having residuals not having zero mean (see `Network.adjust` for more details)
 * **"NET fixing approach"** - select the fixing approach to be used (remember that to use lambda3 you need to have requested the code for applying it), the current options are:
   - **1 - none**
   - **2 - lambda3 search and shrink**
   - **3 - lambda3 integer bootstrapping**
   - **4 - lambda partial**
   - **5 - bayesian**
   - **6 - bayesian best integer equivariant**
   - **7 - sequential best integer equivariant**

**Results to store** 
goGPS is object-oriented, every receiver is considered a unique GNSS_Station, internally the software stores data in two objects: _work_, and _out_. _Work_ is an object of class Receiver_Work_Space and contains the observations and everything is needed for the computation of the results of a single session, _out_ is an object of class Receiver_Output and collects the output of a receiver for all the sessions. The results stored in the _work_ object are pushed to _out_ only when the `PUSHOUT` command is issued. The following flags select the output to be kept. **Note:** the data stored in _out_ can take a lot of RAM space, this is the reason for the possibility of selecting what must be kept.
 * **"Dt (clock errors)"** - Clock Errors
 * **"PWV"** - Precipitable Water Vapour
 * **"ZWD"** - Zenith Wet Delay
 * **"ZTD"** - Zenith Total Delay
 * **"Tropo Gradients"** - Tropospheric gradients (North, East)
 * **"A-priori tropo"** - A-priori tropospheric delay as computed from the model
 * **"P / T / H"** - Pressure / Temperature / Humidity
 * **"Outliers / CS"** - Outliers and Cycle slips (per satellite to keep them small)
 * **"Quality (SNR)"** - This is an index that might be improved in the future (per satellite to keep them small), at the moment it is storing just SNR
 * **"Number of Sat. per Epoch"** - Number of satellites used per epoch (and by constellation) 
 * **"Azimuth / Elevation"** - Azimuth and Elevation of all the satellites in view
 * **"Combined Residuals"** - Residuals of the processing (one per satellite)
 * **"Uncombined Code Res."** - Residuals of the processing for pseudo-range observations (one per observables)
 * **"Uncombined Phase Res."** - Residuals of the processing for carrier-phase observations (one per observables)
 * **"Mapping functions"** - Mapping function of the troposphere (one per satellite)

**Output folder**
In this tab the folder of final storage of the results can be changed. Note that some results will be saved into sub-folders automatically created.

[Go back to the Index of contents](#index-of-contents)

### Tab Regularization
In this tab, the user can set all the regularization values to be used in the estimation process.

![goGPS Regularization](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_Regularization.png?raw=true)

**Regularization options**
 The regularization of the tropospheric parameters plays a very important factor in their estimation. goGPS is very flexible and many parameters can be tweaked:
 * **Clock regularization** - this is a critical parameter, usually it must not be changed, is set to a very high value meaning basically no regularization. (lower value means higher regularization).

**ZTD (and gradients) estimation** - all the options available for the estimation of ZTD are duplicated for ZTD gradients.
 * **Absolute regularization** - this parameter is rarely changed, a strong regularization causes goGPS to estimate a smaller correction to the a-priori value.
 * **First derivate regularization** - a regularization on the first derivate means to limit the speed of variation of ZTD, a smaller value causes a smoother ZTD estimation.
 * **Spline rate** - goGPS usually estimate an epoch by epoch ZTD but this rapidly increases the size of the normal equations matrix; when processing big networks it might be useful to speed-up the computations by using splines. 
 * **Order of the spline** - goGPS is able to use linear or cubic spline for computing ZTD; when "none" is selected the software will compute an epoch by epoch solution.

[Go back to the Index of contents](#index-of-contents)

### Tab Atmosphere
This tab is used to select the options for better management of the atmospheric effects.  

![goGPS Atmosphere](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_Atmosphere.png?raw=true)

**Ionosphere options**
 * **Ionosphere a-priori Model** - in this pop-up menu the user can select the type of a-priori model to be removed from the observations. A good model helps the outlier-detection and pre-processing phase. The available options are:
   - **1 - none**
   - **2 - Klobuchar** - standard Klobuchar model, it requires ionospheric parameters that are usually read from broadcast orbits (goGPS can automatically fetch them)
   - **3 - IONEX** - this is the most precise model that can be applied, it is read from IONEX files, depending on the preferred type selected in the resources tab it can be a model from observed values or just a prediction.

**Tropospheric options**
 * **Mapping function** - in this pop-up menu it is possible to select the tropospheric mapping function, at the moment only 3 models are available, the additional model can be implemented into _Receiver_Work_Space.getSlantMF_.
   - **1 GMF** - Global Mapping Function
   - **2 VMF gridded** - Vienna Mapping Function (they are automatically downloaded when needed)
   - **3 Neil** - Niel Mapping Function
 * **A-priori zenith delay** - this is the a-priori model to be used for computing tropospheric delays, two options are available:
   - **1 Saastamoinen from meteo data** - this option makes goGPS use as a-priori model Saastamoinen, the origin of the data to be used depends on the next field in the GUI
   - **2 VMF gridded zenith delays** - these are Zenith delays coming from the Vienna gridded maps of ZTD
 * **Meteo Data** - here the user can select the source of the meteorological data to be used:
   - **1 Standard Atmosphere** - Standard values of Pressure (1013.25 mbar), Temperature (18 °Celsius), Humidity (50 %)
   - **2 GPT** -
   - **3 MET files** - use the Meteorological RINEX specified in the following fields.
 * **MET files** - goGPS can use meteorological RINEX files to import temperature, pressure, and humidity from meteorological stations. It automatically interpolates the data at the right elevation and at the station positions (See Meteo_Data.getVMS for more details). The syntax for these files are like  

[Go back to the Index of contents](#index-of-contents)

## Bottom Bar
In the bottom bar, the user can find buttons to load and save the current settings and the full path of the current `.ini` file in use. And the button "**go!**"  to start execution.

[Go back to the Index of contents](#index-of-contents)


# goGPS command language
goGPS have been designed to be flexible and expandable. To allow different types of processing and complex operations a pseudo-language have been created. It allows sequential and parallel operations. On this page, the meaning of all the commands can be found. For a short Help on all the commands available type `goHelpCommands` in MATLAB command window.
![goGPS Language Help](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goGPS_LanguageHelp.png?raw=true)
## Index of contents
   1. [Receiver data organization](#receiver-data-organization)
   1. [Minimal script](#minimal-script)
      * [PUSHOUT](#pushout)
   1. [How to select sessions](#how-to-select-sessions)
   1. [How to select receivers](#how-to-select-receivers)
   1. [Main commands](#main-commands)
      * [LOAD](#load)
      * [PREPRO](#prepro)
      * [PPP](#ppp)
      * [NET](#net)
   1. [Looping on receivers](#looping-on-receivers)
   1. [Parallel execution](#parallel-execution)
      * [PINIT](#pinit)
      * [PKILL](#pkill)
      * [PAR](#par)
   1. [Plots and export](#plots-and-export)
      * [SHOW](#show)
      * [EXPORT](#export)
   1. [Validation](#validation)
      * [VALIDATE](#validate)
   1. [Ionospheric interpolation](#ionospheric-interpolation)
      * [SEID](#seid)
      * [SID](#sid)
      * [REMIONO](#remiono)
   1. [Multipath management](#multipath-management)
      * [MPTEST](#mptest)
   1. [Other Commands](#other-commands)
      * [RENAME](#rename)
      * [EMPTY](#empty)
      * [EMPTYWORK](#emptywork)
      * [EMPTYOUT](#emptyout)
      * [AZEL](#azel)
      * [BASICPP](#basicpp)
      * [CODEPP](#codepp)
      * [OUTDET](#outdet)
      * [FIXPOS](#fixpos)
      * [KEEP](#keep)
      * [SYNC](#sync)
      * [REMSAT](#remsat)
      * [REMOBS](#remobs)
      * [REMTMP](#remtmp)

## Receiver data organization
To be able to understand how goGPS works it is necessary to make a brief recap on the internal organization of the data. goGPS is object-oriented, every receiver is considered a unique _GNSS_Station_, internally the software stores data in two objects: _work_, and _out_. _Work_ is an object of class Receiver_Work_Space and contains the observations and everything is needed for the computation of the results of a single session, _out_ is an object of class Receiver_Output and collects the output of a receiver for all the sessions. The results stored in the _work_ object are pushed to _out_ only when the `PUSHOUT` command is issued. The following flags select the output to be kept. **Note:** the data stored in _out_ can take a lot of RAM space, this is the reason for the possibility of selecting what must be kept.

## Minimal script
The goGPS language uses a basic fixed structure to perform commands: an external `FOR` loop running on the sessions to process, and optionally internal loops on the receivers to process. Without a session loop, goGPS cannot do anything, this loop initializes the receiver work-spaces and prepares the needed resources (e.g. load orbits).

A basic example structure is the following:

```
FOR S*
   <some commands>
   PUSHOUT T*
END
<some commands>
```

Every `FOR` loop must be terminated with an `END`. Sessions must be selected according to "[How to select sessions](#how-to-select-sessions)" and receivers according to "[How to select receivers](#how-to-select-receivers)"

###PUSHOUT
To save the results of the processing in the out part of the receiver `PUSHOUT` is called on all the target receivers. At least one target receiver must be specified. The results to be copied from the receiver work-space (of a single session) to the receiver output (of multiple sessions) are specified in the [processing panel](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Settings#tab-processing)

**Usage example:**
```
PUSHOUT T*
```

## How to select sessions
After the keyword `FOR` the letter S (**S**ession) follows, to indicate the number of the session(s) to process iteratively various options are given to the user:
 * **`S*`** - the symbol `'*'` is used to indicate ALL the sessions, and will make goGPS iterate from session 1 (the first) to the last that is set to be processed;
 * **`S1,2,5,6`** - separating numbers with the `','` symbol makes goGPS process the session in the selected order;
 * **`S3:6`** - using the symbol `':'` it is possible to select a list of sessions, in this example, the sessions 3,4,5,6 will be processed;
 * **`S1:7:30`** - similarly to the previous case, like in MATLAB syntax it is possible to provide a _step_ to generate the list of sessions to be processed, in this example, the sessions 1,8,22,29 will be processed;
 * **`S10:END`** - the keyword `END` or `E` indicates the last session.

Combinations of the above are possible: e.g. **`S1:3,6,24:2:30`** will select sessions 1,2,3,6,24,26,28,30.

## How to select receivers
In goGPS language, the user needs to define the receivers that have to be processed or used for particular goals, some commands require different "types" of receivers:
 * **`T*`** this indicates **T**arget receivers, these are the receivers on which the commands will act directly;
 * **`R*`** this indicates **R**eference receivers, these are the receiver used by the commands for special purposes, e.g. reference receiver in a network adjustment, or reference receiver in an interpolation procedure;
 * **`P*`** this indicates receivers to be **P**assed as is, in a parallel loop the receiver indicates with the "P" letter will be sent to all the parallel slaves.
As for the sessions, the user can apply the same numbering convention of the sessions:
 * **`T*`** - the symbol `'*'` is used to indicate ALL the receivers, and will make goGPS iterate from receiver 1 (the first) to the last available;
 * **`T1,2,5,6`** - separating numbers with the `','` symbol makes goGPS process the receiver in the selected order;
 * **`T3:6`** - using the symbol `':'` it is possible to select a list of receivers, in this example, the receivers 3,4,5,6 will be processed;
 * **`T1:7:30`** - similarly to the previous case, like in MATLAB syntax it is possible to provide a _step_ to generate the list of receivers to be processed, in this example, the receivers 1,8,22,29 will be processed;
 * **`T10:END`** the keyword `END` or `E` indicates the last receiver. 

Combinations of the above are possible: e.g. **`T1:3,6,24:2:30`** will select receivers 1,2,3,6,24,26,28,30.

## Main commands
The minimal commands needed for processing GNSS data are three: `LOAD`, `PREPRO`, `PPP` or optionally `NET`, knowing these plus the session loop allows processing some data.

An example of PPP processing can be
```
FOR S*
   LOAD T*
   PREPRO T*
   PPP T*
   PUSHOUT T*
END
```

or a Network adjustment with the first receiver used as reference:

```
FOR S*
   LOAD T*
   PREPRO T*
   NET T* R1
   PUSHOUT T*
END
```

The [setting panels](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Settings#gogps-settings) are used to define the main options of these parameters but some modifiers can be used in the goGPS language commands.

Here it follows the description of each command. 

### LOAD
The load command allows the software to import data from RINEX files. The files that are going to be loaded are selected in [Data Sources tab](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Settings#tab-data-sources) and are relative to the current session. One or multiple targets need to be specified for the command to work. goGPS imports all the data at the rate stored into the RINEX files for all the constellation enabled in [Pre-Processing tab](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Settings#tab-pre-processing) unless some modifiers are used

**Command modifiers:**

 * **-s=<sat_list>** - active constellations (e.g. -s=GRE)
 * **@<rate>** - processing rate in seconds (e.g. @30s, -r=30s)

It is possible to load the data at a different rate (as long as it is a multiple of the data rate stored in the files) and for a subset of constellations. The software read the file in blocks but when these options are enabled goGPS will import only the requested data, i.e. importing a file containing data at 1Hz at a lower rate (e.g. 30s) is faster than importing its original rate.

**Usage example:**
_Import the observations for the GPS and Galileo constellations @30 seconds rate for the current session for all the receivers
```
LOAD T* @30s -s=GE
```

[Go back to the Index of contents](#index-of-contents)

### PREPRO
The pre-processing command is a mandatory command to prepare the loaded observations for further operations. In this function goGPS performs different tasks: 
 1. code **positioning** - needed for computing a good a-priori position of the receiver, this is done unless a-priori coordinates are provided with a coordinate file in [Receiver Info tab](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Settings#tab-receiver-info);
 1. computation of **clock delays** (from code observations)
 1. computation of **satellite positions** at the time of signals transmission
 1. **synchronization** of the receiver to the nominal epoch of observation - low-cost receivers, and some geodetic ones may read the observations with an incorrect inconstant rate. To avoid problems in network solutions, and to restore a constant observation rate goGPS apply a correction term to all the observations to "move" them to the proper epoch. This term is computed by taking the difference of the synthesized pseudo-ranges at the nominal (transmission) time and at the observed (transmission) time (e.g. shifted by a few milliseconds).
 1. computation of a-priori **atmospheric delays**
 1. application of all the **PPP corrections** enabled in the settings
 1. **outliers** detection

**Command modifiers:**

 * **-s=<sat_list>** - active constellations (e.g. -s=GRE)

**Tip:** 

It might be useful to speed-up computation to enable the data of a single constellation by using the **"s="** modifier. goGPS will only perform a faster a-priori position, but all the other observations of different systems will be pre-processed as if they are enabled. Meaning that PPP corrections, atmospheric delays, and outlier detection will be performed on all the data. The command as written in the following usage example can be adopted even when a multi-constellation final result is wanted.
 
**Usage example:**
```
PREPRO T* -s=G
```

[Go back to the Index of contents](#index-of-contents)

### PPP
Perform a Precise Point Positioning on all the target receivers. The strategy to be adopted for the atmospheric parametrization and for the applied corrections must be pre-selected in the settings and can be changed in the GUI before executing the processing.

**Command modifiers:**

 * **-s=<sat_list>** - active constellations (e.g. -s=GRE)
 * **-u** - (flag) use the **U**ncombined engine

When the modifier **-U** is passed to the command the uncombined PPP engine is selected and uncombined residuals are computed.

**Tip:** 

The uncombined engine for PPP solutions is a bit slower than the combined version, at the moment for most of the usage the combined engine is still the best choice.

**Usage example:**
```
PPP T*
```

[Go back to the Index of contents](#index-of-contents)

### NET
Perform a Network adjustment on all the target receivers, using the selected reference receiver(s). This command allows computing individual baselines (e.g. `NET T1:2 R1`) or full network adjustments with many receivers (e.g. `NET T1:10 R*`)

**Command modifiers:**

 * **-s=<sat_list>** - active constellations (e.g. -s=GRE)
 * **@<rate>**       - processing rate in seconds (e.g. @30s, -r=30s)
 * **--iono**        - reduce the system of normal equations for the ionospheric delay - this is an option to enable only for long baselines when the ionospheric effect on the network receivers cannot be neglected 
 * **L<band>**       - band to be used for single frequency adjustment (e.g. L1)
 * **--free**        - when the stations are distant (long baselines) they can be considered uncorrelated, use this flag to compute absolute coordinate position
 * **COO_CRD**       - export coordinates as a .CRD file - this export have been created for debugging testing but it is unsupported, use the `EXPORT` command instead
 * **--clk**         - export the common parameter in network - this export have been created for testing purposes of the combbined engine but it is unsupported
 * **-u**            - (flag) use the **U**ncombined engine

**Tip:** 

The uncombined engine for Network solutions is slower than the combined version but it can perform better and works on **long baselines**.

**Usage example:**
```
NET T* R1 -IONO -U
```

[Go back to the Index of contents](#index-of-contents)

## Looping on receivers
Now that the main commands have been described the for loop on receivers can be introduced. To use it the keyword `FOR` must be followed by the receivers to target and the subset of commands to be executed in the loop must be closed with an END keyword

**Usage example:**
_This example uses the `FOR` loop to perform PPP sequentially, then computes a Network Adjustment on all the receivers_
```
FOR S*
   FOR T*
      LOAD T$
      PREPRO T$ -s=G
      PPP T$
   END
   NET T* R* -free -iono -U
   PUSHOUT T*
END
```

Note that the `$` symbol is used within the target loop, this symbol indicates the current target. At each iteration, it will assume the value of the current target, in this case: 1,2,...till the number of receivers.

## Parallel execution
goGPS uses a custom implementation of parallelism based on a master-slaves approach. This approach may have different drawbacks but does not require the MATLAB Parallel toolbox. goGPS automatically executes new instances of MATLAB in the background (sometimes on the Windows platform some of them fail to start properly and they are visible), then for each of these new instances, the software will start a function capable of waiting or executing limited tasks. To start a slave manually a user can execute:
```
gos = Go_Slave.getInstance(); gos.live();
```
The MATLAB executing goGPS is the Master that dispatches jobs and collects results. All the communications are managed by writing and reading files on a disk, it is important for speed purposes that this folder is created on a fast drive. The "communication" directory path is set in the project ini file and can be modified in the [Advanced tab](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Settings#tab-advanced) of the GUI.

The class `Parallel_Manager` have been created to manage the parallel slaves:
 * **`Parallel_Manager.killAll();`** run this to kill background slaves
 * **`Parallel_Manager.testSlaves();`** run this to test the status of the background slaves
 * **`Parallel_Manager.requestSlaves(<num_of_slaves>);`** run this to initialize "num_of_slaves" slaves
 
To simplify the generation and maintenance of the slaves two goGPS commands have been created:

### PINIT
This command is used to initialize or check the existence of parallel slaves running in the background

**Command modifiers:**

 * **-n=<num of slaves>** - this is the only parameter (mandatory) that specify the number of background tasks to be executed in parallel (e.g. PINIT n=7, or alternative syntax PINIT N7)

**Usage example:**
```
PINIT -n=7
```

[Go back to the Index of contents](#index-of-contents)

### PKILL
This command has no parameters and allows to kill all the slaves in the background (all the MATLAB instances running slaves will be closed)

**Usage example:**
```
PKILL
```

**Note 1:** unfortunately due to bugs or unexpected operations some of the background instances may be still alive even after issuing a `PKILL` command. in this case, it is recommended to manually kill the MATLABs running in the background using a task manager.

**Note 2:** closing goGPS without issuing this command will leave the slaves running in background alive.

[Go back to the Index of contents](#index-of-contents)

### PAR
Once slaves have been created a parallel execution can be used. There are two types of supported parallel executions in goGPS:
 1. Parallel session processing
 2. Parallel targets processing

It is possible to use only one type of execution at a time. They work just like a `FOR` loop but with the keyword `PAR`

**Usage example 1:**
_This example uses the `PAR` loop to perform PPP processing on all the targets parallelizing the sessions_
```
PINIT N5
PAR S*
   FOR T*
      LOAD T$
      PREPRO T$ -s=G
      PPP T$
   END
   NET T* R* -free -iono -U
END
PKILL
```

**Note:** the PAR loop on sessions automatically executes `PUSHOUT` after loading the data processed by each slave. Slaves only save the work-space of each processed receivers and not the output part.

**Usage example 2:**
_This example uses the `PAR` loop to perform parallel PPP processing on all the targets for all the sessions_
```
PINIT N5
FOR S*
   PAR T*
      LOAD T$
      PREPRO T$ -s=G
      PPP T$
      PUSHOUT T$
   END
END
PKILL
```

**Note:** parallel slaves do not receive automatically all the receivers as stored in the Master process; when they start an execution they have empty receivers unless the modifier **P** is used. On the contrary, the object `GNSS_Station` (containing both `work` and `out`) is imported by the master process as is found in the Slave at the end of its computations.

**Command modifiers:**

 * **P*** - work-spaces to be passed to the slaves

This means that this code to compute all the baselines with respect to the first receiver is not working:
```
PINIT N5
FOR S*
   LOAD T1
   PREPRO T1 -s=G
   PPP T1
   PAR T2:END
      NET T1,$ R1   <= the receiver 1 for the slaves is empty
   END
   PUSHOUT T$
END
PKILL
```

while this is ok:
```
PINIT N5
FOR S*
   LOAD T1
   PREPRO T1 -s=G
   PPP T1
   PAR T2:END P1    <= the work-space of receiver 1 is passed to all the slaves
      NET T1,$ R1
   END
   PUSHOUT T$
END
PKILL
```

[Go back to the Index of contents](#index-of-contents)

## Plots and export
goGPS includes many functions for the visualization and export of the results, they are accessible as methods of some classes (e.g. GNSS_Station, Receiver_Work_Space, ...) that can be called from the MATLAB command window, as commands in the goGPS language, and from a dedicated GUI.

The methods of the classes that show plots or maps have the prefix `show`, while the export function prefix `export`.

Many options are available for creating plots, for this reason, a simplified GUI has been created. It can be opened by typing `goInspector` in the MATLAB command window, but it will be automatically displayed by goGPS at the end of its execution if the GUI is enabled.

![goInspector](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goInspector.png?raw=true)

This interface is still under development and more plots and exports will be added in the future.

### SHOW
The command `SHOW` allows creating plots and maps. It requires that results are already computed and stored in the receiver(s). There are many `SHOW` modifiers that can be used, each is a shortcut to a class method. The two tabs "plot 1" and "plot 2" in goInspector provide a fast method to call them manually

![goInspector_plot1_2](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goInspector_plot1_2.png?raw=true)

**Command modifiers:**

 * **-s=<sat_list>** -      Active constellations (e.g. -s=GRE) - not all the SHOW modifiers support this additional flag
 * **-e=<"name">** -        Export with name_postfix - if this flag is passed to the command `SHOW` the generated figures will be exported. "name" is optional and will be appended to an autogenerated name (stored in the UserData property of the MATLAB figure). Automatically goGPS will save the images in the out folder as "PNG" to request a different format "name" can be set to match a particular filetype (e.g. `SHOW T* ZTD -e=".pdf"` will save the image as PDF file, `SHOW T* ZTD -e` will save the ZTD of all the receivers as PNG)
 * **-c** -                 Close figure after export (valid only if the previous export modifier is present)
 * **MAP** -                Map of station coordinates (Google Maps Background) - (one plot for all the receivers)
 * **L_MAP** -              Legacy map of station coordinates (Google Maps background) - (one plot for all the receivers)
 * **G_MAP** -              Map of station coordinates (Google Maps Background) - (one plot for all the receivers)
 * **DTM_MAP** -            Map of station coordinates (DTM background) - (one plot for all the receivers)
 * **G_MAP_R** -            Map of station coordinates (Google Maps Background) + RAOB - (one plot for all the receivers)
 * **DTM_MAP_R** -          Map of station coordinates (DTM background) + RAOB - (one plot for all the receivers)
 * **DA** -                 Data Availability - (one set of plots per receiver)
 * **ENU** -                East Nord Up positions - (one plot per receiver)
 * **PUP** -                Planar Up positions - (one plot per receiver)
 * **ENUBSL** -             East Nord Up baseline - this modifier requires to specify both targets and reference receiver
 * **PUPBSL** -             Planar Up baseline - this modifier requires to specify both targets and reference receiver
 * **XYZ** -                XYZ Earth-Fixed - Earth-Centered positions - (one plot per receiver)
 * **CKW** -                Clock errors of the last session - (one plot per receiver)
 * **CK** -                 Clock errors - (one plot per receiver)
 * **MP1** -                Zernike interpolated multipath maps stored in the target receiver (polar plot) - (one plot per frequency/receiver)
 * **MP2** -                Multipath stacking maps stored in the target receiver (polar plot) - (one plot per frequency/receiver)
 * **SNR** -                Signal to Noise Ratio (polar plot) - (one plot per receiver)
 * **SNRI** -               Signal to Noise Ratio (polar plot, interpolated map) - (one plot per receiver)
 * **OSTAT** -              Observation stats (last session) - (one plot per receiver)
 * **PSTAT** -              Processing stats (multi-session) - (one plot per receiver)
 * **OCS** -                Outliers and cycle slips - (one plot per receiver)
 * **OCSP** -               Outliers and cycle slips (polar plot) - (one plot per receiver)
 * **RES** -                Residual plot - (one plot per receiver)
 * **RES_O_COS** -          Output combined residuals - (one plot per receiver)
 * **RES_W_COS** -          Work-Space combined residuals - (one plot per receiver)
 * **RES_O_PRS** -          Output combined pseudo-range residuals - (one plot per receiver)
 * **RES_W_PRS** -          Work-Space combined pseudo-range residuals - (one plot per receiver)
 * **RES_O_PHS** -          Output combined phase residuals - (one plot per receiver)
 * **RES_W_PHS** -          Work-Space combined phase residuals - (one plot per receiver)
 * **RES_SKY** -            Residual sky plot - (one plot per receiver)
 * **RES_SKYP** -           Residual sky plot (polar plot) - (one plot per receiver)
 * **PTH** -                Pressure / Temperature / Humidity - (one plot per receiver)
 * **NSAT** -               Number of satellite used (multi-receiver)
 * **NSATSS** -             Number of satellite used (sys by sys) - (one plot per receiver)
 * **NSATSSS** -            Smoothed number of satellite used (sys by sys) - (one plot per receiver)
 * **ZTD** -                Zenith Total Delay - (one plot for all the receivers)
 * **ZTD_VSH** -            Zenith Total Delay vs Height - (one plot for all the receivers)
 * **ZHD** -                Zenith Hydrostatic Delay - (one plot for all the receivers)
 * **ZWD** -                Zenith Wet Delay - (one plot for all the receivers)
 * **ZWD_VSH** -            Zenith Wet Delay vs Height - (one plot for all the receivers)
 * **PWV** -                Precipitable Water Vapour - (one plot for all the receivers)
 * **STD** -                Zenith Total Delay with slants - (one plot per receiver)
 * **RES_STD** -            Slants Total Delay residuals (polar plot) - (one plot per receiver)
 * **TGRAD** -              Tropospheric gradients table - Display a line of arrows (100) for each receiver indicating the direction and magnitude of the tropospheric gradient. This display function works best with ~45 receivers.

[Go back to the Index of contents](#index-of-contents)

### EXPORT
This command is used to save on disk the results computed with goGPS.
The tabs "export" in goInspector provides shortcuts to manually call the export methods.

![goInspector_export](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/goInspector_export.png?raw=true)

**Command modifiers:**

 * **CORE_MAT** -           Save the core object as .mat file - this object contains the entire status of the processing, including all the receivers, it is very useful to have it so that the results might be inspected in the future. This modifier ignores the target parameter.
 * **PLAIN_MAT** -          Save the receiver as plain .mat files (no objects) - this modifier is useful when the results of goGPS must be opened from other languages or older MATLABs, the receivers will be exported as a struct.
 * **REC_MAT** -            Receiver object as .mat file - similar to the previous modifier, this option allows to save any receiver as GNSS_Receiver object.
 * **REC_RIN** -            RINEX file containing the actual data stored in rec.work - this export can be useful to save filtered/synced receiver observations. All the applied PPP corrections and satellite clocks will be removed from the observations before the export.
 * **MP** -                 Export the multipath maps stored in the GNSS_Station object.
 * **COO_CRD** -            Coordinates .CRD file - the median coordinates stored in the receiver will be saved in a file that can be used in the [Receiver Info tab](https://github.com/goGPS-Project/goGPS_MATLAB/wiki/Settings#tab-receiver-info).
 * **XYZ_TXT** -            Coordinates XYZ in plain text format.
 * **ENU_TXT** -            Coordinates local ENU in plain text format.
 * **GEO_TXT** -            Coordinates Geodetic in plain text format.
 * **TRP_SNX** -            Tropo parameters (epoch, ZTD, Gradient North, Gradient East) as SINEX file.
 * **TRP_MAT** -            Tropo parameters (position, UTC time, ZTD, ZWD) as .mat file.
 * **TRP_CSV** -            Tropo parameters (epoch, ZTD, ZWD, Gradient East, Gradient North) as .csv file.
 * **TRP_HN** -             Tropo parameters as a HydroNet (CSV like) file.

[Go back to the Index of contents](#index-of-contents)

## Validation
Go goGPS is probably the only GNSS processing software that includes functions for automatic comparison with Radiosondes and IGS official solutions (open an issue if this is wrong, I'm curious ^_^). At the moment it is possible to automatically make 3 automatic comparisons using the command `VALIDATE`.

### VALIDATE
This command allows comparing the results obtained with goGPS with external sources. Be aware that the level and type of regularization chosen, the mapping function used for the tropospheric estimation, and multiple processing options may lead to different results. Comparing results with IGS official solutions have to be done mimicking as much as possible the processing done to obtain those results (same orbits, same mapping functions, same spline rate, ...) otherwise the validation only has a relative value.

**Command modifiers:**

 * **-e=<"name">** -        Export with name_postfix - if this flag is passed to the command `SHOW` the generated figures will be exported. "name" is optional and will be appended to an autogenerated name (stored in the UserData property of the MATLAB figure). Automatically goGPS will save the images in the out folder as "PNG" to request a different format "name" can be set to match a particular filetype (e.g. `VALIDATE T* RAOB -e=".pdf"` will save the images as PDF files, `VALIDATE T* RAOB -e` will save the validation results of all the receivers as PNG)
 * **-c** -                Close figure after export (valid only if the previous export modifier is present)
 * **IGS**-                Use IGS results for validation - this function generates a unique figure containing the statistics of mean and standard deviation for positions and tropospheric parameters for all the stations (the validation works only on IGS permanent stations)
 * **IGS_ZTD**-            Use IGS results for ZTD validation - for every IGS station processed the function will generate a plot with the ZTD estimated by goGPS and the official one of IGS, a table with all the statistics will be shown in the log, and a final map to recap the differences in terms of mean and standard deviation will be displayed.
 * **RAOB**-               Use RAOB for ZTD validation - for every radiosonde launching site the function will generate a plot with the ZTD estimated by goGPS and the one obtained by integration of the RAOB data, a table with all the statistics will be shown in the log, and a final map to recap the differences in terms of mean and standard deviation will be displayed. Radiosondes are usually launched from airports and not all the GNSS stations have close launching sites for making a comparison, goGPS will use the closest site or all the radiosondes falling in the area containing the processed receivers. The data needed by goGPS will be automatically downloaded from the [University of Wyoming](http://weather.uwyo.edu/upperair/sounding.html) website. The ZTD computed by goGPS in the radiosonde launch site is obtained by natural interpolation with all the GNSS stations used for the validation. **Note:** all the radiosondes used for this validation are launched every day at 00 and 12 UTC, only two points per day will be available. **Warning:** The radiosonde ZTD is formed with the integration of a column of water vapor that might not be vertical in the presence of winds, making the value obtained different with respect to the one estimated by goGPS.

![all_RAOB](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/all_RAOB.png?raw=true)

To display this map run `Radiosonde.showMap(Radiosonde.getRaobList)` from the MATLAB command line. If anyone finds that a radiosonde site is missing in goGPS is invited to open a goGPS issue on GitHub. For a map with all the close radiosonde sites exec `SHOW T* DTM_MAP_R` on goGPS inspector.

[Go back to the Index of contents](#index-of-contents)

## Ionospheric interpolation
In the beginning, goGPS have been created to work specifically with low-cost receivers, before 2019 most of them were single-frequency receivers and even today in 2020 most of the low-cost GNSS stations already deployed are not able to track more than one frequency. Having at least two different signals with different wave-lengths is important to compensate for ionospheric delays and apply PPP, to overcome this limit goGPS can interpolate ionospheric delays by using some reference receivers and then generate a synthetic set of L2 observations.

All the interpolating techniques are based on the paper ["Satellite specific Epoch differenced Ionospheric Delay" (Deng et al 2009)](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2009GL040018)

### SEID 
The Satellite specific Epoch differenced Ionospheric Delay algorithm works satellite by satellite in the following way: 
1. compute all the ionospheric piercing point (thin shell approximation) 
2. compute the ionosphere from the geometry-free combination of the reference receivers
3. differentiate the ionosphere in time (i.e. compute the speed of variation)
4. compute the interpolated differenced ionosphere weighted on the inverse of the spherical distance on the target receivers
5. synthesize L2 by adding to L1 the integration of the interpolated values

**Usage example:**
_Using 4 reference stations and one target to compute Precise Point Positioning with the paper version of SEID_
```
PINIT N5
FOR S*
   PAR T*
      LOAD T$
      PREPRO T$ -s=G
   END
   SEID R1:4 T5 PLANE
   PPP T*
END
SHOW T* ZTD
```

**Command modifiers:**

 * **PLANE** - (flag) use a plane for the interpolation of the differenced geometry free, this is the original implementation as described in the paper but requires a minimum of 3 reference receivers.

[Go back to the Index of contents](#index-of-contents)

### SID
SID (Satellite specific Ionospheric Delay) is a modified version of the SEID approach, it does not use time differentiation. The interpolated ionosphere is computed jointly with a Least Squares (LS) adjustment taking as input both pseudo-range and carrier-phase Geometry-Free (GF) combinations, a bias parameter is added for each arc, and the rank-deficiency of the system is resolved by using a Lagrange multiplier (set the mean to the mean of the GF of code).

**Usage example:**
_Using 4 reference stations and one target to compute Precise Point Positioning with SID approach_
```
PINIT N5
FOR S*
   PAR T*
      LOAD T$
      PREPRO T$ -s=G
   END
   SID R1:4 T5 PLANE
   PPP T*
END
SHOW T* ZTD
```

[Go back to the Index of contents](#index-of-contents)

### REMIONO
This command can be used similarly to `SEID` it has been created as an experimental Satellite specific Epoch differenced Ionospheric Delay algorithm, in addition to the original approach it tries to smooth the computed geometry-free. Instead of generating a synthetic L2 it removes the ionospheric delay to the phase observations of the target receiver. Without generating the second frequency it generally behaves worse than the previous approaches because outlier rejection works differently and a stronger regularization is required to have a comparable result, for these reasons the command is present but kept as experimental under testing.

[Go back to the Index of contents](#index-of-contents)

## Multipath management
goGPS integrates the possibility of creating and using multipath maps to mitigate the effect of reflections in the GNSS carrier-phase signals. We create our maps by processing a long period of data (10 days minimum are suggested) with the goGPS uncombined engine. From the obtained carrier phase residuals we can then generate maps of the common "geographical" errors in the polar coordinate system (e.g. Multipath effects coming from structures are always affecting the signals the same way, so each satellite transmitting from a certain elevation and angle is always affected by the same multipath disturbance). Multipath maps can be then used to mitigate the range errors allowing a better positioning. Our preliminary test shows that coordinates estimated for short sessions (e.g. every 15 minutes/hourly) can be improved by about 15-30% depending on the level of multipath.

Before interpolating the residuals these are selected by filtering the worse observations, in particular in addition to the snooping methods applied in the PPP/NET solutions a 3sigma threshold is used to discard outliers. Residuals falling in a bin of 1 degree in latitude and 360 in longitude are used to evaluate latitude by latitude the variance of the residuals, all the observation above the 3sigma level are ignored in the interpolation process (about 1-1.5% of the data are usually disregarded).

![Residuals and outliers](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/ResOutliers.png?raw=true)

To regularize the solution for each cell of 1x1 degree with less than 2 observations 1 null pseudo-observation is added to the dataset to be interpolated.

For each frequency present in the receiver, a multi-step procedure is used to generate the goGPS multipath maps
1. Uncombined carrier-phase residuals are all used in a LS solution to compute the [Zernike expansion](https://www.osapublishing.org/abstract.cfm?uri=josa-66-3-207) coefficients up to degree 43 (can be changed in the code). These coefficients are then used to synthesize a map of 0.5x0.5 degree in the cartesian coordinate system elevation x azimuth (180x720). Zernike polynomials are exceptional to map low degree changes on the disk (e.g. PCV) but for Multipath maps, a higher degree is preferable. Zernike polynomials are capable of describing changes in the signal depending on the distance from the center of the disk, with a radius close to 1, higher frequencies can be represented. In goGPS we perform two analyses at the same degree level using two mapping functions to map the elevation to the unitary radius so that we can better describe the undulations due to multipath. The two mapping functions are:

   * m1 = pi/2 * (1-cos(el))
   * m2 = pi/2 * (1-cos(m1))
   
After a first analysis, the second is performed on the residuals of the first and a synthetic final map is computed by adding the two matrixes.

![MP Step 1, 2](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/MP_Steps_1_2.png?raw=true)
_The figures show the maps retrieved from the two iterations of the Zernike based interpolation for the WTZZ station._

2. Stacking maps can be generated from the residuals of the previous step that provides an interpolation of the lower frequencies. The gridding procedure uses cells of 5 degrees in longitude and 1 in latitude, for each cell the mean is taken, each value is converted in polar coordinates and a final grid at the resolution of 0.5 degrees in cartesian coordinate (elevation x azimuth) is created by bi-linear interpolation in the polar coordinate system.

![MP Step 3](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/MP_Steps_3.png?raw=true)
_This is the final gridding step on the residuals of the WTZZ station._

At the final step, the maps are summed all together to generate two possible applicable models, the first smoother, and the second containing the higher frequencies of the staking map. The user can decide the map that works best in his project by selecting it in the settings.

![MP final maps](https://github.com/goGPS-Project/goGPS_graphics/blob/master/user_manual/MP_Final.png?raw=true)
_On the left the smoother solution, on the right the highest details of the staking are preserved_

### MPEST
The command MPEST can be used on a target receiver previously processed in PPP/NET with the uncombined engine that still contains uncombined phase residuals. It automatically generates two maps for each frequency: one estimated from Zernike interpolation and the second by simple stacking. If multipath mitigation is enabled, at the end of the computations, the command applies the corrections to the phase observations contained into the receiver. The result of the command is stored in the obj of class GNSS_Staion in the field ant_mp.

Use `EXPORT T* MP` to export a multipath map.

**Usage example:**
_Multipath maps creation_
```
PAR S*
   FOR T*
      LOAD T$ @30s
      PREPRO T$ -s=G
      PPP T$ -U
   END
   PUSHOUT T*
END
MPEST T*
EXPORT T* MP
```

[Go back to the Index of contents](#index-of-contents)

## Other Commands
Additional commands may be used to do minor operations, such as clean part of the objects to free up space, rename receivers, compute simplified positions, or filter observations:

### RENAME
Rename is a simple command that allows setting the Marker Name of a receiver, this may be useful when comparing the same receiver processed with different approaches 

**Usage example:**
_Suppose to compare the station ZIMM processed with GPS only, GLONASS only and Galileo only, one may rename the station, loaded tree times from the same RINEX file, as ZIMG, ZIMR, ZIME, to display different labels in the plot_
```
FOR S*   
   LOAD T1 -s=G
   LOAD T2 -s=R
   LOAD T3 -s=E
   PREPRO T*
   PPP T*
   RENAME T1 ZIMG
   RENAME T2 ZIMR
   RENAME T3 ZIME
END
SHOW T* ZTD
```

[Go back to the Index of contents](#index-of-contents)

### EMPTY
This function resets the content of the target GNSS_Station like it was not yet loaded and processed freeing up all of the memory

**Usage example:**
_Computing ZTD for 100 stations freeing the memory after export (one session)_
```
FOR S1
   FOR T1:100
      LOAD T$
      PREPRO T$
      PPP T$
      EXPORT T$ TRP_SNX
      SHOW T$ ZTD -e -c
      EMPTY T$
   END
END
```

[Go back to the Index of contents](#index-of-contents)

### EMPTYWORK
This function resets the content of the Work-space (the current session) of the target GNSS_Station freeing up some of the memory occupied by the receiver

**Usage example:**
_Computing PWV for 100 stations freeing the memory after export (multiple session)_
```
FOR S*
   FOR T1:100
      LOAD T$
      PREPRO T$
      PPP T$
      PUSHOUT T$
      EMPTYWORK T$
   END
END
SHOW T* PWV
```

[Go back to the Index of contents](#index-of-contents)

### EMPTYOUT
This function resets the content of the Output object of the target `GNSS_Station` freeing up some of the memory occupied by the receiver

**Usage example:**
_Computing two images of PWV comparison, one for the first 10 sessions one for the second 10 sessions_
```
FOR S1:10
   FOR T*
      LOAD T$
      PREPRO T$
      PPP T$
      PUSHOUT T$
      EMPTYWORK T$
   END
END
SHOW T* PWV -e

FOR S11:20
   FOR T*
      LOAD T$
      PREPRO T$
      PPP T$
      PUSHOUT T$
      EMPTYWORK T$
   END
END
SHOW T* PWV -e
```

[Go back to the Index of contents](#index-of-contents)

### REMTMP
Similar to the `EMPTYWORK` command this function removes from the Work-Space all the data not needed for the push of the results, might be useful in parallel computations to limit the number of data saved on disk and to free some memory.

**Usage example:**
_Computing ZWD for multiple parallel sessions. Remember that the PAR loop on sessions automatically execute `PUSHOUT` after loading the data processed by each slave and so it requires the object `work` to be exportable (EMPTYWORK cannot be used here)_
```
PINIT N5
PAR S*
   FOR T*
      LOAD T$
      PREPRO T$
      PPP T$
      REMTMP T$
   END
END
SHOW T* ZWD
```

[Go back to the Index of contents](#index-of-contents)

### AZEL
Compute Azimuth and elevation of a receiver, if a position has been computed or it's present in the coordinate file.

**Usage example:**
_Computing azimuth and elevation to display a polar plot of the SNR of a receiver_
```
FOR S1
   LOAD T1
   AZEL T1
   SHOW T1 SNR
END
```

[Go back to the Index of contents](#index-of-contents)

### BASICPP
Compute a simple but fast code position on a target receiver without applying any range correction

**Usage example:**
_Creating an a-priori coordinate file for all the receivers_
```
FOR S1
   LOAD T*
   BASICPP T*
   EXPORT COO_CRD 
END
```

[Go back to the Index of contents](#index-of-contents)

### CODEPP
Running this command goGPS will compute a Code Positioning, this command does not perform any outlier rejection and does not apply any desync of the observations, it should not be used without running a pre-processing on the target receives.

**Usage example:**
```
CODEPP T1
```

[Go back to the Index of contents](#index-of-contents)

### OUTDET
Perform outlier detection on phases, normally this is done in pre-processing but there are cases when this is useful, in fact, PPP and NET processing modify the status of the outliers.

**Usage example:**
_In this case `OUTDET` is used to reset the flagged outliers at each iteration_
```
FOR S*
   LOAD T1
   PREPRO T1 -s=G
   PPP T1
   FOR T2:END P1
      OUTDET T1
      NET T1,$ R1
   END
   PUSHOUT T$
END
```

[Go back to the Index of contents](#index-of-contents)

### FIXPOS
This command allows changing the reference coordinate type (by default it set the position as fixed).

**Command modifiers:**

 * **FROM_WORK** -          (flag) use data from Work Space (current session)
 * **FROM_OUT** -           (flag) use data from Receiver Output object
 * **AS_APR** -             (flag) use position as a new a-priori position (not as fixed)

**Usage example:**
_In this example a PPP solution is computed in the first session, later the other 29 sessions are processed with the fixed position of the first session_
```
FOR S1 
   LOAD T1
   PREPRO T1 -s=G
   PPP T1
   FIXPOS T1
END
FOR S2:30
   LOAD T1
   PREPRO T1 -s=G
   PPP T1
END

```

[Go back to the Index of contents](#index-of-contents)

### KEEP
This command can be used to remove some data from the receiver's work-space by telling goGPS what to keep.

**Command modifiers:**

 * **@<rate>** -        Rate in seconds (e.g. @30s, -r=30s)
 * **-s=<sat_list>      Active constellations (e.g. -s=GRE)
 * **-e=<elevation>     Cut-off elevation in degree (e.g. -e=7)
 * **-q=<snrthr>        SNR threshold in dbHZ (e.g. -q=7)

**Usage example:**
_Keep only observations of GPS satellites having elevation greater than 15deg_
```
KEEP T1 -s=G -e=15
```

[Go back to the Index of contents](#index-of-contents)

### SYNC
Syncronize all the receivers at the same rate, keeping only the epochs between the first and the last among all the receivers.

**Command modifiers:**
 * **@<rate>** -        Rate in seconds (e.g. @30s, -r=30s)

**Usage example:**
```
SYNC T* @60s 
```

[Go back to the Index of contents](#index-of-contents)

### REMSAT
Remove all the observations of a satellite from a target receiver. The function accepts a single list of satellites to be removed with the first letter of the constellation (GREJCI) and two digits for the PRN number.

**format:** `<1ch sat. sys. (GREJCI)><2ch sat. prn>`

**Usage example:**
_Remove GPS satellite with PRN 4 or PRN 29, and the Galileo satellite with PRN 25.
```
REMSAT T* G04,G29,E25
```

[Go back to the Index of contents](#index-of-contents)

### REMOBS
Remove some observation types from a target receiver.

**format:** `<1ch obs. type (CPDS)><1ch freq><1ch tracking>`

**Usage example:**
_Remove from the first receiver all the doppler observations, the SNR of the second frequency and the second frequency of carrier-phases of the civilian tracking_
```
REMOBS T1 D,S2,L2C
```

[Go back to the Index of contents](#index-of-contents)


Instruction during experiment
========

- [Data flow and storage](#user-content-data-flow-and-storage)
    - [Directory structure](#directory-structure)
    - [Autocopy](#autocopy)
    - [Merger](#merger)
- [NTCAP-DAQ station](#user-content-ntcap-daq-station)
- [RSA51 crash recovery](#rsa51-crash-recovery)
- [Spectrum analyzer](#user-content-spectrum-analyzer)
- [Electron cooler](#user-content-electron-cooler)
- [Electronic logbook](#user-content-electronic-logbook)
- [Manual analysis](#user-content-manual-analysis)
    - [Analysis computer](#user-content-analysis-computer)
    - [Using the GUI](#user-content-using-the-gui)
- [Automatic analysis](#user-content-automatic-analysis)



# Data flow and storage

## Important directories

Data is being acquired using 3 spectrum analyzers and 1 oscilloscope. The
spectrum analyzers each save 1 data file per injection. The
oscilloscope has 4 channels, and saves them 2 times during each injection: at
the time of injection and at the time of extraction. As a result we have 11
files per injection. The data is transferred to our analysis PCs automatically
using [autocopy](#autocopy), from there it is automatically merged using the
[merger](#merger).

## Directory structure

The data is copied to the directory `/hera/sids/GO2014` (if this path is not present check 
`/SAT/hera/sids/GO2014`). Each instrument has it's own directory, and oscilloscope channels 
also have separate subdirectories. Merged files from the different folders will be placed in 
the `ROOT` folder. The directory structure can be seen below:

```
/hera/sids/GO2014                           # 2014 data
├── AnalysisResults                         #
│   ├── automatic                           # Output of automatic analysis
│   │   └── figs                            #
│   └── visual                              # Output of visual analysis
│       └── SidsVisualDecayResults.root     #
├── Oscil                                   # Raw data of oscilloscope
│   ├── C1                                  #   first kicker module
│   ├── C2                                  #   second kicker module
│   ├── C3                                  #   third kicker module
│   └── C4                                  #   trigger signal
├── ROOT                                    # Merged data from instruments
├── RSA30                                   # Raw data of RSA30
├── RSA51                                   # Raw data of RSA51
└── RSA52                                   # Raw data of RSA52

```

## Autocopy

1. For the RSA50s open `C:\\autocopy`, for RSA30 open `C:\\Oscillation\Autocopy`, for the oscilloscope open `D:\\autocopy\py`.
2. Double click on `key-apdev26.ppk`.
3. Check the system tray to see if `pagent`is running, click it and check if the key has been   added.
4. Double click on `autocopy.py` to start the script.
5. If asked about which file list to choose, unless you have a good reason not to, select local (l).
6. If the program window says "Got list of processed files." the application has started. Let   it run.
7. If the application needs to catch up with the transferring, it will take a long time 
   before you see the next message saying `PING`. Wait and observe the log.
8. To view the log right click on `autocopy.log` and open it with Notepad++.

The latest entry will always be on the bottom, to reload the log press `File -> Reload from disk`.

## Merger

1. Open a terminal and cd into `/hera/sids/GO2014/Merger`.
2. More than one terminal will be needed (for monitoring) so press Ctrl-Shift-N 2 times.
3. On one of the terminals start the merger program by typing `python merger.py`.
4. On one of the other terminals run the command `watch -n 0.5 merging.log` to monitor the 
   log. Newest entries will be on top.
5. Run `tail -n 40 -F contents.list` to keep track of what is placed in the merged ROOT file.
   Newest entries will be on the bottom.

# NTCAP-DAQ station
To ensure a good synchronization at NTCAP-DAQ station please restart the NTCAP acquisition ~3h. Here is a list to do this properly.
Please mind the following steps in the correct order!

1. Stop ESR cycle.
2. Wait until ESR cycle ends.
3. To align both RSA51 and RSA52 devices at ESR station select from their menu: 
`Tools -> Alignments -> Align Now` and wait until the devices have finished.
4. At NTCAP-DAQ station: 
Press Stop-Button.
5. At NTCAP-DAQ station: 
Wait until program is again in Init'd state . 
6. At NTCAP-DAQ station: Press Start Button at NTCA-DAQ.
7. Start ESR cycle.
8. Please make a note in our NTCAP - logbook with time/ action /directory path.
9. Thank you very much!

Here is a screenshot from the controls of NTCAP-DAQ station. 
You'll find this in the right upper corner of the NTCAP-DAQ-Program. 

![NTCAP](figures/NTCAP.png?raw=true "NTCAP")


# RSA51 crash recovery

**Don't Panic!**

The firmware was updated to the newest version from Tektronix, same version number as the RSA52. The crash should not happen again, but in case it does, follow the instructions below:

* press long to turn off 
* press to turn on again
* wait until boot is complete
* press `recall` by choosing `File -> Open`
* then choose the last good TIQ file `ACQ Data with Setup`
* click open
* close Data and Setup

## Individual Settings:

### settings control panel:

* on the Freq and Span tab:
	* span 15 kHz
* on the amplitude scale tab:
	* color power: min -90 dBm and max -71 dBm
* on time and frequency scale:
	* Time per div: 20 seconds

### trigger control panel:
* on the event tab:
	* triggered
	* trig in front
* on the Advanced tab:
	* position 1%
* on the actions tab:
	* save acq data
	* max total files should be off

### on the acquisition control panel:

* on the sampling parameters tab:
	* Adjust is: ACQ BW, ACQ Length combo box
	* ACQ BW : 19.53 kHz
	* ACQ Length: 70 s

### Analysis control panel:
* Analysis time tab:
	* time zero reference should be trigger
	* Analysis length same as above = 70 s

### Setup --> Configure I/O
* Trig 1 should be 50 Ohm
 
### File --> Acquisition Save Options
* All files section:
	* Check automatically increment file name
* Acquisition data files:
	* Entire IQ record

### Tools --> Alignment
* choose run alignment only when the align now is pressed
then press align now once and wait until finished

## Starting procedure
 
* Save a dummy TIQ file with the name of RSA51.TIQ in the directory `c:\\oscillation\`
* this dummy file should be deleted afterwards
* press run/stop button




# Spectrum analyzer

Steps to getting the S/As running:

## RSA50s

1. Launch the spectrum analyzer application
2. Align the instrument by `Tools -> Alignments -> Align Now`
3. Load the last acquired file from `C:\\oscillation\`
4. Press the `Acquire` button and check the `Sampling parameters` tab to see if
   the proper acq. bandwidth (19.5/39.1 kHz for RSA51/RSA52) and acquisition length
   (70 s) are set
5. Press the `Trigger` button and check that on the left side `Triggered` is
   selected and that `Save acquisition on trigger` is selected in the `Action`
   tab
6. Press the `Run/Stop` button to start acquiring data

# Electron cooler
Check the electron cooler screen
![ECooler](figures/ecoolscreen.png?raw=true "ECooler")


# Electronic logbook

To document your work use the elog. Start elog in an internet browser. The url of the elog is:
  ```bash
http://lxg0187:8080
  ```
username and password are available on the first page of the logbook.
There are 3 logbooks available:
* GO2014 automatic analysis
* GO2014 visual analysis
* GO2014 general log
Use the logbook 'GO2014 visual analysis' to document the work with the visual
analysis and the logbook 'GO2014 general log' to save information of general
interest for the experiment.


# Manual analysis

## Analysis computer

### Login into lxg0188
The manual analysis is performed on the lxg0188 computer.
username and password are available on the first page of the logbook.

### Open 2 terminals

One terminal (terminal_1) is used to start the analysis program.
The second terminal (terminal_2) is used to display the available files to
analyse.

  ```bash
terminal_2> cd $ESRDATAPATHIN
  ```

$ESRDATAPATHIN points to the directory with the root files to analyze. These input root files are the output of the time2root, and contain the time-resolved multitaper spectra from the different instruments with the corresponding header information and the injection/extraction kicker signals.
You can display the available files using the command 'ls -al' in terminal_2.


## Using the GUI
You can start the visual analysis program on a terminal using the following command:
  ```bash
  terminal_1> startVisualAnalysis.sh UserName InputFileName
  ```
UserName is a string to identify the person doing the analysis.
InputFileName is the name of the file to analyse (the files can be listed in
terminal_2 using the command 'ls -al')

![example](figures/guiDoc1.png?raw=true "Example")


#### Showfiles script
The showfiles.sh script displays the files that have been analyzed or that have not been analyzed yet.
The Commands are the following:  
```bash
    showfiles.sh all        # show all files
    showfiles.sh done       # show already analyzed files
    showfiles.sh done detail    # show the number of times the files have been analyzed as well
    showfiles.sh left       # show the files not yet analyzed
  ```

#### startVisualAnalysis.sh parameters

 The output file of the manual analysis is stored on hera :
  ```bash
  $ESRDATAPATHOUT/SidsVisualDecayResults.root
  ```
 In case some parameters of the manual analysis script need to be adjusted, the script is found in
    ```bash
  /data.local2/software/SIDSRoot/build/bin/startVisualAnalysis.sh
    ```
  Parameter of interest that might need to be changed are:

  ```bash
    binDistancePDfreq="52"    # distance in bin between parent and daughter freq
    binPWindow="10"              # half projection window (parent)
    binDWindow="10"              # half projection window (daughter)
    binningTraces="20"           # Rebinning of parent and daughter traces
    binningFreq2dHisto="2"    # Rebinning w.r.t freq of the 2D histogram
    binSigmaPeak="4.0"         # sigma (in bin) of peaks
    thresholdPeak="0.2"         # threshold for peak detection
    detectorID="RSA51"         # "RSA51", "RSA52", or "RSA30"
    zmax="3.5e-8"               # set maximum z axis of 2D histogram
    ZSliderScale="5.e-9"        # Scale of the slider position
  ```

The rest of the parameters should not be changed.


# Automatic analysis







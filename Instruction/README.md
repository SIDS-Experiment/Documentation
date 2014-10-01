Instruction during experiment
========

- [Data flow and storage](#user-content-data-flow-and-storage)
    - [Directory structure](#directory-structure)
    - [Autocopy](#autocopy)
    - [Merger](#merger)
- [Spectrum analyzer](#user-content-spectrum-analyzer)
- [Electronic logbook](#user-content-electronic-logbook)
- [Manual analysis](#user-content-manual-analysis)
    - [Analysis computer](#user-content-analysis-computer)
    - [Using the GUI](#user-content-using-the-gui)
- [Automatic analysis](#user-content-automatic-analysis)



# Data flow and storage

## Important directories

```bash
/hera/sids/GO2014/							# 2014 data
/hera/sids/GO2014/RSA30						# Raw data of RSA30
/hera/sids/GO2014/RSA51						# Raw data of RSA51
/hera/sids/GO2014/RSA52						# Raw data of RSA52
/hera/sids/GO2014/Oscil						# Raw inj/extr kicker data
/hera/sids/GO2014/ROOT						# or $ESRDATAPATHIN -> Preprocessed data (containing all information needed for manual or automatic analysis)
/hera/sids/GO2014/AnalysisResults/visual	# or $ESRDATAPATHOUT -> decay time data (output of manual/visual analysis)
```

Data is being acquired using 3 spectrum analyzers and 1 oscilloscope. The
spectrum analyzers each save 1 data file during each injection. The
oscilloscope has 4 channels, and saves them 2 times during each injection: at
the time of injection and at the time of extraction. As a result we have 11
files per injection. The data is transferred to our analysis PCs automatically
using [autocopy](#autocopy) where it is automatically merged using the
[merger](#merger).

## Directory structure

The data is copied to the directory `/hera/sids/GO2014`. Each instrument has
it's own directory, and oscilloscope channels also have separate
subdirectories. Merged files from the different folders will be placed in the
`ROOT` folder. The directory structure can be seen here.

```
.
├── Oscil
│   ├── C1
│   ├── C2
│   ├── C3
│   └── C4
├── ROOT
├── RSA30
├── RSA51
└── RSA52
```

## Autocopy

## Merger


# Spectrum analyzer


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


 The output file of the manual analysis is stored on hera :
  ```bash
  $ESRDATAPATHOUT/SidsVisualDecayResults.root
  ```
 In case some parameters of the manual analysis script need to be adjusted, the script is found in
    ```bash
  /data.local2/software/SIDSRoot/build/bin/startVisualAnalysis.sh
    ```
  Parameter of interest that might need to change are:

  ```bash
    binDistancePDfreq="52"    # distance in bin between parent and daughter freq
    binPWindow="10"              # half projection window (parent)
    binDWindow="10"              # half projection window (daughter)
    binningTraces="20"           # Rebinning of parent and daughter traces
    binningFreq2dHisto="2"    # Rebinning w.r.t freq of the 2D histogram
    binSigmaPeak="4.0"         # sigma (in bin) of peaks
    thresholdPeak="0.2"         # threshold for peak detection
    detectorID="RSA51"         # "RSA51", "RSA52", or "RSA30"
  ```

The rest of the parameters should not be changed.


# Automatic analysis







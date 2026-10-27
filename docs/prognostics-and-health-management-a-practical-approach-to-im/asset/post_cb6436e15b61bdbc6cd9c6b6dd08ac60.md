
## Feature Vector Framework

The Feature Vector block comprises three sections, – a primary section, a data-smoothing section, and an FFP-to-DPS transformation section:

FD = feature data = FD0*(1 + dP/P0)
FFP = (FD – FD0 – NM)/FD0 where NM = FNM*FD0/100 or
FFP = (FD – FD0 – NM) when FDNOMDIV = 0

The primary section contains six entries to specify transforming CBD to FD:

- *FNM* specifies the NM, a percentage of the nominal FD value (FD0)
- *FAILM* specifies the percent failure margin: 70 means failure when FFP = 0.70
- *FDNOM* specifies the data value considered to be the nominal FD value (FD0)
- *FDNOMV* specifies the percent expected variance in FD0
- *FDNOMMIN* specifies the minimum value to be used for FD0.
  - When measured FD0 is less than FDNOMMIN, use FDNOMIN.
  - Else use measured FD0.
- *FDNOMDIV* specifies whether division is (nonzero value) or is not (zero value) to be performed.

The data smoothing section contains four entries for data smoothing:

- *FD0PTS* specifies the number of CBD points to average, to calculate the value for FD0.
  - When specified as zero (0), FDNOM is used.
  - When variance in calculation exceeds FDNOMV, FDNOM is used.
- *FDPTS* specifies the number of FD points to average for smoothing.
- *FFPPTS* specifies the number of FFP points to average for smoothing.
- *FFSPTS* specifies the number of FFS points to average for smoothing.

The FFP-to-DPS transform section contains two entries for transforming FFP signature data to DPS data:

- *XDPS* specifies the FFP model for selecting the FFP-to-DPS transformation model:
  - 0 means do not transform.
  - 1 means use FFP = FD0*(dP/P0)^n.
  - 2 means use FFP = FD0*([1/(1 - dP/P0)]^n - 1).
  - 3 means use FFP = FD0*(1-[1/(1 + dP/P0)]^n).
  - 4 means use FFP = FD0*((1 + dP/P0)^n - 1).
  - 5 means use FFP = FD0*(1 - (1 - dP/P0)^n).
  - 6 means use FFP = FD0*(exp(dP/P0) - 1).
  - 7 means use FFP = FD0*(1 - exp(-dP/P0)).
- *XDPSNV* specifies the value to use for n or lambda (lifetime).

## Prediction Framework

The prediction framework section (Figure 6.61) contains two entries for specifying values used in producing prediction information:

- *PITTFF0* specifies a time to failure (TTF) value after the onset of degradation:
  - The default value is 200 sampling periods.
- *PIFFSMOD* specifies the expected shape of the input data:
  - 1 means a concave characteristic curve.

```
%**Feature Vector          (A)
FNM      = 2.5 ;      % Noise margin - % of FD0
FAILM    = 70 ;       % Failure margin - % above 0
FDNOM    = 0 ;        % Feature Data (FD), Nominal value
FDNOMV   = 3 ;        % FD Nominal variance (%)
FDNOMMIN = 1 ;        % minimum nominal value: if less, perform DIV
FDNOMDIV = 0 ;        % 0 = subtract only (no division) - small signal
                      % else FFP = (FD - DIV)/DIV
```

```
(B)
FD0PTS   = 20 ;       % # data points to average to calculate FD0
FDPTS    = 4 ;        % Rolling FD (CBD) points to average: up to 4
FFPPTS   = 4 ;        % Rolling FFP points to average: up to 4
FFSPTS   = 4 ;        % Rolling FFS points to average: up to 4
```

```
%**FFP to DPS Transform    (C)
XDPS     = 0 ;        % none
%        = 1 ;        % FFP = FD0*(dP/P0)^n
%        = 2 ;        % FFP = FD0*([1/(1 - dP/P0)]^n - 1)
%        = 3 ;        % FFP = FD0*[1-{1/(1 + dP/P0)}^n]
%        = 4 ;        % FFP = FD0*([1 + dP/P0]^n - 1)
%        = 5 ;        % FFP = FD0*(1 - (1 - dP/P0)^n)
%        = 6 ;        % FFP = FD0*(exp(dP/P0) - 1)
%        = 7 ;        % FFP = FD0*(1 - exp(-dP/P0))
XDPSNV   = 1 ;        % n or Lambda (life) value
```

Figure 6.60 NDEF: feature-vector framework – (a) primary, (b) smoothing, (c) FFP-DPS transform.

```
%**Prediction (Prognostic) Information
PITTFF0  = 4800 ;     % estimated time (hours) after onset of damage
PIFFSMOD = 2 ;       % model 1=Concave, 2=Linear, 3=Convex, 4=complex
PIFFSNM  = 3 ;       % noise margin (%) for estimating begin degradation
```

Figure 6.61 NDEF: Prediction Framework.

- 2 means a linear curve – this is the default value.
- 3 means a convex curve.
- 4 means a complex (irregular) curve.

- *PIFFSNM* specifies the NM (%) for estimating beginning of degradation (BD).

An operational note about PIFFSMOD: your PHM system is prepackaged with a basic set of prediction algorithms embedded in a software program called DXARULE (Adaptive Remaining Useful Life Estimator) at no additional charge, because it does not provide special handling of nonlinear input data (PIFFSMOD is ignored). Your customer has the option of acquiring and using an advanced version, ARULEAV.

## Performance Services/Graphics

The Performance Services/Graphics section (Figure 6.62) contains three entries of specifications related to performance:

- *PGSW* is reserved for a future, planned capability (graphical user interface).
- *PGALPHA* specifies the alpha (accuracy) percent for evaluating predication accuracy.
- *PSSW* specifies whether performance metrics are (1) or are not (0) to be written to a logging file.

```
%**Performance Services/Graphics
PGSW      = 1 ;        % Future
PGALPHA   = 10 ;       % Prognostic alpha for Prognostic Horizon
PSSW      = 1 ;        % Write PI results to file, 0 = no, 1 = yes
```

Figure 6.62 NDEF: Performance Services/Graphics.

```
%**Input & Output Files***********************************
FDINLIB   = 'C:\DXPHMLIB\DXINDATA\' ;  % Input data library
OUTLIB    = 'C:\DXPHMLIB\DXOUTDATA\' ; % Output data library
INFILE    = 'DXDATFR1 ' ;               % File name for input data
OUTFILE   = 'DXDATFR1 ' ;               % File name for output data
INTYPE    = '.txt' ;                    % Input file type, text
OUTTYPE   = '.csv' ;                    % Output file type, text
```

Figure 6.63 NDEF: Input & Output Files.

## Input & Output Files

The Input & Output Files section (Figure 6.63) contains six entries to specify the input and output files for data:

- *FDINLIB* specifies the input data library.
- *OUTLIB* specifies the output data library.
- *INFILE* specifies the name of the input-data file.
- *OUTFILE* specifies the name of the output-data file.
- *INTYPE* specifies the type of the input-data file (.txt is the default).
- *OUTTYPE* specifies the type of the output-data file (.csv is the default).

## Checkpoint Library and File Names

The work files section (Figure 6.64) contains nine entries to specify the names of the library, the name prefixes, and the type to be used as work files to save and restore the operating environment to support a checkpoint and restart:

- *CPTLIB* specifies the checkpoint (and restore) library for work files.
- *CPTTYP* specifies the type of the work files.
- *DDWFN* specifies the name prefix of the work file for a device driver program.
- *FVWFN* specifies the name prefix of the work file for a feature vector program.

```
%**Checkpoint Library & File Name**************************
CPTLIB    = 'C:\DXPHMLIB\DXCPT\' ;      % Checkpoint restore
CPTTYP    = 'csv'                     % file name type
DDWFN     = 'DDW_' ; % Device Driver work
FVWFN     = 'FVW_' ; % Feature Vector work
PIWFN     = 'PIW_' ; % Prognostic Information work
PSWFN     = 'PSW_' ; % Prognostic Services work
VSWFN     = 'VSW_' ; % Visual Services work (future)
EFWFN     = 'EFW_' ; % End File Services work (future)
PAWFN     = 'PAW_' ; % Performance Analysis work
```

Figure 6.64 NDEF: Checkpoint Library & File Name.

- *PIWFN* specifies the name prefix of the work file for a prognostic information program.
- *PSWFN* specifies the name prefix of the work file for a prognostic services program.
- *VSWFN* specifies the name prefix of the work file for a visual services program.
- *EFWFN* specifies the name prefix of the work file for an end file program.
- *PAWFN* specifies the name prefix of the work file for a performance analysis program.

## Device Driver Program ID and Data Units of Measure

The data units of measure section (Figure 6.65) contains three entries to define the units of measure for input data:

- *DDPID* specifies the program identifier for a device driver program.
- *DDUT* specifies the text to be used for units of time.
- *DDUA* specifies the text to be used for units of amplitude.

You choose this format because, in the future, you might decide to specify units of measure for other forms of data, such as that from a feature vector program (FV PID).

## Program Identifiers

The program identifiers section (Figure 6.66) contains six entries to specify the identifiers (numeric values) of the programs, other than the device driver, to be used to process the node:

- *FV PID* specifies the numeric identifier for a feature vector program.
- *PIPID* specifies the numeric identifier for a prognostic information program.
- *PSPID* specifies the numeric identifier for a prognostic services program.
- *VSPID* specifies the numeric identifier for a visual services program.
- *EFPID* specifies the numeric identifier for an end file program.
- *PAPID* specifies the numeric identifier for a performance analysis program.

## End of Definition

The program identifier section is followed by an End of Definition (ENDDEF) entry (Figure 6.67), as an explicit end rather than an implied end.

```
%**Device Driver Program ID and Units of Measure**************
DDPID    = 1 ;    % Required entry    DXDDTXT01
DDUT     = '[hr]' ; % Units of time
DDUA     = '[kHz]' ;% Units of amplitude
```

Figure 6.65 NDEF: Device Driver Program ID and Units of Measure.

```
%**Other Program IDs******************************************
FV PID   = 1 ;    % Required entry    DXFV1S01
PIPID    = 2 ;    % Required entry    DXARULE
PSPID    = 1 ;    % Optional entry    DXPS01
VSPID    = 0 ;    % Place holder
EFPID    = 0 ;    % Place holder
PAPID    = 0 ;    % Optional entry    DXPA01
```

Figure 6.66 NDEF: Other Program IDs.

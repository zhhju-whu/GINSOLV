# GINSOLV and GINSPLOT

English | [简体中文](README.zh-CN.md)

GINSOLV is an application for post-processing multi-constellation,
multi-frequency GNSS and GNSS/INS data. It provides a Qt interface for selecting
a processing configuration, reviewing the required files, running the solver,
and monitoring processing progress.

GINSPLOT is the companion application for solution visualization and
error analysis.

## Main Features

- **SPP** - standalone single-point positioning using broadcast navigation data.
- **DGNSS** - code-based differential positioning with rover and reference
  observations.
- **RTK** - carrier-phase relative positioning with kinematic, static,
  static-start, moving-baseline, and known-position variants.
- **PPP** - Precise Point Positioning with Ionosphere-Free (IF) or Uncombined
  (UC) processing, using precise orbit, clock, antenna, and bias products.
- **PPP-AR** - Precise Point Positioning with Ambiguity Resolution using
  Uncombined (UC) observations and compatible code and phase-bias products.
- **PPP-RTK** - Precise Point Positioning - Real-Time Kinematic using
  Uncombined (UC) per-band observations, atmospheric states, and regional
  corrections. The supplied Tsukuba configuration uses recorded QZSS CLAS L6
  corrections.
- **GNSS/INS integration** - loosely coupled (LC) and tightly coupled (TC)
  processing with configurable IMU characteristics, lever arm, mounting angle,
  and aiding options.

Additional processing capabilities include:

- GPS, GLONASS, Galileo, QZSS, BDS, NavIC, and SBAS observations;
- multi-frequency processing with explicit physical-band selection;
- forward, backward, combined, and combined-without-reset processing;
- configurable broadcast, precise, SBAS, and SSR ephemeris sources;
- ionospheric, tropospheric, antenna, tide, and Earth-rotation corrections;
- IGS, IGS-RTS, MADOCA, HAS, and CLAS correction-service inputs;
- configurable ambiguity resolution, measurement weighting, residual rejection,
  and solution validation.

The availability of a constellation, signal, correction model, ambiguity mode,
or GNSS/INS combination depends on the processing configuration and the supplied
observations, products, and IMU data.

## Inputs

Depending on the selected processing method, GINSOLV can use:

- rover and reference GNSS observations in RINEX format;
- prepared smartphone GNSS observations and IMU measurements;
- broadcast navigation messages;
- precise orbit and clock products;
- code and phase-bias products;
- antenna calibration, Earth-rotation, ionosphere, and ocean-loading products;
- recorded SSR, MADOCA, HAS, or CLAS correction messages;
- receiver coordinates and reference trajectories.

## Outputs

GINSOLV writes a `.pos` solution file at the path selected in the configuration.
The solution can contain position, velocity, attitude, covariance, and quality
fields according to the enabled output options.

Optional diagnostic outputs are:

- `.stat` - solution states, satellite status, and residual records according to
  the configured status level;
- `.trace` - processing diagnostics according to the configured trace level.

Disabled STAT or trace output is not created.

## MATLAB Installation

The numerical and plotting code runs with a full 64-bit MATLAB installation and
a valid MATLAB license: Windows MATLAB for the Windows applications, and native
Linux MATLAB for the Linux applications. MATLAB Runtime alone is not sufficient.

GINSOLV and GINSPLOT use the executable selected by `MATLAB_EXE` when specified.
Otherwise, they search PATH and standard installation folders, plus the MATLAB
installation registry on Windows, and select the newest installed release.
Set `MATLAB_EXE` to the desired `bin/matlab.exe` on Windows or `bin/matlab` on
Linux to override this selection.

GINSPLOT needs internet access when Satellite must retrieve uncached map tiles.
If those tiles cannot be loaded, it displays a network-error message and keeps
the other views usable. Standard plots and processing with local input products
work offline.

## Quick Start

1. Start `GINSOLV.exe` on Windows or `./GINSOLV` on Linux from `GINSOLV/bin`.
2. Select a complete `prcopt_*.m` configuration file.
3. Review the resolved input and output paths.
4. Select **RUN**.

The configuration determines the positioning method, constellations, bands,
models, correction products, processing direction, INS coupling, and output
files. Configuration options use numeric values, with the available values and
their meanings documented in comments beside each option.

## GINSPLOT

GINSPLOT reads a `.pos` solution, an associated `.stat` file when available,
and an optional `reference.csv`. Native code performs file reading and data
preparation, Qt provides the user interface, and the installed MATLAB renders
the plot area.

Available views include:

- standard or satellite-map trajectory;
- East, North, and Up velocity components;
- Pitch, Roll, and Yaw attitude components;
- position, velocity, and attitude errors with RMS values;
- number of satellites, ambiguity ratio, and discrete solution status;
- code or carrier-phase residual time series and empirical probability-density
  histograms;
- code or carrier-phase residuals versus elevation and SNR;
- SNR by physical band.

System, satellite, physical-band, and solution-status filters support multiple
selections. The Residual control switches between code and carrier phase.

To view a solution:

1. Start `GINSPLOT.exe` on Windows or `./GINSPLOT` on Linux.
2. Select a `.pos` solution file. The application associates files with the
   same base name and searches for an available reference file.
3. Select **RUN**, then choose views and filters in the result window.

## Release Layout

```text
release/                         (Windows)
|-- README.md
|-- README.zh-CN.md
|-- GINSOLV/
|   `-- bin/
|       |-- GINSOLV.exe
|       |-- GINSPLOT.exe
|       `-- core/
|           `-- ginsplotio.exe
`-- Data/

release-linux/                   (Linux)
|-- README.md
|-- README.zh-CN.md
|-- GINSOLV/
|   `-- bin/
|       |-- GINSOLV
|       |-- GINSPLOT
|       `-- core/
|           `-- ginsplotio
`-- Data/
```

`Data` is stored beside `GINSOLV` in the supplied release. It contains input
data, complete processing configurations, and reference trajectories. Result
files are created at the paths specified by each configuration.

## System Requirements

The environments used for testing are:

- 64-bit Windows build 26200.9457 with MATLAB R2025b Update 1.
- Ubuntu 24.04.3 x86-64 under WSL2 with native Linux MATLAB R2024b Update 3.

# MagnetoPyCalibration

This repository contains files and information used to calibrate the data from the Rinehart group SQUID magnetometer. It is meant to be used within [magnetopy](https://github.com/RinehartGroup/MagnetoPy.git).

## Installation

Run the following command in the terminal while in your `magnetopy` environment:

```bash
mp-calibration <url to calibration repository>
```

For the Rinehart group, this is:

```bash
mp-calibration https://github.com/RinehartGroup/MagnetoPyCalibration.git
```

This will clone the calibration repository into `~/.magnetopy/calibration`, replacing any calibration data already installed there.

## Usage

`magnetopy` contains functions and methods for

- manual calculation of magnetic moments from raw (voltage vs. position) data
- [correction for the absolute field error](https://qdusa.com/siteDocs/appNotes/1500-021.pdf)

These functionalities require instrument-specific and sometimes sequence-specific data. Thus, since `magnetopy` is meant for general use, research groups should use their own calibration repositories to store their data.

## Contents

[calibration.json](calibration.json) has the following structure:

```json
{
  "moment": "moment.dat",
  "mvsh": {
    "sequence_1": "mvsh_seq1.dat",
    "sequence_2": "mvsh_seq2.dat"
  }
}
```

The actual files are stored in [calibration_files](calibration_files). It's assumed that, when needed, a .rw.dat file containing the unprocessed data is also stored in this folder (with the default convention of having the same name as the .dat file, but with the .rw.dat extension).

Only two categories of calibration files are currently support.

1. "moment"

   Used for the determination of the [system-specific calibration factor](https://qdusa.com/siteDocs/appNotes/1500-023.pdf). There should only ever need to be one of these files.

2. "mvsh"

   Used for [determining the "true field" during an M vs H sequence](https://qdusa.com/siteDocs/appNotes/1500-021.pdf). Per the application note, all measurements should be collected at 298 K. The temperature of the calibration data does not need to match the temperature of the target data, but the sequence of fields should be the exact same.

   The "mvsh" keys are the names of the sequences. These names are referred to by `magnetopy` for easy access to the correct calibration data. See the `magnetopy` documentation for more details.

### Sequences summary

- sequence_1 (mvsh_seq1.dat)

  | start (Oe) | stop (Oe) | step (Oe) |
  | ---------- | --------- | --------- |
  | -70000     | -10000    | 5000      |
  | -9500      | -1000     | 500       |
  | -800       | 800       | 10        |
  | 1000       | 9500      | 500       |
  | 10000      | 70000     | 5000      |
  | 65000      | 10000     | 5000      |
  | 9500       | 1000      | 500       |
  | 800        | -800      | 10        |
  | -1000      | -9500     | 500       |
  | -10000     | -70000    | 5000      |

## Contributing

When adding an M vs H sequence to this repository, be sure to:

1. add the .dat and .rw.dat files to the [calibration_files](calibration_files) folder. Consider renaming the files to "mvsh_seqX.dat" and "mvsh_seqX.rw.dat" for consistency.

2. add the sequence to the "mvsh" key in [calibration.json](calibration.json). The key should be "sequence_X" where X is the next available integer.

3. Add a description of the sequence to the [Sequences summary](#sequences-summary) table above.

# GPX HR Merger

![Python Version](https://img.shields.io/badge/python-3.6%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)

A command-line utility designed to repair, merge, and calibrate activity data. This tool merges Heart Rate data from an external source into a GPS track and recalibrates the total distance to match a specific target (e.g., treadmill display correction).

## Overview

Many athletes record activities using multiple devices—for example, capturing GPS on a smartphone (via Strava/Zwift) while recording Heart Rate on a dedicated watch. This often results in two separate files and mismatched distance data due to GPS drift or treadmill calibration differences.

**GPX HR Merger** solves this by:
1.  Parsing a source GPS file and a separate Heart Rate GPX file.
2.  Synchronizing Heart Rate data to the GPS track based on timestamps.
3.  Recalibrating the entire session's distance to match a user-provided target.
4.  Exporting a compliant `.tcx` file ready for upload to fitness platforms.

## Features

* **Timestamp Synchronization:** Matches HR data points to GPS track points within a configurable time window (default: 5 seconds).
* **Drift Correction:** Calculates the raw GPS distance using the Haversine formula and applies a correction ratio to match the target distance exactly.
* **Platform Compatibility:** Generates standard TCX (Training Center XML) files compatible with Strava, Garmin Connect, TrainingPeaks, and others.
* **Zero Dependencies:** Written in pure Python using only the standard library.

## 📱 Telegram Bot

If you prefer a quick, mobile-friendly solution without using the command line, you can use the Telegram bot: **[@gpx_hr_merger_bot](https://t.me/gpx_hr_merger_bot)**.

> **⚠️ DISCLAIMER:** > Unlike the local script, the Telegram bot does not require you to rename the files to `GPS.gpx` and `HR.gpx`. Instead, it automatically assigns them based on file size: it assumes that the **heavier (larger) file is the HR file**, and the **lighter (smaller) file is the GPS track**. 

If you want to use the bot in a different way, need a custom implementation, or have any issues, feel free to contact me on GitHub or via email at **porporafederico@gmail.com**.

## Requirements

* Python 3.6 or higher.
* No external packages (`pip install`) are required for the local script.

## Installation

Clone the repository or download the script directly:

    git clone https://github.com/YOUR_USERNAME/gpx-hr-merger.git
    cd gpx-hr-merger

## Usage

### 1. File Preparation
Ensure you have your two source files in the project directory:
* **GPS source:** Rename to `GPS.gpx` (Contains location/elevation).
* **Heart Rate source:** Rename to `HR.gpx` (Contains heart rate data).

### 2. Execution
Run the script from the terminal, providing the **target distance in kilometers** as an argument.

Syntax:

    python gpx_hr_merger.py <TARGET_DISTANCE_KM>

Example (correcting a run to 10.5 km):

    python gpx_hr_merger.py 10.5

### 3. Output
The script will generate a file named `output_fixed.tcx` in the same directory. This file contains the merged data and corrected distance, ready for upload.

## Project Structure

    .
    ├── gpx_hr_merger.py    # Main executable script
    ├── GPS.gpx             # Input file (Location)
    ├── HR.gpx              # Input file (Heart Rate)
    └── README.md           # Documentation

## Technical Details

The distance correction is linear. The script calculates the ratio between the `Target Distance` and the `Calculated GPS Distance`. This ratio is applied as a multiplier to the distance delta of every individual trackpoint, preserving the pace variations relative to the original GPS data while fixing the total length.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

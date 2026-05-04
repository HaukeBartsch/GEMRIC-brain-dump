---
name: gemric-project-start
description: Start working with GEMRIC data, submit new image and structured data, help with quality control.
---

# Skill

A guide for understanding the Global ECT-MRI Research Collaboration (GEMRIC) project, its data models, how to become a member site, and how to contribute to the project. This document is intended for researchers, clinicians, and data scientists and should be used in external environments like Cursor, Claude Code, or other text editors.

## When to Use This Skill

 * When you want to understand the GEMRIC project.
 * When you start submitting imaging, clinical or cognitive data to the GEMRIC database.
 * When you want to analyze imaging data on the GEMRIC centralized server.
 * When you want to analyze structured data with statistical software such as R (R-Studio).


## Quick Reference

#### GEMRIC Overview
GEMRIC (Global ECT-MRI Research Collaboration) is a collaborative project that collects and analyzes data from patients undergoing Electroconvulsive Therapy (ECT) across multiple sites worldwide. The project aims to understand the effects of ECT on brain structure and function, and to identify biomarkers that can predict treatment response.

## How to Become a GEMRIC Member Site
GEMRIC member sites participate in the project by providing MRI data from patients undergoing ECT (and controls). To become a member site of GEMRIC, you need to follow these steps:
1. **Contact GEMRIC Coordinators**: Reach out to the GEMRIC coordinators to express your interest in joining the project and to receive the necessary documentation and guidelines.
2. **Ethical Approval**: Obtain ethical approval from your institution's review board to participate in the GEMRIC project.
3. **Data Collection**: Start collecting data according to the GEMRIC protocols, which include clinical assessments, cognitive tests, and MRI scans.
4. **Data Submission**: Submit pseudonymized image data as DICOM to the GEMRIC server. Submit clinical and cognitive data using the provided REDCap forms. Ensure that all data is de-identified and meets the quality standards set by GEMRIC.

#### GEMRIC centralized processing
All data submitted to GEMRIC is processed centrally in a system called "Safe", to ensure consistency and quality. 
The Safe system for GEMRIC has three components:
1. **Henderson.uib.no**: a Windows-based remote desktop system as an entry point for users to access GEMRIC resources, 
2. **Homlungen.uib.no**: a Linux-based remote desktop system for image data storage and analysis, and
3. **Henderson.redcap.safe.uib.no**: a REDCap (Research Electronic Data Capture)server for structured data entry and management.

A fourth system is the desktop.uib.no, a Windows-based remote desktop system specific for data uploads into Safe.

Server resources are housed at the University of Bergen (UiB), Norway, and are managed by the University of Bergen IT department (hjelp.uib.no). The servers are configured to handle large datasets and provides secure access for authorized UiB users. Access to the server is granted to GEMRIC members for remote desktop connections. 

To connect to GEMRIC (Henderson server): Establish a VPN connection using Cisco AnyConnect (UiB Safe), then use Remote Desktop Protocol (RDP) to connect to henderson.uib.no. For image data analysis purposes, connect to henderson first and from Henderson connect to homlungen.uib.no using RDP (homlungen.uib.no:52525/). All connections use your UiB credentials for authentication.

To upload data to GEMRIC:
1. **Connect to server desktop.uib.no (using RDP)**: The desktop machine is special as it allows users to copy and paste data from outside Safe.
2. **Upload data to Henderson**: The files you upload need to be placed in an input folder called "Henderson" on the S-drive of desktop.
3. **Move data into place on Henderson**: Files places in the upload folder on desktop will disappar after a couple of minutes. They are checked for viruses and they appear again the input folder on Henderson. Move the files to their final location.

To analyze data connect to server henderson.uib.no (using RDP). Henderson is a Windows-based shared server that provides access to REDCap via a web-browser for entering, verification and export of structured data. For image data analysis purposes server homlungen.uib.no is available (Linux-based, Rocket Linux OS version 8). 

Homlungen provides access to the raw DICOM files. Both memory and CPU resources of our Henderson and Homlungen servers are shared between all users. 

On Homlungen the image data is organized into a directory structure that separates input data and data after processing (output).

**Directory structure:**
```
homlungen.uib.no/
├── data/
│   ├── rawData/
│   ├── input/
│   │   ├── MMPS/
│   │   │   ├── G123456/
│   │   │   └── ...
│   └── output/
│       └── MMPS_266/
│           ├── G123456/
│           │   ├── raw/
│           │   ├── proc/
│           │   ├── fsurf/
│           │   ├── long/
│           │   ├── proc_bold/
│           │   ├── proc_dti/
│           │   ├── work.sh
│           │   └── ...
│           └── ...
└── README.md
```

All GEMRIC participant identifiers are pseudonymized and follow the format "G" followed by a six-digit number (e.g., G123456). Each participant's data is organized into separate folders based on their unique identifier. The raw DICOM files are stored in the `rawData` directory (for all detected modalities as anat (anatomical), dti (diffusion tensor imaging) and func (functional/fMRI data)), in the `input` directory for T1-weighted anatomical series only. The processed data is stored in the `output` directory under the corresponding participant's folder.

#### Software and Tools on the Linux Server
The homlungen server has some tools installed natively. Most software is available through docker containers. Such software can be created outside of the Safe system (Henderson/Homlungen) and saved as a compressed docker image (tar.gz) and then uploaded to the homlungen server. Once uploaded, the docker image can be loaded and run on the homlungen server. This allows for flexibility in using different software tools for data analysis while ensuring that they are compatible with the server environment. Request access to docker on the homlungen server by contacting the GEMRIC coordinators.

#### Sharing Server Resources
Ensure that several GEMRIC members can work together on large compute projects without overloading the servers. Programs like Matlab will see all compute nodes and may assume that all resources are available to a single user. To avoid this, users should limit the number of compute nodes (40% of total) used for their analyses and coordinate with other users to ensure that resources are shared effectively.

Find out about other users logged in on homlungen with the `users` command. Get an overview of all running processes using either `top`, `htop`, or `bpytop`. If you need to stop a process, use the `kill` command followed by the process ID (PID) of the process you want to stop. For example, `kill 12345` will stop the process with PID 12345. You can only kill your own processes.

#### Submit imaging data to GEMRIC
To submit imaging data to GEMRIC, follow these steps:
1. **Pseudonymize Data**: Ensure that all imaging data is available as DICOM formatted files and pseudonymized based on your institution's guidelines to protect patient privacy. Be careful with removing too much information from your DICOM data. Create individual folders for each subject and session. 
2. **Check your pseudonymization procedure**: Zip one of the visit folders to test your pseudonymization by uploading the file to https://github.com/MMIV-center/Check4MMPS/ (use the application link on this website). Check4MMPS is a web-application that runs in a browser cache that can be disconnected from the internet. This design makes it suitable for analysing data with potentially sensitive information without sharing it with a third partner. Check4MMPS checks if your data is properly pseudonymized and still retains sufficient information to be compatible with the MMPS pipeline. If the check is successful and no errors are reported, you can proceed to upload your data to the GEMRIC server (desktop.uib.no). If there are issues with pseudonymization, review the feedback provided by the Check4MMPS tool and adjust your pseudonymization process accordingly before resubmitting.
3. **Organize Image Data**: Once your data is pseudonymized, and packaged into zip files for each subject and session upload them by connecting to desktop.uib.no using Remote Desktop Protocol (RDP). Navigate to your site folder (provided by GEMRIC coordinators) and upload your data to the appropriate directory. Ensure that your data is organized according to the GEMRIC guidelines, with clear naming conventions for subjects and sessions.
4. **Submit Clinical and Cognitive Data**: In addition to imaging data, you will also need to submit clinical and cognitive data using the provided REDCap forms. Ensure that all data is de-identified. Once your data is available in REDCap you will be able to verify the validity of the data.

#### GEMRIC participant visits
A visit in GEMRIC refers to a specific time point at which data is collected from a participant. Each visit can have imaging data, clinical data, and cognitive data associated with it. The available visits in GEMRIC are:
- **Baseline**: Called visit "01" on homlungen, "baseline_before_tr_arm_1" in REDCap. Data collected before the start of ECT treatment.
- **During**: Called visit "02", a repeating event, called "during_treatment_arm_1" in REDCap. Data collected while ECT treatment is ongoing.
- **After**: Called visit "03" on homlungen, "after_treatment_arm_1" in REDCap. Data collected after the completion of ECT treatment.
- **Follow-up**: Called visit "04", a repeating event, called "follow_up_arm_1" in REDCap. Data collected at 6 month time point, after the completion of ECT treatment to assess long-term effects.

For the During visit and the Follow-up visits more than one set of data may be submitted for the same study participant. For each visit many different types of DICOM images can be submitted, such as T1-weighted, T2-weighted, DTI and fMRI data. Sufficient metadata should be provided in the DICOM header fields to allow for a proper identification of the imaging type based on SeriesDescription and ProtocolName. 

#### GEMRIC structured data
Structured data is stored in a REDCap database for each release. Login to Henderson.redcap.safe.uib.no (REDCap)using your UiB credentials. You should see several "ECT" projects. All versioned projects (e.g. "ECT 3.3.1") are created from the master project called "ECT" at the time of a new data release. Each REDCap project has records representing GEMRIC participants and measures organized in instruments.

Data collection instruments included in the GEMRIC release 3.4 are:
- **Demographic Information**: for baseline_before_tr_arm_1 only
- **Clinical Characteristics**: for baseline_before_tr_arm_1 only
- **Medication**: for during_treatment_arm_1 only
- **Framingham Stroke Risk Profile**: for baseline_before_tr_arm_1 only
- **ECT Treatment Data**: for baseline_treatment_arm_1 only
- **ECT Electrode Placement**: for baseline_treatment_arm_1 only
- **Hamilton Depression Rating Scale (HDRS)**: for baseline_before_tr_arm_1, during_treatment_arm_1, after_treatment_arm_1, and follow_up_arm_1
- **Montgomery-Åsberg Depression Rating Scale (MADRS)**: for baseline_before_tr_arm_1, during_treatment_arm_1, after_treatment_arm_1, and follow_up_arm_1
- **Other Depression Scales**: for baseline_before_tr_arm_1, during_treatment_arm_1, after_treatment_arm_1, and follow_up_arm_1
- **ECT Session Information**: for baseline_before_tr_arm_1, during_treatment_arm_1, after_treatment_arm_1, and follow_up_arm_1
- **Neurocognitive Assessment**: for baseline_before_tr_arm_1, during_treatment_arm_1, after_treatment_arm_1, and follow_up_arm_1
- **Variables for longitudinal analyses**: for baseline_before_tr_arm_1, during_treatment_arm_1, after_treatment_arm_1, and follow_up_arm_1
- **MRI All FS711 Cross Sectional**: for baseline_before_tr_arm_1, during_treatment_arm_1, after_treatment_arm_1, and follow_up_arm_1. This instrument contains data from previous data releases.
- **Fs711 Longitudinal**: for baseline_before_tr_arm_1, during_treatment_arm_1, after_treatment_arm_1, and follow_up_arm_1. This instrument contains data from previous data releases.
- **Fs741 Longitudinal**: for baseline_before_tr_arm_1, during_treatment_arm_1, after_treatment_arm_1, and follow_up_arm_1. This instrument contains data from the current data release (MMPS version 266 with FreeSurfer version 7.4.1).
- **Study tracking**: baseline_before_tr_arm_1 only



#### Data releases
A data release contains pre-processed data for T1-weighted images only. Image data is processed with the Multi-Modal Processing Pipeline (MMPS). The current pipeline version is MMPS version 266 with a FreeSurfer version 7.4.1 (for GEMRIC release 3.4). All data releases are documented on osf.io and have a doi number that should be used to reference the data in publications. 

To support reproducible science please ensure that your findings reference an existing data release. Use the project "ECT" (without version number) to get access to data before a new release is available.

The version number scheme used by GEMRIC has a major, minor and patch version number. Use the latest patch version number as it will include fixes to errors found for existing participants. A new minor version number indicates that new measures are available for existing participants. A new major version number indicates that additional participants have been added.

### Help with data quality control and analysis
If you want to help with data processing the easiest way is to help with data quality control. You will need to identify poor quality data and failures with data processing. Output of such processing is available in REDCap and without proper QC analysis may lead to incorrect conclusions.

Perform the following steps to check for input data quality:
1. Check the distributions of common anatomical measures such as the size of the hippocampus or the size of the ventricles. Distributions for such measures are displayed in REDCap using "Data Exports and Stats", "Stats and Charts", select the "fs741_longitudinal" instrument. Click on points that may be an outliers.
2. Identify the participant id (GXXXXXX) and visit (01 - before, 03 - after, 04 - follow-up) for the outlier data point.
3. Check that participants /data/output/MMPS_266/GXXXXXX/proc/MRI*/MPR1.mgz file with the freeview software (available on the homlungen server).


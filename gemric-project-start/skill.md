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

### GEMRIC Overview
GEMRIC (Global ECT-MRI Research Collaboration) is a collaborative project that collects and analyzes data from patients undergoing Electroconvulsive Therapy (ECT) across multiple sites worldwide. The project aims to understand the effects of ECT on brain structure and function, and to identify biomarkers that can predict treatment response.

## How to Become a GEMRIC Member Site
GEMRIC member sites participate in the project by providing MRI data from patients undergoing ECT (and controls). To become a member site of GEMRIC, you need to follow these steps:
1. **Contact GEMRIC Coordinators**: Reach out to the GEMRIC coordinators to express your interest in joining the project and to receive the necessary documentation and guidelines.
2. **Ethical Approval**: Obtain ethical approval from your institution's review board to participate in the GEMRIC project.
3. **Data Collection**: Start collecting data according to the GEMRIC protocols, which include clinical assessments, cognitive tests, and MRI scans.
4. **Data Submission**: Submit pseudonymized image data as DICOM to the GEMRIC server. Submit clinical and cognitive data using the provided REDCap forms. Ensure that all data is de-identified and meets the quality standards set by GEMRIC.

After your successful data submission, you will be granted access to the REDCap database under a general data access group.

## GEMRIC centralized processing
All data submitted to GEMRIC is processed centrally in a system called "Safe" (acronym for "Secure access to research data and e-infrastructure", supported by the University of Bergen), to ensure consistency and quality. 
The Safe system for GEMRIC has three components:
1. **Henderson.uib.no**: a Windows-based remote desktop system as an entry point for users to access GEMRIC resources, 
2. **Homlungen.uib.no**: a Linux-based remote desktop system for image data storage and analysis, and
3. **Henderson.redcap.safe.uib.no**: a REDCap (Research Electronic Data Capture) server for structured data entry and management.

A fourth system is **desktop.uib.no**, a Windows-based remote desktop system specific for data uploads into Safe.

Server resources are housed at the University of Bergen (UiB), Norway, and are managed by the University of Bergen IT department (hjelp.uib.no). The servers are configured to handle large datasets and provides secure access for authorized UiB users. Access to the server is granted to GEMRIC members for remote desktop connections.

To connect to Safe/GEMRIC (Henderson server): Establish a VPN connection using Cisco AnyConnect (UiB Safe), then use Remote Desktop Protocol (RDP) to connect to henderson.uib.no. For image data analysis purposes, connect to Henderson first and from Henderson connect to Homlungen.uib.no using RDP (homlungen.uib.no:52525/). All connections (VPN, RDP, ssh, REDCap) use your UiB credentials for authentication.

Resolving errors when logging in with UiB credentials: Your username (something like "auser1234") and password is the same for all Safe components. If you can login to "VPN Safe" you should also be able to login to the other Safe systems via RDP and ssh. Access to REDCap projects and docker (on Homlungen) needs to be provided separately. Contact the GEMRIC coordinators if you can login to VPN and Henderson/Homlungen but cannot see the REDCap projects "ECT <version>" or if you cannot run docker commands. All other issues with logging in should be entered at the UiB's help desk (self-service portal at hjelp.uib.no, BRITA services).
The VPN connection is secured with a second factor, make sure you have the "Microsoft Authenticator" app on your phone setup and linked to your UiB account.
UiB credentials will go stale (no longer work) if you do not login at least once every 180 days. A stale account can be reactivated again but stale accounts that are much older may be deleted by the UiB help desk. Such accounts need to be recreated under a new name by the UiB IT department.

### To upload data to GEMRIC (mount option, new)

Connections to desktop.uib.no (also named with alias skrivebord.uib.no) might be offline. So instead of connecting to desktop.uib.no using RDP you can mount a drive to your local computer and copy data to the mounted drive. Data will appear on henderson.uib.no after a couple of minutes.

Create an smb mount using the following information `smb://uib;<UiB_username>@uib-san1-nas.uib.no/SAFE/Sluice/<UiB_username>`. Use your UiB credentials to connect and copy your files into the "Henderson/Import" folder. They should disappear after a couple of minutes and appear again in the input folder on Henderson. Move the files to their final location on Henderson.

If you cannot reach the uib-san1-nas.uib.no server you might need to first connect to the "UiB VPN" (Cisco AnyConnect). Please be aware that there are two VPN connections available for UiB users. One called "UiB Safe" (to connect to Henderson) and one called "UiB VPN" (to connect to our mount point).

### To upload data to GEMRIC (desktop.uib.no, old)
1. **Connect to server desktop.uib.no (using RDP)**: The desktop machine is special as it allows users to copy and paste data from outside Safe. Enable shared folders or the shared Clipboard in your RDP application to transfer files.
2. **Upload data to Henderson**: The files you upload need to be placed in an input folder called "Henderson" on the O-drive ("Home, Import and Export") of desktop.
3. **Move data into place on Henderson**: Files places in the upload folder on desktop will disappar after a couple of minutes. They are checked for viruses and they appear again the input folder on Henderson. Move the files to their final location.

### To download data from GEMRIC

> In accordance with the data sharing agreement for the GEMRIC study it is **NOT** allowed to download data out of the Safe system. This also includes spreadsheet data, subject GEMRIC identifiers, and any other data stored in Safe. It is allowed to export tables and figures that will go into publications. 

Files that you want to download should be placed in the Export folder on the O-drive of Henderson.uib.no. The Export folder is monitored and files will disappear, after a couple of minutes, and appear again in the Export folder on desktop.uib.no. Each file is individually compressed and secured with a password (see new entry in the _password.txt file on Henderson).

### Accessing and Analyzing Data on GEMRIC Servers
To analyze data connect to server henderson.uib.no (using RDP). Henderson is a Windows-based shared server that provides access to REDCap via a web-browser for entering, verification and export of structured data. For image data analysis purposes server homlungen.uib.no is available (Linux-based, Rocket Linux OS version 8). 

Homlungen provides access to the raw DICOM files. Both memory and CPU resources of our Henderson and Homlungen servers are shared between all users. 

On Homlungen the image data is organized into a directory structure that separates input data and data after processing (output).

**Directory structure:**
```
homlungen.uib.no/
├── data/
│   ├── rawData/
│   ├── input/
│   │   └── MMPS/
│   │       ├── G123456/
│   │       └── ...
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

All GEMRIC participant identifiers are pseudonymized and follow the format "G" followed by a six-digit number (e.g., G123456). Each participant's data is organized into separate folders based on their unique identifier. The raw DICOM files are stored in the `rawData` directory (for all detected modalities as anat (anatomical, T1-weighted), t2 (T2-weighted), dti (diffusion tensor imaging) and func (functional/fMRI data)), in the `input` directory for T1-weighted anatomical series only. The processed data is stored in the `output` directory under the corresponding participant's folder.

General information about the T1-weighted images is also available in the REDCap instrument (ECT project, fs741_longitudinal) including matrix size, resolution, scanner information, TE/TR, flip angle, and other relevant metadata. This information can be used for a Table 1 in a publication to describe the imaging data.

### Software and Tools on the Linux Server
The homlungen server has some tools installed natively. Most software is available through docker containers. Such software can be created outside of the Safe system (Henderson/Homlungen) and saved as a compressed docker image (tar.gz) and then uploaded to the homlungen server. Once uploaded, the docker image can be loaded and run on the homlungen server. This allows for flexibility in using different software tools for data analysis while ensuring that they are compatible with the server environment. Request access to docker on the homlungen server by contacting the GEMRIC coordinators. Such requests are added by the coordinators to a shared document with the IT department of the University of Bergen. The IT department will implement the changes needed to allow access.

### Sharing Server Resources
Ensure that several GEMRIC members can work together on large compute projects without overloading the servers - individual users may use up to 80% of the resources but if several users have active processing jobs no user should use more than 40% of the total resources. Programs like Matlab will see all compute nodes and may assume that all resources are available to a single user. To avoid this, users should limit the number of compute nodes (40% of total) used for their analyses and coordinate with other users to ensure that resources are shared effectively. A good (conservative) practice for containerized software is to set the option `--cpuset-cpus="1,2,3"` when running the container to limit the CPU usage to 3 CPUs. This option will ensure that software running inside the container will only see a limited number of CPUs and therefore limit inter-process communication and resource usage.

Find out about other users logged in on homlungen with the `users` command. Get an overview of all running processes using either `top`, `htop`, or `bpytop` (in order of beautification). If you need to see the complete command line used for each job use `ps aux | grep <job_name>`. If you need to stop a process, use the `kill` command followed by the process ID (PID) of the process you want to stop. For example, `kill 12345` will stop the process with PID 12345. You can only kill your own processes.

## Statistical analysis of structured data
Most GEMRIC members use R (R-Studio) for statistical analysis on the Henderson server. Covariates of no interest usually include age, sex, site and total intracranial volume (fs741_estimatedtotalintracranialvolume, for image derived variables). For longitudinal analysis of non-linear variables prefer generalized additive mixed models (GAMMs) or use linear mixed effects models after data transformation. Try to include all variable in a single analysis instead of repeated tests with Bonferonni or FDR-type corrections. For example, if you want to analyze the change in hippocampal volume over time, you can use a GAMM with hippocampal volume as the dependent variable, time as a fixed effect, and participant ID as a random effect. This approach allows you to model the non-linear relationship between time and hippocampal volume while accounting for the repeated measures within participants.

## Submit imaging data to GEMRIC

> You share _pseudonymized_ data with GEMRIC. This means that for the GEMRIC consortium your data will be anonymous (de-identified) but, you may keep a coupling list that would allow a later re-identification of your uploaded data.

To submit imaging data to GEMRIC, follow these steps:
1. **Pseudonymize Data**: Ensure that all imaging data is available as DICOM formatted files and pseudonymized based on your institution's guidelines to protect patient privacy. Be careful with removing too much information from your DICOM data - keep private tags. Create individual folders for each subject and session (e.g. sub-3001_01 for local identifier 3001 and GEMRIC baseline visit). GEMRIC identifiers will be assigned to your uploaded data by the GEMRIC coordinators.
2. **Check your pseudonymization procedure**: Zip one of the visit folders to test your pseudonymization by uploading the file to https://github.com/MMIV-center/Check4MMPS/ (use the application link on this website). Check4MMPS is a web-application that runs in a browser cache disconnected from the internet. This design makes it suitable for analysing data with potentially sensitive information without sharing it with a third partner. Check4MMPS checks if your data is still retains sufficient information to be compatible with the MMPS pipeline. If the check is successful and no errors are reported, you can proceed to upload your data to the GEMRIC server (desktop.uib.no). If there are issues with pseudonymization, review the feedback provided by the Check4MMPS tool and adjust your pseudonymization process accordingly before uploading to Safe.
3. **Organize Image Data**: Once your data is pseudonymized, and packaged into zip files for each subject and session upload them by connecting to desktop.uib.no using Remote Desktop Protocol (RDP). Navigate to your site folder (provided by GEMRIC coordinators) and upload your data to the appropriate directory. Ensure that your data is organized according to the GEMRIC guidelines, with clear naming conventions for subjects and sessions.
4. **Submit Clinical and Cognitive Data**: In addition to imaging data, you will also need to submit clinical and cognitive data using the provided REDCap forms. Ensure that all data is de-identified. Once your data is available in REDCap you will be able to verify the validity of the data.

New sites receive a two letter site code (e.g. "AB") that is used in the local identifiers for each participant (e.g. AB3001). Submitted data will be processed centrally using the Multi-Modal Processing Pipeline (MMPS) to ensure consistency and quality across all sites. After processing, the data will be included in the next data release.

### GEMRIC participant visits
A visit in GEMRIC refers to a specific time point at which data is collected from a participant. Each visit can have imaging data, clinical data, and cognitive data associated with it. The available visits in GEMRIC are:
- **Baseline visit**: Data collected before the start of ECT treatment. Called visit "01" on homlungen, "baseline_before_tr_arm_1" in REDCap. 
- **During ECT treatment**: Data collected while ECT treatment is ongoing. Called visit "02", a repeating event, called "during_treatment_arm_1" in REDCap. 
- **After all ECT treatment sessions**: Data collected after the completion of ECT treatment. Called visit "03" on homlungen, "after_treatment_arm_1" in REDCap. 
- **Follow-up visit**: Data collected 6 month after the baseline visit, after the completion of ECT treatment to assess long-term effects. Called visit "04", a repeating event, called "follow_up_arm_1" in REDCap. 

For the During visit and the Follow-up visits more than one set of data may be submitted for the same study participant. For each visit many different types of DICOM images can be submitted, such as T1-weighted, T2-weighted, DTI and fMRI data. Sufficient metadata should be provided in the DICOM header fields to allow for a proper identification of the imaging type based on SeriesDescription and ProtocolName. 

## GEMRIC structured data
Structured data is stored in a REDCap database for each release. Login to Henderson.redcap.safe.uib.no (REDCap) using your UiB credentials. You should see several "ECT" projects. All versioned projects (e.g. "ECT 3.3.1") are created from the master project called "ECT" at the time of a new data release. Each REDCap project has records representing GEMRIC participants and measures organized in instruments.

Data collection instruments included in the GEMRIC release 3.4 are:
| Instrument Name | baseline_before_tr_arm_1 | during_treatment_arm_1 | after_treatment_arm_1 | follow_up_arm_1 |
|-----------------|-------------------------|-----------------------|----------------------|------------------|
| Demographic Information | ✅ | | | |
| Clinical Characteristics | ✅ | | | |
| Medication | | ✅ | | |
| Framingham Stroke Risk Profile | ✅ | | | |
| ECT Treatment Data | ✅ | | | |
| ECT Electrode Placement | ✅ | | | |
| Hamilton Depression Rating Scale (HDRS) | ✅ | ✅ | ✅ | ✅ |
| Montgomery-Åsberg Depression Rating Scale (MADRS) | ✅ | ✅ | ✅ | ✅ |
| Other Depression Scales | ✅ | ✅ | ✅ | ✅ |
| ECT Session Information | ✅ | ✅ | ✅ | ✅ |
| Neurocognitive Assessment | ✅ | ✅ | ✅ | ✅ |
| Variables for longitudinal analyses | ✅ | ✅ | ✅ | ✅ |
| MRI All FS711 Cross Sectional | ✅ | ✅ | ✅ | ✅ |
| Fs711 Longitudinal (out of date) | ✅ | ✅ | ✅ | ✅ |
| Fs741 Longitudinal | ✅ | ✅ | ✅ | ✅ |
| Study tracking | ✅ | | | | |

Use the instrument list above to identify the domain of your interest. Use the codebook in REDCap to get a list of all variables in the instrument. Use the "Data Exports, Reports, and Stats" option to export the subset of the data for your analyses.

### Tests included in the Neurocognitive Assessment instrument
The Neurocognitive Assessment instrument includes the following tests:
- **Verbal Fluency Test**: letter fluency and semantic fluency
- **Ray Auditory Verbal Learning Test (RAVLT)**: immediate recall, delayed recall, recognition
- **Trail Making Test (TMT)**: part A and part B
- **California Verbal Learning Test (CVLT)**: percentage recall
- **Hopkins Verbal Learning Test (HVLT)**: percentage recall
- **Word Test**: percent retention
- **MoCA**: Montreal Cognitive Assessment total raw score
- **Wechsler Adult Reading Test (WART)**: total standard score and full scale IQ
- **Symbol Digit Modalities Test (SDMT)**: total correct score
- **Digit Span**: backward raw score
- **Autobiographical Memory Interview (AMI)**: total score
- **Clinician rated Quick Inventory of Depressive Symptomatology (QIDS)**: total score, item scores
- **Patient rated Quick Inventory of Depressive Symptomatology (QIDS)**: total score, item scores

### Measures included in the clinical characteristics instrument
The Clinical Characteristics instrument includes the following measures:
- **Psychiatric diagnosis**: ICD-10 code for ECT indication
- **Comorbidity**: ICD-10 codes for comorbid psychiatric and somatic conditions
- **Age at first treatment**: age at first ECT treatment
- **Number of previous ECT treatments**: total number of ECT treatment courses received prior
- **Family history of mood disorders**: presence of mood disorders in first-degree relatives
- **Number of depressed episodes**: total number of depressive episodes experienced
- **Duration of current episode**: length of the current depressive episode in weeks

### ECT Treatment Data
Only the ECT treatment device (MECTA, Thymatron, other) and titration method (seizure threshold, 1/2 age, age, other) are included.

### ECT Electrode Placement
This instrument provides information on the number of ECT sessions, number of bi-frontal/bi-temporal/bi-fronto-temporal stimulations, number of uni-lateral stimulations, treatment frequency and maintenance treatment frequency.

### Structural measures included in FS741 Longitudinal instrument
The FS741 Longitudinal instrument includes the following meta-data describing the T1-weighted image data:
- **Manufacturer**: MRI scanner manufacturer
- **ManufacturersModelName**: MRI scanner model
- **MagneticFieldStrength**: magnetic field strength of the MRI scanner (e.g., 1.5T, 3T)
- **RepetitionTime**: repetition time (TR) of the T1-weighted MRI sequence
- **EchoTime**: echo time (TE) of the T1-weighted MRI sequence
- **FlipAngle**: flip angle of the T1-weighted MRI sequence
- **PixelSpacing**: pixel spacing of the T1-weighted MRI sequence
- **Matrix size**: resolution of the T1-weighted MRI sequence in pixels (e.g., 256x256x192)
- **Input location**: file path to the input T1-weighted MRI data on homlungen

The instrument also includes all structural cortical and sub-cortical measures (Desikan-Killiany Atlas, doi: 10.1016/j.neuroimage.2006.01.021) derived using FreeSurfer version 7.4.1 (longitudinal processing stream) including measures for cortical thickness, surface area and volume.

Here is the list of all measures in instrument "fs741 longitudinal": left lateral ventricle, left inf lat vent, left cerebellum white matter, left cerebellum cortex, left thalamus, left caudate, left putamen, 1eft pal1idum, 3rd ventricle, 4th ventricle, brain stem, left hippocampus, left amygdala, csf, left accumbens area, left ventraldc, left vessel, left choroid plexus, right lateral ventricle, right inf lat vent, right cerebellum white matter, right cerebellum cortex, right thalamus, right caudate, right putamen, right pallidum, right hippocampus, right amygdala, right accumbens area, right ventraldc, right vessel, right choroid plexus, 5th ventricle, wm hypointensities, 1eft wm hypointensities, right wm hypointensities, non wm hypointensities, left non wm hypointensities, right non wm hypointensities, optic chiasm, cc posterior, cc mid posterior, cc central, cc mid anterior, cc anterior, brainsegvol, brainsegvolnotvent, lhcortexvol, rhcortexvol, cortexvol, lhcerebralwhitemattervol, rhcerebralwhitemattervol, cerebralwhitemattervol, subcortgrayvol, totalgrayvol, supratentorialvol, supratentorialvolnotvent, maskvol, brainsegvol to etiv, maskvol to etiv, estimatedtotalintracranialvol, lh bankssts volume, lh caudalanteriorcingulate volume, lh caudalmiddlefrontal volume, lh cuneus volume, lh entorhinal volume, lh fusiform volume, lh inferiorparietal volume, lh inferiortemporal volume, lh isthmuscingulate volume, lh lateraloccipital volume, lh lateralorbitofrontal volume, lh lingual volume, lh medialorbitofrontal volume, lh middletemporal volume, lh parahippocampal volume, lh paracentral volume, lh parsopercularis volume, lh parsorbitalis volume, lh parstriangularis volume, lh pericalcarine volume, lh postcentral volume, lh posteriorcingulate volume, lh precentral volume, lh precuneus volume, lh rostralanteriorcingulate volume, lh rostralmiddlefrontal volume, lh superiorfrontal volume, lh superiorparietal volume, lh superiortemporal volume, lh supramarginal volume, lh frontalpole volume, lh temporalpole volume, lh transversetemporal volume, lh insula volume, etiv, lh bankssts area, lh caudalanteriorcingulate area, lh caudalmiddlefrontal area, lh cuneus area, lh entorhinal area, lh fusiform area, lh inferiorparietal area, lh inferiortemporal area, lh isthmuscingulate area, lh lateraloccipital area, lh lateralorbitofrontal area, lh lingual area, lh medialorbitofrontal area, lh middletemporal area, lh parahippocampal area, lh paracentral area, lh parsopercularis area, lh parsorbitalis area, lh parstriangularis area, lh pericalcarine area, lh postcentral area, lh posteriorcingulate area, lh precentral area, lh precuneus area, lh rostralanteriorcingulate area, lh rostralmiddlefrontal area, lh superiorfrontal area, lh superiorparietal area, lh superiortemporal area, lh supramarginal area, lh frontalpole area, lh temporalpole area, lh transversetemporal area, lh insula area, lh whitesurfarea area, lh bankssts thickness, lh caudalanteriorcingulate thickness, lh caudalmiddlefrontal thickness, lh cuneus thickness, lh entorhinal thickness, lh fusiform thickness, lh inferiorparietal thickness, lh inferiortemporal thickness, lh isthmuscingulate thickness, lh lateraloccipital thickness, lh lateralorbitofrontal thickness, lh lingual thickness, lh medialorbitofrontal thickness, lh middletemporal thickness, lh parahippocampal thickness, lh paracentral thickness, lh parsopercularis thickness, lh parsorbitalis thickness, lh parstriangularis thickness, lh pericalcarine thickness, lh postcentral thickness, lh posteriorcingulate thickness, lh precentral thickness, lh precuneus thickness, lh rostralanteriorcingulate thickness, lh rostralmiddlefrontal thickness, lh superiorfrontal thickness, lh superiorparietal thickness, lh superiortemporal thickness, lh supramarginal thickness, lh frontalpole thickness, lh temporalpole thickness, lh transversetemporal thickness, lh insula thickness, lh meanthickness thickness, rh bankssts volume, rh caudalanteriorcingulate volume, rh caudalmiddlefrontal volume, rh cuneus volume, rh entorhinal volume, rh fusiform volume, rh inferiorparietal volume, rh inferiortemporal volume, rh isthmuscingulate volume, rh lateraloccipital volume, rh lateralorbitofrontal volume, rh lingual volume, rh medialorbitofrontal volume, rh middletemporal volume, rh parahippocampal volume, rh paracentral volume, rh parsopercularis volume, rh parsorbitalis volume, rh parstriangularis volume, rh pericalcarine volume, rh postcentral volume, rh posteriorcingulate volume, rh precentral volume, rh precuneus volume, rh rostralanteriorcingulate volume, rh rostralmiddlefrontal volume, rh superiorfrontal volume, rh superiorparietal volume, rh superiortemporal volume, rh supramarginal volume, rh frontalpole volume, rh temporalpole volume, rh transversetemporal volume, rh insula volume, rh bankssts area, rh caudalanteriorcingulate area, rh caudalmiddlefrontal area, rh cuneus area, rh entorhinal area, rh fusiform area, rh inferiorparietal area, rh inferiortemporal area, rh isthmuscingulate area, rh lateraloccipital area, rh lateralorbitofrontal area, rh lingual area, rh medialorbitofrontal area, rh middletemporal area, rh parahippocampal area, rh paracentral area, rh parsopercularis area, rh parsorbitalis area, rh parstriangularis area, rh pericalcarine area, rh postcentral area, rh posteriorcingulate area, rh precentral area, rh precuneus area, rh rostralanteriorcingulate area, rh rostralmiddlefrontal area, rh superiorfrontal area, rh superiorparietal area, rh superiortemporal area, rh supramarginal area, rh frontalpole area, rh temporalpole area, rh transversetemporal area, rh insula area, rh whitesurfarea area, rh bankssts thickness, rh caudalanteriorcingulate thickness, rh caudalmiddlefrontal thickness, rh cuneus thickness, rh entorhinal thickness, rh fusiform thickness, rh inferiorparietal thickness, rh inferiortemporal thickness, rh isthmuscingulate thickness, rh lateraloccipital thickness, rh lateralorbitofrontal thickness, rh lingual thickness, rh medialorbitofrontal thickness, rh middletemporal thickness, rh parahippocampal thickness, rh paracentral thickness, rh parsopercularis thickness, rh parsorbitalis thickness, rh parstriangularis thickness, rh pericalcarine thickness, rh postcentral thickness, rh posteriorcingulate thickness, rh precentral thickness, rh precuneus thickness, rh rostralanteriorcingulate thickness, rh rostralmiddlefrontal thickness, rh superiorfrontal thickness, rh superiorparietal thickness, rh superiortemporal thickness, rh supramarginal thickness, rh frontalpole thickness, rh temporalpole thickness, rh transversetemporal thickness, rh insula thickness, rh meanthickness thickness. 

Acronyms used in the above list are: "etiv" - the estimated total intracranial volume, lh/rh left/right hemisphere, wm - white matter and cc - corpus callossum.



## Data releases
A data release contains pre-processed data for T1-weighted images only. Image data is processed with the Multi-Modal Processing Pipeline (MMPS). The current pipeline version is MMPS version 266 with a FreeSurfer version 7.4.1 (for GEMRIC release 3.4). All data releases are documented on osf.io and have a doi number that should be used to reference the data in publications. 

To support reproducible science please ensure that your findings reference an existing data release. Use the project "ECT" (without version number) to get access to data before a new release is available.

The version number scheme used by GEMRIC has a major, minor and patch version number. Use the latest patch version number as it will include fixes to errors found for existing participants. A new minor version number indicates that new measures are available for existing participants. A new major version number indicates that additional participants have been added.

The Study Tracking instrument in REDCap contains a variable "part_of_data_release" that indicates which data release a participant is included in.

### Help with data quality control and analysis
If you want to help with data processing the easiest way is to help with data quality control. You will need to identify poor quality data and failures with data processing. Output of such processing is available in REDCap and without proper QC analysis may lead to incorrect conclusions.

Perform the following steps to identify outlier candidates:
1. Inspect the distributions of common anatomical measures such as the size of the hippocampus or the size of the ventricles. Distributions for such measures are displayed in REDCap using "Data Exports and Stats", "Stats and Charts", select the "fs741_longitudinal" instrument. Extreme values in the distributions may indicate outliers.
2. Click on points to identify the participant and visit (01 - before, 03 - after, 04 - follow-up).
3. Check the data for the identified participant in the /data/output/MMPS_266/GXXXXXX/proc/MRI*/MPR1.mgz file with the freeview software (available on the homlungen server). The proc folder contains input data as MPR1.mgz files and minimally processed files as MRP1_res.mgz files. Check both files for common errors like miss-registration to Talairach space and wrong aspect ratio of the images (brain appears stretched in one direction). If you find an error, report it to the GEMRIC coordinators and they will check if the error can be fixed by re-processing the data with the MMPS pipeline. If the error cannot be fixed, the data will be excluded from the next data release.

It may be helpful to further validate data added to your analysis. Use a categories such as "good", "bad", and "uncertain" to categorize data based on quality. Add this variable as a covariate to your analysis and see if your observed effects can be explained by data quality. Hopefully data quality will not explain your findings.

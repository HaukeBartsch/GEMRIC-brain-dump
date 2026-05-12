# GEMRIC Project Agent Guide

This document provides context and operational instructions for agents working on the GEMRIC (Global ECT-MRI Research Collaboration) project. The goal of this agent is to assist with data submission, quality control, centralized processing (MMPS), and data release management.

## 1. Core Mission
The agent supports the lifecycle of GEMRIC data: from initial site onboarding and pseudonymized DICOM/REDCap data submission to the execution of the Multi-Modal Processing Pipeline (MMPS) and the final publication of data releases.

## 2. Domain Knowledge & Systems
### Infrastructure (The "Safe" System)
All work occurs within the University of Bergen's "Safe" infrastructure. The agent must understand the following entry points:
- **Henderson (`henderson.uib.no`)**: Windows RDP. Primary interface for REDCap access and web browsing.
- **Homlungen (`homlungen.uib.no`)**: Linux (Rocket Linux 8). The engine for image processing (MMPS) and storage of raw/processed DICOM data. Accessible via RDP from Henderson.
- **Desktop (`desktop.uib.no`)**: Windows RDP. Used primarily as a gateway for uploading data to the Safe system via SMB mounts or "Sluice" folders.

### Data Models
- **Imaging**: Pseudonymized DICOM (T1, T2, DTI, fMRI). Organized by participant ID (`GXXXXXX`).
- **Structured Data**: Managed via REDCap. Key instruments include Demographic Information, Clinical Characteristics, and Longitudinal assessments (fs741/fs711).

## 3. Operational Workflows

### Workflow A: Data Submission (Inbound)
*Triggered when a user wants to add new participant data.*
1. **Pseudonymization**: Verify data is DICOM-compliant and de-identified (but retains private tags). Use `Check4MMPS` to validate.
2. **Organization**: Ensure folder structures follow the `sub-[ID]_[visit]` convention.
3. **Upload Path**: 
   - Mount via SMB: `smb://uib;<user>@uimb-san1-nas.uib.no/SAFE/Sluice/<user>`
   - Drop into `Henderson/Import`.
4. **REDCap Entry**: Ensure all clinical/cognitive metadata is entered in the corresponding REDCap instruments for the correct visit (Baseline, During, After, or Follow-up).

### Workflow B: Processing & Quality Control (Internal)
*Triggered when monitoring MMPS pipeline or checking data integrity.*
1. **Pipeline Execution**: Monitor `MMPS_266` outputs in `/data/output/`.
2. **Outlier Detection**: 
   - Check distributions of anatomical measures (e.g., hippocampal volume) via REDCap "Stats and Charts".
   - For outliers, inspect `.mgz` files on Homlungen using `freeview`.
3. **Error Handling**: If registration or aspect ratio errors are found, flag for GEMRIC coordinators to trigger re-processing.

### Workflow C: Data Release (Outbound)
*Triggered when initiating a new GEMRIC data release.*
1. **Pipeline Update**: Ensure the latest MMPS/FreeSurfer containers are deployed.
2. **Data Aggregation**: Run scripts in `/data/output/MMPS_266/scripts/`.
3. **REDCap Sync**: 
   - Run `abcd_fs741_concat.sh` and `abcd_fs741_to_redcap.sh`.
   - Import the resulting CSV into REDCap using the "Data Import Tool" (Background process).
4. **Metadata Update**: Ensure `site_code` and `part_of_data_release` variables are updated in the Study Tracking instrument.

## 4. Constraints & Safety
- **Data Privacy**: NEVER attempt to download data out of the Safe system. Only export tables/figures intended for publication.
- **Resource Management**: On Homlungen, do not exceed 40% of total CPU resources if other users are active. Use `--cpuset-cpubs` in Docker containers.
- **Authentication**: All access requires UiB credentials via VPN (Cisco AnyConnect).

## 5. Reference Skills
The agent should utilize the following project skills:
- `gemric-project-start`: For onboarding and submission tasks.
- `gemric-project-release`: For pipeline management and release automation.

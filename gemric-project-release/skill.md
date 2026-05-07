---
name: gemric-project-release
description: Create a GEMRIC data release.
---

# GEMRIC data release

This skill describes the process of creating a GEMRIC data release, which involves compiling and organizing imaging and structured data into a standardized format for distribution to researchers. The data release includes both raw DICOM images and structured metadata stored in REDCap databases.

### Create a new MMPS pipeline
Download the latest MMPS source code from San Diego. Add the newest minor version of FreeSurfer and create the docker container for MMPS. As MMPS is using Matlab make sure that you have a current license file that is included into the MMPS container. Inside the Safe system a Matlab license server is available and hjelp.uib.no lists details on the correct license server and license file syntax. Upload the container as a tar.gz file to Homlungen.uib.no.

### Run the MMPS pipeline on all data
Use the scripts in /data/output/MMPS_266/scripts/ and adjust them to the new data release. 

### Import FreeSurfer data into REDCap
FreeSurfer data needs to be converted to a REDCap import file. All variables need to exist in a REDCap instrument.

After processing using MMPS and populated proc and fsurf folders (including longitudinal processing) three steps are required:
1. Run the MMPS summarize script.
2. Run the scripts/abcd_fs741_concat.sh.
3. Run the scripts/abcd_fs741_to_redcap.sh to create a REDCap import file.

The file will appear with a date stamp in the /data/output/MMPS_266/Redcap_fs741_year-month-day.csv folder. Import the file into REDCap using the "Data Import Tool" in the "ECT" project. Use the "Import as background process" option.

### Special REDCap variables

The site_code field in the instrument Study Tracking contains a single letter code for each site (like "A"). This column needs to be re-generated if data is added to REDCap. This column is added by a script in /home/hba069/src/add_site_for_release.php based on the data access group for each subject. Run the script after new participants are added to REDCap. Make sure that all participant belong to their respective data site based data access groups.

The "part_of_data_release" variable also in Study Tracking should be updated before a data release. The variable contains a checkbox for each data release (3.3, 3.4, etc.).

# Generate EIDs
The files in this directory are used to generate the list of EIDs (participant IDs)/CWA files that meet the inclusion critera for processing. The scripts in this folder remove records that do not have sufficent wear time (defined as 20 hours a day in our paper)

The directory also contains the final list of EIDs that were processed in our paper. Note that this list contains less files than the script generates as they are additional inclusion critera that are processed in the plotting files in stage 3 of the processing.

File descripition:
`full_eid_list.txt` The full list of EIDs (pariticpants) that were available to this project
`good_eids.txt` The full list of the EIDs that were processed in our Energy Harvesting Biobank paper (67,024 participants)
`checked_records.txt` a sample output from `generate_cwa_weartime.py` which is used in the next stage of processing. This contains the filenames of the cwa files that are used in the processing
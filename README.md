# UKBiobankKineticEnergyHarvesting
This repository contains software for processing the UK Biobank accelerometer datasets to generate estimates of a kinetic energy harvester output. This code allows output to be stratified by range of factors as provided by the metadata in the UK Biobank.

If using this code please cite the paper:
[C. Beach and A. J. Casson, "Estimation of kinetic energy harvesting potential for self-powered wearable devices with 67,000 participants from the UK Biobank," in *IEEE*, vol. x, no. x, pp. x–y, 2021, doi: DOI](https://ieeexplore.ieee.org/xpl/RecentIssue.jsp?punumber=6221020)

This software is developed from our origional fork of the [biobankAccelerometerAnalysis](https://github.com/activityMonitoring/biobankAccelerometerAnalysis) from the University of Oxford, and our version of this code developed for processing on University of Manchester high-performance computing infrastructure, [biobankActivityCSF](https://github.com/CASSON-LAB/BiobankActivityCSF)

This code has been developed for running on the [CSF3](http://ri.itservices.manchester.ac.uk/csf3/) at the University of Manchester, which is a High Performance Computing (HPC) cluster, running SGE (a batch scheduling system) on Linux. However, with minimial modification it is possible to run this code on other HPC systems (and on the desktop, but this will take a very long time to process all UK Biobank files).

## Dataset
This code relies on the bulk accelerometery data in CWA that was collected by the UK Biobank and is available under [Data-Field 90001](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=90001). This data can be accessed by users making an application to the [UK Biobank](http://www.ukbiobank.ac.uk/using-the-resource/). Along with the bulk CWA data, this project also makes use of the following data fields (this list may not be exaustive exclusive list, and you may not require all of these fields depending on your analysis):
 - [31](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=31) (sex)
 - [34](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=34) (year of birth)
 - [52](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=52) (month of birth)
 - [53](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=53) (date of attending assessment centre)
 - [90053](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=90053) (wear duration during Monday)
 - [90054](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=90054) (wear duration during Tuesday)
 - [90055](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=90055) (wear duration during Wednesday)
 - [90056](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=90056) (wear duration during Thursday)
 - [90057](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=90057) (wear duration during Friday)
 - [90058](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=90058) (wear duration during Saturday)
 - [90059](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=90059) (wear duration during Sunday)
 - [2443](https://biobank.ctsu.ox.ac.uk/crystal/field.cgi?id=2443) (diabetes diagonised by doctor)

For users at the University of Manchester, the bulk data and the metadata has already been downloaded to the CSF3 and can be accessed at `/mnt/data-sets/ukbiobank/activity-data/bulk/` and the metadata at `/mnt/data-sets/ukbiobank/activity-data/casson_proj33693/ukb23305.csv`. To access these files you need to be added to the UNIX groups `dataset-ukbiobank-activity-data` and `ja01_casson_activ_dat_proj33693`, please note this is only for those working under UK Biobank project number 33693, and those with different project numbers will have different participant numbers. As the UK Biobank is no longer providing participant linking data, others not on this project will have to download the cwa data again (24 TB).

**Note that those in NIBS updated versions of these csv files can also be access at on the lab network shared area in `data/uk_biobank/install_files`, `ukb44242.csv` and `ukb44242.csv`**

## Using the code
The software is split into three broad parts:
1. Generating list of participant numbers to process that meet requirements. 
  
  This step reduces processing requirements by only processing files from participants that met inclusion critera (such as wear-time). In theory all files could be processed and then the analysis in step 3 could just be run on the files you we are interested in, but to speed up processing it is recommened to remove any participants from processing at this stage.
  
2. Generating energy harvester output for each cwa record
  
  This stage converts the accelerometer waveform into power output from the energy harvester over time. This stage is the most computationally intensive and takes around 2 weeks to run on the CSF3 (but this figure will vary depending on how busy the system is). This stage takes .cwa files as the input and outputs .h5 files (one for each participant).
 
3. Generating plots and summary stastitics of data

  The final stage takes all the .h5 files generated in the previous stage, imports them and then allows stratificaiton of the data to create plots, averages of the data and summary stastitics. As all .h5 files are loaded into RAM, this stage is very IO and memory intensive.
  

## IRMA-SPY Documentation
<hr>

![alt text](./images/irma-spy.jpg)

 _**This documentation assumes you have the four required Docker containers already running. If not, please start with [setup instructions](./README.md)._

<hr>

 ## Contents
1. [Stage your fastqs](#stage-your-demultiplexed-data)
2. ...
<hr>

## Stage your demultiplexed data
1. In your "FLU_SC2_SEQUENCING" folder, make a folder for your run. It is very helpful to name your runs with a consistent naming system, such as "YEAR-MONTH-DATE-FLOWCELL_ID", ie. 2022-11-13-ABC1234. **_DO NOT PUT SPACES OR SLASHES IN YOU RUN FOLDER NAMES!_**
2. Copy the run's demultiplexed fastqs into the RUN-FOLDER.
    1. If this run is from an Oxford Nanopore Technologies' (ONT) sequencer, copy the "fastq_pass" directory from the sequencer's output into the RUN-FOLDER. On a Mk1C instrument, this is found in /data/RUN_NAME/EXPERIMENT_NAME/NANOPORE-NAME/fastq_pass where RUN_NAME and EXPERIMENT_NAME are defined by you in MinKNOW when you set up the sequencing run on the device and the NANOPORE-NAME is created by the instrument during the run.
    2. If this run is from an Illumina instrument, create another folder inside your RUN-FOLDER called "fastqs" and copy the folder containing demultiplexed fastqs into this newly created fastqs folder so that the folder structurs is FLU_SC2_SEQUENCING/RUN-FOLDER/fastqs/demultiplexed-fastqs/sample-reads.fastq. There will be two fastq files per sample.
3. Copy the run's demultiplexing summary file into the RUN-FOLDER.
    1. From ONT, this file is in the folder that also contains the "fastq_pass" folder and is called "sequencing_summary.txt".
    2. From Illumina, this file is called "laneBarcode.html".

[Return to contents](#contents)

## Open IRMA-SPY
1. Open Docker Desktop and navigate to the `Containers` tab on the left sidebar. Assure that the following containers are running or run each of them:
    1. irma-1.0.2p3
    2. dais-ribsome-1.2.1
    3. sc2-spike-seq
    4. irma-spy
    
    ![alt text](./images/dockerdesktop_run_containers.png)
2. Run IRMA-SPY by clicking the icon of the box with the arrow pointing to the top left. This will open IRMA-SPY into your default internet browser.

![alt text](./images/dockerdesktop_launch_irmaspy.png)
3. Click the `REFRESH RUN LISTING` button and select your run from the dropdown box.

![alt text](./images/spy-selectRun.png)

4. Now, enter your `Bar



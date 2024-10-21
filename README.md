# DM-FS (Death Modulated Fatal Shootings)

This GitHub repository contains the code used to create the databases contained within the DM-FS paper.

## Installation

Create a new Python virtual environment, ideally using Python 3.10. Then, navigate to the code folder and use the package manager [pip](https://pip.pypa.io/en/stable/) to install the dependencies.

```bash
pip install -r requirements.txt
```

## Setup

The code requires the user to undertake the following:
1. Download the three crowdsourced databases per the instructions on the data descriptor. Put them in the "res/1.0 - Original Fatal Shooting Databases" directory.
2. Manually convert the LOEKA reports into a tabular format, following the methods in the data descriptor. Put the end result in "res/2.0 - DM-FS with Manual Corrections." This step isn't needed until ORI Assignment.
3. Download the BJS crosswalk file following the methods in the dataset descriptor. Put the end result in "res/2.0 - DM-FS with Manual Corrections." This step, once again, isn't needed until ORI assignment.

See "Note on Replication" below for further context.

## Code Logic: Create DM-FS Civilians
The code is broken into two Jupyter notebook files.

**1) Create DM-FS for Manual Corrections.ipynb.** This file creates a preliminary version of DM-FS Civilians. Namely, it implements Database Merge discussed in the dataset descriptor: it ingests the three crowdsourced databases (FE, MPV, WP), merges them together, and then outputs an early version of DM-FS Civilians that needs manual corrections.

- ***Input.*** Add the three crowdsourced database files to "res/1.0 - Original Fatal Shooting Databases"
- ***Auxilary Files.*** The merger will require the user to verify (i) all approximate string matches ("fuzzy matches") and (ii) all suspicious matches. All verification that the user undertakes will appear in the directory "1.1 - DM-FS Loop Manual Matches". It will be saved as a CSV file so other users can replicate the decisions.
- ***Output.*** A preliminary DM-FS Civilians database will appear in "res/1.2 - DM-FS Before Manual Corrections", as well as a portion of DM-FS Civilians that needs manual corrections.

The file 'manual_correction_to_make.csv' will appear in the output directory. It is expected that the user will open this CSV file and apply manual corrections to the values that the algorithm highlighted --- i.e., any value that has an "ERROR" string in it. Once the user fixes a particular value, the user is also expected to go to the right of the CSV file and identify the database that is at fault for the discrepancy. For example, if there is a date discrepancy between FE, WP, and MPV, and FE was the only database that had the date correct, then the user should go to "date_fe", and set that value to False (indicating FE is not at fault), and set "date_MPV" and "date_WP" to True (indicating both MPV and WP are at fault for a discrepancy).



**2) Appy Manual Corrections and Display Discrepancy Results.ipynb** This file applies the manual corrections made in the previous step to DM-FS Civilians. Namely, it first applies the discrepancies after doing basic data input checks. Second, it displays the results of the discrepancies --- results that were used in Table 6 of the paper. Third, it creates Figure 2, the Venn Diagram that visualizes which fatal shootings belong to which database

- ***Input.*** There should be two crucial files in the directory "res/1.2 - DM-FS Before Manual Corrections". First, it should have the file "manual_corrections_completed.csv", which contains all the corrected manual corrections. Namely, this file should be a copied-and-pasted version of "manual_correction_to_make.csv" (output of the first Jupyter Notebook), albeit with all the manual corrections made and discrepancies identified. Second, it should have the file "DMFS_no_manual_corrections.csv", which should have been outputted from the first Jupyter Notebook.
- ***Output.*** Outputs the file "DM-FS with Manual Corrections.csv" to "res/2.0 - DM-FS with Manual Corrections/" 

## Code Logic: Assign ORI Codes
The code is broken into one Jupyter notebook file.

**3) ORIs.ipynb**. This file recreates the ORI assignment found in the dataset descriptor. Namely, it assigns each agency from DMFS Officers and DMFS Civilians an ORI code from the BJS crosswalk file. The end result will be two DMFS databases, Officers and Civilians, in which each LEA has its own ORI code, in addition to other relevant variables from the crosswalk (e.g., agency classification, FIPS code, etc.)

- ***Input.*** There should be three files in "res/2.0 - DM-FS with Manual Corrections". First, there should be the DM-FS Civilians database with manual corrections applied called "DM-FS with Manual Corrections.csv." Second, there should be the DM-FS Officers file, which was manually assembled from LEOKA Reports called "M-FS - Officers before ORIs.csv". Third, there should be the BJS crosswalk file called "ORI_Crosswalk.csv". See paper for details
- ***Auxilary Files.*** The algorithm will require the user to undertake (i) manual link review and (ii) manually assign ORI codes. The output of both appears in "res/3.0 - Manual ORI Matches", which can be used for replication.
- ***Output.*** Two files will appear in the directory "res/3.1 - Penultimate Databases": DMFS Civilians and Officers.


## Code Logic: Final Steps
The code is broken into one Jupyter notebook file.

**4) Calculate Deaths, Filter, Redact, etc.ipynb** This file implements the "Final Steps" portion of the dataset descriptor. Namely, it calculates civilian and officers deaths, redacts DMFS, exports the full version, and exports a cleaned version for ease of use.

- ***Input.*** There should be two files that appear in "res/3.1 - Penultimate Databases": DMFS Civilians and Officers, which should have been automatically generated from ORI assignment.
- ***Output.*** Four files will output: DM-FS Civilians and DM-FS Civilians (cleaned); DM-FS Officers and DM-FS Officers (cleaned).


## Note on Replication
The journal in which the dataset is published prohibits the publication of the databases that went into the construction of DM-FS Civilians (i.e., WP, MPV, and FE), as well as LEOKA. This is because these databases contain direct personal identifiers, and the journal is not willing to publish datasets with such identifiers. Thus, to replicate the code, the user needs to download/assemble the databases himself/herself following the methods in the dataset descriptor.

In this regard, exact replication is unlikely. Indeed, parts of this code - particularly in the third notebook, ORI assignment, as well as the first notebook, the database merge - operate by using the *exact* database indices that appeared in the author's database. Thus, even if the user downloads his/her own copy of WP, MPV, and FE, then unless the database indices are exactly the same as the authors, exact replication is unlikely to occur.

However, the code can still be used to understand the general process by which DM-FS was created.

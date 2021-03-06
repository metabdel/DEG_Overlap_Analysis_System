# DEG Overlap Analysis System

## System Description

## MySQL Database

### Database Overview

The main database consists of ten relationaly linked tables
![alt text](https://github.com/afaranda/DEG_Overlap_Analysis_System/blob/master/DEG_Overlap_ER_Diagram.png)
**EXPERIMENT**

   |Column|Description|
   |-----|-----------|
   | id | The unique integer identifying this experiment |
   | lab | The lab where this experiment was conducted |
   | title | The descriptive title of this experiment |
   | repository | The external database where data from this experiment is stored |
   | external_accession | Unique identifier assigned to this experiment by the repository |
   | organism | The organism used in this experiment |
   | proc_method | Description of the method used to process and analyze all samples in this experiment |

   
**SAMPLE**

   |Column|Description|
   |-----|-----------|
   | experiment_id | The unique integer id of the experiment this sample is a part of |
   | label | The alphanumeric label of this sample, that uniquely identifies it within an experiment |
   | external_accession | Unique identifier assigned to this sample by the external repository |
   | short_name | Name used to represent this sample in downstream analysis (may be label) |
   | platform | The experimental platform used to analyse this sample |
   | proc_batch | The batch of samples that this sample was processed with |
   
**SAMPLE\_CAT\_ATR**

   |Column|Description|
   |-----|-----------|

**SAMPLE\_NUM\_ATR**

   |Column|Description|
   |-----|-----------|
   
**GROUP**

   |Column|Description|
   |-----|-----------|

**GROUP\_ASSIGN**

   |Column|Description|
   |-----|-----------|

**CONTRAST**

   |Column|Description|
   |-----|-----------|

**GENE**

   |Column|Description|
   |-----|-----------|

**PAIRWISE\_RESULT**

   |Column|Description|
   |-----|-----------|

**SAMPLE\_MEASUREMENT**

   |Column|Description|
   |-----|-----------|

### MySQL Database Implementation Files
 - **DEG\_Overlap\_Model.mwb**<br>
   ER-Diagram of the Database developed in MySQL-Workbench
   
   
 - **DEG\_Overlap\_Create\_Script.sql**<br>
   DDL Script containing Table Definitions for the main system.
   This file is generated by MySQL-Workbench using the "Forward Engineering"
   Function. All Primary / Foreign key constraints are defined in 
   the ER-Diagram. Running this script against a MySQL server creates the 
   database **DEG_Overlap_Analysis_System**, with ten tables

 - **Load_Experiment.sql**<br>
   Defines stored procedures to create/remove ETL staging tables, validate data being
   loaded in, and load data from staging tables into main database. Also creates several
   views used for diagnostic purposes.
   
 - **DEG_Overlap_ETL_Functions.R**<br>
   This 'R' script defines all functions used to push data from the 'R' environment
   into the MySQL data. 

 - **Template_Overlap_ETL.R**<br>
   This template-script templates the ETL process for loading transcriptomic analysis
   data into the database.  The template contains several example tables, and comments
   indicating where the user should update fields with appropriate information.  To use
   this template, make an appropriately named copy that can be run in batch mode.

### MySQL Database Validation Files
The following scripts can be run against an empty database, immediately after creating
main tables and loading stored ETL procedures.


 - **Valid\_Pairwise\_Load\_symbol.sql**<br>
   Inserts a set of valid records into staging tables except stg\_SAMPLE\_MEASUREMENT
   Measurements in stg\_PAIRWISE\_RESULT are mapped to the 'symbol' column of table 'GENE'


 - **Valid\_Pairwise\_Load\_external\_id.sql**<br>
   Inserts a set of valid records into staging tables including stg\_SAMPLE\_MEASUREMENT
   Measurements in stg\_PAIRWISE\_RESULT are mapped to the 'external_id' column of table 'GENE

 - **Pairwise\_Load\_external\_id_foreign_key_errors.sql**<br>
   Inserts a set of invalid records into staging tables including stg\_SAMPLE\_MEASUREMENT
   Measurements in stg\_PAIRWISE\_RESULT are mapped to the 'external_id' column of table 'GENE'
   Foreign Key errors have been palced in data for tables 'stg\_SAMPLE\_CAT\_ATR', 'stg\_GROUP\_ASSIGN'
   and 'stg\_PAIRWISE\_RESULT'.

### ETL Process
   - Login to a mysql terminal session, create tables and etl procedures in new database 
     "DEG_Overlap_Analysis_System"
     ```
     mysql> source DEG_Overlap_Create_Script.sql;
     mysql> source Load_Experiment.sql;
     
     ```
   - Load GENE tables If "--secure-file-priv is enabled, the following must be run as root"
   ```
   $ sudo R CMD BATCH Gene_Table_ETL.R
   ```
   
   - Run ETL script(s) for each experiment to load pairwise / measurement results
   ```
   $ sudo R CMD BATCH Pax6_Overlap_ETL.R
   ```
### Analysis and Reporting Tools

- **Overlap_Comparison_Functions.R**<br>
- **Excel_Reports.R**<br>
- **Excel_Write_Functions.R**<br>
- **Generic_Excel_Report**<br>




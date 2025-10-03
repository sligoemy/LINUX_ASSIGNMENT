
#1.	 LINUX ASSIGNMENT

## Setting up the Environment
I connected to the virtual machine with the username
	ssd sligoemy@***.**.***.**

## Creating the directories
	mkdir data_pipeline/

	cd  data_pipeline/
	
	mkdir input/ output/ logs/

	
First line creates a directory called "Data_Pipeline"

Second line changes directory to the created pipeline

Third line creates three sub-directories... "input", "output", "logs"

## Data Ingestion and Preprocessing
I ingested the data using Secure Copy Protocol (SCP). This enabled me copy the file into my Virtual Environment

	 scp ~/Downloads/sales_data.csv sligoemy@***.**.***.***:~/data_pipeline/input/


As earlier mentioned, this enables copy the sales_data file into the input subdirectory in my VM. 

Next is to create the preprocess.sh file to clean and prepare the data files. But first I have to change the directory to input subdirectory

	cd datapipeline/input

	touch preprocess.sh

This creates the preprocess file. To write the script in preprocess.sh, I have to enter into it. 

	nano preprocess.sh

Removing the extra Column and Filtering out  the row that contains Status = failed

	#!/bin/bash	

	INPUT=~/data_pipeline/input/sales_data.csv
	OUTPUT=~/data_pipeline/output/cleaned_sales_data.csv
	LOGS=~/data_pipeline/logs/preprocess.log

	cut -d',' -f1-6 "$INPUT" |
	grep -v ",Failed$" > "$OUTPUT"
	echo "Preprocessing complete. Cleaned file saved to $OUTPUT" >> "$LOGS"

Make preprocess.sh executable

	chmod +x preprocess.sh

## Automate the Pipeline with Cron JObs
This is basically meant to run preprocess.sh at 12:00 midnight. 

	crontab -e

	0 0 * * * /data_pipeline/input/preprocess.sh >> /data_pipeline/logs/preprocess.log 2>&1

	crontab -l

First line is get into the Cron Envirobment

Second sets up preprocess.sh in the input sub-directory to run at 12:00 midnight and send logs to preprocess.log in the input sub-directory.
Close the cron environment then list the cron jobs using the 3rd line.
 
## Logging and Monitoring
Change directory to logs

	cd logs/
Create monitor.sh 
	touch monitor.sh

Enter monitor.sh
	nano monitor.sh

	#!/bin/bash

	LOGDIR=/home/sligoemy/data_pipeline/logs/preprocess.log
	FIND=$(grep -i "Error\|Failed" "$LOGDIR")

	if [ -s $FIND ]; then
        	echo "Error Found in $LOGDIR"
	else
        	echo "No Error Found"
        	echo $FIND
	fi
	
Two variables were created, first was linking to preprocess.log file, second was searching for "Errors" or "Failed" irrespective of the case.
If errors are found, it prints "Errors found in Preprocess.log", and if not it prints "No Error Found"

you can now move to the cron environment. 
	crontab -e

In the cron environment, we would run the monitor.sh at 12:05am everyday  to check if preprocess.log contains an error

	5 0 * * * /data_pipeline/logs/monitor.sh

##Permissions and Security
To setup the input folder to be writable only to users

	chmod go -w input
	chmod go -r log
This would remove write#1.	 LINUX ASSIGNMENT

## Setting up the Environment
I connected to the virtual machine with the username
	ssd sligoemy@***.**.***.**

## Creating the directories
	mkdir data_pipeline/

	cd  data_pipeline/
	
	mkdir input/ output/ logs/

	
First line creates a directory called "Data_Pipeline"

Second line changes directory to the created pipeline

Third line creates three sub-directories... "input", "output", "logs"

## Data Ingestion and Preprocessing
I ingested the data using Secure Copy Protocol (SCP). This enabled me copy the file into my Virtual Environment

	 scp ~/Downloads/sales_data.csv sligoemy@***.**.***.***:~/data_pipeline/input/


As earlier mentioned, this enables copy the sales_data file into the input subdirectory in my VM. 

Next is to create the preprocess.sh file to clean and prepare the data files. But first I have to change the directory to input subdirectory

	cd datapipeline/input

	touch preprocess.sh

This creates the preprocess file. To write the script in preprocess.sh, I have to enter into it. 

	nano preprocess.sh

Removing the extra Column and Filtering out  the row that contains Status = failed

	#!/bin/bash	

	INPUT=~/data_pipeline/input/sales_data.csv
	OUTPUT=~/data_pipeline/output/cleaned_sales_data.csv
	LOGS=~/data_pipeline/logs/preprocess.log

	cut -d',' -f1-6 "$INPUT" |
	grep -v ",Failed$" > "$OUTPUT"
	echo "Preprocessing complete. Cleaned file saved to $OUTPUT" >> "$LOGS"

Make preprocess.sh executable

	chmod +x preprocess.sh

## Automate the Pipeline with Cron JObs
This is basically meant to run preprocess.sh at 12:00 midnight. 

	crontab -e

	0 0 * * * /data_pipeline/input/preprocess.sh >> /data_pipeline/logs/preprocess.log 2>&1

	crontab -l

First line is get into the Cron Envirobment

Second sets up preprocess.sh in the input sub-directory to run at 12:00 midnight and send logs to preprocess.log in the input sub-directory.
Close the cron environment then list the cron jobs using the 3rd line.
 
## Logging and Monitoring
Change directory to logs

	cd logs/
Create monitor.sh 
	touch monitor.sh

Enter monitor.sh
	nano monitor.sh

	#!/bin/bash

	LOGDIR=/home/sligoemy/data_pipeline/logs/preprocess.log
	FIND=$(grep -i "Error\|Failed" "$LOGDIR")

	if [ -s $FIND ]; then
        	echo "Error Found in $LOGDIR"
	else
        	echo "No Error Found"
        	echo $FIND
	fi
	
Two variables were created, first was linking to preprocess.log file, second was searching for "Errors" or "Failed" irrespective of the case.
If errors are found, it prints "Errors found in Preprocess.log", and if not it prints "No Error Found"

you can now move to the cron environment. 
	crontab -e

In the cron environment, we would run the monitor.sh at 12:05am everyday  to check if preprocess.log contains an error

	5 0 * * * /data_pipeline/logs/monitor.sh




#Permissions and Security
By default all access is given to everyone including users, groups and other. 

To remove write access from group and others on the input directory. 
	chmod go -w input

To remove read access from group and others in the logs directory.
	chmod go -r logs 


To confirm access.

	ls -la
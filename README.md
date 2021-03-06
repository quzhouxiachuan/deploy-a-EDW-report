# EDW on-boarding training: from taskmaster to deploy a report
## Task Master
#### 1. go to TaskMaster, on upper right, click on 'New Task' 
#### 2. fill out the form. The following is an example for this: 
 ```
  mail: nmedw@northwestern.edu
  summary: edw onboard training final project 
  type: cohort 
  assigned to: Yu Deng 
  classificiation: Research 
  organization: FSM (based on which organization you are at, select different types)
  IRB Number: STU00022017
```
#### 3. click on save 
#### 4. click on 'request approval from data stewards', click on 'create cohorts' 
#### 5. this brings you to a creating cohort page, fill out the information accordingly. Select source type SQL. You can leave the 'reports' and 'users' session blank. To fill out the source data session, your query may look like this: 
``` 
SELECT TOP 10 mrd_pt_id
FROM edw.[mrd].[patient_deidentified] p
```
#### 6. click on 'insert', a cohort will be created, you will get a cohort ID. In my case, my cohort ID is 2446. 

## Report Writer 
#### 1. stay on your taskmaster page, click on the top right 'Report Writer'. This step allows you to link the cohort id you just created to your reportwriter cohort id 
#### 2. When fill out the reportwriter, your query may look like this: 
```SQL
SELECT p.mrd_pt_id
 , cp.patient_study_id
 , p.race_nm
 , p.gender_nm
 FROM edw.[mrd].[patient_deidentified] p
  JOIN edw.[apps].[cohort_patients] cp
 ON cp.[mrd_pt_id] = p.[mrd_pt_id]
 AND cp.[is_dltd_flg] = 0
 AND cp.[cohort_id] = @cohort_id --2446, you dont have to put any specific cohort_id here, it will automatically be linked to the cohort of your taskmaster 
 JOIN edw.[apps].[cohorts] c
 ON c.[cohort_id] = cp.[cohort_id]
 AND c.[is_dltd_flg] = 0
```
#### 3. click on'create solution' to download your report. Unzip your solution. 
After unzip it, you will get a .sln file and a folder. In my case, I have edw_onboard_training_YD folder and edw_onboard_training_YD.sln. PLEASE BE SURE THAT the folder and file are under the same directory. In my case, both  edw_onboard_training_YD folder and edw_onboard_training_YD.sln are in directory: 'C:\Users\ydw529\Downloads'

## visual studio 
#### prior to running visual studio, PLEASE MAKE SURE your SSMS is connect to EDW (I am not sure if this step is required, but just to be on the safe side)
#### 1. create a batch script named 'ssrs'. Put the exactly same command on the script except changing user name to your own NM id: 
```
runas /netonly /user:NMH\NM_YOURNM_ID "C:\Program Files (x86)\Microsoft Visual Studio 9.0\Common7\IDE\devenv.exe"
```
#### 2. run your script (double click on ssrs), enter your nmh password when asked. This step helps connect visual studio to edw
#### 3. by running the script, visual studio will pop up itself(just like when we are trying to connect edw ssms). Click on upper left 'file', click on 'open', click on 'project/solution'. Choose your solution file to import, in my case, it is 'edw_onboard_training_YD.sln'. 
``` 
Do you want to upgrade this report server project to the lastest version? 
``` 
click on yes
 We are almost done!! Hanging there! 
#### 4. After successfully importing your file, click on 'Preview', enter your cohort identification number(IMPORTANT, OR ELSE YOU WILL SEE NOTHING). In my case, I put 2446. Click on 'view report'. 

#### 5. on the top left title row, click on 'Build', select 'deploy...(this should be your report name)'. 

## ReportDeployer
#### 1.go back to EDW website, go to ReportDeloyer, click on the link of the report you just deployed, this brings you to a different page. Click on 'submit for review'. Send the email. Please remember to fill out the blank in the automated email. 

To be continued... 





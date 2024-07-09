# Montefiore-Data-Assessment

The data assessment consisted of two tasks;

1. A SQL Knowledge Assessment:
  - Assess the candidate ability to perform basic joins, use of case statement, and understanding data and table relationships.
  - There was an excel file provided with the following tables:
      - DEPT: DEPT_NAME, DEPT_ID , FPG_DEPT_FLAG
      - PROVIDER: DOC_NAME, DOC_ID
      - PATIENT: PAT_ID , PATIENT, DOB (date of birth), CUR_DOC_ID
      - DOC DEPT: DOC_ID, LINE, DEPT_ID, FPG_DEPT_FLAG
      - VISIT: PAT_ID , VISIT_ID, DEPT_ID, VISIT_DOC_ID, VISIT_DATE
   
      - The conditons for the SQL Assessment are the following:
        1. Only Patients that had a visit
        2. Visit that happened at FPG dept (FPG_DEPT_FLAG = 1)
        3. Show both CUR_DOC name and VISIT_DOC Name
        4. Flag if the CUR_DOC is an FPG Provider (Use the [DOC_DEPT] table, the flag should look
        at Line = 1 FPG_DEPT_FLAG = 1)
        5. Create a Flag if the CUR_DOC is the same as VISIT_DOC
        6. Create Column Age at Visit (in years)
       
      - The output should contain the following columns:
        - PAT_ID, PATIENT, DOB, CUR_DOC (Name), VISIT DATE, VISIT_ID, VISIT DOC (Name), DEPT NAME, VISIT AT FPG (Flag, could be Y/N or 1/0), CUR_DOC_FPG (flag see #4, could be Y/N or 1/0), CUR DOC IS VIS DOC (flag see #5, could be Y/N or 1/0), Age at Visit

2. An Excel Test

The excel test provides an excel file with explicit instructions and the datasets to complete the tasks.

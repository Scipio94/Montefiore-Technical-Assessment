~~~ SQL
/*TEMP Table 1*/
CREATE TEMP TABLE t1 AS -- Creating TEMP Table
SELECT *,
  CASE -- CASE statement for Visit At FPG column
    WHEN FPG_DEPT_FLAG = 1 THEN 'Y'
    ELSE 'N' END AS VISIT_AT_FPG
FROM
(SELECT *
FROM `single-being-353600.Montefiore_Data_Assessment.Pat` AS p --
INNER JOIN `single-being-353600.Montefiore_Data_Assessment.Visit` AS v -- INNER JOIN patients with visits
USING (PAT_ID)) AS sub
JOIN `single-being-353600.Montefiore_Data_Assessment.DEPT` AS d -- JOIN to return department name
USING(DEPT_ID);

/*TEMP Table 2 */
CREATE TEMP TABLE t2 AS  -- Creating TEMP Table
SELECT -- Returning relevant columns
  DISTINCT PAT_ID,PATIENT,DOB,sub1.DOC_NAME AS CUR_DOC, 
  p1.DOC_ID,VISIT_DATE, VISIT_ID, p1.DOC_NAME AS VISIT_DOC, 
  DEPT_NAME, ROUND(EXTRACT(DAY FROM (VISIT_DATE- DOB))/365.25) AS Age_At_Visit,
  VISIT_AT_FPG, LINE,sub1.CUR_DOC_ID
FROM
(SELECT *
FROM
(SELECT *,
FROM t1
JOIN `single-being-353600.Montefiore_Data_Assessment.DOC_DEPT` AS d
USING(DEPT_ID)) AS sub -- creating subquery sub
JOIN `single-being-353600.Montefiore_Data_Assessment.Provider` AS p -- LEFT JOIN to return CUR_DOC
ON CAST(sub.CUR_DOC_ID AS string) = p.DOC_ID) AS sub1 -- creating subquery sub1
JOIN `single-being-353600.Montefiore_Data_Assessment.Provider` AS p1-- LEFT JOIN to return VISIT_DOC
ON CAST(sub1.VISIT_DOC_ID AS string) = p1.DOC_ID;

/*TEMP Table 3 - CUR_DOC IS FPG_Provider*/
CREATE TEMP TABLE t3 AS  -- Creating TEMP Table
SELECT *,
  CASE -- CASE statement to determine if CUR_DOC IS FPG_Provider
    WHEN LINE = 1 AND FPG_DEPT_FLAG = 1 THEN 'Y'
    ELSE 'N' END AS CUR_DOC_FPG_PROVIDER
FROM `single-being-353600.Montefiore_Data_Assessment.DOC_DEPT`
WHERE LINE = 1 AND FPG_DEPT_FLAG = 1 ;-- Filtering to return CUR_DOC IS FPG_Provider

SELECT -- Final file
  DISTINCT PAT_ID,PATIENT,DOB,CUR_DOC,VISIT_DATE,
  VISIT_ID,VISIT_DOC,DEPT_NAME,VISIT_AT_FPG,
  CASE
    WHEN CUR_DOC_FPG_PROVIDER IS NULL THEN 'N' -- CUR_DOC_FPG_PROVIDER CASE Statement
    ELSE CUR_DOC_FPG_PROVIDER END AS CUR_DOC_FPG_PROVIDER,
  CASE -- CASE statement for CUR_DOC_IS_VIS_DOC column 
    WHEN CUR_DOC = VISIT_DOC THEN 'Y'
    ELSE 'N' END AS CUR_DOC_IS_VIS_DOC,
  Age_At_Visit
FROM t2
LEFT JOIN t3 -- LEFT JOIN to return CUR_DOC IS FPG_Provider
ON t2.CUR_DOC_ID = t3.DOC_ID
ORDER BY t2.PAT_ID;


~~~

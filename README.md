# SQL užklausos duomenims paimti iš KTU IF Moodle duomenų bazės
Šios SQL užklausos buvo panaudotos kuriant ankstyvojo įspėjimo sistemą studijų programoje. Pateikiami tik pavyzdžiai, kurie **neatskleidžia jokios jautrios informacijos**.

```sql
-- Studijų modulio identifikacijos numerio paieška
SELECT * 
FROM `mdl_course` 
WHERE shortname LIKE '%VMP%';

-- Kiek kartų per savaitę jungtasi prie modulių KTU IF Moodle VMA
SELECT 
    L.userid, 
    U.firstname, 
    U.lastname, 
    WEEK(DATE_FORMAT(FROM_UNIXTIME(L.timecreated), '%Y-%m-%d')) AS week_number, 
    COUNT(*) AS access_count
FROM mdl_logstore_standard_log L
JOIN mdl_user U ON U.id = L.userid
WHERE L.courseid = '561' 
  AND YEAR(FROM_UNIXTIME(L.timecreated)) = 2021
GROUP BY L.userid, U.firstname, U.lastname, week_number;

-- Kiek paspaudimų atlikta moduliuose KTU IF Moodle VMA
SELECT 
    L.userid, 
    U.firstname, 
    U.lastname, 
    WEEK(DATE_FORMAT(FROM_UNIXTIME(L.timecreated), '%Y-%m-%d')) AS week_number, 
    COUNT(*) AS click_count
FROM mdl_logstore_standard_log L
JOIN mdl_user U ON U.id = L.userid
WHERE L.courseid = '561' 
  AND YEAR(FROM_UNIXTIME(L.timecreated)) = 2021
  AND L.action = 'viewed'
GROUP BY L.userid, U.firstname, U.lastname, week_number;

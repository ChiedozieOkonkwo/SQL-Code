/*

Task: List of Truenat site, number of MTB tests conducted, number of positive cases/MTB detected, Yield of positive cases from tests conducted, Erroros in the tests conducted, Error rates across the top 10 performances for the reporting period.

Dashboard: newtools
Tables: truenat and truenat_errors

*/


USE newtools;

SELECT 
    t.state,
    t.truenat_site,
    t.month,
    t.mtb_tests_conducted AS 'MTB Tests Conducted',
    t.mtb_rif_tests_conducted AS 'MTB Detected',
    CONCAT((t.mtb_rif_tests_conducted / t.mtb_tests_conducted) * 100,
            '%') AS 'MTB Yield',
    SUM(te.mtb_tests_errors + te.mtb_tests_invalids + te.mtb_tests_other_unsuccessful_results) AS 'MTB Test Errors',
    CONCAT((SUM(te.mtb_tests_errors + te.mtb_tests_invalids + te.mtb_tests_other_unsuccessful_results) / t.mtb_tests_conducted) * 100,
            '%') AS 'Error Rate'
FROM
    truenat, t
        INNER JOIN
    truenat_errors, te USING (truenat_site , month)
GROUP BY truenat_site , month
ORDER BY t.mtb_rif_tests_conducted DESC
LIMIT 10;

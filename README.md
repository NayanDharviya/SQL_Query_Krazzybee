
Bro, I am requesting you please consider my profile, I have done hardwork for it and ready to more. I am really sorry for this things.
I know its an not good way to talk, but I don't have any other option.

thanks for checking the query, hope to good result.

WITH
  cte1 AS (
  SELECT
    *,
    DENSE_RANK() OVER(ORDER BY EXTRACT(year FROM business_date)) prev_year
  FROM
    `Jio.krazzybee`
  ORDER BY
    EXTRACT(year
    FROM
      business_date) ),
  cte2 AS (
  SELECT
    DISTINCT EXTRACT(year
    FROM
      c1.business_date) year,
    c1.city_id c1_city,
    c1.prev_year c1_year,
    c2.city_id c2_city,
    c2.prev_year c2_year
  FROM
    cte1 c1
  JOIN
    cte1 c2
  ON
    (c2.prev_year = (c1.prev_year-1 )
      AND (c1.city_id = c2.city_id)) )
SELECT
  EXTRACT(year
  FROM
    business_date) year,
  COUNT(*)
FROM
  cte1
WHERE
  prev_year= (
  SELECT
    MIN(prev_year)
  FROM
    cte1)
GROUP BY
  EXTRACT(year
  FROM
    business_date)
UNION ALL
SELECT
  year,
  COUNT(*)
FROM
  cte2
GROUP BY
  year


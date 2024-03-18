# SQL_Query_Krazzybee

with cte1 as
(select *,
dense_rank() over(order by extract(year from business_date)) prev_year from `Jio.krazzybee`
order by extract(year from business_date)
),
cte2 as 
(
select
  distinct extract(year from c1.business_date) year, c1.city_id c1_city, c1.prev_year c1_year
  , c2.city_id c2_city, c2.prev_year c2_year
from cte1 c1
 join cte1 c2
on (c2.prev_year = (c1.prev_year-1 ) and (c1.city_id = c2.city_id)) 
)

select extract(year from business_date) year, count(*) from cte1 where prev_year= (select min(prev_year) from cte1) group by  extract(year from business_date)
union all
select year, count(*) from cte2 group by  year


-- select * from cte1

# Soru 1) 1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.
```sql
with medals as 
(
select country,count(medal) as total_medal,sport
from beyza_canbay.summer_medals 
where year>=1980
group by country,sport
order by total_medal desc
)

Select distinct sport,
nth_value(country,1 IGNORE NULLS) over(partition by sport order by total_medal desc ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as first_country,
nth_value(country,3 IGNORE NULLS) over(partition by sport order by total_medal desc ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as third_country,
nth_value(country,5 IGNORE NULLS) over(partition by sport order by total_medal desc ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as fifth_country,

from medals 
group by sport,Country,total_medal
```
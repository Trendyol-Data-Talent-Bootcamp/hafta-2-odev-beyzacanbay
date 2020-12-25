# Soru 2) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.
```sql
select 
  athlete,
from
(
  select
    a.athlete,
    a.year ayear,
    b.year byear,
    (b.year - a.year) yeardiff,
    dense_rank() over (partition by a.athlete order by (b.year - a.year) desc) rank
  from
    `beyza_canbay.summer_medals`  a
    join `beyza_canbay.summer_medals` b on a.athlete = b.athlete 
        and b.year > a.year
  where
    b.year - a.year = 
      (select 4*(count(*)-1)
         from `beyza_canbay.summer_medals` a1
        where a.athlete  = a1.athlete 
             and a1.year between a.year and b.year)
)
where
  rank = 1 and
  yeardiff >= 8
  
order by yeardiff desc;
```
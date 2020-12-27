# Soru 3) "sample.pageview": tablosunda 1 gün içerisinde trendyol.com a gelen tüm ziyaretlerin logu var. Bu çalışmada çıkarmak istediğimiz bilgi, günün her bir dakikası için aktif kullanıcı sayısının hesaplanması.
```sql
with visits as (
select timestamp_trunc(view_ts,minute) view_ts_slice,
  approx_count_distinct(deviceid) cnt_approx,
from beyza_canbay.pageview
group by 1
order by view_ts_slice  )

select view_ts_slice, sum(cnt_approx) OVER (ORDER BY view_ts_slice ROWS 5 PRECEDING) AS active_users from visits;
```
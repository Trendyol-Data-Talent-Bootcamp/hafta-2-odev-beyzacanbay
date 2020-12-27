# Soru 4)
İki tablonun merge işlemi için aşağıdaki sorgu yazılmıştır.
```sql
MERGE beyza_canbay.content_category as t
USING `beyza_canbay.content_category_20201222_00_59`  as s
ON t.id = s.id
WHEN MATCHED AND t.cdc_date!=s.cdc_date THEN
UPDATE SET t.cdc_date = s.cdc_date
WHEN NOT MATCHED BY SOURCE THEN 
UPDATE SET t.is_deleted = true
WHEN NOT MATCHED BY TARGET THEN
INSERT(cdc_date,is_deleted,id,category)
VALUES(s.cdc_date,s.is_deleted,s.id,s.category);
```
Merge işleminin test edilmesi için aşağıdaki sorgu çalıştırılabilir.

```sql
select * from 
(select *,farm_fingerprint(to_json_string(t1)) as _hash
from `beyza_canbay.content_category` t1 )  as table_1
full outer join 
(select *,farm_fingerprint(to_json_string(t2)) as _hash
from `beyza_canbay.content_category_target`  t2 ) as table_2 on table_1._hash=table_2._hash
where table_1._hash = table_2._hash and (table_1._hash is null or table_2._hash is null)
```
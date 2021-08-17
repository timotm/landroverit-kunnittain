```SQL
with get_counts as 
(select kunta_nimi, count(1) filter (where merkkiselvakielinen ilike '%land%rover%' or merkkiselvakielinen ilike '%range%rover') as lr_lkm, count(1) as auto_lkm from ajoneuvo a natural join kunta group by kunta_nimi) 
select *, round((1000.0 * lr_lkm) / auto_lkm, 2) as lr_promille from get_counts order by lr_promille desc;
```
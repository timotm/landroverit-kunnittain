```SQL
with get_counts as 
(select kunta_nimi, kunta_asukasluku, count(1) as auto_lkm, count(1) filter (where merkkiselvakielinen ilike '%land%rover%' or merkkiselvakielinen ilike '%range%rover') as lr_lkm from ajoneuvo a natural join kunta2 group by kunta_nimi, kunta_asukasluku) 
select *, round(auto_lkm::numeric / kunta_asukasluku::numeric , 2) as autoja_per_asukas, round((1000.0 * lr_lkm) / auto_lkm, 2) as lr_promille_autot, round((1000.0 * lr_lkm) / kunta_asukasluku, 2) as lr_promille_asukas from get_counts order by lr_promille_asukas desc;
```
See [https://timotm.github.io/landroverit-kunnittain/](https://timotm.github.io/landroverit-kunnittain/g)


```SQL
CREATE TABLE ajoneuvo (
    ajoneuvoluokka text,
    ensirekisterointipvm text,
    ajoneuvoryhma text,
    ajoneuvonkaytto text,
    variantti text,
    versio text,
    kayttoonottopvm text,
    vari text,
    ovienlukumaara text,
    korityyppi text,
    ohjaamotyyppi text,
    istumapaikkojenlkm text,
    omamassa text,
    teknsuursallkokmassa text,
    tieliiksuursallkokmassa text,
    ajonkokpituus text,
    ajonleveys text,
    ajonkorkeus text,
    kayttovoima text,
    iskutilavuus text,
    suurinnettoteho text,
    sylintereidenlkm text,
    ahdin text,
    sahkohybridi text,
    sahkohybridinluokka text,
    merkkiselvakielinen text,
    mallimerkinta text,
    vaihteisto text,
    vaihteidenlkm text,
    kaupallinennimi text,
    voimanvaljatehostamistapa text,
    tyyppihyvaksyntanro text,
    yksittaiskayttovoima text,
    kunta text,
    co2 text,
    matkamittarilukema text,
    valmistenumero2 text,
    jarnro text
);

CREATE TABLE kunta2 (
    kunta text primary key,
    kunta_nimi text,
    kunta_asukasluku integer
);

COPY ajoneuvo from './TieliikenneAvoinData_5_14_utf.csv' delimiter ';' csv header;

COPY kunta2 from './kuntadata.csv' delimiter ',' csv
```

```SQL
with get_counts as 
(select kunta_nimi, kunta_asukasluku, count(1) as auto_lkm, count(1) filter (where merkkiselvakielinen ilike '%land%rover%' or merkkiselvakielinen ilike '%range%rover') as lr_lkm from ajoneuvo a natural join kunta2 group by kunta_nimi, kunta_asukasluku) 
select *, round(auto_lkm::numeric / kunta_asukasluku::numeric , 2) as autoja_per_asukas, round((1000.0 * lr_lkm) / auto_lkm, 2) as lr_promille_autot, round((1000.0 * lr_lkm) / kunta_asukasluku, 2) as lr_promille_asukas from get_counts order by lr_promille_asukas desc;
```
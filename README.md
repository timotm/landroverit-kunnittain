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

 CREATE TABLE kayttovoima (
     kayttovoima text primary key, kayttovoima_nimi text
);

COPY ajoneuvo from './TieliikenneAvoinData_5_14_utf.csv' delimiter ';' csv header;

COPY kunta2 from './kuntadata.csv' delimiter ',' csv

COPY kayttovoima from stdin delimiter E'\t' csv;

01	Bensiini
02	Dieselöljy
03	Polttoöljy
04	Sähkö
05	Vety
06	Kaasu
07	Metanoli
10	Biodiesel
11	LPG
13	CNG
31	Moottoripetroli
32	Diesel/Puu
33	Bensiini/Puu
34	Bensiini + moottoripetroli
37	Etanoli
38	Bensiini/CNG
39	Bensiini/Sähkö
40	Bensiini/Etanoli
41	Bensiini/Metanoli
42	Bensiini/LPG
43	Diesel/CNG
44	Diesel/Sähkö
45	Diesel/Etanoli
46	Diesel/Metanoli
47	Diesel/LPG
48	Diesel/Biodiesel
49	Diesel/Biodiesel/Sähkö
50	Diesel/Biodiesel/Etanoli
51	Diesel/Biodiesel/Metanoli
52	Diesel/Biodiesel/LPG
53	Diesel/Biodiesel/CNG
54	Vety/Sähkö
55	Dieselöljy/Muu
56	H-ryhmän maakaasu
57	L-ryhmän maakaasu
58	HL-ryhmän maakaasu
59	CNG/Biometaani
60	Biometaani
61	Puu
62	Etanoli (ED95)
63	Etanoli (E85)
64	Vety-maakaasuseos
65	LNG
66	LNG20
67	Diesel/LNG
68	Diesel/LNG20
X	Ei sovellettavissa
Y	Muu
\.

CREATE TABLE sahkohybridinluokka (
    sahkohybridinluokka text primary key,
    sahkohybridinluokka_nimi text
);

COPY sahkohybridinluokka from stdin delimiter E'\t' csv;

01	Sähköverkosta ladattava
02	Pelkästään polttomoottorilla ladattava
03	Sähköverkosta ladattava polttokennohybridi
04	Pelkästään polttokennolla ladattava
\.
```

```SQL
with get_counts as
(select kunta_nimi, kunta_asukasluku, count(1) as auto_lkm, count(1) filter (where merkkiselvakielinen ilike '%land%rover%' or merkkiselvakielinen ilike '%range%rover') as lr_lkm from ajoneuvo a natural join kunta2 group by kunta_nimi, kunta_asukasluku)
select *, round(auto_lkm::numeric / kunta_asukasluku::numeric , 2) as autoja_per_asukas, round((1000.0 * lr_lkm) / auto_lkm, 2) as lr_promille_autot, round((1000.0 * lr_lkm) / kunta_asukasluku, 2) as lr_promille_asukas from get_counts order by lr_promille_asukas desc;
```

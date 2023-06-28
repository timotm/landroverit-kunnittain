See [https://timotm.github.io/landroverit-kunnittain/](https://timotm.github.io/landroverit-kunnittain/g)

Kuntanumerot [Tilastokeskuksen palvelusta](https://www2.tilastokeskus.fi/fi/luokitukset/kunta/kunta_1_20230101/export/)

Kuntien asukasluvut [Tilastokeskuksen palvelusta](https://statfin.stat.fi/PxWeb/pxweb/fi/StatFin/StatFin__vaerak/statfin_vaerak_pxt_11ra.px/table/tableViewLayout1/?loadedQueryId=45ba5f20-2525-44b7-96f0-0459758e1b5b&timeType=top&timeValue=1)

ISO-8859-1 -> UTF-8:
```shell
iconv -f ISO-8859-1 -t utf-8 < Ajoneuvojen_avoin_data_5_20.csv > Ajoneuvojen_avoin_data_5_20_utf.csv 

iconv -f ISO-8859-1 -t utf-8 < ~/kunta_1_20230101.csv  | tr -d \' > kunta_1_20230101_utf.csv

iconv -f ISO-8859-1 -t utf-8 < 001_11ra_2022_20230626-143820.csv >  001_11ra_2022_20230626-143820_utf.csv
```

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
    ovienLukumaara text,
    korityyppi text,
    ohjaamotyyppi text,
    istumapaikkojenLkm text,
    omamassa text,
    teknSuurSallKokmassa text,
    tieliikSuurSallKokmassa text,
    ajonKokPituus text,
    ajonLeveys text,
    ajonKorkeus text,
    kayttovoima text,
    iskutilavuus text,
    suurinNettoteho text,
    sylintereidenLkm text,
    ahdin text,
    sahkohybridi text,
    sahkohybridinluokka text,
    merkkiSelvakielinen text,
    mallimerkinta text,
    vaihteisto text,
    vaihteidenLkm text,
    kaupallinenNimi text,
    voimanvalJaTehostamistapa text,
    tyyppihyvaksyntanro text,
    yksittaisKayttovoima text,
    kunta text,
    NEDC_Co2 integer,
    NEDC2_Co2 integer,
    WLTP_Co2 integer,
    WLTP2_Co2 integer,
    matkamittarilukema text,
    valmistenumero2 text,
    jarnro text
);

COPY ajoneuvo from '/tmp/Ajoneuvojen_avoin_data_5_20_utf.csv' delimiter ';' csv header;

CREATE TABLE kunta (
    kunta text primary key,
    kunta_taso integer,
    kunta_nimi text,
    kunta_tyhja_kentta text
);

CREATE TABLE kunta_asukasluku (
    kunta_nimi text primary key,
    kunta_asukasluku_ei_kaytossa text,
    kunta_asukasluku integer
);

COPY kunta from '/tmp/kunta_1_20230101_utf.csv' delimiter ';' csv header

 copy kunta_asukasluku from '/tmp/001_11ra_2022_20230626-143820_utf.csv' delimiter ';' csv header;

 CREATE TABLE kayttovoima (
     kayttovoima text primary key, kayttovoima_nimi text
);

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

CREATE TABLE ajoneuvoluokka (
    ajoneuvoluokka text primary key,
    ajoneuvoluokka_nimi text not null
);

COPY ajoneuvoluokka from stdin delimiter E'\t' csv;
C1	Traktori
C2	Traktori
C3	Traktori
C4	Traktori
C5	Traktori
KNP	Kevyt nelipyörä
L1	Mopo
L1e	Mopo
L2	Mopo
L2e	Mopo
L3	Moottoripyörä
L3e	Moottoripyörä
L4	Moottoripyörä
L4e	Moottoripyörä
L5	Kolmi- tai nelipyörä
L5e	Kolmipyörä
L6e	Kevyt nelipyörä
L7e	Nelipyörä
LTR	Liikennetraktori
M1	Henkilöauto
M1G	Henkilöauto
M2	Linja-auto
M2G	Linja-auto
M3	Linja-auto
M3G	Linja-auto
MA	Maastoajoneuvo
MTK	Moottorityökone
MUU	Muu
N1	Pakettiauto
N1G	Pakettiauto
N2	Kuorma-auto
N2G	Kuorma-auto
N3	Kuorma-auto
N3G	Kuorma-auto
O1	Kevyt perävaunu
O2	Perävaunu
O3	Perävaunu
O4	Perävaunu
Ra1	Traktorin perävaunu
Ra2	Traktorin perävaunu
Ra3	Traktorin perävaunu
Ra4	Traktorin perävaunu
Rb1	Traktorin perävaunu
Rb2	Traktorin perävaunu
Rb3	Traktorin perävaunu
Rb4	Traktorin perävaunu
Sa1	Vedettävä kone
Sa2	Vedettävä kone
Sb1	Vedettävä kone
Sb2	Vedettävä kone
T	Traktori
T1	Traktori
T2	Traktori
T3	Traktori
T4	Traktori
T5	Traktori
\.

```

```SQL

CREATE TABLE vari (
    vari text primary key,
    vari_nimi text not null
);

COPY vari from stdin delimiter E'\t' csv;
0	Musta
1	Ruskea (beige)
2	Punainen
3	Oranssi
4	Keltainen
5	Vihreä
6	Sininen
7	Violetti
8	Harmaa
9	Valkoinen
X	Monivär.
Y	Hopea
Z	Turkoosi
\.

```

```SQL
with get_counts as
(
select
	kunta_nimi,
	kunta_asukasluku,
	count(1) as auto_lkm,
	count(1) filter (
        where (merkkiselvakielinen ilike '%land%rover%'
            or merkkiselvakielinen ilike '%range%rover%')
        and mallimerkinta not ilike '%jaguar%'
        and mallimerkinta not ilike '%i-pace%') as lr_lkm,
	count(1) filter (
        where (merkkiselvakielinen ilike '%land%rover%'
            or merkkiselvakielinen ilike '%range%rover%')
        and mallimerkinta ilike '%discovery%') as discovery_lkm,
	count(1) filter (
        where (merkkiselvakielinen ilike '%land%rover%'
            or merkkiselvakielinen ilike '%range%rover%')
        and mallimerkinta ilike '%evo%ue%') as evoque_lkm,
	count(1) filter (
        where (merkkiselvakielinen ilike '%land%rover%'
            or merkkiselvakielinen ilike '%range%rover%')
        and mallimerkinta ilike '%rover sport%') as rrs_lkm,
	count(1) filter (
        where (merkkiselvakielinen ilike '%land%rover%'
            or merkkiselvakielinen ilike '%range%rover%')
        and mallimerkinta ilike '%freelander%') as freelander_lkm,
	count(1) filter (
        where (merkkiselvakielinen ilike '%land%rover%'
            or merkkiselvakielinen ilike '%range%rover%')
        and (mallimerkinta ilike '%defender%'
            or mallimerkinta ilike '%serie%'
            or mallimerkinta ilike '%110%')
        and kayttoonottopvm >= '20191101') as defender_2020_lkm,
	count(1) filter (
        where (merkkiselvakielinen ilike '%land%rover%'
            or merkkiselvakielinen ilike '%range%rover%')
        and (mallimerkinta ilike '%defender%'
            or mallimerkinta ilike '%serie%'
            or mallimerkinta ilike '%110%')
        and kayttoonottopvm < '20191101') as defender_lkm,
	count(1) filter (
        where (merkkiselvakielinen ilike '%land%rover%'
            or merkkiselvakielinen ilike '%range%rover%')
        and mallimerkinta ilike '%velar%') as velar_lkm,
	count(1) filter (
        where (merkkiselvakielinen ilike '%land%rover%'
            or merkkiselvakielinen ilike '%range%rover%')
        and mallimerkinta ilike '%fire%en%ne%') as fe_lkm
from
	ajoneuvo a
join kunta on
	kunta.kunta = a.kunta
natural join kunta_asukasluku ka
group by
	kunta_nimi,
	kunta_asukasluku)
select
	kunta_nimi,
	kunta_asukasluku,
	auto_lkm,
	lr_lkm,
	round(auto_lkm::numeric / kunta_asukasluku::numeric, 2) as autoja_per_asukas,
	round((1000.0 * lr_lkm) / auto_lkm,	2) as lr_promille_autot,
	round((1000.0 * lr_lkm) / kunta_asukasluku,
	2) as lr_promille_asukas,
	discovery_lkm,
	freelander_lkm,
	rrs_lkm,
	lr_lkm - discovery_lkm - evoque_lkm - rrs_lkm - freelander_lkm - defender_2020_lkm - defender_lkm - velar_lkm - fe_lkm as rr_lkm,
	evoque_lkm,
	defender_lkm,
	velar_lkm,
	defender_2020_lkm,
	fe_lkm
from
	get_counts
order by
	lr_promille_autot desc;
```


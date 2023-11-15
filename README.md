# Projeto-Augusto-EBAC

Esse projeto fala um pouco a respeito dos marombas dos últimos anos do campeonato classi men´s physic, segue um rank abaixo de toda a tabela.

Pegamos as infos do Kaggle, e dei uma tratada na base, usando esses comandos para criar a tabela a seguir:

CREATE EXTERNAL TABLE IF NOT EXISTS `default`.`mr_olympia` (
  `rank` int,
  `ano` bigint,
  `premio` bigint,
  `local` string,
  `pais_do_campeao` string,
  `campeao` string,
  `pais_do_segundo_colocado` string,
  `segundo_colocado` string,
  `pais_do_terceiro_colocado` string,
  `terceiro_colocado` string
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES ('field.delim' = ',')
STORED AS INPUTFORMAT 'org.apache.hadoop.mapred.TextInputFormat' OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION 's3://segunda-tentativa/'
TBLPROPERTIES ('classification' = 'csv');


Usando o comando :
SELECT * FROM "default"."mr_olympia" limit 10;

#	rank	ano	premio	local	pais_do_campeao	campeao	pais_do_segundo_colocado	segundo_colocado	pais_do_terceiro_colocado	terceiro_colocado
1	0		12500	Las Vegas United States	United States	Danny Hester	Iran	Arash Rahbar	Bosnia and Herzegovina	Sadik Hadzovic
2	1	2017	20000	Las Vegas United States	United States	Breon Ansley	Canada	Chris Bumstead	United States	George Peterson
3	2	2018	20000	Las Vegas United States	United States	Breon Ansley	Canada	Chris Bumstead	United States	George Peterson
4	3	2019	30000	Las Vegas United States	Canada	Chris Bumstead	United States	Breon Ansley	United States	George Peterson
5	4	2020	30000	Orlando United States	Canada	Chris Bumstead	United States	Terrence Ruffin	United States	Breon Ansley
6	5	2021	50000	Orlando United States	Canada	Chris Bumstead	United States	Terrence Ruffin	United States	Breon Ansley
7	6	2022	50000	Las Vegas United States	Canada	Chris Bumstead	Brazil	Ramon Queiroz	Germany	Urs Kalecinski
8	7	2023	50000	Las Vegas United States	Canada	Chris Bumstead	Brazil	Ramon Queiroz	Germany	Urs Kalecinski

Para sabermos quem foram os três últimos campeões dos últimos anos da categoria mais disputada do mr olympia, usamos a consulta:

SELECT DISTINCT campeao FROM mr_olympia

#	campeao
1	Danny Hester
2	Breon Ansley
3	Chris Bumstead

E também para sabermos quem ficaram em segundo nessa competição, aqui lembramos também que ficar em segundo nem sempre é uma derrota, principalmente por que tem brasileiro na disputa
usamos a consulta:

SELECT DISTINCT pais_do_segundo_colocado, segundo_colocado FROM mr_olympia


#	segundo_colocado
1	United States	Terrence Ruffin
2	Brazil	Ramon Queiroz
3	Iran	Arash Rahbar
4	Canada	Chris Bumstead
5	United States	Breon Ansley

Temos também como saber qual dos anos pagou melhor pelo esforço de nossos atletas, afinal um ano só fazendo dieta e treinando pesado, é merecido uma quantia em dinheiro boa
Usamos a consulta:

select distinct premio,ano from mr_olympia
where premio >100
order by premio, ano asc

#	premio	ano
1	12500	2016
2	20000	2017
3	20000	2018
4	30000	2019
5	30000	2020
6	50000	2021
7	50000	2022
8	50000	2023

Segue um rank também de como ficaria, o campeão do mr olympia, o segundo colocado e o terceiro colocado
Usamos a consulta:

select distinct ano, campeao, segundo_colocado, terceiro_colocado from mr_olympia
order by ano  asc

#	ano	campeao	segundo_colocado	terceiro_colocado
1	2017	Breon Ansley	Chris Bumstead	George Peterson
2	2018	Breon Ansley	Chris Bumstead	George Peterson
3	2019	Chris Bumstead	Breon Ansley	George Peterson
4	2020	Chris Bumstead	Terrence Ruffin	Breon Ansley
5	2021	Chris Bumstead	Terrence Ruffin	Breon Ansley
6	2022	Chris Bumstead	Ramon Queiroz	Urs Kalecinski
7	2023	Chris Bumstead	Ramon Queiroz	Urs Kalecinski

O local que foram disputadas a competição, foi apenas nos EUA, em duas cidades diferentes, a cidade dos parques e a cidade das apostas:
Usamos a consulta:

select distinct local from mr_olympia

#	local
1	Las Vegas United States
2	Orlando United States

Segue também um ranking de países participantes:

Usamos a consulta:

select distinct pais_do_campeao, pais_do_segundo_colocado, pais_do_terceiro_colocado from mr_olympia

#	pais_do_campeao	pais_do_segundo_colocado	pais_do_terceiro_colocado
1	Canada	 Brazil 	Germany
2	United States	Iran	Bosnia and Herzegovina
3	United States	Canada	United States
4	Canada	United States	United States






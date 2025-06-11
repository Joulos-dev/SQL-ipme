
# 1/ Afficher le nom des pokémons n’étant pas la forme par défaut (colonne is_default)

```sql
SELECT *
FROM pokemon
WHERE is_default = 0;
```


# 2/ Afficher la moyenne de chaque statistique pour tous les pokemons (hp, atk, def, spa, spd et spe)

```sql

SELECT	AVG(hp) AS 'avg hp',
	AVG(atk) AS 'avg atk',
	AVG(def) AS 'avg def',
	AVG(spa) AS 'avg spa',
	AVG(spd) AS 'avg spd',
	AVG(spe) AS 'avg spe'
FROM pokemon;
```


# 3/ Afficher le pokémon le plus lourd

```sql
SELECT *
FROM pokemon
ORDER BY weight DESC
LIMIT 1;
```


# 4/ Afficher le pokémon le plus petit

```sql
SELECT *
FROM pokemon
ORDER BY height DESC
LIMIT 1;
```


# 5/ Afficher le pokémon le plus lent

```sql
SELECT *
FROM pokemon
ORDER BY spe
LIMIT 1;
```

Exemple avec une sous-requête :

On récupère d'abord la vitesse minimum parmis tous les Pokémons :

```sql
SELECT MIN(spe) FROM pokemon;
```

On l'intègre dans une requête récupérant les informations que l'on souhaite et réutilisant cette donnée :

```sql
SELECT *
FROM pokemon
WHERE spe = (SELECT MIN(spe) FROM pokemon)
ORDER BY name;
```


# 6/ Afficher les pokémons par atk, dont l’atk est supérieur à 150

```sql
SELECT name, atk
FROM pokemon
WHERE atk >= 150;
```


# 7/ Afficher la somme des statistiques avec le nom du pokémon lié

```sql
SELECT	name,
	(atk + def + spa + spd + spe + hp) AS 'stats'
		
FROM pokemon;
```


# 8/ Afficher les pokémons dont le « slug » est égal au « name_api »

```sql
SELECT *
FROM pokemon
WHERE slug = name_api;
```

# 9/ Afficher les pokémons dont l’atk est supérieure à la moyenne d’atk de tous les pokémons

```sql
-- Récupère la moyenne d'atk parmis tous les pokémons
SELECT AVG(atk) FROM pokemon; 

SELECT *
FROM pokemon
WHERE atk >= (SELECT AVG(atk) FROM pokemon);
```


# 10/ Afficher les pokémons dont le name contient le mot « Mew »

```sql
SELECT *
FROM pokemon
WHERE name LIKE '%Mew%';
```


# 11/ Afficher les pokémons dont le name est inférieure à 3 lettres

```sql
SELECT *
FROM pokemon
WHERE LENGTH(name) <= 3;
```


# 12/ Afficher le pokémon dont le name est le plus long

```sql
SELECT *
FROM pokemon
ORDER BY LENGTH(name) DESC
LIMIT 1;
```


# 13/ Afficher le pokémon ayant la somme de statistique (hp, atk, def, spa, spd, spe) la plus élevée

```sql
SELECT	name,
	(atk + def + spa + spd + spe + hp) AS stats
FROM pokemon
ORDER BY stats DESC
LIMIT 1;
```


# 14/ Afficher le pokémon le plus facile à monter de niveau (base_experience la plus basse)

```sql
SELECT	name,
	base_experience
FROM pokemon
ORDER BY base_experience
LIMIT 1;
```

# 15/ Afficher le pokémon le rapport « efficace » pour chaque pokémon
# - Rapport efficace : (somme des statistiques / base_experience) * 100
# (arrondie à 2 après la virgule et avec un % à la fin)

```sql
SELECT	*,
  	CONCAT(ROUND((base_experience / (atk + def + spa + spd + spe + hp)) * 100, 2), '%')

FROM pokemon
ORDER BY ROUND((base_experience / (atk + def + spa + spd + spe + hp)) * 100, 2) DESC;
```
		
		
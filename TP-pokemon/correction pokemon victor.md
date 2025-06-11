### 1. Afficher le nom des pokémons n’étant pas la forme par défaut (colonne is_default)

SELECT * FROM `pokemon` WHERE NOT is_default;

### 2. Afficher la moyenne de chaque statistique pour tous les pokemons (hp, atk, def, spa, spd et spe)

SELECT
    AVG(hp) 'Moyenne HP',
    AVG(atk) 'Moyenne attaque',
    AVG(def) 'Moyenne défense',
    AVG(spa) 'Moyenne attaque spéciale',
    AVG(spd) 'Moyenne défense spéciale',
    AVG(spe) 'Moyenne vitesse'
FROM `pokemon`;

### 3. Afficher le pokémon le plus lourd

SELECT name, weight FROM `pokemon`
ORDER BY weight DESC
LIMIT 1;

### 4. Afficher le pokémon le plus petit

SELECT name, height FROM `pokemon`
ORDER BY height
LIMIT 1;

### 5. Afficher le pokémon le plus lent

SELECT name, spe FROM `pokemon`
ORDER BY spe
LIMIT 1;

### 6. Afficher les pokémons par atk, dont l’atk est supérieur à 150

SELECT name FROM `pokemon` WHERE atk > 150;

### 7. Afficher la somme des statistiques avec le nom du pokémon lié

SELECT
	name,
	(hp + atk + def + spa + spd + spe) 'Somme stats'
FROM `pokemon`
WHERE name LIKE 'Caratroc';

### 8. Afficher les pokémons dont le « slug » est égal au « name_api »

SELECT name FROM `pokemon` WHERE name_api = slug;

### 9. Afficher les pokémons dont l’atk est supérieure à la moyenne d’atk de tous les pokémons

???

### 10. Afficher les pokémons dont le name contient le mot « Mew »

SELECT name FROM `pokemon` WHERE name LIKE "%mew%;

### 11. Afficher les pokémons dont le name est inférieure à 3 lettres

SELECT name FROM pokemon WHERE LENGTH(name) < 4;

### 12. Afficher le pokémon dont le name est le plus long

SELECT name FROM pokemon ORDER BY LENGTH(name) DESC LIMIT 1;

### 13. Afficher le pokémon ayant la somme de statistique (hp, atk, def, spa, spd, spe) la plus élevée

SELECT
	name,
    (hp + atk + def + spa + spd + spe) "base_stats"
    FROM pokemon
    ORDER BY "base_stats" DESC LIMIT 1;

### 14. Afficher le pokémon le plus facile à monter de niveau (base_experience la plus basse)

SELECT name FROM pokemon ORDER BY base_experience LIMIT 1;

### 15. Afficher le pokémon ayant le rapport « efficace » pour chaque pokémon
#### Rapport efficace : (somme des statistiques / base_experience) * 100, arrondie à 2 après la virgule et avec un % à la fin)

SELECT
	name,
    (hp + atk + def + spa + spd + spe) "base_stats",
    ROUND((hp + atk + def + spa + spd + spe) / base_experience * 100, 2) "rapport_efficace"
FROM pokemon;

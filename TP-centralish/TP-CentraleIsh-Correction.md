Explication de la base :

-   address : une adresse, si un "user_uuid" est renseigné, alors cette adresse est celle d'un utilisateur (en plus de potentiellement être celle d'une vente)
-   brand : les marques de voiture
-   favorite : les annonces mises en favoris par les utilisateurs
-   fuel : les types de carburant
-   image : les images des annonces
-   listing : les annonces
-   model : les modèles de voitures
-   user : les utilisateurs

# 1/ Afficher les informations des utilisateurs et, s'ils en possèdent une, leur adresse

```sql
SELECT
    birth_at,
    created_at,
    email,
    first_name,
    last_name,
    city,
    street_name,
    street_number,
    zip_code

FROM user
LEFT JOIN address
ON user.uuid = address.user_uuid;

```

# 2/ Afficher les 20 dernières annonces, on affichera seulement les colonnes suivantes : title, price et mileage. Price sera concaténé à "€" et mileage à "km" (PS : les prices sont en centimes, il faut les diviser par 100)

```sql

SELECT
	title,
	CONCAT((price/100), "€"),
    CONCAT(mileage, "km"),
    created_at

FROM listing

ORDER BY created_at DESC LIMIT 20;


```

# 3/ Afficher le nombre d'annonces par marque de voiture

```sql

SELECT  brand.name AS 'marque',
        COUNT(*)

FROM brand

JOIN model ON model.brand_id = brand.id
JOIN listing ON listing.model_id = model.id

GROUP BY brand.name;

```

# 4/ Afficher les annonces de voitures de la marque "Renault" ayant au moins 50000km

```sql

SELECT
		title AS 'Annonce',
		CONCAT(mileage, ' km') AS 'kilometrage'


FROM listing

JOIN model ON listing.model_id = model.id

WHERE model.brand_id = 10
AND listing.mileage >= 50000;


```

# 5/ Afficher les annonces de voitures de la marque "Ford" ayant au moins 30000km et un prix maxmimum de 12000€ (Attention aux centimes...)

```sql

SELECT
		title AS 'Annonce',
		CONCAT(mileage, ' km') AS 'kilometrage',
        CONCAT(ROUND(listing.price/100, 2), '€') AS 'price'


FROM listing

JOIN model ON listing.model_id = model.id

WHERE model.brand_id = 5
AND listing.mileage >= 30000
AND listing.price/100 <= 12000;



```

# 6/ Afficher les emails des utilisateurs ayant fait au moins une annonce dans les 6 derniers mois

```sql

SELECT
	user.email,
	listing.created_at AS ' date de parution de lannonce'

from user
JOIN listing ON user.uuid = listing.owner_uuid

WHERE DATE_SUB(NOW(), INTERVAL 6 MONTH) < listing.created_at;

```

# 7/ Afficher le nombre d'annonces déposées par années (Exemple : "2025 → 10", "2024 → 59", etc...)

```sql

SELECT
	YEAR(created_at) ,
    COUNT(*)
FROM listing
GROUP BY YEAR(created_at);

```

# 8/ Afficher les 5 modèles de voitures les plus mis en vente

```sql

SELECT
	model.name AS 'nom du modele',
    COUNT(*) AS 'nombre vendu'

FROM listing

JOIN model ON listing.model_id = model.id

GROUP BY listing.model_id
ORDER BY COUNT(*) DESC LIMIT 5;

```

# 9/ Afficher les annonces pour les voitures électriques

```sql




```

# 10/ Afficher les modèles de voiture n'ayant pas de modèle électrique en annonce

```sql
SELECT model.name, model.id
FROM model
WHERE model.id NOT IN (
	SELECT DISTINCT model_id
	FROM `listing`
	WHERE `fuel_id` = 2
);


```

# 11/ Afficher les 20 dernières annonces, mais de la page 3 (on considère que l'on a 20 annonces par page)

```sql

SELECT *
FROM listing
ORDER BY title
LIMIT 20 OFFSET 40;

```

# 12/ Afficher le nombre d'adresses enregistrées par ville, les trier par ordre décroissant

```sql

SELECT
	address.city AS ' Ville ' ,
	COUNT(*) AS 'nombre dadresse'
FROM address
GROUP BY address.city
ORDER BY COUNT(*) DESC;

```

# 13/ Afficher les adresses, dont le "street_name" est présent plus d'une fois, et trier par ordre décroissant

```sql
SELECT
	street_name AS `Nom d'une rue `,
	COUNT(*) AS `Nombre de nom d'une rue`
FROM address
GROUP BY street_name
HAVING COUNT(*) > 1
ORDER BY `Nombre de nom d'une rue` ASC
```

# 14/ Afficher les user n'ayant pas défini d'adresses (leur "uuid" n'est pas présent dans la table "address" )

```sql

SELECT

	user.first_name,
    user.last_name

FROM user
LEFT JOIN address ON user.uuid = address.user_uuid
WHERE address.user_uuid IS NULL;

```

# 15/ Afficher les annonces n'ayant aucune image (si il y en a ?)

```sql
SELECT
		*
FROM listing
LEFT JOIN image ON listing.uuid = image.listing_uuid
WHERE image.listing_uuid IS NULL;



```

# 16/ Afficher les annonces ayant plusieurs images

```sql

SELECT
		listing.title,
		COUNT(*) AS `nombre d'images`
FROM listing
LEFT JOIN image ON listing.uuid = image.listing_uuid
GROUP BY listing.uuid
HAVING COUNT(*) > 1
ORDER BY COUNT(*) DESC ;


```

# 17/ Afficher par marque, leur somme totale mise en vente, les trier par ordre décroissant

```sql
SELECT  brand.name AS 'marque',
        SUM(listing.price) AS total

FROM brand

JOIN model ON model.brand_id = brand.id
JOIN listing ON listing.model_id = model.id

GROUP BY brand.name

ORDER BY total DESC;
```

# 18/ Afficher le nombre d'annonce déposée par mois, depuis le début de l'année 2025

```sql
SELECT MONTH(listing.created_at), COUNT(*)
FROM listing
WHERE listing.created_at LIKE '2025%'
GROUP BY MONTH(listing.created_at)
ORDER BY COUNT(*) DESC;
```

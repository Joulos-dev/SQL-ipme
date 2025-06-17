Explication de la base :

-   address : les adresses des utilisateurs
-   category : les categories des produits vendus
-   characteristic : les caractéristiques possible des produits, comme la "Matière", "La taille", "Le poids", etc
-   comment : les commentaires des produits
-   order : les commandes des utilisateurs
-   product : les produits en ventes
-   product_categories : les catégories liés aux produits
-   product_characteristic : les caractéristiques des produits
-   product_order : les produits contenu dans une commande
-   user : les utilisateurs

(PS : le price est en centimes, là encore il faut le diviser par 100 pour l'avoir en €, il faudra le faire à chaque fois que le prix sera demandé)

Récapitulatif des "status" de commande :

-   100 : à payer
-   110 : en cours de préparation
-   120 : livraison lancée
-   130 : en cours de livraison
-   140 : livrée

### 1/ Affichez les 9 produits les plus vendus, avec seulement les colonnes "product.id", "product.label", "product.price", "product.reference"

```sql

SELECT
	product.id,
    product.reference,
    product.label,
    product.price,
    SUM(quantity) AS 'quantité_total'

FROM product
JOIN product_order ON product_order.product_id = product.id

GROUP BY product_order.product_id
ORDER BY `quantité_total` DESC LIMIT 9;

```

### 2/ Affichez les 4 catégories les plus vendus, avec seulement les colonnes "category.label", "category.id"

```sql

SELECT
	c1.label,
    c1.id,
    SUM(product_order.quantity) AS 'quantité_total'

FROM category c1 -- category AS c1

JOIN product_categories ON product_categories.categories_id = c1.id
JOIN product ON product.id = product_categories.products_id
JOIN product_order ON product_order.product_id = product.id

GROUP BY product_categories.categories_id
ORDER BY `quantité_total` DESC LIMIT 4;

```

### 3/ Affichez les 9 produits vendus dernièrement, avec seulement les colonnes "product.id", "product.label", "product.price", "product.reference"

```sql

SELECT
	product.id,
    product.label,
    product.price,
    product.reference,
    `order`.`created_at`

FROM product

JOIN product_order ON product_order.product_id = product.id
JOIN `order` ON `order`.id = product_order.order_id

ORDER BY `order`.`created_at` DESC LIMIT 9;


```

### 4/ Affichez les 4 derniers commentaires postés, avec seulement les colonnes : "product.id", "user.id", "user.username", "product.label", "review.rating", "review.created_at"

```sql

SELECT
	product.id,
    user.id,
    user.username,
    product.label,
    comment.rating,
    comment.created_at

FROM product

JOIN comment ON comment.product_id = product.id
JOIN user ON user.id = comment.user_id

ORDER BY comment.created_at DESC LIMIT 4;



```

### 5/ Affichez les informations de l'utilisateur avec ses adresses, avec seulement les colonnes : "user.id", "user.username", "user.first_name", "user.last_name", "address.\*"

```sql

SELECT
	user.id,
    user.username,
    user.first_name,
    user.last_name,
    address.*

FROM user

JOIN address ON address.user_id = user.id;



```

### 6/ Affichez le ou les utilisateurs ayant renseigné le plus d'adresses

```sql

SELECT
	user.id,
    user.username,
    user.first_name,
    user.last_name,
    COUNT(*)


FROM user

JOIN address ON address.user_id = user.id

GROUP BY address.user_id
ORDER BY COUNT(*) DESC;


```

### 7/ Afficher les produits de la catégorie "Books" et de ses sous-catégories

```sql


SELECT DISTINCT product_categories.products_id
FROM product_categories, category parent
LEFT JOIN category child ON child.parent_id = parent.id
WHERE parent.label = 'Books'
AND (
    product_categories.categories_id = parent.id
    OR
    product_categories.categories_id = child.id
);
```

### 8/ Affichez les produits de la catégorie "Books" et de ses sous-catégories, mais cette fois, ceux de la page 4, on affichera 12 produits par page. De plus, afficher le total de produits à afficher, par exemple pour faire un affichage comme celui-là : "48 sur 168"

```sql

SELECT     product.id,
        product.label,
        c.label,
                   (
                       SELECT COUNT(DISTINCT product_categories.products_id)
                       FROM product_categories, category parent
                       LEFT JOIN category child ON child.parent_id = parent.id
                       WHERE parent.label = 'Books'
                       AND (
                           product_categories.categories_id = parent.id
                           OR
                           product_categories.categories_id = child.id
                       )
                   ) AS "nbElements"

FROM product_categories

JOIN category c ON c.id = categories_id
JOIN product ON product_categories.products_id = product.id

WHERE c.parent_id = (
    SELECT id
    FROM category c2
    WHERE c2.label = 'Books'
)
OR c.id = (
    SELECT id
    FROM category c2
    WHERE c2.label = 'Books'
)

LIMIT 12  OFFSET 35;


```

### 9/ Affichez un récapitulatif des commandes de l'utilisateur, par exemple pour celui d'id "5". On affichera les colonnes suivantes : "order.address" (toutes les infos de celle-ci), "order.created_at", "order.status", "prix total de la commande" (à vous de trouver comment l'avoir... sachant que la colonne promotion est une réduction du prix de la commande en %)

```sql
SELECT
	address.*,
	o.created_at,
	o.status,
    SUM(product.price * product_order.quantity) * (1 - o.promotion/100) AS 'prix de la commande'


FROM `order` o

JOIN address ON o.address_id = address.id
JOIN product_order ON o.id = product_order.order_id
JOIN product ON product_order.order_id = product.id

GROUP BY o.id;
```

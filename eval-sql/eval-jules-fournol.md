// 1 //

```sql

SELECT
	album.name,
	artist.name,
    YEAR(album.published_at)

FROM album
JOIN artist ON album.artist_id = artist.id

ORDER BY album.published_at DESC LIMIT 9;

```

// 2 //

```sql

SELECT
	account.name,
    SUM(playlist.account_id) AS 'nb de playlist'

FROM playlist

JOIN account ON playlist.account_id = account.id

GROUP BY playlist.account_id
ORDER BY SUM(playlist.account_id) DESC;

```

// 3 //

```sql

SELECT
	album.name AS 'nom album',
    COUNT(song.name) AS 'nb de chansons',
    SUM(duration) AS 'duree total'

FROM album

JOIN album_song ON album.id = album_song.album_id
JOIN song ON album_song.song_id = song.id


GROUP BY album.name

```

// 4 //

```sql

SELECT
	playlist.name AS 'nom playlist',
    COUNT(song.name) AS 'nb de chansons',
    SUM(song.duration) AS 'duree total'

FROM playlist
JOIN playlist_song ON playlist.id = playlist_song.playlist_id
JOIN song ON playlist_song.song_id = song.id

GROUP BY playlist.name

```

// 5 //

```sql

SELECT
	artist.name,
    YEAR(artist.created_at) AS 'Année de début'

FROM artist

ORDER BY YEAR(artist.created_at) LIMIT 1;

```

// 6 //

```sql

SELECT
	ROUND(YEAR(NOW()) - AVG(YEAR(account.birth_date )),2) AS 'moyenne dage des utilisateurs'

 FROM account;



```

// 7 //

```sql

SELECT
    COUNT(account.newsletter) AS 'Abonnée a la newsletter'

FROM account
WHERE account.newsletter = 1;


```

// 8 //

```sql

SELECT
    gender,
    COUNT(*) AS 'nombre total'

FROM account

GROUP BY gender

```

// 9 //

```sql

SELECT
	subscription.name,
    COUNT(account_subscription.subscription_id) AS 'nb de subscription en 2024'

FROM subscription

JOIN account_subscription ON subscription.id = account_subscription.subscription_id

WHERE YEAR(account_subscription.effective_at) LIKE '2024%'
OR YEAR(account_subscription.finished_at) LIKE '2024%'
GROUP BY subscription.name

```

// 10 //

```sql

SELECT
	account.name

FROM account
JOIN playlist ON playlist.account_id = account.id

WHERE account.name NOT IN (SELECT playlist.account_id FROM playlist)
GROUP BY account.name

```

// 11 //

```sql

SELECT
	playlist.name AS 'nom de la playlist',
    account.name AS 'propriétaire de la playlist',
    SUM(account_like_playlist.playlist_id) AS 'nb de likes par playlist'

FROM playlist

JOIN account ON playlist.account_id = account.id
JOIN account_like_playlist ON account.id = account_like_playlist.account_id

GROUP BY playlist.name;

```

// 12 //

```sql

SELECT

    CONCAT( MAX(song.duration) - MIN(song.duration) ) AS 'écart de durée'

FROM song

```

// 13 //

```sql

SELECT
	DISTINCT COUNT(song.name) AS ' nombre de chansons sortie par années',
	YEAR(album.published_at) "année"

FROM album

JOIN album_song ON album.id = album_song.album_id
JOIN song ON album_song.song_id = song.id

GROUP BY YEAR(album.published_at) DESC LIMIT 5;

```

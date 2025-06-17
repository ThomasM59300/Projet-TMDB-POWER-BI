# Requetes SQL – Projet TMDb Power BI

Ce fichier ne répertorie que quelques requêtes qui ont été effectuées pour récupérer des données déjà transformées (via la requête SQL) et prêtes à l'emploi, pour analyse. Certaine requêtes ont également été effectué pour des debug ou des vérifications de résultat (voir si le résultat de visuels / mesures concordaient dans Power BI et SQL).

---

## Récupération des réalisateurs les plus populaires. 

```SQL
SELECT nom, realisateur_id, moyenne_note 
FROM realisateurs AS r
JOIN (
	SELECT realisateur_id, AVG(note_moyenne) AS moyenne_note
	FROM films AS f
	JOIN films_realisateurs AS fr ON f.id = fr.film_id 
	GROUP BY realisateur_id
	HAVING COUNT(*) > 12) AS f
ON f.realisateur_id = r.id 
ORDER BY moyenne_note DESC
LIMIT 30;
```

Explication : 


## Vérification & Debug

```SQL
SELECT (SUM(revenu) - SUM(budget)) AS test
FROM films_genres AS fg
JOIN films AS f
ON f.id = fg.film_id 
WHERE fg.genre_id = 10770 AND budget > 0 AND revenu > 0
```

```SQL
SELECT SUM(budget) AS test
FROM films_genres AS fg
JOIN films AS f
ON f.id = fg.film_id 
WHERE fg.genre_id = 37 AND budget > 0 AND revenu > 0

UNION ALL 

SELECT SUM(revenu) AS test
FROM films_genres AS fg
JOIN films AS f
ON f.id = fg.film_id 
WHERE fg.genre_id = 37 AND budget > 0 AND revenu > 0
```

Explication : 

### Vérification rentabilité Western

```SQL
SELECT SUM(revenu) / SUM(budget) AS test
FROM films_genres AS fg
JOIN films AS f
ON f.id = fg.film_id 
WHERE fg.genre_id = 37 AND budget > 0 AND revenu > 0
```

Explication : 

## Récupération des acteurs les plus populaires

### Conditions strictes 

```SQL
SELECT 
	id, nom,
	CASE WHEN sexe = 2 THEN 'Homme' ELSE 'Femme' END AS sexe, 
	ROUND(avg::numeric, 2) AS popularite
FROM acteurs AS a
JOIN (
SELECT acteur_id, AVG(popularite) -- ne change rien car la popu est la même pour tte les lignes d'un acteurs ** 
FROM films_acteurs 
GROUP BY acteur_id
HAVING COUNT(*) > 30) AS b
ON b.acteur_id = a.id
ORDER BY avg DESC 
LIMIT 100; 
```

Explication : 

### Conditions souples

```SQL
SELECT 
	id, nom,
	CASE WHEN sexe = 2 THEN 'Homme' ELSE 'Femme' END AS sexe, 
	popularite
FROM acteurs AS a
JOIN (
SELECT acteur_id, ROUND(AVG(popularite)::numeric, 2) AS popularite
FROM films_acteurs 
GROUP BY acteur_id
HAVING COUNT(*) > 0) AS b
ON b.acteur_id = a.id
WHERE popularite > 1; 
```

Explication : 


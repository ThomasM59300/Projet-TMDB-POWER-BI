# Mesures DAX – Projet TMDb Power BI

Ce fichier documente les principales **mesures DAX** utilisées dans le tableau de bord Power BI du projet d’analyse de données TMDb.

## Mesures de comptages

```dax
Nb Acteurs = DISTINCTCOUNT(acteurs[id])
```


```dax
Nb Films = COUNTROWS(films)
```


```dax
Nb Films Par Genre = 
CALCULATE(
    COUNTROWS(films_genres),
    USERELATIONSHIP(films_genres[genre_id], genres[id])
)
```



```dax
Nb Films Par Realisateur =
CALCULATE(
    COUNTROWS(films_realisateurs),
    USERELATIONSHIP(films_realisateurs[realisateur_id], realisateurs[id])
)
```

## Création table de date (pour les filtres)

```dax
TableDates = 
VAR debDate = MIN(films[date_sortie])
VAR finDate = MAX(films[date_sortie])
RETURN 
    ADDCOLUMNS(
        CALENDAR(debDate, finDate), 
        "Annee", YEAR([Date]), 
        "AnneeTxt", FORMAT([Date], "yyyy"),
        "Trimestre", "T"&QUARTER([Date]), 
        "Mois", MONTH([Date]), 
        "MoisTxt", FORMAT([Date], "mmm")
    )

```

## Mesures diverses et variées (selon onglet)

```dax
Est Top 3 = 
VAR RangFilm = RANKX(
    ALLSELECTED(films),
    AVERAGE(films[note_moyenne]),
    ,
    DESC,
    DENSE
)
RETURN
IF(RangFilm <= 3, 1, 0)
```


```dax
Benefice total = 
VAR Filtres = 
    FILTER(
        films, 
        films[budget] > 0 && films[revenu] > 0
    )

VAR SumBenef = SUMX(Filtres, films[revenu] - films[budget])

RETURN 
    SumBenef
```




```dax
Retour Sur Investissement segment = 
VAR Filtres = 
    FILTER(
        films,
        films[budget] > 0 && films[revenu] > 0
    )

VAR SumB = SUMX(Filtres, films[budget])
VAR SumR = SUMX(Filtres, films[revenu])

RETURN
IF(
    SumB > 0,
    (SumR - SumB) / SumB,
    BLANK()
)
```



```dax
RSI total = 
VAR DonneesFiltrees = 
    FILTER(
        ALL(films),
        films[budget] > 0 && films[revenu] > 0
    )

VAR SumB = SUMX(DonneesFiltrees, films[budget])
VAR SumR = SUMX(DonneesFiltrees, films[revenu])

RETURN
IF(
    SumB > 0,
    (SumR - SumB) / SumB,
    BLANK()
)
```

```dax
Somme budget = 
VAR Filtres = 
    FILTER(
        films, 
        films[budget] > 0 && films[revenu] > 0
    )

VAR SumBudget = SUMX(Filtres, films[budget])

RETURN
    SumBudget
```


```dax
Rentabilite Moyenne Acteur = 
CALCULATE(
    AVERAGEX(
        films,
        films[revenu] - films[budget]
    ),
    films[budget] > 0,
    films[revenu] > 0
)
```


```dax
Rentabilite Moyenne Acteur = 
CALCULATE(
    AVERAGEX(
        films,
        films[revenu] - films[budget]
    ),
    films[budget] > 0,
    films[revenu] > 0
)
```

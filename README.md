## Partie 2 – Analyse des données TMDb avec Power BI

### Objectif 

Cette deuxième partie du projet a pour but d’explorer, transformer et visualiser les données issues de l'API TMDb à l'aide de **Power BI**. L’analyse vise à dégager des tendances clés dans les films (genres populaires, budgets, scores, etc.) et à construire un tableau de bord interactif pour un usage analytique.

Le rapport est disponible au format pbix en téléchargement, mais pour une meilleure expérience, il est préférable de l'utiliser via le service Power BI : https://app.powerbi.com/links/P7ioLOm_bh?ctid=a7b3a854-5b11-4927-8868-91a5f35bc887&pbi_source=linkShare

--- 

### Étapes réalisées

1. **Connexion à la base PostgreSQL**  
   - Power BI a été connecté à la base PostgreSQL contenant les données collectées via l’API TMDb (voir partie 1 du projet).

2. **Nettoyage et transformation des données (Power Query & SQL)**

   - Formatage des dates et des durées, et d'autres types de colonnes.
   - Création de colonnes personnalisées (ex : année de sortie, ratio budget/revenu).
   - Jointure entre les différentes tables (films, genres, production, etc.).

3. **Modélisation des données**

   - Mise en place des relations entre les tables (1-n, n-n).
   - Choix d’un schéma en étoile pour faciliter les analyses (en respectant les normes de tables de faits + dimensions).

4. **Création des mesures DAX**

   - Revenu sur investissement, rentabilité (des réalisateurs, des genres...), revenu moyen par genre.

5. **Conception du tableau de bord interactif**

   - KPI clés : budget moyen, note moyenne, nombre total de films par réalisateurs, etc...
   - Graphiques dynamiques :
     - Visuels sur les acteurs
     - Visuels sur les genres
     - Visuels sur la rentabilité
     - Top 10 des films selon différents critères.
   - Filtres interactifs (année, genre, réalisateurs, sexe, etc.).
  
--- 

### Fichiers associés

- `TMDB_power_bi.pbix` : fichier Power BI contenant le tableau de bord complet.
- `captures/` : dossier contenant des captures d’écran du dashboard.
- `mesures_dax.md` : fichier listant les principales mesures DAX utilisées.
- `requetes_sql.md` : fichier listant les principales requêtes SQL pour récupérer des données transformées.
- `modele.png` : aperçu du modèle de données.
  

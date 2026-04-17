# Goku & Chi-Chi World Tour (goku-world-tour)

## Contexte
Un projet ludo-éducatif conçu pour Louis (le fils de Denis) afin de lui faire découvrir la géographie et les monuments du monde entier à travers ses personnages de manga préférés. Le site est hébergé sur GitHub Pages (`sined79/goku-world-tour`).

## Architecture & Automatisation
Ce projet est entièrement piloté par un cron OpenClaw (`goku_world_tour`) qui s'exécute toutes les 4 heures.

**Le cycle de vie du Cron :**
1. **Extraction :** Le script `scripts/pop_goku_country.py` dépile la prochaine destination depuis `scripts/goku_countries.json`. Il récupère le pays, le monument, le scénario, la latitude et la longitude.
2. **Génération d'Image :** OpenClaw utilise son outil `image_generate` pour créer un poster vertical (ratio 2:3) de Goku et Chi-Chi devant le monument, dans un style "Anime high quality".
3. **Notification :** L'image générée et l'histoire sont envoyées par e-mail à Denis et Louis (via `scripts/send_safe_email.py`).
4. **Publication :** Le script `scripts/publish_goku_git.py` copie l'image dans le dossier du site, met à jour `data.json` avec les informations géographiques, commit et pousse sur GitHub.

## Fonctionnalités du Site Web
- **Ordre d'affichage :** Les étapes s'affichent chronologiquement de haut en bas (Étape 1, 2, 3...). Un bouton "Découvrir la dernière aventure !" permet de scroller tout en bas grâce au smooth scroll.
- **Scénarisation :** Chaque étape comporte une mini-histoire textuelle immersive ("Le Radar à Dragon Balls s'affole...").
- **Flip Cards & Carte Interactive (Leaflet/OSM) :**
  - Au clic sur le bouton "Le Trajet du Nuage Magique", la carte se retourne en 3D.
  - Le dos de la carte affiche une carte OpenStreetMap centrée sur le pays actuel.
  - **Le Tracé :** Une ligne pointillée jaune/orange relie le pays précédent au pays actuel.
  - **Les Marqueurs :** Des icônes vectorielles SVG de *Dragon Balls* sont utilisées (1 étoile pour le pays de départ, 4 étoiles pour la destination).
- **Pré-calcul GPS :** Les coordonnées géographiques (lat/lon) doivent impérativement être pré-calculées et injectées dans le `data.json` par le script Python. Leaflet ne doit **pas** faire de géocodage à la volée (pour éviter les limites de requêtes de Nominatim).

## Règles de mise à jour
Toute modification du design doit conserver l'approche "enfant" (couleurs vives, boutons arrondis, badge d'étape) tout en restant très lisible et compatible mobile.
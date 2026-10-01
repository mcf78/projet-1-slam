# E-enseignement

Site pédagogique réalisé dans le cadre de l'épreuve E5 (Ynov Campus). Il présente l'équipe, des cours de programmation, des exercices corrigés en plusieurs langages et la documentation du projet.

Site statique en HTML, CSS et JavaScript, sans framework ni étape de compilation.

## Les pages

| Page          | Fichier                    | Contenu                                                                             |
| ------------- | -------------------------- | ----------------------------------------------------------------------------------- |
| Accueil       | `index.html`               | L'équipe, les langages pratiqués, les entreprises d'alternance et de stage          |
| Cours         | `pages/cours.html`         | Neuf notions de base, de l'algorithme à sa traduction en C, Java, JavaScript et PHP, puis un récapitulatif des syntaxes |
| Exercices     | `pages/exercices.html`     | Six exercices, énoncés et corrigés en algo, C, Java, JavaScript et PHP, avec coloration du code |
| Documentation | `pages/documentation.html` | Cahier des charges, charte graphique, diagrammes, maquette et outils du projet      |

## Lancer le site

Aucune installation n'est nécessaire, au choix :

- ouvrir `index.html` dans un navigateur ;
- utiliser l'extension **Live Server** de VS Code ;
- placer le dossier dans un serveur local, par exemple le dossier `htdocs` de XAMPP.

Une connexion internet est conseillée : les polices (Google Fonts) et la coloration du code (highlight.js) sont chargées depuis un CDN. Sans connexion, le site reste utilisable, avec des polices de remplacement et du code non coloré.

## Structure

```
e5-website/
├── index.html              # Page d'accueil
├── pages/
│   ├── cours.html
│   ├── exercices.html
│   └── documentation.html
├── css/
│   ├── global.css          # Variables (couleurs, tailles, espacements) et composants communs
│   ├── layout.css          # Structure commune : barre du haut, menu, pied de page
│   ├── accueil.css         # Un fichier par page pour ses styles propres
│   ├── cours.css
│   ├── exercices.css
│   └── documentation.css
├── js/
│   ├── script.js           # Script commun (pages exercices et documentation)
│   ├── exercices.js        # Données des exercices et affichage de la page
│   └── cours.js            # Coloration du code de la page cours
├── images/icons/           # Logo Ynov
├── documents/              # CV de l'équipe
│   ├── Documentation/      # Livrables affichés sur la page documentation
│   └── contenu-cours/      # Contenu rédigé des cours et exercices (Markdown)
└── docker-compose.yml      # Bonus : lancement avec Docker
```

Chaque page charge `global.css`, puis `layout.css`, puis sa propre feuille de style. Cet ordre permet à une page d'ajuster un style commun sans `!important`.

## Fonctionnement

### Exercices

Tout le contenu de la page vient de l'objet `exercices` dans `js/exercices.js`. Le langage et l'exercice affichés sont lus dans l'URL :

```
pages/exercices.html?language=C&exo=2
```

Sans paramètre, la page affiche l'exercice 1 en algorithme.

Pour **ajouter un exercice**, il suffit d'ajouter une entrée dans l'objet `exercices` : numéro, nom, énoncé, résultat attendu et code pour chaque langage. La liste des exercices se met à jour toute seule, sans toucher au HTML.

Le bouton « Executer » ne lance pas réellement le programme : un navigateur ne peut pas exécuter du C, du Java ou du PHP. Il affiche le résultat attendu dans la zone « Sortie ».

### Cours

Le dépliage des aperçus fonctionne **sans JavaScript**, grâce à des boutons radio cachés et au sélecteur CSS `:checked`. Le détail est commenté dans `pages/cours.html` et `css/cours.css`. JavaScript ne sert qu'à colorer les blocs de code.

## Conventions

- Tailles, couleurs et espacements passent par les variables de `global.css` (`--fs-label`, `--color-primary`, `--space-3`…) plutôt que par des valeurs écrites en dur.
- Code et commentaires en français.
- Messages de commit au format `type(portée): message`, par exemple `style(cours): ajout du survol sur la liste`.

## L'équipe

- Moncef Mallem
- Christophe Aleixo
- Alexis De Jesus
- Thomas Montout

---

## Bonus : lancer le site avec Docker

Le site peut aussi être servi par un conteneur **nginx** léger (`nginx:alpine`), nommé `e-enseignement`. Les fichiers du projet y sont montés en lecture seule : une modification dans le code est visible immédiatement, sans reconstruire le conteneur.

**Prérequis** : [Docker](https://docs.docker.com/get-docker/) avec Docker Compose (inclus dans Docker Desktop).

```bash
git clone https://github.com/FlytziTv/e5-website.git
cd e5-website
docker compose up -d
```

Le site est alors disponible sur [http://localhost:6969](http://localhost:6969).

Pour arrêter le conteneur :

```bash
docker compose down
```

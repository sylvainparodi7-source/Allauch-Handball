# AHB — Site V3 avec Pages CMS, GitHub et Netlify

## Ce que contient cette version

Le site V2.4 est conservé : présentation du club, **12 catégories** avec horaires, entraîneurs et liens FFHandball, tarifs 2026–2027, encadrement, comité directeur, coordonnées et formulaire Netlify.

Les **actualités** sont dans `content/news/*.json` (un article = un fichier) et les **équipes** dans `content/teams.json`.

Pages CMS affiche une interface de rédaction à partir de `.pages.yml`. Chaque modification enregistrée modifie le dépôt GitHub. Netlify reconstruit le site depuis ce dépôt.

## Installation sans terminal

1. Créer un compte GitHub et un dépôt **privé** nommé par exemple `allauch-handball-site`. Le dépôt doit appartenir à un compte contrôlé par le club, et non uniquement au compte personnel d'un dirigeant.
2. Décompresser le ZIP. Sur la page du nouveau dépôt GitHub, choisir **Add file → Upload files**, puis déposer **le contenu** du dossier `ahb-site-v3`, y compris le fichier caché `.pages.yml`. Vérifier que `.pages.yml`, `netlify.toml`, `content/`, `assets/`, `scripts/` et `template.html` se trouvent **à la racine** du dépôt. Ne pas uploader le dossier `dist/` (il est régénéré automatiquement).
3. Dans Netlify, choisir **Add new project → Import an existing project → GitHub**, sélectionner le dépôt. La configuration `netlify.toml` définit la commande `python3 scripts/build.py` et le répertoire à publier `dist`. Lancer le déploiement.
4. Aller sur https://app.pagescms.org, se connecter avec GitHub, installer l'application Pages CMS sur le dépôt du club, puis ouvrir celui-ci. Le menu contient **Actualités** et **Équipes et entraînements**.
5. Dans **Actualités**, créer un article, compléter `title`, `category`, `date`, `summary`, `body`, éventuellement `image`, et cocher `published`. Enregistrer. GitHub reçoit le changement, puis Netlify redéploie automatiquement.
6. Pour les demandes de contact : dans Netlify, activer la détection des formulaires si nécessaire ; puis configurer les **form submission notifications** à `6313005@sud.ffhandball.net`, publier une fois le formulaire reconnu et réaliser un test réel. Le code du formulaire n'envoie pas de message SMTP directement.
7. Lorsque tout est vérifié, ajouter le domaine acheté dans **Domain management** de Netlify et configurer ses DNS. Ne pas modifier les enregistrements MX du domaine de messagerie en faisant cette opération.

## Publication des actualités : principes

- `published: false` : brouillon, non visible sur le site ; `published: true` : visible si `date` n'est pas dans le futur.
- Le titre et le résumé s'affichent dans la carte d'actualité. Le texte complet est visible dans une zone « Lire la suite » lorsque différent du résumé.
- Les images téléchargées par Pages CMS sont enregistrées dans `assets/` et leur chemin est inscrit dans le fichier de l'article.
- Le résultat d'une modification est **public après le déploiement Netlify**, pas avant.
- Les résultats FFHandball restent **des liens externes**, sans récupération automatique de scores.

## Administration et sécurité

- L'ancienne administration de démonstration utilisant `localStorage` a été retirée. **Pages CMS**, accessible via une connexion GitHub, devient l'éditeur des actualités et des catégories.
- Autoriser seulement les comptes GitHub des dirigeants habilités, idéalement avec authentification multifacteur. Les droits sur le dépôt donnent des droits de modification du site.
- Vérifier les autorisations de publication des photographies, particulièrement pour les mineurs.
- Ne jamais placer de listes de licenciés, fichiers financiers ou autres documents privés dans ce dépôt destiné à la publication.
- Conserver les accès GitHub, Netlify, DNS et domaine au nom de l'association, avec au moins deux responsables.
- **Ce package ne crée aucun compte et n'est pas encore déployé** : une première connexion GitHub/Netlify/Pages CMS reste nécessaire.

## Test local (facultatif)

```bash
python3 scripts/build.py
python3 -m http.server 8000 --directory dist
```
Ouvrir http://localhost:8000. Ne pas ouvrir directement `template.html`, qui est seulement le modèle de génération. Pour un test local sans Pages CMS, modifier les fichiers JSON et relancer le build.

## Fichiers

- `.pages.yml` : configuration du CMS
- `content/news/*.json` : articles
- `content/teams.json` : fiches des catégories
- `template.html` : page et design d'origine
- `scripts/build.py` + `scripts/site.js` : génération du site et affichage des contenus
- `netlify.toml` : construction et publication automatique
- `dist/` : sortie générée localement, non nécessaire dans le dépôt
- `assets/` : photos et logo AHB

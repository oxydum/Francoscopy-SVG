# 📝 Francoscopy

**Un Wiki personnel moderne, entièrement contenu dans un fichier SVG.**

Francoscopy est une application de prise de notes et de gestion de connaissances (PKM) qui repousse les limites du format SVG. Pas de serveur, pas de base de données, pas de build Webpack/Vite : juste un seul fichier \`.svg\` que vous ouvrez dans votre navigateur.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Format](https://img.shields.io/badge/format-SVG%20%2B%20ECMAScript-orange.svg)
![Size](https://img.shields.io/badge/size-One%20File-green.svg)

---

## ✨ Fonctionnalités

### 🗺️ Le Canvas Spatial
Oubliez l'organisation arborescente classique. Francoscopy utilise un Canvas où chaque page est un bloc vectoriel.

* **Drag & Drop** : Déplacez vos blocs pour organiser visuellement vos idées.
* **Redimensionnement** : Étirez les blocs pour leur donner la taille que vous souhaitez.
* **Visibilité** : Masquez les blocs perturbateurs et restaurez-les depuis le menu "Pages".

### 📝 Éditeur Hybride (Markdown + Wiki)
Une syntaxe riche qui combine le meilleur du Markdown et des conventions Wiki :

* **Markdown complet** : Listes, gras, italique, blocs de code (moteur \`marked.js\`).
* **Titres avec sous-titres** : \`== Titre | Sous-titre ==\`
* **Attributs / Métadonnées** : \`@clé: valeur\` (rendu avec un badge latéral élégant).
* **Liens internes** : \`[[Nom de la page]]\` (navigation instantanée).
* **Images** : \`!url_de_l_image!\` (avec gestion du CORS et erreurs masquées).

### 🔍 Recherche Plein Texte
Trouvez n'importe quelle information en un clin d'œil grâce au moteur \`MiniSearch\`. La recherche filtre les pages en temps réel dans l'onglet "Pages".

### 🎨 Personnalisation Avancée

* **Couleur de fond des pages** : Avec gestion de la transparence (opacité).
* **Couleur de la barre de titre** : Identifiez visuellement vos catégories de pages.
* **Thème de l'interface** : Modifiez la couleur principale, le fond et le texte global.

### 💾 Persistance et Sécurité

* **Sauvegarde automatique** : Vos données sont sauvegardées en arrière-plan dans l'\`IndexedDB\` de votre navigateur (\`localForage\`).
* **Historique (Ctrl+Z)** : Annulez vos modifications globales grâce à la comparaison de différences (\`DeepDiff\`).
* **Import / Export** : Exportez votre Wiki en JSON (nommé avec la date du jour) pour l'archiver ou le partager.

---

## 🚀 Comment l'utiliser ?

1. Téléchargez le fichier \`francoscopy.svg\`.
2. Ouvrez-le avec n'importe quel navigateur moderne (Firefox, Chrome, Edge).
3. C'est tout ! Commencez à écrire.

> **Note sur le protocole file:// :**
> Bien que le fichier fonctionne en local, certaines fonctionnalités (comme le chargement initial des librairies via CDN ou le CORS des images) fonctionnent mieux si vous ouvrez le fichier via un serveur local (ex: l'extension *Live Server* de VS Code, ou \`python -m http.server\`).

---

## 🛠️ Architecture et Défis Techniques

Francoscopy est une expérience technique explorant les limites de \`foreignObject\` en SVG.

**La stack (injectée via CDN) :**

* **[Marked.js](https://marked.js.org/)** : Parseur Markdown rapide et extensible.
* **[MiniSearch](https://lucaong.github.io/minisearch/)** : Moteur de recherche full-text côté client.
* **[LocalForage](https://github.com/localForage/localForage)** : Stockage asynchrone hors-ligne (IndexedDB).
* **[DeepDiff](https://github.com/flitbit/diff)** : Calcul de différences pour l'historique d'annulation.

**Les pièges de foreignObject contournés :**

1. **Espace de noms XML** : Le contenu HTML injecté doit impérativement porter le namespace \`xmlns="http://www.w3.org/1999/xhtml"\`.
2. **Héritage CSS** : Le HTML dans un \`foreignObject\` n'hérite pas des styles CSS du parent SVG. Les polices et couleurs doivent être réinitialisées manuellement.
3. **Événements (Event Bubbling)** : Le drag & drop SVG et les clics HTML s'entrechoquent. L'utilisation de \`evt.stopPropagation()\` est critique pour pouvoir cliquer sur un lien wiki sans déclencher le déplacement du bloc.
4. **Manipulation du DOM** : La méthode \`innerHTML\` est préférable à \`document.createElementNS\` pour mettre à jour dynamiquement des listes HTML dans le SVG, sous peine de voir les éléments rendus invisibles par le parseur de certains navigateurs.

---

## 📖 Syntaxe du Wiki

\`\`\`text
== Mon Titre | Mon Sous-titre ==

@Auteur: Nom
@Statut: Brouillon

Bienvenue sur cette page. Voici un **texte en gras** et du *texte en italique*.

- Liste item 1
- Liste item 2

Un bloc de code : \`console.log('hello')\`

Un lien vers une autre page : [[Concepts]]

Une image : !https://upload.wikimedia.org/wikipedia/commons/4/4f/SVG_Logo.svg!
\`\`\`

---

## 📁 Mode Hors-Ligne (Option A)

Par défaut, Francoscopy charge ses librairies depuis un CDN (nécessite internet au premier chargement). Pour une utilisation 100% hors-ligne :

1. Téléchargez les 4 fichiers JS minifiés (\`marked.min.js\`, \`index.min.js\`, \`localforage.min.js\`, \`deep-diff.min.js\`).
2. Placez-les dans le même dossier que \`francoscopy.svg\`.
3. Dans le fichier SVG, commentez les lignes du CDN et décommentez les lignes locales :

\`\`\`xml
<!-- Utilisation locale -->
<script xlink:href="marked.min.js"></script>
<script xlink:href="index.min.js"></script>
<script xlink:href="localforage.min.js"></script>
<script xlink:href="deep-diff.min.js"></script>
\`\`\`

---

## 📜 Licence

Ce projet est sous licence MIT. Il est gratuit pour tout le monde. N'hésitez pas à le forker, le modifier et l'améliorer !

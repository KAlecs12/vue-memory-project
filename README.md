# Vue Memory Project

## Installation

Installer les dépendances :

```bash
# yarn
yarn install

# npm
npm install
```

Démarrer le serveur local :
```bash
npm run dev
```
Build le projet :
```bash
npm run build & npx http-server dist
```


## Fonctionnalités :

- Cette application permet de réviser différents thèmes à l'aide de cartes de révision.
- Les cartes de révision contiennent un recto et un verso avec du texte et/ou des éléments multimédias.
- Les thèmes peuvent être organisés en catégories créées par l'utilisateur.
- Les utilisateurs ont la possibilité de créer leurs propres thèmes et cartes de révision.
- Les utilisateurs peuvent définir le nombre de niveaux et de nouvelles cartes à réviser chaque jour pour chaque thème.
- La révision commence par le niveau le plus élevé avec le nombre de nouvelles cartes sélectionné, puis passe au niveau 1.
- L'application peut configurer des rappels quotidiens si l'utilisateur autorise les notifications du navigateur.
- Cette application est responsive et peut être utilisée hors-ligne en mode "build".


## Organisation :
```
vue-memory-project/
├── public/
│   ├── all necessary assets
├── src/
│   ├── router/
│   │   ├── index.js
│   ├── stores/
│   │   ├── counter.js
│   ├── views/
│   │   ├── Cards.vue
│   │   ├── Categories.vue
│   │   ├── Revision.vue
│   │   ├── Themes.vue
├── .gitignore
├── index.html
├── index.css
├── package.json
├── package-lock.json
├── README.md
└── tailwind.config.js
└── vite.config.js
└── postcss.config.js
```

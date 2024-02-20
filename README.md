# Docker-react-node

## `Lancement de l'application`

**`Docker compose up`**

***Ouvrez [http://localhost:3000](http://localhost:3000) pour acceder à l'application React.***

## `Environnement de développement`

Voici un petit récapitulatif de l'architecture en développement (dev):

- **Application client :** c'est l'application SPA servie aux utilisateurs.
J'ai utilisé React car je suis spécialisé sur celui-ci, mais cela fonctionnerait exactement pareil pour Angular et Vue.
L'application client utilise Webpack dev server en développement : il s'agit d'un serveur Node.js qui permet de détecter les changements dans le code source, de recompiler le code et de rafraichir la page du navigateur (cela permet d'obtenir et d'utiliser le live-reload).

- **Reverse-proxy :** il s'agit du point d'entrée de l'application qui va se charger ici uniquement de répartir les requêtes entrantes entre mes différents services.
Pour ce fait, j'ai utilisé nginx avec une configuration très basique.
Il va rediriger :
  - les requêtes commençant par /api vers mon API qui sera un serveur Node.js
  - les requêtes commençant par /sockjs-node vers le serveur de développement Webpack (la petite difficulté etait de gérer les Websockets qui sont nécessaires pour Webpack dev server afin d'avertir le navigateur qu'il doit recharger la page après une recompilation)
  - les autres requêtes vers Webpack dev server qui sert mon application React en développement.
- **Base de données :** c'est une base de données NoSQL MongoDB auquelle mon serveur Node.js se connectera.

## `Environnement de production`

- **Reverse-proxy :** comme pour l'environnement dev, il s'agit du point d'entrée de l'application qui va se charger ici uniquement de répartir les requêtes entrantes entre mes services.
Il va rediriger :
  - les requêtes commençant par /api vers mon API qui sera un serveur Node.js.
  - les autres requêtes vers un autre service nginx qui va uniquement servir le build de mon application React de manière optimisée.
- **Base de données :** idem que l'environnement de dev

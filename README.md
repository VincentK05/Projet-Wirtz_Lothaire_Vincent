# LuxResto
## Sommaire
- Description
- Utilisation
- Explication du code
- Principales difficultées
- Développement
## Description
Vous vous apprêtez à explorer le plus beau (et unique...) Grand Duché au monde, le pays de toutes les opportunités! Cependant, bien que le Luxembourg ne soit pas célèbre pour sa gastronomie, à part peut-être son incontournable 'Bouneschlupp', vous ne savez pas où satisfaire vos papilles! C'est là que LuxResto intervient, pour vous aider à dénicher de délicieuses pépites culinaires.
\
\
LuxResto est votre meilleur allié pour trouver le restaurant parfait en fonction de vos envies gastronomiques, de vos exigences en matière de qualité et de votre emplacement. Que vous ayez envie de déguster une spécialité locale ou de vous aventurer vers d'autres horizons culinaires, nous avons le plan idéal pour vous.

## Utilisation
## Explication du code
## Principales difficultées
- Trouver le site à scraper pour récupérer les données
  Tous les sites ne permettent pas le scrapage sans difficultées. Certaines pages web ont des systèmes de sécurité en place qui 'bloquent' les utilisateur qui tentent de les scraper. Le processus pour trouver le site convenable a ainsi été fait d'essais et d'erreurs. Cependant, pour éviter des ennuis inutiles, nous avons pris des précautions pour minimiser le risque de se faire 'bloquer du site choisi.
1. Lors du lancement de l'instance du navigateur chrome avec webDriver, nous avons ajouté une option qui randomise l'user-agent utilisé pour accéder à la page 'resto.lu'. Ainsi, a chaque fois que le code est lancé, c'est un autre user-agent qui est utilisé, ce qui permet de ne pas paraitre suspect en se connectant plusieurs fois d'affilé.
2. Lors du passage automatique à la page suivante du site, nous avons ajouté une condition time.sleep afin que le programme attende quelques secondes (durée randomisée entre 7s et 16s) entre deux pages. Ceci évite de dévoiler le traitement automatisé.
3. Pour s'assurer de ne pas perdre les données récupérées avant que le programme ne récupère les données de toutes les pages, nous avons rajouté une ligne de code indiquant que les données soient stockées dans un dossier json toutes les 10 pages (json.dump).
## Développement


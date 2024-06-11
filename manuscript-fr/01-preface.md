# Preface

L'idée pour ce livre est apparue après mon intervention "Refactoring Like a Superhero" [^talk]  que j'ai réalisée en Janvier 2022. Pour cette intervention, j'ai récolté de diverses techniques de refactorisation que je souhaitait partager.

A un certain point, il m'est apparu que je ne pourrait pas faire tenir tout ce dont je voulais parler dans une intervention de 40 minutes. Je devais faire des choix et n'aborder que les pratiques et les techniques les plus importantes.

Le matériaux expurgé n'était néanmoins pas sans intérêt. Il contenait des détails et des clarifications à propos des techniques, leurs usages et leur applicabilitée. J'ai donc décidé qu'il vallais mieux changer le format, au travers d'un livre en ligne, plutôt que de me débarasser de cette documentation. C'est ainsi qu'est né le projet de ce livre. 

## Qui pourrais être intéressé par ce livre 

Ce livre peut-être utile pour les développeurs de _web services_ et d'applications utilisateur utilisant des langagues de "haut niveau" et qui ont quelques années d'expérience. 

Les développeurs de bilbiothèques pourraient y trouver quelques idées, mais je me concentre principalement sur les développeurs d'applications car j'ai plus d'expérience dans leur développement. 

Ce livre ne prends pas en compte les besoins et contraintes du développement "bas niveau". Certaines techniques pourraient entre en contradiction les bonnes pratiques des languages "bas niveau". Si vous écrivez du code "proche de la machine", s'il vous plait, faites attention et avancez à vos risques et périls.  

## Ce que ce livre n'est pas

Je ne prétends pas démontrer la "seule et unique façon correcte" de refactoriser et d'écrire du code dans ce livre. Si vous avec beaucoup d'expérience, vous connaissez probablement la plupart des techniques présentées ici et vous vous êtes fait votre propre opinion. 

Ce livre n'est pas non plus un manuel "étape par étape" universellement applicable à tous vos projets. Mon expérience de développeur biaise mes habitudes de code et mon style de programmation. Votre expérience, vos projets et vos habitudes peuvent différer des miennes et donc vos opinions ne seront pas forcément les mêmes. Ce n'est pas grave. 

La but de ce livre est de décrire un ensemble de techniques qui _m'ont_ aidé à écrire du code qui _à l'air joli_ pour moi et les personnes avec qui j'ai travaillé. 

Toutes les pratiques n'ont pas à être appliquées à chaque. Utiliser une technique dépends grandement du projet, du contexte, des ressources disponibles, et du but recherché pour cette refactorisation. Essayez de choisir les techniques qui apportent le plus grand bénéfice avec le moins d'investissement.

Si quelque chose dans ce livre vous semble utile, discutez en avec vos collègues et d'autres développeurs. Assurez vous que vous et votre équipe êtes à la même page sur la marche à suivre, ces bénéfices et ces impacts. N'imposez pas quelque chose de controversé à votre équipe. 

## Limitations et Applicabilité

Toutes les techniques décrites ici sont compilées depuis les livres que j'ai lu ainsi que mon expérience en développement. J'ai passé la plaupart de temps à développer des applications moyennes et parfois des larges.

Mon expérience dicte ce que je considère comme du bon code. Ce livre représente ma perception du développement en 2022. Mon opinion a pu changer si vous liser ceci dans le future.

| Au fait 🐝                                                                                                                   |
| :--------------------------------------------------------------------------------------------------------------------------- |
| Je vais mettre à jour le contenu de ce livre si mon opinion change, mais je ne peux pas garantir que je le ferais sans délai |

Lors de votre lecture de ce livre, soyez conscient des biais cognitif de son auteur. Considérez les techniques avant de les utiliser et considerez attentivement leir applicabilité dans votre projet.  

## Bon à savoir avant de lire

Dans le texte, je considère que vous êtes déjà familiers avec le concepte de refactorisation[^term] et que vous avez quelques années d'expérience en matière de programmation. Je considère que vous avez déjà rencontré des problèmes avec la décomposition des taches, des abstractions incorectes, de la séparation de responsabilités entre modules, etc.

Je considère que vous êtes familiers avec ces "programming buzzwords":
- Séparation de responsabilité
- Couplage et cohésions
- Style déclaratif
- Niveau d'abstractions
- Séparation command-requête
- Pipeline fonctionnel
- Coeur fonctionnel dans une invite de commande
- L'architecture en couche, "Ports and Adapters"
- La direction de dépendences
- Immutabilité et architecture "stateless"
- "12-factor applications"

Vous n'avez pas besoin de les maitrisers pleinement. Nous allons les explorer tout au long de ce livre. Mais il peut-être utile d'avoir une idée générale de leur signification.

## Pourquoi un autre livre

Il existe beaucoup de livres qui traitent de refactorisation, pourquoi avons besoin d'encore un autre? 

Dans l'ensemble, nous n'en avons pas beoin. Tous les principes et techniques décris ici, le sont également dans d'autres livres, parfois de manière plus détaillée, plus logique et parfois plus correcte.

J'ai besoin de cet ouvrage, premièrement pour moi même:
- Pour systématiser ce que je sais aujourd'hui
- Pour me référer à un sujet particulier lorsque je discute d'un problème.
- Pour marquer le niveau de mon savoir pour identifier les points d'amélioration et ce que je dois apprendre.

En revanche, j'ai pensé que ce texte pouvais être utile pour quelqu'un d'autre. J'espère que vous trouverez ce livre utile. 

[^talk]: “Refactor Like a Superhero” Talk, https://bespoyasov.me/talks/refactor-like-a-superhero/
[^term]: Code Refactoring, Wikipedia, https://en.wikipedia.org/wiki/Code_refactoring
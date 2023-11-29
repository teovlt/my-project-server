# Compte Rendu Projet de Git R5.07

29/11/2023
Teo Villet
Mayeul Deries
Mounir Madmar
Terry Marrot

## Workflow : Merge

## Ojectif : Mettre en pratique les principes DevOps en développant deux fonctionnalités pour un jeu d'échec.

## 1. Organisation du travail au sein de l'équipe 🤝

Nous avons constitué deux binômes pour le développement de deux fonctionnalités au sein du projet. La première fonctionnalité concerne la **promotion** d'un pion aux échecs lorsqu'il atteint la rangée adverse la plus éloignée (rangée 8 pour les blancs, rangée 1 pour les noirs). La seconde fonctionnalité porte sur l'amélioration de l'expérience utilisateur en introduisant un aperçu **visuel indiquant** à l'utilisateur les cases possibles pour chaque pièce lorsqu'elle est sélectionnée.

Comme demandé dans le sujet, nous avons choisi d'adopter une approche structurée en créant deux branches secondaires dédiées à chaque **fonctionnalité**. Cette démarche vise à assurer une gestion claire et efficace des différentes évolutions du code. Nous avons développé nos fonctionnalités selon une approche TDD permettant de faciliter le développement et d'éviter d'ajouter du code superflue.

Pour faciliter la **coordination** au sein de notre équipe, nous avons opté pour la mise en place d'un **workflow merge** afin de garantir la continuité du développement tout en minimisant les conflits potentiels entre les fonctionnalités en cours de réalisation. De plus, pour assurer la propreté et l'harmonie du workflow git, nous avons décidé de mettre en place une structure simple pour les messages de commit. Nous avons choisi de rédiger systématiquement nos commits en **anglais** en les composant d'un verbe d'action (add, delete, fix…), suivi des fonctionnalités ajoutées, supprimées ou modifiées.

En parallèle, afin de renforcer la qualité du code produit, nous avons introduit le concept de **"Merge Request"**. Ce mécanisme offre un suivi rigoureux du développement et de l'intégration en soumettant chaque branche secondaire à l'approbation des membres du binôme n'ayant pas directement contribué à la Merge Request. Cette démarche favorise une évaluation croisée du code de qui permet de garantir un niveau élevé de qualité dans l'intégration des fonctionnalités au code principal.

## 2. Intégration continue 🔄

Avant d'entamer le développement des fonctionnalités, nous avons instauré un processus d'**intégration continue (CI)** afin d'assurer la qualité du code tout au long du cycle de développement.

Pour cela, nous avons commencé par créer un fichier
.gitignore
. en lui fournissant certains mots-clés liés aux langages et aux outils utilisés. Dans notre cas nous avons choisi de créer le fichier
.gitignore
à partir des mots clés **VisualStudioCode** et **Cypress**.

Puis nous avons créé un fichier
.gitlab-ci.yml
. à la racine du projet. Ce fichier définit les étapes du **pipeline d'intégration continue**, déterminant les actions à entreprendre à chaque modification du code. Pour cela, nous avons utilisé la même image docker que lors du **tp pipeline**. Nous avons créé **4 jobs**.

- Le job **build** permet d’installer les **dépendances** nécessaires à l'exécution des tests.

- Le job **test** permet d'exécuter les **tests unitaires** afin de s'assurer que tous les tests passent bien sur notre version du code source.

- Le job **lint** permet d'exécuter le linter qui permet de s’assurer de garder un code propre et harmonieux.

- Le job **coverage** permet de vérifier la couverture du code.

Enfin, nous avons ajouté des **dépendances supplémentaires**, telles que mocha-junit-reporter qui permet la génération de rapports de tests au format XML.

Pour finir, nous avons complété la mise en place de l'**intégration continue** en générant un fichier
.eslintrc.json
. Ce fichier a été configuré pour définir les règles de linting avec **ESLint**, renforçant ainsi la qualité du code en identifiant et corrigeant automatiquement les violations des **conventions de codage définies par l'équipe**.

## 3. Promotion d’un pion ♕

## Partie API

Le premier binôme a développé la fonctionnalité de **promotion** d’un pion. Pour respecter l’approche TDD, nous avons commencé par écrire les tests unitaires. Nous avons choisi d’écrire 2 suites de tests, l’une permettant de tester si un pion est dans une position où il peut être promu. Pour cela, nous avons imaginé une méthode **canPromote()**, inspirée de la méthode canMove() déjà présente dans le code à l’origine. Cette méthode permet simplement de vérifier si un pion blanc est placé au rank 8 ou si un pion noir est placé au rank 1, qui sont les conditions requises à un pion pour etre promu. Nous avons écris 2 tests conçus de la même manière permettant de tester qu’un pion peut être promu ou ne pas l'être selon sa couleur et son emplacement sur le plateau.

```javascript
it('should be able to promote a pawn at rank 8', function () {
  const chessboard = new Chessboard()
  const pawn = new Pawn({ chessboard, color: Color.WHITE, rank: 8, file: 3 })

  assert.strictEqual(pawn.canPromote(), true)
})
```

Puis nous avons développé une suite de tests permettant de tester une méthode de promotion qui permettrait, dans le cas où un pion est dans une position où il peut être promu (canPromote), de le promouvoir en **reine**, **tour**, **fou** ou **cavalier**. Nous avons pour cette méthode écrit 5 tests permettant de tester chacun des cas de promotion ainsi qu’un cas impossible.

```javascript
it('should promote a pawn to a queen', function () {
  chessboard.pieces.push(pawn)
  pawn.promote('queen')

  const promotedPiece = chessboard.getPiece(8, 3)
  assert(promotedPiece instanceof Queen)
})
```

Après l'écriture des tests unitaires, nous nous sommes assurés que ceux-ci ne passaient pas (car les méthodes n’étaient pas encore implémentées), puis nous avons développé les deux méthodes **canPromote()** et **promote()**.

## Revue de code

Lors de la phase de revue de code, plusieurs améliorations significatives ont été apportées au projet. Tout d'abord, des **commentaires** superflus présents dans le code ont été identifiés et supprimés, contribuant ainsi à une base de code plus claire et concise. De plus, des ajustements ont été effectués pour éliminer des espaces inutiles dans l'interface, améliorant ainsi la **lisibilité** du code.

Une autre modification **importante** a été de vérifier si des commentaires n’étaient pas en français, conformément à nos conventions établies l’anglais était la langue choisie et de les corriger si besoin. Cette uniformisation linguistique favorise la cohérence et la compréhension au sein de l'équipe de développement.
La détection d'une classe inutilisée a également été réalisée, permettant ainsi une optimisation du code en éliminant du superflu. Cette action contribue non seulement à la performance du programme, mais simplifie également la maintenance future.

Enfin, une correction mineure a été apportée au niveau du CSS afin d'améliorer la visibilité des cases disponibles. Cette attention aux détails garantit une meilleure **expérience utilisateur** et renforce la qualité globale du projet.

En **conclusion**, la revue de code a joué un rôle essentiel dans l'optimisation du code, la correction des anomalies et l'harmonisation des conventions, contribuant ainsi à la qualité et à la robustesse du projet.

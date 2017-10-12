# Workflow - Pipeline

## Git

Voici les opérations de base de git:

### Au début (une seule fois à priori)

#### git clone

Récupérer un projet Git, c'est à dire l'ensemble d'un projet, avec toutes les versions précédentes, modifications, tags, etc...

Pour pouvoir participer à un projet tracké par Git, il faut faire ceci afin de rejoindre un nouveau repo, dans notre cas, une seule fois, comme ceci:

	git clone https://github.com/hyperteach/git-cheatsheet

On peut éventuellement vous demander votre login et mot de passe GitHub pour continuer


Vous pouvez aussi mettre votre nom d'utilisateur directement dans la commande, ainsi vous n'aurez pas à le retaper à chaque opération distante:

	git clone https://MON_PSEUDO@github.com/hyperteach/git-cheatsheet

#### git init

Créer un nouveau projet Git   
   
Pour créer un nouveau projet Git, il faut placer son terminal dans le dossier qu'on veut utiliser, puis:

	git init

Si vous voulez que ce projet soit hébergé sur un serveur distant, il faut mettre ça en place (par exemple sur [github.com](https://github.com/), puis une fois qu'on a l'adresse du repo distant, faire:

	git add origin https://github.com/MON_PSEUDO/NOM_DU_PROJET
	git push -u origin master


### Opérations distantes

#### git fetch

Demande au repo distant s'il y a des modifications, et récupère toutes les modifications de la branche actuelle:

    git fetch

L'option `--all` permet de demander aussi s'il y a des modifications sur les autres branches:

    git fetch --all

#### git pull

Cette opération est en fait un mix de `git fetch` et `git merge`. Elle est souvent plus utile car il est rare de vouloir faire un `git fetch`sans faire un `git merge` tout de suite derrière.
Elle sert lorsqu'on souhaite rapatrier les modification des autres membres de l'équipe et elle doit être faite avant l'opération `git push` (un message d'erreur se chargera de vous prévenir si vous oubliez).

    git pull

Elle peut aussi être utilisée avec l'option `--all` afin de récupérer aussi les autres branches du projet. Je conseille de faire ceci afin d'avoir une meilleure vue d'ensemble du projet lorsque les autres travaillent sur d'autres features.

    git pull --all

#### git push

L'opération `git push`prend le travail qui a été fait sur la branche actuelle, et envoie ces données au repo distant. C'est la *seule* façon de rendre disponible le travail qui a été fait aux autres membres de l'équipe:

    git push
    
Si la branche actuelle n'a jamais été poussée sur le repo distant il faut la faire inscrire au repo distant avec l'option `-u` (après on fait `git push` sans options)

    git push -u origin nom_de_la_branche

A faire par habitude à la fin de la journée, afin que l'équipe puisse consulter les changements au début de la prochaine journée. A faire aussi lorsqu'on souhaite partager immédiatement les changements en cours (par éxemple un artiste ajoute un modèle à intégrer, ou un codeur répare un bug bloquant).

### Opérations locales

Les opérations distantes ne sont pas les opérations les plus importantes d'un projet Git. Git n'est pas seulement un outil de partage, mais surtout un outil de gestion de projet, permettant, s'il est utilisé correctement, de retrouver facilement des bugs, de revenir sur ses pas, et d'avoir une vue claire de la progression historique d'un projet.

La gestion se fait localement, puis est partagée avec le reste de l'équipe avec les commandes `git pull` et `git push`.

pour gérer un projet Git, il est important de comprendre que *l'atome* d'un Git, c'est le __Commit__. Analogue au "Revisions" de SVN.

Il éxiste trois "types" de Commit:

+ Le Commit initial
    + Il n'a aucun commit précédent. Dans notre cas, c'est un projet Unity vide.
+ Le Commit "normal"
    + Il décrit un ensemble de changements. E.g: Un ajout de fonction à une classe.
+ Le Merge
    + Le Merge a deux parents. Il contient idéalement la somme du meilleur de ceux-ci.

### git status

Affiche l'état du projet git. Contient souvent aussi des conseils sur quelles commandes utiliser pour passer à la suite.

Voici le résultat pour moi actuellement:

    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    nothing to commit, working directory clean

Ceci signifie qu'au dernières nouvelles, mon projet Git est synchro avec le repo distant, qu'aucun fichier tracké n'a été modifié, et qu'il n'y a pas de nouveaux fichiers.

N'hésitez pas à user et à abuser de cette commande. Elle ne peut jamais faire de mal, et aide souvent à retrouver son chemin (après tout, comment décider quoi faire si on ne sait pas où on en est?)

### git add

Ajoute un fichier nouveau, modifié ou supprimé à l'espace de travail Git (appellé "Staging"). Après avoir travaillé, il faut faire une ou plusieurs commandes `git add` pour selectionner les fichiers à ajouter à son prochain Commit. `git add` est toujours utilisé avec des options ou des arguments.

Ajouter un fichier spécifié (Utile si on souhaite selectionner avec précision les fichiers à ajouter au Commit en évitant d'autres, ou alors ajouter un nouveau fichier qui n'éxistait pas auparavant):

    git add Chemin/Vers/UnFichierModifie.cs
    
Ajouter tous les fichiers trackés (Utile si on n'a pas ajouté de nouveaux fichiers, et qu'on souhaite enregistrer tous ses changements, c'est à dire 90% des cas)

    git add -u
    
(Ne pas hésiter à faire un `git add -u` suivi de quelques `git add NouveauFichier.cs` pour les nouveaux fichiers)

Et enfin, la commande attrape tout (même quelques trucs qu'on préfèrerait mieux ne pas attraper). On peut s'en servir aussi, mais il devient fortement conseillé de consulter le `git status` pour vérifier qu'il n'y a pas d'invités surprise:

    git add .

### git commit

Après avoir fait ses `git add`, on crée un nouveau Commit:

    git commit

Appellée seule, cette commande ouvre un console Vim pour éditer son message. Personellement, je ne comprends rien à Vim, du coup j'édite le message directement dans la comment `git commit` grâce à l'option `-m`

    git commit -m "Fixes lighting on oversaturated lights"

Petit rappel, `git add` et `git commit` sont des opérations *locales*, mes co-équipiers ne voient pas encore les changements. Si veux partager mon travail, je dois donc faire un `git push` maintenant. En revanche, comme il est si facile de créer de nouveaux Commits sans embêter personne, il devient super utile de créer plein de petits Commits au fil de la journée de travail, puis de faire un `git push` quand on a fini.

### git merge

Lorsque deux branches ont divergé (en local parce qu'on travaille sur plusieurs branches, ou lorsque plusieurs développeurs travaillent sur la même branche en simultané), on peut rapatrier ces changements dans la branche actuelle grâce à la commande `git merge`

Rapatrier les changements divergents du repo distant (appellé automatiquement par `git pull`):

    git merge
    
Rapatrier les changement d'une autre branche vers la branche actuelle:

    git merge nom_de_la_branche

S'il y a des conflits que Git ne sait pas résoudre tout seul, il vous indiquera quels fichiers il n'a pas su résoudre. On verra plus bas comment résoudre les conflits.

### git checkout

Sert à deux choses (éssentiellement):   
+ Récupérer un fichier depuis un autre endroit dans le projet (une version précédente, ou sur une autre branche).
+ Se déplacer sur une autre branche (et en créer une nouvelle au besoin)

Récupérer un fichier depuis une autre branche:

    git checkout nom_de_la_branche Chemin/Vers/LeFichierEnQuestion.cs

Récupérer un fichier à un Commit donné (besoin des quelques premiers caractères du code d'un commit, visibles dans `git log` ou [sur la page GitHub](https://github.com/hyperteach/git-cheatsheet/commits/master)):

    git checkout 8b1c5d Chemin/Vers/LeFichierEnQuestion.cs

Se déplacer sur une branche éxistante:

    git checkout nom_de_la_branche

Se déplacer sur une nouvelle branche (option `-b`)

    git checkout -b nom_de_la_branche
    
Utilisez `git checkout -b` pour créer des nouvelles branches dès que vous vous attelez à une nouvelle tâche, même si elle semble être finissable en un ou deux Commits seulement. Ca ne coûte rien, et ça permet beaucoup plus de flexibilité dans son travail.

## Exemple d'utilisation:

Je décide d'ajouter une fonction au projet:

    git checkout -b fight_mechanics_01

Je commence à travailler, faisant des `git add`, `git commit` à chaque étape.

Edgar se retourne, et me dit "Hé Kevin, ton shader là, il marche pas du tout avec les textures transparentes, il me faut fissa un fix!". Mes modifications sur les mécaniques de combat n'étant pas terminés (et peut-être fourré de bugs dangereux), je décide de créer une nouvelle branche depuis le master.

     git checkout master
     git checkout -b shader_transparency_fix
    
Je suis sur une nouvelle branche qui part du master, ne contenant donc pas mes changements sur fight_mechanics_01.   
Je corrige les shaders afin de répondre à la demande d'Edgar, faisant des `git add` et `git commit`à chaque étape.   
Après une heure de travail, je pense avoir réglé le problème. Je pousse la branche au repo distant afin qu'Edgar puisse vérifier.

    git push -u origin shader_transparency_fix
    
Edgar de son coté, récupère cette branche pour voir si ça convient.

    git pull --all
    git checkout shader_transparency_fix
    git pull
    
"C'est pas mal, mais on a vachement perdu en performances." me dit Edgar.   
Je continue encore un peu le travail sur les shaders, puis, satisfait des tests de performance, j'intégre ces changements au master.

    git checkout master
    git pull
    git merge shader_transparency_fix

(Je fais des trucs de ninja décrits plus tard pour résoudre les conflits éventuels).   
Ensuite je pousse ces changements au repo distant.

    git pull
    git push

Je retourne sur ma branche pour le système de combat et je continue à bosser.

    git checkout fight_mechanics_01

Une fois que je suis satisfait du système de combat, je l'intègre, lui aussi, au master

    git checkout master
    git pull
    git merge fight_mechanics_01
    
Le système de combat ne touchant peu aux autres fichiers du projet, ce merge se fait sans encombre, je peux donc tout pousser et rentrer à la maison.

    git push

## Le merge manuel

Parfois, lors d'un `git merge` ou d'un `git pull` un fichier ne peut pas être reconcilié automatiquement. pas de panique, il éxiste des solutions simples.

### `git checkout --keep-theirs` et `git checkout --keep-ours`

Ces deux commandes écrasent la fichier avec une copie parfaite de la version rapatriée (`--keep-theirs`) ou de la version qui éxistait avant le début du merge (`--keep-ours`). Imaginons que Edgar et moi avons tous des modifié une scène .unity et une texture .png   
Je décide de garder ma version de la scène, et de récupérer la version d'Edgar de la texture:

    git checkout --keep-theirs Assets/Textures/Jacket.png
    git checkout --keep-ours Assets/Scenes/Level_01.unity

### `git mergetool`

Le mergetool est un outil permettant de créer une nouvelle version d'un fichier de code source, en fusionnant manuellement deux versions différentes. Il faut installer et configurer un outil de mergetool, je précognise [meld](http://blog.nerdbank.net/2011/10/how-to-get-meld-working-with-git-on.html). Puis utiliser la commande `git mergtool`. Utilisée sans options, elle lancera le logiciel de mergetool pour chaque fichier encore en conflit.

    git mergetool

Ou on peut l'appeller sur un fichier individuel:

    git mergetool Assets/Scripts/Player.cs
    
### `git commit`

Après avoir corrigé les conflits, on crée le Commit de merge à la main en faisant simplement

    git commit
    
L'interface Vim s'ouvre avec un message de commit par défaut. Faites `échap`, `:` puis `x` pour quitter Vim et finaliser le Commit.

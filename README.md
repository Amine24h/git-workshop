Configuration du GIT
-----

La première chose à faire est de configurer votre identité (nom utilisateur et mot de passe).

    $ git config --global user.name "Mon nome"
    $ git config --global user.email mon.email@example.com

Commencer la journée avec GIT
---------------------

Soit cloner ce dépôt:

    $ git clone https://github.com/Amine24h/git-workshop.git
    $ cd git-workshop

Soit créer un nouveau dépôt:

    $ mkdir git-workshop
    $ cd git-workshop
    $ git init

Comment savoir qu'un dépôt GIT est en place ? il faut juste vérifier la présence
d'un dossier caché nommé `.git`. C'est là où GIT sauvegarde tous les données et l'historique.

    $ ls .git

Vous allez retrouver :

    config  description  HEAD  hooks  info  objects  refs logs

Staging area
----------------

Commençant par l'ajout des fichiers.

    $ code ali.txt mohamed.txt

`code` c'est l'éditeur de texte de Microsoft (Visual studio code)

Dans GIT, on commence par l'ajout des fichiers au `staging area` en utilisant
`git add`. Cette manipulation permet au développeur de préparer ça commit (de la cuisiner
avant de la servir). Une fois les changements sont prèt, on va les commiter via
la commande `git commit`.

Ajoutant les fichiers au `staging area`

    $ git add ali.txt mohamed.txt

Commiter les changements
--------------------------

    $ git commit -m "Add new files"

Voir l'historique
-------------------

Pour voir l'ensemble des commits (l'historique), on utilise `git log`

    $ git log

Le log va afficher l'ensemble des commit trier par date (ordre descendant).
Pour chaque commit on va avoir les informations suivantes : commit (SHA),
Author, Date, message.

Pour avoir plus de détails sur une commit, utiliser `git show`.

    $ git show

Vous allez avoir un affichage qui ressemble à ça:

    commit 4ceeaea505c453996b63c70128ee24b8fea818f9
    Author: amine24h
    Date:    Sun Feb 26 10:47:11 2017 +0100

        Initial commit

    diff --git a/.gitignore b/.gitignore
    new file mode 100644
    index 0000000..4d45b00
    --- /dev/null
    +++ b/.gitignore
    @@ -0,0 +1,12 @@
    +node_modules
    +jspm_packages
    +npm-debug.log
    +debug.log
    +src/**/*.js
    +!src/systemjs.config.extras.js
    +!src/systemjs.config.js
    +*.js.map
    +e2e/**/*.js
    +e2e/**/*.js.map
    +_test-output
    +_temp
    \ No newline at end of file

Voir les changements
----------------------

On peut comparer le `working copy` avec `staging area` afin de déterminer
ou de voir les changemnts apportés.

Pour faire il faut ajouter du textz au fichier `ali.txt` et sauvegarder le fichier.

Pour voir ce qui a été changé, il suffit juste d'utiliser la commende `git diff`.

    $ git diff

Vérifier l'état du `working copy` et `staging area`
----------------------------------------------------

Maintenant on va ajouter le fichier `ali.txt` au `staging area`. Souvenez-vous comment ?

Aprés on va vérifier si le fichier `ali.txt` est dans le `staging area`

    $ git status

Annuler les changements
-------------------------

Soit disons que nous n'avons pas aprécié les derniers changements. Le `staging area` 
nous permet de revenir en arrière facilement avant de commiter.

Pour faire :

    $ git reset HEAD ali.txt

    Unstaged changes after reset:
    M   ali.txt

Comparer le `git status` actuel avec le celui de la section précédente.

Dans certains cas, on veut nettoyer et revenir vers le dernier état sauvegarder.

Pour accomplir ça, on utilise `git checkout`:

    $ git checkout ali.txt

Branchement
-------------

Dans les grands projets un dépôt GIT possède au moins deux branches `master` et `develop`.
Une fois le code est testé et prêt pour la production on merge la branche `develop`
dans la branche `master`.

GIT a facilité la création et la manipulation des branches.

Pour lister les branches on exécute la commande:

    $ git branch

    master
    * develop

Pour créer une nouvelle branche :

    $ git branch new-feature

Pour switcher entre les branches:

    $ git checkout new-feature

Ajoutons des modifications dans la nouvelle branche:

    $ code new-file.txt
    $ git add new-file.txt
    $ git commit -m "Added new new file 'new-file'"

Maintenant, on va comparer ces changements par raport à la branche `master`:

    $ git diff master

Jouer au cache‐cache
---------------------

GIT est assez intelligent pour gérer les fichiers quand on switche entre les branches.
Essayez de switcher entre les branches master `master` et `new-feature` pour voir
la déférence.

    $ git checkout master
    $ ls

    ali.txt   mohamed.txt

    $ git checkout new-feature
    $ ls

    ali.txt   mohamed.txt   new-file.txt

Merger les branches
-------

Afin de merger les changements de deux déférentes branches, on doit utilise
la commande `git merge`.

    $ git checkout master
    $ git merge new-feature

On va merger la branche `new-feature` dans la branche `master`. Par défault l'algorithme
de merge va utiliser la politique `fast-forward` sinon il va utiliser `3-way-merge`

Les conflits de merge
-----------------------

GIT peut merger automatiquement, mais dans certains cas la même ligne d'un fichier
est modifiée. Dans ce cas-là une résolution manuelle du conflit est nécessaire.

Modifier le fichier `mohamed.txt`:

    $ git checkout new-feature
    $ code mohamed.txt

Créer une nouvelle branche `conflict-branch` et modifier le fichier `mohamed.txt`:

    $ git checkout -b conflict-branch
    $ code mohamed.txt

Merger la branche `conflict-branch` dans `new-feature`:

    $ git checkout new-feature
    $ git merge conflict-branch

Corriger les conflits de merge
---------------------------------

Une fois vous avez merger les deux branches, vous allez vous retrouver dans un état
de merge.

Soit annuler le merge avec la commande:

    $ git merge --abort

Soit corriger les conflits manuellement:

    $ git status
    $ code mohamed.txt
    $ git add mohamed.txt
    $ git commit -m "Fixed conflict"

Bravo, vous avez réglé le conflit.

Bitbucket
------

Afin qu'on puisse collaborer avec d'autre développeurs, un serveur pour héberger
nos dépôts GIT est nécessaire.

Pour notre workshop on va utiliser Bitbucket.

En premier lieu on doit se connecter sur Bitbucket pour voir nos projets et dépôts.

Dépôts distants
---------------

Pour voir la liste des dépôts distants:

    $ git remote -v

Ajouter un dépôt distant:

    $ git remote add new-remote https://remote.dz/git-repo

Pousser les changements:

    $ git push remote new-feature

Pousser des tags:

    $ git checkout new-feature
    $ git tag -a V1.0 -m "first release"
    $ git push new-remote --tags

Récupérer les derniers changements:

    $ git fetch new-remote new-feature
    $ git merge new-remote/new-feature
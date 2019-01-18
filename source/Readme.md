# Documentation du shopozor

Ce dépôt sert de documentation pour le projet de shopozor. Le langage utilisé
est le RestructuredText avec les extensions propres à Sphinx.

La documentation est publiée sur http://docs.shopozor.surge.sh/ ou http://dev.docs.shopozor.surge.sh/ (version développeurs).

## Prérequis sur Ubuntu

Il faut installer pipenv

```bash
sudo pip3 install pipenv
```

## Étapes à suivre

Cloner le dépôt

```bash
git clone git@github.com:softozor/shopozor-docs.git
cd shopozor-docs
# initialisation de l'environnement virutel
pipenv shell
# installation des dépendances (Sphinx etc ...)
pipenv install
```

## Commandes propres à Sphinx

Pour lancer la compilation du projet et publier les modifications, il y a plusieurs "commandes" dans le `Makefile`.

### Mode développement

Lorsque l'on crée des modifications sur la documentation, on peut lancer la commande 

```
make livehtml
```

dans la racine du dossier (celui où se trouve le fichier `Makefile`) et cela va lancer un serveur de développement qui dispose du LiveReload : dès qu'un fichier `.rst` est modifié, la projet est recompilé et le site mis à jour dans le navigateur.

L'adresse pour visualiser le résultat est http://localhost:8080/

### Publication de la documentation sur surge.sh

Pour publier la documentation sur surge.sh, il faut installer `surge.sh` avec 

```
yarn global add surge
```

et s'identifier avec 

```
surge login
```

Ensuite, on peut faire 

```
make deploy
```

Il faut pour cela avoir les autorisations pour publier sur le site  http://docs.shopozor.surge.sh/ et  http://dev.docs.shopozor.surge.sh/. L'utilisateur pour ces sites est cedonner 'at' gmail.com.


=============
Configuration
=============

Un petit test pour voir comment tout cela fonctionne ... Mais ça ne se met pas à jour tout seul ... et il n'y a pas pas la coloration syntaxique malheureusement ...

Chargement de ``shopozor/settings.py``
======================================

Pour pouvoir définir notre propre config dans le fichier
``shopozor/settings.py``, il faut régler la variable d'environnement avec

::

    export DJANGO_SETTINGS_MODULE="shopozor.settings"

..  tip::

    Pour une utilisation avec pipenv, rajouter

    ::

        DJANGO_SETTINGS_MODULE="shopozor.settings"

    dans le fichier :file:`.env` à la racine.

    Pour une utilisation avec ``virtualenv``, modifier le script ``activate``.

Il y a une autre possibilité de spécifier le module de ``settings`` à charger,
l'option ``--settings`` de ``manange.py`` (cf.
https://django-configurations.readthedocs.io/en/stable/) :

::

    python manage.py runserver --settings shopozor.settings


Configuration ``dev``, ``staging`` et ``prod``
==============================================

On peut même utiliser l'héritage de classe pour permettre de définir des
paramètres de base communs à toutes les configurations différentes et ensuite
faire des classes spécialisées pour les différents environnements. Le package
``django-configurations`` est spécialement prévu pour cela.

..  tip::

    Principe de ``django-configurations`` : https://django-configurations.readthedocs.io/en/stable/patterns/

On peut alors spécifier le fichier de configuration à charger ainsi que la
configuration spécifique (classe à charger) ::

    python manage.py runserver --settings=mysite.settings --configuration=Dev
    python manage.py runserver --settings=mysite.settings --configuration=Staging
    python manage.py runserver --settings=mysite.settings --configuration=Production



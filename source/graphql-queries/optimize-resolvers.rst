===========================
Optimisation des resolveurs
===========================

Recherches dans le code actuel
==============================

Il y a manifestement déjà des mécanismes en place pour optimiser les résolveurs
en donnant des indications sur la manière d'effectuer le SQL. Il faudrait
approfondir le sujet..

Exemple dans :file:`saleor/graphql/product/types.py`
----------------------------------------------------

..  literalinclude:: code/product-types.py
    :language: python
    :linenos:
    :pyobject: ProductVariant
    :emphasize-lines: 39

On peut voir l'import à la ligne 5 du fichier 

..  code-block:: python
    :linenos:
    :emphasize-lines: 5

    import re
    from textwrap import dedent

    import graphene
    import graphene_django_optimizer as gql_optimizer
    from django.db.models import Prefetch
    from graphene import relay
    from graphql.error import GraphQLError


Éléments à étudier
------------------

..  todo::

    Il faudrait comprendre également le package ``graphene_django_optimizer``
    (https://github.com/tfoxy/graphene-django-optimizer).


..  todo::

    Pour pouvoir bien tirer parti du package ``graphene_django_optimizer``, il
    fault comprendre les options d'optimisation des requêtes intégrées à l'ORM
    Django. En particulier, il faut se familiariser avec les ``QuerySets`` dans
    Django : https://docs.djangoproject.com/fr/2.1/ref/models/querysets/


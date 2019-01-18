=========================
Extension des permissions
=========================

Fonctionnement des permissions dans Saleor/GraphQL
--------------------------------------------------

Les permissions sont gérées dans les mutations et les requêtes GraphQL grâce aux
décorateurs provenant de ``graphql_jwt``
(https://django-graphql-jwt.domake.io/en/stable/).

..  admonition:: Décorateurs pertinents
    :class: attention

    *   ``permission_required``
    *   ``login_required``
    *   etc ...

Exemple : permissions sur une mutation GraphQL
++++++++++++++++++++++++++++++++++++++++++++++

Le code ci-dessous montre un exemple d'utilisation du décorateur
``permission_required`` dans la classe ``OrderUpdateShipping`` dans le
fichier :file:`saleor/graphql/order/mutations/orders.py`.

..  literalinclude:: code/orders.py
    :language: python
    :linenos:
    :pyobject: OrderUpdateShipping
    :emphasize-lines: 17

L'argument ``order.manage_orders`` est une permission définie dans
``salor/core/permissions.py`` de la manière suivante :


..  literalinclude:: code/permissions.py
    :language: python
    :linenos:



Il faut utiliser la variable ``ROOT_URLCONF``
(https://docs.djangoproject.com/fr/2.1/ref/settings/#std:setting-ROOT_URLCONF)
pour spécifier le module Python qui indiquera la correspondance entre les urls
et les vues. Dans notre cas, il faut donc remplacer le fichier
:file:`saleor/urls.py` qui définit l'endpoint ``/graphql`` par notre propre
fichier ``urls.py``, probablement :file:`shopozor/urls.py`.


..  admonition:: Questions en suspend
    :class: error

    *   Comment faire pour étendre la liste des permissions du fichier
        :file:`saleor/core/permissions.py` ? 
        
        *   Il ne suffit probablement pas de simplement importer la liste
            ``MODELS_PERMISSIONS`` et l'étendre depuis notre propre module car
            les modifications risquent de ne pas être prises en compte (à
            tester)


Groupes d'utilisateurs
======================

..  todo::

    Il faudrait mieux comprendre comment fonctionnent les groupes dans Django.
    Est-ce que l'on peut intégrer des utilisateurs dans des groupes et attribuer
    les permissions aux groupes ?

    Je ne trouve rien de tel dans le schéma GraphQL ni dans les modèles Django.
    Il me semble que les permissions sont directement assignées aux
    utilisateurs.

    Il me semble que pour l'instant, on peut juste dire qu'un utilisateur fait
    partie du staff

    ::

        is_staff = models.BooleanField(default=False)


    Manifestement, il n'y a pas de groupes. Les permissions sont directement
    assignées aux utilisateurs avec un Mixin. Il faudrait comprendre comment cela
    fonctionne (:file:`saleor/account/models.py`)

    ..  tip::

        Jeter un oeil à https://docs.djangoproject.com/fr/2.1/ref/models/options/#permissions

    ..  code-block:: python
        :linenos:
        :emphasize-lines: 5, 21-31

        class User(PermissionsMixin, AbstractBaseUser):
            email = models.EmailField(unique=True)
            addresses = models.ManyToManyField(
                Address, blank=True, related_name='user_addresses')
            is_staff = models.BooleanField(default=False)
            token = models.UUIDField(default=get_token, editable=False, unique=True)
            is_active = models.BooleanField(default=True)
            note = models.TextField(null=True, blank=True)
            date_joined = models.DateTimeField(default=timezone.now, editable=False)
            default_shipping_address = models.ForeignKey(
                Address, related_name='+', null=True, blank=True,
                on_delete=models.SET_NULL)
            default_billing_address = models.ForeignKey(
                Address, related_name='+', null=True, blank=True,
                on_delete=models.SET_NULL)

            USERNAME_FIELD = 'email'

            objects = UserManager()

            class Meta:
                permissions = (
                    (
                        'manage_users', pgettext_lazy(
                            'Permission description', 'Manage customers.')),
                    (
                        'manage_staff', pgettext_lazy(
                            'Permission description', 'Manage staff.')),
                    (
                        'impersonate_users', pgettext_lazy(
                            'Permission description', 'Impersonate customers.')))

            def get_full_name(self):
                return self.email

            def get_short_name(self):
                return self.email

            def get_ajax_label(self):
                address = self.default_billing_address
                if address:
                    return '%s %s (%s)' % (
                        address.first_name, address.last_name, self.email)
                return self.email



En attente
==========

Il faut encore noter que le schéma GraphQL est défini dans
:file:`saleor/graphql/api.py` et que ce fichier exporte la variable ``schema``.
Les lignes importantes sont mises en évidence

..  literalinclude:: code/api.py
    :language: python
    :linenos:
    :emphasize-lines: 19, 29, 36

La stratégie va être de créer de nouvelles classes ``ShopozorQuery(Query)`` et
``ShopozorMutations(Mutations)`` et d'exporter ensuite notre schéma avec 

::

    schema = graphene.Schema(ShopozorQuery, ShopozorMutations)



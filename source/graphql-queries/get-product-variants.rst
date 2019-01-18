Toutes les variantes d'un produit
=================================

Pour obtenir les variantes d'un produit, il suffit d'avoir l'ID du produit

..  code-block:: plain

    query {
      products(first: 1) {
        edges {
    	    node {
            name
            id
          }  
        }
      }
    }

..  admonition:: Output

    ::

        {
            "data": {
                "products": {
                    "edges": [{
                        "node": {
                            "name": "Nebula Night Sky Paint",
                            "id": "UHJvZHVjdDo2MQ=="
                        }
                    }, ]
                }
            }
        }

Et ensuite, cherches les variantes d'un des produits

..  code-block:: plain

    query {
      product(id:"UHJvZHVjdDo2MQ==") {
        variants {
           id
            sku
          name
        }
      }
    }


..  admonition:: Ouput

    ::
    

        {
            "data": {
              "product": {
                "variants": [
                  {
                    "id": "UHJvZHVjdFZhcmlhbnQ6MTcy",
                    "sku": "546451997",
                    "name": "2.5l"
                  },
                  {
                    "id": "UHJvZHVjdFZhcmlhbnQ6MTcz",
                    "sku": "546451999",
                    "name": "5l"
                  },
                  {
                    "id": "UHJvZHVjdFZhcmlhbnQ6MTcx",
                    "sku": "546451996",
                    "name": "1l"
                  }
                ]
              }
            }
          }
##Doctrine
### Fixtures 
Les fixtures sont des données qui sont injecté lors du déploiement d'un projet sur un environnement données. Elles permettent de pré-intégrer certains comportement, données ou exemple dans votre programme Symfony intégrant doctrine.

La commande par défaut est simplement : 
`php bin/console d:f:l`

Mais vous pouvez l'agrémenter de plusieurs autres parametres :  
```php
php app/console doctrine:fixtures:load --fixtures=/path/to/fixture1 --fixtures=/path/to/fixture2 --append --em=foo_manager php```


Le parametre `--append` Permet de ne pas purger la base avant d'ajouter vos fixtures, mais simplement de les ajouter à la suite

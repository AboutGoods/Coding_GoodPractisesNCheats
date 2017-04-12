# Ajout des migrations Symfony


#### Ajout du bundle migration dans composer
```composer require doctrine/doctrine-migrations-bundle "^1.0"```

#### Ajout de la référence du Bundle dans le AppKernel.php

```
<?php
// app/AppKernel.php
   public function registerBundles()
   {
       $bundles = array(
           //...
           new Doctrine\Bundle\MigrationsBundle\DoctrineMigrationsBundle(),
       );
   }
   ?>
```

#### Configuration

Dans **app/config/config.yml**

```
doctrine_migrations:
    dir_name: "%kernel.root_dir%/DoctrineMigrations"
    namespace: Application\Migrations
    table_name: migration_versions
    name: Application Migrations
```

#### Migrations automatiques par différence

Lors d'une modification de l'entité.
Génération d'une classe de migration avec les différences avec la BDD


```php bin/console doctrine:migrations:diff```


#### Appliquer la migration

```php bin/console doctrine:migrations:migrate```


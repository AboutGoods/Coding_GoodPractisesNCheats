# Intro by https://github.com/hhamon/lpdim2016/blob/master/README.md

Framework PHP & Application de Blog
===================================

Ceci est la version finale du « mini framework » PHP développé en cours du 4 au
8 janvier 2015. Le dossier `src/` contient à la fois le code générique du
framework dans le namespace `Framework` et le code du blog dans le namespace
`Application`.

Cette section décrit les différents composants du projet développés pendant le
cours et améliorés par la suite. Le cours *Modules Technos Web* porte en
priorité sur l'apprentissage du langage PHP en mode orienté objet ainsi que sur
des concepts plus fondamentaux tels que le Web, le protocole HTTP, la sécurité
et l'architecture logicielle.

Le Langage PHP
--------------

### Historique de PHP

PHP est l'acronyme récursif pour Hypertext Preprocessor. C'est un langage de
programmation interprété développé initialement par Rasmus Lerdorf en 1994. À sa
sortie, PHP s'appelait PHP/FI, pour *Form Interpreter*. Rasmus a développé PHP
pour ses besoins personnels. Il souhaitait comptabiliser dynamiquement les
visites sur son site et récupérer les données envoyées par des formulaires HTML
afin de les traiter au niveau du serveur. PHP est aussi un langage de
programmation multi-plateformes. Il fonctionne aussi bien sur Windows, que Linux
et Mac OS X. Enfin, c'est aussi est un langage Open-Source, gratuit et publié
sous licence PHP. L'ouverture de son code source permet à n'importe quel
développeur ayant des connaissances en langage C de contribuer au code et le
modifier pour ses propres besoins. À ce jour, les statistiques estiment que plus
de 80% des sites Internet de la planète sont développés avec PHP.

PHP est développé sur la base du langage C. Les scripts sont interprétés à
l'exécution par le moteur PHP (le *Zend Engine*) et sont transformés à la volée
en *opcode*. L'opcode est du code binaire machine comparable à l'assembleur. Il
s'agit d'une liste d'instructions opérationnelles primitives.

C'est en 1997 pour leur projet d'études universitaires qu'Andi Gutmans et Zeev
Suraski décident de modifier le moteur PHP pour introduire un système
d'extensions. C'est principalement cet atout qui a popularisé PHP notamment en
favorisant le support de bases de données relationnelles telles que MySQL.

À la fin de l'année 1998, une nouvelle version de PHP est publiée : PHP 4. C'est
sans doute la version de PHP qui a connu le plus gros succès. Cette nouvelle
mouture intègre un tout nouveau coeur réécrit *from scratch*. C'est aussi
l'arrivée du moteur *Zend Engine* en Mai 2000. La version 4 de PHP est arrivée
avec un nombre important de nouvelles fonctionnalités comme le support des
sessions, la « bufferisation » de sortie et l'intégration avec d'autres serveurs
web. C'est aussi dans PHP 4 que les premières implémentations des concepts de la
programmation orientée objet ont fait leur apparition. En effet, il était déjà
possible de structurer le code avec des classes. Les objets étaient toutefois
passés par copie en argument des fonctions et méthodes mais ce défaut de
conception n'a pas empêché l'arrivée des premiers frameworks et bibliothèques de
code professionnels tels que Creole DB, Smarty et CakePHP.

Ce n'est qu'en juin 2004 que PHP 5.0 voit le jour. Cette version intègre une
nouvelle mouture du moteur interne, le *Zend Engine 2*. Ce moteur vient avec de
bien meilleures performances et offre au langage PHP de nouvelles
fonctionnalités :

* extension `filter`,
* nouvelles fonctions de manipulation des tableaux,
* extension `iconv`,
* gestion des flux,
* etc.

Mais la vraie révolution de PHP 5.0, c'est l'introduction du support complet de
la programmation orientée objet. Le modèle objet de PHP 5.0 est similaire à
celui de Java. De plus, ,PHP 5.0 et 5.1 viennent avec de nouvelles extensions
orientées objet telles que PDO, SimpleXML, DOM et MySQLi. PHP 5.2 fera son
apparition quelques années plus tard introduisant de nouvelles fonctionnalités
et de meilleures performances.

En 2005, une initiative est lancée par Andrei Zmievski pour développer PHP 6 en
introduisant un support complet de l'Unicode. Malheureusement, les changements
nécessaires au niveau du moteur *Zend Engine* et des extensions natives pour
supporter l'Unicode se sont avérés bien trop importants et n'ont pas permis à
PHP 6 de voir le jour. Le projet a été avorté quelques années plus tard.

Après quelques années d'incertitude concernant l'avenir de PHP, la *Core Team*
décide de reprendre le travail sur la base de PHP 5. La version PHP 5.3 voit
donc le jour en juin 2009 et s'accompagne de nouvelles fonctionnalités majeures
comme les espaces de nommage (*namespaces*), les fonctions anonymes, le
ramasse-miettes (*garbage collector*), les extensions Intl, Phar, Fileinfo et
Sqlite3.

PHP 5.4 est publié deux ans plus tard et fournit entre autres le support des
*traits* comme nouveauté majeure. C'est aussi dans PHP 5.4 que les algorithmes
de gestion de la mémoire et des variables ont été largement optimisés pour
offrir de bien meilleure performances par rapport aux versions précédentes. Avec
un cycle régulier de publication des nouvelles versions tous les deux ans en
moyenne, les versions PHP 5.5 et 5.6 voient le jour avec leurs lots de nouvelles
fonctionnalités (générateurs, mot-clé *finally*, cache d'opcode natif, etc.).

Ce n'est qu'à la fin de l'année 2015 que la nouvelle version majeure de PHP est
publiée : PHP 7.0, anciennement appelé *PHPNG* (*New Generation*). Cette
nouvelle mouture intègre un tout nouveau moteur PHP réécrit *from scratch* à
l'initiative de Nikita Popov. PHP 7 est annoncée avec des performances deux fois
supérieures en moyenne par rapport aux versions précédentes de PHP tout en
garantissant une compatibilité rétrograde de la syntaxe. PHP 7 vient aussi avec
tout un tas de nouvelles fonctionnalités dont le tant attendu *typage strict*.

En parallèle, Facebook développe un fork similaire de PHP 7 appelé *HHVM*
(*HipHop Virtual Machine*) qui compile du langage Hack (sorte de syntaxe PHP) en
code C++.

### Contributeurs au Langage PHP

Les développeurs et mainteneurs du coeur de PHP forment aujourd'hui une équipe restreinte d'une quinzaine de bénévoles tout au plus :

* **Rasmus Lerdorf** : le créateur original de PHP,
* **Zeev Suraski** : l'un des co-créateurs de PHP 3 et co-foundateur de la société Zend,
* **Andi Gutmans** : l'un des co-créateurs de PHP 3 et co-foundateur de la société Zend,
* **Pierre Joye** : développeur indépendant en charge du support de PHP sur les plateformes Microsoft Windows et Azure Cloud,
* **Sara Golemon** : développeuse Hack/HHVM (Facebook), contributrice à PHP,
* **Anthony Ferrara** : contributeur à PHP,
* **Julien Pauli** : contributeur à PHP, release manager de PHP 5.5 / 5.6,
* **Derick Rethans** : contributeur historique à PHP, auteur de l'extension XDebug,
* **Lukas Kawe Smith** : release manager de PHP 5.3,
* **Nikita Popov** : core contributeur à PHP 7.0,
* **Dmitry Stogov** : core contributeur à PHP,
* **Sascha Schumann** : core contributeur à PHP,
* **Anatol Belski** : core contributeur à PHP,
* **Joe Watkins** : core contributeur à PHP,
* **Ilia Alshanetsky** : core contributeur à PHP,
* **Johannes Schlueter** : core contributeur à PHP, développeur chez Oracle,

Et bien d'autres contributeurs historiques depuis les débuts du projet :
[crédits](http://php.net/credits.php).

### Commandes PHP Utiles

Cette section dresse la liste de toutes les options utiles de l'utilitaire `php`
en ligne de commande dans un terminal.

#### Afficher la Version de PHP

Dans un onglet du terminal, l'exécution de la commande `php -v` affiche la
version de PHP et du *Zend Engine*. La sortie est similaire à celle ci-dessous.

```
$ php -v
PHP 5.6.17 (cli) (built: Jan  8 2016 23:49:08)
Copyright (c) 1997-2015 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2015 Zend Technologies
    with Zend OPcache v7.0.6-dev, Copyright (c) 1999-2015, by Zend Technologies
    with Xdebug v2.3.3, Copyright (c) 2002-2015, by Derick Rethans
    with blackfire v0.24.1, https://blackfire.io/, by SensioLabs
```

#### Lister les Modules Installés

La commande `php -m` affiche la liste des modules et extensions actuellement
installés et actifs.

```
$ php -m
[PHP Modules]
apc
apcu
bcmath
blackfire
bz2
...
xsl
Zend OPcache
zip
zlib

[Zend Modules]
Xdebug
Zend OPcache
blackfire
```

#### Lister les Fichiers de Configuration de PHP

La commande `php --ini` affiche la liste du/des fichiers de configuration
`php.ini` chargés.

```
$ php --ini
Configuration File (php.ini) Path: /opt/local/etc/php56
Loaded Configuration File:         /opt/local/etc/php56/php.ini
Scan for additional .ini files in: /opt/local/var/db/php56
Additional .ini files parsed:      /opt/local/var/db/php56/APCu.ini,
/opt/local/var/db/php56/blackfire.ini,
/opt/local/var/db/php56/curl.ini,
/opt/local/var/db/php56/exif.ini,
...
```

#### Afficher la Configuration de PHP

La commande `php -i` affiche la liste des paramètres de configuration de PHP et
les valeurs courantes pour chaque module installé et actif.

```
$ php -i
phpinfo()
PHP Version => 5.6.17

System => Darwin C02MC0KAF5YV.local 14.5.0 Darwin Kernel Version 14.5.0: Tue Sep  1 21:23:09 PDT 2015; root:xnu-2782.50.1~1/RELEASE_X86_64 x86_64
Build Date => Jan  8 2016 23:48:23
Configure Command =>  './configure'  '--prefix=/opt/local' '--mandir=/opt/local/share/man' '--infodir=/opt/local/share/info' '--program-suffix=56' '--includedir=/opt/local/include/php56' '--libdir=/opt/local/lib/php56' '--with-config-file-path=/opt/local/etc/php56' '--with-config-file-scan-dir=/opt/local/var/db/php56' '--disable-all' '--enable-bcmath' '--enable-ctype' '--enable-dom' '--enable-filter' '--enable-hash' '--enable-json' '--enable-libxml' '--enable-pdo' '--enable-session' '--enable-simplexml' '--enable-tokenizer' '--enable-xml' '--enable-xmlreader' '--enable-xmlwriter' '--with-bz2=/opt/local' '--with-mhash=/opt/local' '--with-pcre-regex=/opt/local' '--with-libxml-dir=/opt/local' '--with-zlib=/opt/local' '--without-pear' '--disable-cgi' '--enable-cli' '--enable-fileinfo' '--enable-phar' '--disable-fpm' '--with-libedit=/opt/local' 'CC=/usr/bin/clang' 'CFLAGS=-pipe '-Os' '-arch' 'LDFLAGS=-L/opt/local/lib '-Wl,-headerpad_max_install_names' '-arch' 'CPPFLAGS=-I/opt/local/include' 'CXX=/usr/bin/clang++' 'CXXFLAGS=-pipe '-Os' '-arch' '-stdlib=libc++''

...
```

#### Vérifier la Syntaxe d'un Script PHP

La commande `php -l` (`l` pour `Lint`) affiche si oui ou non le fichier PHP
passé en paramètre contient une erreur de syntaxe.

```
$ php -l test.php
PHP Parse error:  syntax error, unexpected ''Hello World' (T_ENCAPSED_AND_WHITESPACE) in test.php on line 3

Parse error: syntax error, unexpected ''Hello World' (T_ENCAPSED_AND_WHITESPACE) in test.php on line 3

Errors parsing test.php
```

#### Démarrer le Serveur Web Natif de PHP

Depuis PHP 5.4, la commande `php -S` démarre le serveur natif de PHP. Cette
commande prend en paramètre le nom de domaine et le numéro de port sur lequel le
serveur web natif écoute. Une option facultative `-t` permet aussi de définir le
dossier qui sert de racine du serveur web (*Document Root*).

```
$ php -S localhost:8000 -t web/
PHP 5.6.17 Development Server started at Mon Jan 11 19:39:36 2016
Listening on http://localhost:8000
Document root is /Volumes/Development/Sites/LPDIM2016/web
Press Ctrl-C to quit.
```

#### Obtenir de la Documentation

Quand une connexion Internet n'est pas à portée pour consulter la documentation
officielle sur le site `php.net`, la commande `php` suivie d'une option `--rf`,
`--rc`, `--re`, `--rz` ou `--ri` retourne le manuel d'utilisation d'une
structure du langage.

```
$ php -h
Usage: php [options] [-f] <file> [--] [args...]
   php [options] -r <code> [--] [args...]
   php [options] [-B <begin_code>] -R <code> [-E <end_code>] [--] [args...]
   php [options] [-B <begin_code>] -F <file> [-E <end_code>] [--] [args...]
   php [options] -S <addr>:<port> [-t docroot]
   php [options] -- [args...]
   php [options] -a

...

  --rf <name>      Show information about function <name>.
  --rc <name>      Show information about class <name>.
  --re <name>      Show information about extension <name>.
  --rz <name>      Show information about Zend extension <name>.
  --ri
```

Par exemple, la commande `php --rc SimpleXMLElement` retourne la documentation
technique complète de la classe `SimpleXMLElement`. La commande `php --rf`
affiche quant à elle la documentation technique d'une fonction native du
langage.

```
$ php --rf strtoupper
Function [ <internal:standard> function strtoupper ] {

  - Parameters [1] {
    Parameter #0 [ <required> $str ]
  }
}
```

Installation du projet
----------------------

Commencez par cloner le dépôt de code Github à l'aide de la commande suivante :

    $ git clone https://github.com/hhamon/lpdim2016.git Blog

Puis, exécutez la série de commandes suivantes pour installer le projet :

    $ cd Blog/
    $ cp app/config/settings.yml.dist app/config/settings.yml
    $ php composer.phar install
    $ chmod -R 777 app/cache/ app/logs/

*Attention :* assurez-vous d'avoir le fichier `composer.phar` dans le dossier
`bin/` du projet. Vous pouvez télécharger l'utilitaire `composer.phar` depuis le
site officiel de Composer.

    https://getcomposer.org/

Il ne vous reste alors plus qu'à créer une base de données sur votre serveur
MySQL à l'aide d'un gestionnaire client (ligne de commande, PHPMyAdmin, MySQL
Workbench etc.). Lorsque la base de données est créée, exécutez le fichier
`app/data/db.sql` pour construire les tables et insérer des données d'exemple.
Enfin, n'oubliez pas de configurer les accès à la base de données.

```yaml
# config/settings.yml
parameters:
    database.dsn:      mysql:host=localhost;port=3306;dbname=lp_dim
    database.user:     root
    database.password: ~
    app.environment: dev
    # Switch this setting to false to disable stack traces in error pages.
    app.debug: true
```

Exécution des tests unitaires
-----------------------------

Les tests unitaires sont écrits et exécutés à l'aide de l'outil `PHPUnit`.
PHPUnit est un framework d'automatisation des tests unitaires développé par
Sebastian Bergmann. Il s'agit d'un portage en PHP du très célèbre `JUnit`.
PHPUnit offre toute une palette de fonctionnalités telles que l'exécution des
tests unitaires, la génération des rapports de couverture de code (HTML, Clover,
etc.) ou bien encore la génération de rapports compatibles `JUnit`.

C'est la commande `bin/phpunit.phar` qui suffit à lancer la suite de tests
unitaires.

    $ php bin/phpunit.phar

Cette commande lit automatiquement le fichier de configuration `phpunit.xml` à
la racine du projet afin de savoir où se trouvent les fichiers de tests
unitaires à exécuter. L'exécution de cette commande produit un résultat
similaire à celui ci-dessous :

    PHPUnit 5.0.10 by Sebastian Bergmann and contributors.
    
    ...............................................................  63 / 136 ( 46%)
    ............................................................... 126 / 136 ( 92%)
    ..........                                                      136 / 136 (100%)
    
    Time: 2.52 seconds, Memory: 14.75Mb
    
    OK (136 tests, 330 assertions)

Enfin, il a été vu en cours que PHPUnit est aussi capable de générer des
rapports de couverture de code aux formats HTML et Clover (XML). Il suffit pour
cela d'exécuter la commande `phpunit.phar` avec les options `--coverage-html` et
`--coverage-clover` comme ci-dessous :

    $ php bin/phpunit.phar --coverage-html ./coverage --coverage-clover ./clover.xml

Le dossier `coverage/` ainsi créé à la racine du projet contient un fichier
`index.html` à ouvrir dans un navigateur. Le fichier `clover.xml` peut quant à
lui être utilisé par PHPStorm via le plugin `PHPUnit Code Coverage`.

Composer
--------

Composer est un gestionnaire de dépendances pour les applications PHP modernes.
Il s'agit d'un outil similaire à `apt-get` / `aptitude` pour Ubuntu, `npm` pour
NodeJS ou bien encore `gem` pour Ruby. Composer sert à installer les paquets PHP
tiers dont le projet a besoin pour fonctionner mais aussi de les mettre à jour
quand de nouvelles versions de ces derniers sont disponibles. Par défaut, les
paquets sont installés dans le dossier `vendor/` que Composer crée à la racine
du projet.

    $ tree -L 2 vendor/
    vendor/
    ├── autoload.php
    ├── composer
    │   ├── ClassLoader.php
    │   ├── LICENSE
    │   ├── autoload_classmap.php
    │   ├── autoload_namespaces.php
    │   ├── autoload_psr4.php
    │   ├── autoload_real.php
    │   └── installed.json
    ├── monolog
    │   └── monolog
    ├── psr
    │  └── log
    ├── symfony
    │  └── yaml
    └── twig
        └── twig

Par exemple, comme le montre le snippet ci-dessous, le projet réalisé en cours a
besoin d'un certain nombre d'outils tiers tels que Twig, Symfony et Monolog. Ces
dépendances sont décrites à la section `require` du fichier `composer.json`
situé à la racine du projet.

```php
{
    "name": "hhamon/lpdim2016",
    "description": "Another PHP framework",
    "type": "library",
    "license": "proprietary",
    "authors": [
        {
            "name": "Hugo Hamon",
            "email": "hugo.hamon@sensiolabs.com"
        }
    ],
    "autoload": {
        "psr-4": {
            "": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "Tests\\": "tests/"
        }
    },
    "minimum-stability": "alpha",
    "require": {
        "twig/twig": "^1.23",
        "symfony/yaml": "^3.0",
        "monolog/monolog": "^1.17"
    }
}
```

Chaque paquet possède un nom du type `organisation/paquet` auquel est associée
une expression de version. Le symbole *tilde* - `~` - signifie *à partir de*
mais il exclut cependant les versions majeures. Le symbole `^` est identique à
`~` mais il inclut aussi les versions majeures. Par conséquent, Composer ira
chercher pour chaque paquet la version la plus à jour à partir de celle indiquée
dans le fichier `composer.json`. Il est aussi possible d'exprimer les versions à
l'aide d'une valeur fixe (`3.0.1`), d'un intervalle (`>=2.5-dev,<2.6-dev`),
d'une branche Git (`dev-master`) ou bien encore d'un numéro de commit dans une
branche (`dev-master#ea6f2130b4cab88635d35a848d37b35f1e43d407`).

La commande `composer.phar require` sert à ajouter de nouvelles dépendances au
projet. La commande ci-dessous ajoute le paquet `symfony/finder` au projet et
Composer se charge de le télécharger, l'installer et l'initialiser pour qu'il
soit prêt à l'emploi. À condition bien sûr que ce paquet ne soit pas
incompatible avec une dépendance déjà installée.

    $ php bin/composer.phar require symfony/finder

Pour mettre à jour un paquet ou bien toutes les dépendances, il suffit
d'exécuter la commande `update` de l'utilitaire `composer.phar`. De plus, pour
trouver un paquet, Composer s'appuie sur l'annuaire
[Packagist.org](http://packagist.org) qui référence tous les paquets Open-Source
connus.

    $ # MAJ toutes les dépendances
    $ php bin/composer.phar update
    $
    $ # MAJ que le paquet symfony/yaml
    $ php bin/composer.phar update symfony/yaml

L'installation d'un nouveau paquet ou bien la mise à jour d'une ou des
dépendances entraîne la régénération du fichier `composer.lock`. Ce fichier
contient la liste de toutes les dépendances installées dans le dossier `vendor/`
ainsi que les numéros des versions précisément installées. Il convient donc de
commiter ce fichier dans le dépôt Git car la commande `install` de Composer
installe les dépendances telles qu'elles sont décrites dans le fichier
`composer.lock` sans chercher à les mettre à jour. C'est donc idéal pour
s'assurer que les versions des paquets seront les bonnes en production lors du
déploiement ou bien tout simplement lorsqu'un autre développeur récupère le
code du dépôt.

Lorsque toutes les dépendances sont installées, les classes des paquets sont
directement instanciables dans le code sans avoir besoin de les inclure au
préalable avec les instructions `require` ou `include` de PHP. Composer vient
avec des mécanismes d'autochargement des classes pour chaque paquet installé. La
section `autoload` du fichier `composer.json` décrit quant à elle les mécanismes
d'autochargement des classes du projet.

```json
{
    "autoload": {
        "psr-4": {
            "Acme\\": "acme/",
            "": "src/"
        }
    }
}
```

La description ci-dessus signifie que les classes du projet à autocharger
doivent tout d'abord respecter la convention `PSR-4` établie par le
[PHP-FIG](http://www.php-fig.org/). Les classes dont l'espace de nommage débute
par la chaîne `Acme\` sont à trouver dans un dossier `acme/` tandis que toutes
les autres classes seront autochargées depuis le dossier `src/`.

    $ php bin/composer.phar dumpautoload --optimize

La commande `dumpautoload` de l'utilitaire `composer.phar` régénère les fichiers
d'autochargement si nécessaire. Elle s'accompagne aussi d'une option
`--optimize` qui optimise le processus d'autochargement. Cette option est
particulièrement utile sur un serveur de production.

PSR & FIG
---------

L'acronyme *PSR* signifie *PHP Standard Recommendation* ou littéralement
*Recommandation Standard PHP* en français. Une *PSR* est en fait une règle
standard et universelle de codage définie par le *PHP FIG* afin que les
frameworks et bibliothèques de code modernes soient interopérables. Il existe
aujourd'hui plusieurs standards *PSR* édités par le *FIG*.

*FIG* est l'acronyme pour *Framework Interoperability Group*. Il s'agit d'un
groupe de développeurs PHP fondé à la fin de l'année 2009 à la suite de la
conférence annuelle ZendCon aux USA. Les principaux développeurs des frameworks
et bibliothèques PHP tels que Symfony, Zend, CakePHP, Doctrine, Twig, Drupal,
etc. qui étaient présents à cette conférence ont eu l'idée de se rassembler et
mettre en commun leur savoir pour définir des règles d'interopérabilité du code.
L'objectif était ainsi de pouvoir plus facilement utiliser le code d'un
framework dans un autre sans trop d'efforts.

Depuis le début de l'année 2010, plusieurs règles *PSR* ont vu le jour pour
standardiser entre autres les mécanismes d'autochargement des classes ou bien
les styles et conventions de codage. Le tableau ci-dessous dresse la liste des
PSRs actuellement validés ou bien en cours de travail.

| PSR    | Statut   | Explications
|--------|----------|------------------------------------------------------------------------------------------|
| PSR-0  | Obsolète | Mécanismes standards d'autochargement des classes.                                       |
| PSR-1  | Acceptée | Conventions et styles de codage obligatoires.                                            |
| PSR-2  | Acceptée | Conventions et styles de codage facultatifs.                                             |
| PSR-3  | Acceptée | Interface commune pour développer des implémentations d'objet « logger ».                |
| PSR-4  | Acceptée | Amélioration de PSR-0 et dépréciation des classes préfixées.                             |
| PSR-5  | En cours | Standards de documentation API du code pour les outils comme phpDocumentor.              |
| PSR-6  | Acceptée | Interface commune pour développer des bibliothèques d'adatateurs d'outils de cache.      |
| PSR-7  | Acceptée | Interface commune pour modéliser des messages HTTP (requêtes et réponses).               |
| PSR-8  | En cours | (Easter Egg) Interface commune pour envoyer des câlins (hugs) à la communauté PHP.       |
| PSR-9  | En cours | Format standard pour reporter des failles de sécurité dans un logiciel.                  |
| PSR-10 | En cours | Processus de communication standard des failles de sécurité d'un logiciel.               |
| PSR-11 | En cours | Interface commune pour définir des conteneurs d'injection de dépendances interopérables. |
| PSR-12 | En cours | Évolution de PSR-2 qui prend en compte les nouvelles règles syntaxiques de PHP 7.        |

Cette liste est disponible sur le site du [PHP-FIG](http://www.php-fig.org/).

Conception du Framework
-----------------------

Les sections qui suivent décrivent les différents composants bas niveau qui font
office de fondations du framework.

### Le Composant `Framework\Http`

Le coeur du framework repose sur un noyau léger (« *micro-kernel* ») responsable
de transformer un objet de requête (classe `Framework\Http\Request`) en objet de
réponse (classe `Framework\Http\Response`). Ces deux objets modélisent les
messages du protocole HTTP sous forme orientée objet pour rendre le code plus
naturel et intuitif à utiliser que les APIs natives du langage PHP (variables
superglobales, fonction `header()`, instruction `echo`, etc.).

Une requête HTTP prend la forme suivante :

```
POST /movie HTTPS/1.1
host: www.allocinoche.com
user-agent: Firefox/33.0
content-type: application/json
content-length: 61

{ 'movie': 'Intersté Lard', 'director': 'Christopher Mulan' }
```

Une réponse HTTP à la requête précédente aura par exemple la forme ci-dessous :

```
HTTPS/1.1 201 Created
content-type: application/json
content-length: 74
host: www.allocinoche.com
location: /movie/12345/interste-lard.html

{ 'id': 12345, 'movie': 'Intersté Lard', 'director': 'Christopher Mulan' }
```

Dans le prologue de la réponse, le code à trois chiffres s'intitule le
*code de statut* et détermine le statut de la réponse. Les codes de statut sont
classés en cinq catégories :

| Intervalle | Description        |
|------------|--------------------|
| 100 à 199  | Information        |
| 200 à 299  | Succès             |
| 300 à 399  | Redirections       |
| 400 à 499  | Erreurs du client  |
| 500 à 599  | Erreurs du serveur |

Les codes de statut les plus courants et qui sont à connaître en priorité sont
les suivants :

| Code   | Raison                          | Signification                                                               |
|--------|---------------------------------|-----------------------------------------------------------------------------|
| `200`  | `OK`                            | Tout s'est bien passé                                                       |
| `201`  | `Created`                       | La ressource vient d'être créée sur le seveur                               |
| `204`  | `No Content`                    | Aucun contenu, utilisé pour accuser une suppression réussie de la ressource |
| `301`  | `Moved Permanently`             | Redirection permanente vers la ressource                                    |
| `302`  | `Moved Temporarily`             | Redirection temporaire vers la ressource                                    |
| `304`  | `Not Modified`                  | La ressource est la même sur le client et le serveur                        |
| `400`  | `Bad Request`                   | Le client a formulé une mauvaise requête                                    |
| `401`  | `Unauthorized`                  | Accès refusé si l'utilisateur n'est pas authentifié                         |
| `402`  | `Payment Required`              | Un paiement est nécessaire pour continuer                                   |
| `403`  | `Forbidden`                     | Accès refusé faute de droits suffisants                                     |
| `404`  | `Not Found`                     | La ressource n'existe pas ou n'existe plus                                  |
| `405`  | `Method Not Allowed`            | La méthode HTTP utilisée n'est pas autorisée                                |
| `406`  | `Not Acceptable`                | La requête n'est pas acceptable par le serveur                              |
| `451`  | `Unavailable For Legal Reasons` | La ressource est indisponible pour des raisons de censure                   |
| `500`  | `Internal Server Error`         | Une erreur interne est survenue sur le serveur                              |
| `502`  | `Bad Gateway`                   | Échec de transmission de la requête à l'interface PHP / Python...           |
| `503`  | `Service Unavailable`           | Le service est indisponible, serveur surchargé                              |

La classe `Framework\Http\Request` modélise une requête HTTP standard composée
d'un prologue, d'une liste d'en-têtes et d'un corps facultatif. L'objet de
requête peut être construit directement à partir de son constructeur ou bien à
partir des variables superglobales de PHP ou d'un message HTTP complet.

```php
use Framework\Http\Request

$request = new Request('POST', '/movie', 'HTTPS', '1.1');
$request = Request::createFromGlobals();
$request = Request::createFromMessage("POST /movie HTTPS/1.1\n...");
```

L'objet de requête possède une série de méthodes publiques pour récupérer de
manière atomique des informations telles que l'URL, les données passées en POST,
les en-têtes ou bien encore le corps. Le listing de code ci-dessous décrit une
partie de l'API implémentée par la classe `Framework\Http\Request`.

```php
# Static methods
Request::registerSupportedVerbs($verbs);
Request::createFromGlobals();
Request::createFromMessage($httpMessage);

# Instance Getter methods
$request->getScheme();
$request->getVersion();
$request->getHeaders();
$request->getHeader($name);
$request->getPath();
$request->getMethod();
$request->isMethod($method);
$request->getBody();
$request->getMessage();
$request->getAttribute($name, $default = null);
$request->getPostParameter();

# Instance Setter methods
$request->setAttributes($attributes);
```

La classe `Framework\Http\Response` modélise une réponse HTTP standard composée
d'un prologue, d'une liste d'en-têtes et d'un corps facultatif. L'objet de
réponse peut être construit directement à partir de son constructeur ou bien à
partir d'un objet de requête ou d'un message HTTP complet.

```php
use Framework\Http\Response

$response = new Response(200, 'HTTP', '1.0', [ 'Content-Type' => 'text/html' ], '<html>...');
$response = Response::createFromRequest($request);
$response = Response::createFromMessage('HTTP/1.1 304 Not Modified');
```

L'objet de réponse possède une série de méthodes publiques pour récupérer de
manière atomique des informations telles que le code de statut, les en-têtes ou
bien le corps. Le snippet ci-dessous présente l'API implémentée par la classe
`Framework\Http\Response`.

```php
# Static methods
Response::createFromRequest($request);
Response::createFromMessage($httpMessage);

# Instance Getter methods
$response->getScheme();
$response->getVersion();
$response->getHeaders();
$response->getHeader($name);
$response->getBody();
$response->getMessage();
$response->getStatusCode();
$response->getReasonPhrase();

# Instance utility methods
$response->prepare($request);
$response->send();
```

La classe `Framework\Http\RedirectResponse` modélise quant à elle une réponse de
redirection. Son constructeur reçoit l'URL vers laquelle le client doit être
redirigé. Le code de statut est fixé à `302` par défaut mais peut être n'importe
lequel dans l'intervalle `300` à `308`. L'objet de redirection force l'url dans
une en-tête HTTP de type `Location`.

Enfin, toutes les classes de requêtes et de réponses dérivent du même super-type
abstrait `Framework\Http\AbstractMessage`. Cette classe *abstraite* n'est pas
instanciable et solutionne trois problématiques : 

1. Définir un super-type commun,
2. Factoriser des attributs et méthodes communs aux classes dérivées,
3. Éviter la duplication de code dans les classes dérivées.

La classe `AbstractMessage` définit aussi la méthode `getMessage()` comme finale
à l'aide du mot-clé `final`. Ce mot-clé empêche les classes dérivées de
redéfinir cette méthode et d'en changer son comportement. Ainsi, le framework
est garanti de toujours obtenir un message HTTP composé d'en-têtes et d'un
corps.

De plus, la méthode `getMessage()` est une implémentation du
*patron de conception* (aka *Design Pattern*) « **Patron de Méthode** »
(aka *Template Method*) qui permet d'empêcher la redéfinition complète d'un
algorithme mais autorise néanmoins la redéfinition de certaines parties de
celui-ci au travers de méthodes protégées judicieusement choisies. Le mot-clé
`final` sur la signature de la méthode interdit la redéfinition de celle-ci dans
les classes dérivées. Cependant, la méthode `getMessage()` autorise la
redéfinition d'une étape de l'algorithme complet grâce à la méthode protégée
`createPrologue()`. Cette dernière est d'ailleurs déclarée abstraite (mot-clé
`abstract`) pour obliger les classes dérivées à l'implémenter.

**Important** : il suffit qu'il y ait seulement une seule méthode déclarée
abstraite dans une classe pour que celle-ci doive aussi être déclarée abstraite.

Pour plus d'informations sur l'API du composant `Http`, consultez les tests
unitaires.

### Le Composant `Framework\Routing`

L'espace de nommage `Framework\Routing` offre des classes pour router les URLs
HTTP sur des contrôleurs de l'application. La configuration des routes est
définie à l'aide d'objets de type `Route` et `RouteCollection`. Chaque objet de
route stocke un tableau associatif de paramètres qui renferme entre autres la
classe de contrôleur à instancier à la clé `_controller`. Chaque classe de
contrôleur doit implémenter la méthode magique `__invoke()` afin d'être
*invocable* par le framework. De plus, chaque route encapsule son chemin, ses
paramètres associés, les contraintes des variables de l'URL ainsi que les
contraintes sur les méthodes HTTP autorisées.

```php
use Framework\Routing\Route;

$route = new Route(
    '/hello/{name}',
    'Application\\Controller\\HelloWorldAction',
    ['GET', 'POST'],
    ['name' => '[a-z]+']
);
```

La liste des routes est définie par un objet de type
`Framework\Routing\RouteCollection`. Chaque objet de route peut être ajouté à la
collection grâce à sa méthode `add()`. L'objet de collection de routes est aussi
une implémentation du patron de conception *Itérateur* (*Iterator*). Grâce à
l'implémentation des méthodes de l'interface native `Iterator` de PHP, il est
possible d'utiliser la collection comme un argument de la structure `foreach`.

```php
use Framework\Routing\Route;
use Framework\Routing\RouteCollection;

$routes = new RouteCollection();
$routes->add('home', new Route('/', 'Application\\Controller\\HomepageAction', ['GET']));
$routes->add('hello', new Route('/', 'Application\\Controller\\HelloAction', ['GET']));
$routes->add('contact', new Route('/', 'Application\\Controller\\ContactAction', ['GET', 'POST']));

foreach ($routes as $name => $route) {
    echo $route->getPath();
}
```

La classe `RouteCollection` vient aussi avec des méthodes `match()`, `merge()`
et `getName()`. La première permet de retourner l'objet de route qui correspond
aux paramètres de requête reçus en argument. La méthode `merge()` prend une
instance de `RouteCollection` à fusionner dans l'instance courante. La méthode
récupère les routes de la collection passée en argument et les ajoute à l'objet
courant. Enfin, la méthode `getName()` retourne le nom de la route pour l'objet
de route passé en argument.

La méthode `getRoutes()` de la collection de routes retourne un tableau
associatif d'objets de type `Framework\Routing\Route`. La clé associative dans
le tableau retourné correspond au nom donné à la route tandis que la valeur
correspond à l'objet `Route`. Cette méthode est appelée par le routeur qui
conserve une référence à la collection de routes.

Le routeur de type `Framework\Routing\Router` connaît la liste de toutes les
routes grâce à la collection de routes, et se charge de faire correspondre l'URL
de la requête à une route. La méthode `match()` du routeur reçoit un objet
`RequestContext` duquel il extrait l'URL demandée par le client et la méthode
HTTP utilisée. Si l'URL et la méthode HTTP correspondent à une route, alors le
routeur retourne simplement un tableau associatif des attributs de la route
(son nom, la classe de contrôleur associée et les paramètres de la route). À
l'inverse, si l'URL correspond à aucune des routes enregistrées, le routeur lève
une exception de type `Framework\Routing\RouteNotFoundException`.

Le composant `Router` fournit des stratégies d'import de fichiers de
configuration des routes. Les routes peuvent être aussi bien définies
directement dans un fichier PHP pur comme dans un format statique tel que le
XML. La classe `XmlFileLoader` est responsable de lire un fichier XML de
définition des routes et d'importer celles-ci dans le routeur. La classe
`CompositeFileLoader` est une stratégie composite qui permet de charger un
fichier de définition des routes quelle que soit son extension (php ou xml).

Pour plus d'informations sur l'API du composant `Routing`, consultez les tests
unitaires.

### Le Composant `Kernel`

La classe `Kernel` constitue le coeur du « micro-framework » développé en TP.
C'est lui qui est responsable de traduire un objet de requête en un objet de
réponse. Pour ce faire, il utilise la méthode `handle()` de l'interface
`Framework\KernelInterface` et s'appuie sur son registre de services
(*Service Locator*) passé à son constructeur. Cette méthode délègue ensuite à un
service `http_kernel` de type `Framework\HttpKernel` qui lui aussi implémente
l'interface `Framework\KernelInterface`. L'objet `Kernel` agit donc comme une
implémentation du patron *Proxy* en décorant l'objet `HttpKernel`.

La méthode `handle()` de l'objet `HttpKernel` propage des événements de
pré-traitement et de post-traitement de la requête grâce au service
`event_manager` de type `Framework\EventManager\EventManager`. Ces événements
rendent possible l'extension du noyau en certains points stratégiques du
traitement de la requête sans changer l'algorithme de la méthode `handle()`.
Celle-ci est d'ailleurs déclarée finale. Le fait d'utiliser un gestionnaire
d'événements dans cette méthode finale pour étendre dynamiquement le noyau est
une autre implémentation du patron de conception *Patron/Gabarit de Méthode*.

**Note :** le composant `EventManager` est une implémentation du patron de
conception *Médiateur* (aka *Mediator*), lui même inspiré du patron de
conception *Observateur* (aka *Observer*). Le Médiateur est un patron de
conception comportemental qui favorise le découplage entre des objets qui
doivent communiquer ensemble.

La méthode privée `doHandleRequest()` de la classe `Framework\HttpKernel` tente
de convertir l'objet de requête en objet de réponse en invoquant un contrôleur.
Avant de parvenir au contrôleur, le noyau doit d'abord demander au service
routeur si l'URL de la requête correspond à une route enregistrée. Cette
opération est réalisée par l'objet `Framework\RouterListener` qui écoute le tout
premier événement propagé `Framework\KernelEvents::REQUEST`. Si le routeur
parvient à faire correspondre la requête du client avec une route, alors les
paramètres de celle-ci sont stockés dans la requête en tant qu'attributs pour
un usage ultérieur. Si aucune route ne correspond à la requête, le routeur émet
alors une exception qui est instantanément capturée et transformée en objet
réponse par le noyau HTTP.

Puis, le noyau HTTP récupère depuis la fabrique de contrôleurs, le contrôleur à
invoquer. La fabrique le contrôleurs construit le contrôleur à partir du tableau
d'attributs de la requête. Si celui-ci ne contient pas suffisamment d'éléments
pour construire le contrôleur, la fabrique lève alors une exception et
interrompt le flux d'exécution du code.

Ensuite, le noyau HTTP propage un événement `Framework\KernelEvents::CONTROLLER`
qui permet aux écouteurs de celui-ci de modifier le contrôleur juste avant qu'il
soit invoqué.

Il ne reste alors au noyau HTTP plus qu'à invoquer dynamiquement le contrôleur à
l'aide de la fonction native PHP `call_user_func_array()`. Le contrôleur reçoit
de la part du noyau la référence à la requête et retourne à ce dernier un objet
de réponse. Si aucune réponse n'est retournée, le noyau HTTP lève une exception.

Lorsque l'objet de réponse est retourné, le noyau HTTP propage un événement
`Framework\KernelEvents::RESPONSE`. N'importe lequel des écouteurs qui écoutent
cet événement peut modifier la réponse générée.

Le noyau de l'application est utilisé comme une implémentation du patron
d'architecture *Contrôleur Frontal* (aka *Front Controller*). Il s'agit du point
d'entrée unique sur l'application. C'est le fichier `web/index.php` qui démarre
et utilise le noyau.

```php
# web/index.php
require_once __DIR__.'/../bootstrap.php';

use Framework\Http\Request;
use Framework\Http\StreamableInterface;
use Framework\Kernel;

$serviceLocator = require __DIR__.'/../bootstrap.php';

$kernel = new Kernel($serviceLocator);
$response = $kernel->handle(Request::createFromGlobals());

if ($response instanceof StreamableInterface) {
    $response->send();
}
```

Si une exception est levée au cours du flux du traitement de la requête, le
noyau HTTP l'attrape et propage un événement `Framework\KernelEvents::EXCEPTION`
pour la traiter. Les écouteurs de cet événement peuvent enregistrer l'exception
dans un journal d'erreurs puis générer une vue adaptée à retourner au client. La
classe `Application\ErrorHandler` génère les pages d'erreurs appropriées pour
l'utilisateur final en fonction du type de l'exception à traiter. La classe
`LoggerHandler` se charge d'enregistrer le message de l'exception dans un
journal d'erreurs grâce à une implémentation de « *logger* » de la bibliothèque
Open-Source Monolog.

### Le Composant `Templating`

Le composant `Templating` fournit deux interfaces
(`Framework\Templating\RendererInterface` et `Framework\Templating\ResponseRendererInterface`)
pour développer des moteurs de rendu. Par défaut, le composant vient avec une
implémentation basique de moteur de rendu. Il s'agit de la classe
`Framework\Templating\PhpRenderer`. Cet objet implémente l'interface
`Framework\Templating\ResponseRendererInterface` et offre un
mécanisme simple pour évaluer des gabarits PHP purs. La seconde implémentation,
la classe `Framework\Templating\BracketRenderer` remplace des variables du
template délimitées par des doubles crochets : `[[name]]`.

```php
use Framework\Template\BracketRenderer

$engine = new BracketRenderer(__DIR__.'/app/views');

$output = $engine->render('movie/show.tpl', [
    'title' => 'Jurassic Pork',
    'director' => 'Steven Spielbergue',
]);
```

Le composant `Templating` est aussi livré avec une classe
`Framework\Templating\TwigRendererAdapter` qui adapte le moteur de rendu
**Twig** dans le framework. En effet, l'API attendue par le framework est
différente de celle qu'expose Twig. L'interface
`Framework\Templating\ResponseRendererInterface` et la classe concrète
`Twig_Environment` sont incompatibles par nature, ce qui empêche d'utiliser Twig
comme moteur de rendu dans le framework.

Pour répondre à cette problématique sans trop d'efforts, le framework embarque
un *adaptateur* qui rend compatible l'API publique de l'objet `Twig_Environment`
avec l'interface `Framework\Templating\ResponseRendererInterface` attendue par
le framework. Le patron de conception *Adaptateur* (aka *Adapter*) implémenté
par la classe `Framework\Templating\TwigRendererAdapter` favorise la
*composition d'objets* plutôt que *l'héritage* pour résoudre ce problème.

```php
use Framework\Templating\TwigRendererAdapter;

$twig = new \Twig_Environment(new \Twig_Loader_Filesystem(__DIR__.'/app/views'), [
    'debug'            => true,
    'auto_reload'      => true,
    'strict_variables' => true,
    'cache'            => __DIR__.'/cache/twig',
]);

$engine = new TwigRendererAdapter($twig);

$output = $engine->renderResponse('movie/show.twig', [
    'title' => 'Jurassic Pork',
    'director' => 'Steven Spielbergue',
]);
```

Pour plus d'informations concernant le composant `Templating`, consultez les
fichiers de tests unitaires.

### Le Composant `ServiceLocator`

Le composant `ServiceLocator` offre une implémentation pragmatique de
*registre de services* pour fabriquer des services à la demande, et ainsi
optimiser les performances de l'application. Seuls les classes des objets
réellement nécessaires pour l'action demandée seront instanciées. Les services
sont ensuite partagés par le registre pour ne pas les récréer inutilement s'ils
sont demandés à nouveau par le code client.

La classe `Framework\ServiceLocator\ServiceLocator` est une implémentation
basique d'un registre de services. L'objet `ServiceLocator` permet non seulement
d'enregistrer des définitions de services à l'aide de fonctions anonymes
(lambdas et closures) mais aussi de stocker des paramètres de configuration. Ces
derniers servent généralement à paramétrer les services enregistrés.

```php
use Application\Repository\BlogPostRepository;
use Framework\ServiceLocator\ServiceLocator;

$locator->setParameter('database.dsn', 'mysql:host=localhost;dbname=lp_dim_2016');
$locator->setParameter('database.user', 'root');
$locator->setParameter('database.password', '');
$locator->setParameter('database.options', [
    \PDO::ATTR_AUTOCOMMIT => false,
    \PDO::ATTR_ERRMODE => \PDO::ERRMODE_EXCEPTION,
    \PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES 'UTF8'",
);

$locator->register('repository.blog_post', function (ServiceLocator $locator) {
    return new BlogPostRepository($locator->getService('database'));
});

$locator->registerService('database', function (ServiceLocator $locator) {
    return new \PDO(
        $locator->getParameter('database.dsn'),
        $locator->getParameter('database.user'),
        $locator->getParameter('database.password'),
        $locator->getParameter('database.config')
    );
});

$post = $locator->getService('repository.blog_post')->find(42);
```

> L'utilisation de fonctions anonymes (*Lambda* ou *Closures*) permet de
> retarder jusqu'au tout dernier moment l'exécution du code qui fabrique le
> service. C'est le principe du « *lazy loading* » ou exécution retardée.

L'objet `ServiceLocator` est instancié et configuré depuis le contrôleur frontal
`web/index.php`. Les registres de services sont aussi connus sous les
appellations de *conteneurs de services*, *conteneurs d'injection de dépendance*
ou bien encore *conteneurs d'inversion de contrôle*.

Pour en savoir plus sur le composant `ServiceLocator`, consultez les tests
unitaires et le *Contrôleur Frontal* (le fichier `web/index.php`).

### Le Composant `EventManager`

Le composant `EventManager` fournit une implémentation du patron de conception
*Médiateur*, lui même inspiré du patron *Observateur*. Son objectif consiste à
découpler des objets qui possèdent de nombreuses connexions avec d'autres. La
classe `Framework\EventManager\EventManager` joue ce rôle de médiateur.

Les écouteurs configurés sont d'abord enregistrés sur le médiateur. Un écouteur
n'est ni plus ni moins qu'une structure invocable en PHP :

* une chaîne de caractères contenant le nom d'une fonction PHP,
* un objet qui implémente la méthode magique `__invoke()`,
* un tableau contenant un objet et le nom d'une méthode publique à appeler sur
  celui-ci,
* un tableau contenant le nom d'une classe et le nom d'une méthode statique
  publique de cette classe,
* une fonction anonyme (`Closure`).

Chaque écouteur écoute un événement spécifique. Plusieurs écouteurs peuvent être
attachés sur le même événement et déclenchés avec des priorités différentes.

```php
use Framework\EventManager\EventManager;
use Framework\ExceptionEvent;

$eventManager = new EventManager();
$eventManager->addEventListener('kernel.exception', function (ExceptionEvent $event) {
    $exception = $event->getException();
    $request = $event->getRequest();
    // ...
});
$eventManager->addEventListener(
    'kernel.exception',
    function (ExceptionEvent $event) {
        // ...
    },
    100 // Priorité
});
```

Chaque écouteur écoute un événement, ici `kernel.exception`. Lorsqu'il sera
déclenché plus tard, l'écouteur recevra en argument un objet d'événement qui
encapsule certaines informations utiles pour l'écouteur. Dans cet exemple,
l'événement `kernel.exception` est en fait un objet de type `ExceptionEvent` qui
encapsule des objets tels que la requête et l'exception à gérer.

Lorsque tous les écouteurs sont enregistrés sur le gestionnaire d'événements, ce
dernier peut alors propager des événements depuis le code client qui le
référence. Cela permet ainsi de découpler l'objet client de ses nombreux
écouteurs.

```php
class HttpKernel implements KernelInterface
{
    private $dispatcher;

    final public function handle(Request $request)
    {
        try {
            $response = $this->doHandleRequest($request);
        } catch (\Exception $exception) {
            $event = $this->dispatcher->dispatch(KernelEvents::EXCEPTION, new ExceptionEvent($exception, $request));
            if (!$event->hasResponse()) {
                throw new \RuntimeException('No exception handler has generated a Response', 0, $exception);
            }
    
            $response = $event->getResponse();
        }
  
        return $this->doHandleResponse($request, $response);
    }
}
```

À l'appel de la fonction `dispatch()`, le répartiteur d'événements invoque
successivement dans l'ordre de priorité les écouteurs de l'événement et transmet
à chacun l'objet `$event`. Si un écouteur est capable de fixer un objet
`Response` dans l'objet `ExceptionEvent` alors la propagation de l'événement est
immédiatement stoppée et les écouteurs enregistrés restants ne sont pas appelés.

```php
namespace Framework\EventManager;

class EventManager implements EventManagerInterface
{
    public function dispatch($name, Event $event)
    {
        if (!$this->hasListeners($name)) {
            return $event;
        }

        foreach ($this->getSortedListeners($name) as $listener) {
            call_user_func_array($listener, [ $event ]);
            if ($event->isStopped()) {
                // Stop looping if a listener
                // has stopped the event propagation.
                break;
            }
        }

        return $event;
    }

    // ...
}
```

Dans l'application de blog, des instances des classes `ErrorHandler` et
`LoggerHandler` de l'espace de nommage `Application` sont enregistrés
comme écouteurs de l'événement `kernel.exception`. Le répartiteur d'événements
est quant à lui enregistré comme service dans le registre de services.

### Le Composant `Session`

Le composant `Session` fournit une fine couche d'abstraction par dessus le
mécanisme de session de base de PHP. Grâce à cette abstraction, il est désormais
plus facile d'adapter le stockage des données de la session utilisateur dans
différentes technologies : système de fichier, Redis, Memcached, APC, etc.

La classe `Framework\Session\Session` agit comme une sorte de façade et offre
une API simplifiée pour manipuler la session. Cette classe implémente le contrat
de l'interface `Framework\Session\SessionInterface` qui définit des méthodes
publiques pour démarrer la session, la détruire, stocker des valeurs à
l'intérieur ou bien récupérer ces dernières.

```php
namespace Framework\Session;

interface SessionInterface
{
    /**
     * Starts the session.
     *
     * @return bool True if the session is already started, false otherwise
     */
    public function start();

    /**
     * Destroys the session.
     *
     * @return bool True if the session was correctly destroyed, false otherwise.
     */
    public function destroy();

    /**
     * Stores new data into the session.
     *
     * @param string $key   The session variable name
     * @param mixed  $value The session variable value (must be serializable at some point)
     *
     * @return bool True if succeeded, false otherwise.
     */
    public function store($key, $value);

    /**
     * Fetches a data from the session.
     *
     * @param string $key     The session variable name
     * @param mixed  $default The default value to return
     *
     * @return mixed|null
     */
    public function fetch($key, $default = null);

    /**
     * Returns the session's unique identifier.
     *
     * @return string
     */
    public function getId();

    /**
     * Saves the session data to the persistent storage.
     *
     * @return bool True if successful, false otherwise
     */
    public function save();
}
```
Pour implémenter complètement ce contrat, la classe `Session` dépend d'un
adaptateur de système de stockage. Il s'agit d'un objet qui répond au contrat de
l'interface `Framework\Session\Driver\DriverInterface`. Cette définit les
méthodes publiques qui permettent de manipuler le système de stockage des
données et offrir une abstraction des fonctions natives de PHP de manipulation
des fichiers, de bases de données ou bien de cache clé/valeur comme Redis, APCu
ou Memcached.

Le composant vient avec deux implémentations par défaut de cette interface. La
première est la classe `ArrayDriver`. Son usage est uniquement réservé aux tests
unitaires puisque cet objet simule une session non persistente dans un tableau
de données. C'est la raison pour laquelle sa documentation d'API utilise le
marqueur `@internal` pour signifier son usage strictement interne. La seconde
implémentation de la `DriverInterface` est la classe `NativeDriver`. Il s'agit
d'une classe qui encapsule les appels au système natif de gestion des sessions
de PHP.

> Dans l'application du blog suivant le principe MVC, la classe `Session` est
> définie dans le registre de services comme un service configuré avec son
> adaptateur natif.

Application de Blog suivant le patron MVC
-----------------------------------------

Les sections suivantes décrivent les choix techniques implémentés pour réaliser
une application de blogging construite autour du patron MVC. Les trois couches
du modèle MVC sont détaillées ci-dessous.

### La Couche Contrôleur

La couche *Contrôleur* encapsule la logique applicative. Elle est responsable
d'analyser la requête de l'utilisateur afin d'en déduire le *Modèle* à
interroger et la *Vue* à rendre. Il s'agit donc d'une simple colle entre les
couches de *Modèle* et de *Vue*. C'est aussi la glue entre le code de
l'application et le code du framework (l'infrastructure). Par conséquent, un
*Contrôleur* ne contient que très peu de lignes de code (pas plus de 20 lignes
de code en général).

Dans l'application de blogging, les *Contrôleurs* sont des objets dont les
classes se trouvent dans l'espace de nommage `Application\Controller`. Chaque
classe de contrôleur représente une seule et unique fonctionnalité de
l'application (afficher tous les billets, créer un billet, consulter un billet
etc.). Tous ces objets ont la particularité d'implémenter la méthode magique
`__invoke()` qui permet de les utiliser comme s'ils étaient des fonctions PHP
classiques.

```php
namespace Application\Controller\Blog;

use Framework\AbstractAction;
use Framework\Http\HttpNotFoundException;
use Framework\Http\Request;

class GetPostAction extends AbstractAction
{
    public function __invoke(Request $request)
    {
        $repository = $this->getService('repository.blog_post');
  
        $id = $request->getAttribute('id');
        if (!$post = $repository->find($id)) {
            throw new HttpNotFoundException(sprintf('No blog post found for id #%u.', $id));
        }
  
        return $this->render('blog/show.twig', ['post' => $post]);
    }
}
```

Dans l'exemple ci-dessus, la méthode `__invoke()` de la classe
`Application\Controller\Blog\GetPostAction` a pour tâche de retrouver un billet
du blog par son identifiant unique. Pour ce faire, l'action analyse d'abord la
requête afin de récupérer l'identifiant unique du billet dans ses attributs.
L'attribut `id` est en fait un paramètre dynamique de la route `blog_post`.Ce
paramètre doit forcément être un ou plusieurs chiffres qui se suivent comme le
décrit le marqueur `<requirement/>` du fichier de configuration des routes
(`app/config/routes.xml`).

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<routes>
    <route name="blog_post" path="/blog/article-{id}.html" methods="GET">
        <param key="_controller">Application\Controller\Blog\GetPostAction</param>
        <requirement key="id">\d+</requirement>
    </route>
</routes>
```

Le *Contrôleur* choisit ensuite le *Modèle* à interroger. Il s'agit ici du
service *repository.blog_post* qui encapsule les appels à la base de données. Ce
dernier est capable de récupérer un billet par son identifiant unique grâce à sa
méthode `find()`. Si aucun billet publié correspondant à l'identifiant unique
n'a été trouvé en base de données, l'action lève alors une exception de type
`Framework\Http\HttpNotFoundException`.

En remontant jusqu'au noyau, cette exception sera ensuite convertie en réponse
HTTP de type `404`. Enfin, si le billet a bien été récupéré, le *Contrôleur*
choisit alors de rendre la *Vue* adaptée. Dans cet exemple, la *Vue* est
modélisée par le gabarit Twig `blog/show.twig` qui reçoit le billet dans une
variable `post`. La méthode `render()` de la superclasse abstraite
`Framework\Controller\AbstractController` évalue le gabarit avec ses variables,
puis construit un objet de type `Framework\Http\Response` qui encapsule le
résultat final.

### La Couche Modèle

La couche *Modèle* est la plus importante des trois dans l'architecture MVC
puisqu'elle encapsule toute la *logique métier* de l'application. La logique
métier comprend aussi bien l'interfaçage avec un système de données (une base de
données MySQL par exemple) que les objets en charge de manipuler les données du
système. Ainsi tout objet qui encapsule une logique d'accès, de manipulation ou
de validation des données fait partie du *Modèle*.

Dans l'application de blogging, la couche de *Modèle* est représentée par un
objet `Application\Repository\BlogPostRepository`. Un *entrepôt* (aka
*repository*) est un objet qui représente toute une collection de données et qui
expose des fonctions pour accéder à ces dernières. L'objet `BlogPostRepository`
fait ainsi la passerelle avec la table `blog_post` dans la base de données
relationnelle MySQL.

```php
namespace Application\Repository;

class BlogPostRepository
{
    /**
     * The database handler.
     *
     * @var \PDO
     */
    private $dbh;

    /**
     * Constructor.
     *
     * @param \PDO $database
     */
    public function __construct(\PDO $database)
    {
        $this->dbh = $dbh;
    }
    
    /**
     * Returns one blog post by its unique identifier.
     *
     * @param int|string $id The unique identifier
     *
     * @return array
     */
    public function find($id)
    {
        if (!ctype_digit($id)) {
            throw new \InvalidArgumentException('$id must be an integer.');
        }

        $query = <<<SQL
SELECT * FROM blog_post
WHERE id = :id AND (published_at IS NULL OR published_at < NOW())
SQL;

        $stmt = $this->database->prepare($query);
        $stmt->bindValue('id', $id, \PDO::PARAM_INT);
        $stmt->execute();

        return $stmt->fetch(\PDO::FETCH_ASSOC);
    }

    // ...
}
```

Cet objet `Application\Repository\BlogPostRepository` est défini comme un
service dans le registre des services. Il reçoit dans son constructeur une
dépendance de type `\PDO` (aka *PHP Data Object*). La classe `PDO` est le
connecteur unifié aux bases de données de PHP comme l'est JDBC en Java. PDO
s'interface sans aucun problème avec des moteurs de bases de données standards
tels que `SQLite`, `MySQL`, `Mariadb`, `PostgreSQL`, `Oracle`, `Sybase` etc. Cet
objet est d'ailleurs lui aussi défini comme un service dans le registre des
services.

La responsabilité de l'entrepôt `BlogPostRepository` consiste à faire une
interface avec la table `blog_post` en encapsulant toutes les requêtes SQL
exécutée sur cette dernière. L'exécution des requêtes SQL est déléguée à l'objet
`PDO` qui construit des *requêtes préparées* ainsi qu'une *transaction*
supplémentaire dans le cas d'opérations d'écriture sur la base.

En centralisant les requêtes SQL dans l'entrepôt, cela offre aussi aux
développeurs la possibilité de les adapter plus tard s'ils décident de changer
le système de stockage des données sous-jacent. Dans ce cas, il suffira de
réécrire seulement la couche des entrepôts tandis que le reste du code de
l'application sera inchangé et fonctionnel.

### La Couche Vue

La couche `Vue` (ou `Présentation`) représente les données à présenter au
client. Il s'agit par exemple d'une réponse dans un format textuel tel que HTML,
XML, JSON etc. ou bien dans un format binaire comme une image ou un fichier PDF.
Il peut aussi tout simplement s'agir d'une réponse de redirection vers une autre
page.

> Comme la notion de *Vue* du patron MVC n'est pas spécialement adaptée à la
> topologie du Web, Paul M. Jones propose une redéfinition plus subtile de MVC :
> le patron [ADR](http://pmjones.io/adr/).
>
> *ADR* est l'acronyme pour *Action*, *Domain* et *Responder*. Dans
> l'architecture ADR, la couche *Action* correspond au *Contrôleur*, la couche
> *Domain* correspond au *Modèle* et, enfin, la couche *Responder* à la *Vue*.
>
> C'est en fait la couche de présentation des données qui est remaniée dans le
> patron ADR. Contrairement aux applications traditionnelles dites *mainframe*,
> les applications Web ne retournent pas nécessairement un résultat *visuel*.
> Une réponse HTTP est parfois constituée que d'en-têtes comme dans le cas d'une
> redirection vers une autre ressource. C'est pourquoi Paul M. Jones préfère
> promouvoir le concept de *Répondeur* plutôt que de *Vue* ou de *Présentation*.

Dans l'application de blogging développée en TP, la couche `Vue` choisie repose
sur le moteur de rendu [Twig](http://twig.sensiolabs.org). Twig est aujourd'hui
une bibliothèque Open-Source développée et maintenue par SensioLabs. Ce moteur
de rendu offre une palette de fonctionnalités modernes telles que l'héritage de
gabarits, l'échappement automatique des variables, les filtres de formatage ou
bien les inclusions de vues.

```html+twig
{% extends "layout.twig" %}

{% block title 'Mon Blog' %}

{% block body %}
    <h1>Bienvenue sur mon blog</h1>

    {% for post in posts %}
        <h2>{{ post.title }}</h2>
        {{ include('blog/details.twig') }}
    {% endfor %}

{% endblock %}
```

Le snippet de code Twig ci-dessus utilise l'héritage de gabarits (mot-clé
`extends`), la boucle `for` et l'inclusion de gabarit (fonction `include()`). Il
utilise aussi l'abstraction des types de variables grâce à la notation `.` pour
accéder aux attributs d'un tableau ou d'un objet (ex: `post.title`).

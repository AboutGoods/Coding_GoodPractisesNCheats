#### Intro by https://github.com/hhamon/lpdim2016/blob/master/README.md

Gymnastique des Objets
----------------------

La gymnastique des objets ou « *Objects Calisthenics* » en anglais est une série
d'exercices pour s'entraîner à améliorer la qualité générale du code que l'on
écrit. Il s'agit de neuf règles de codage à suivre pour rendre le code meilleur.
Cette série d'exercices de gymnastique intellectuelle a été inventée par Jeff
Bay dans son ouvrage [ThoughtWorks Anthology](https://pragprog.com/book/twa/thoughtworks-anthology),
publié dans les éditions *The Pragmatic Programmers*. Ces règles sont
universelles et agnostiques du langage de programmation employé. Il s'agit de
les mettre en pratique quel que soit le langage de programmation orienté objet
utilisé. Les neuf règles de la gymnastique des objets sont décrites ci-dessous.

### 1. Seulement un niveau d'indentation

Pour améliorer la lisibilité du code, sa testabilité, sa maintenabilité et sa
réutilisabilité, il est recommandé de ne pas dépasser plus d'un niveau
d'indentation par fonction. Le code qui se trouve dans chaque niveau
supplémentaire peut être déporté dans une nouvelle fonction. Le code ci-après
montre une fonction qui ne respecte pas encore cette règle.

```php
class RouteCollection
{
    public function merge(RouteCollection $routes, $override = false)
    {
        foreach ($routes as $name => $route) {
            if (isset($this->routes[$name]) && !$override) {
                throw new \InvalidArgumentException(sprintf(
                    'A route already exists for the name "%s".',
                    $name
                ));
            }

            $this->routes[$name] = $route;
        }
    }
}
```

Dans cet exemple, on constate aisément que la méthode `merge()` de la classe
`RouteCollection` dispose de deux niveaux d'indentation. La première indentation
devant le mot-clé `foreach` compte pour un niveau 0. Par conséquent, le code à
l'intérieur de la boucle doit être extrait et encapsulé dans une méthode dédiée
comme le montre le listing ci-dessous :

```php
class RouteCollection
{
    public function merge(RouteCollection $routes, $override = false)
    {
        foreach ($routes as $name => $route) {
            $this->add($name, $route, $override);
        }
    }

    public function add($name, Route $route, $override = false)
    {
        if (isset($this->routes[$name]) && !$override) {
            throw new \InvalidArgumentException(sprintf(
                'A route already exists for the name "%s".',
                $name
            ));
        }

        $this->routes[$name] = $route;
    }
}
```

Avec ce changement simple, la méthode `merge()` ne révèle plus qu'un seul niveau
d'indentation et il en va de même pour la méthode `add()`. Si cette dernière
avait eu plus d'un niveau d'indentation, alors il aurait fallu aussi la remanier
pour la simplifier et ainsi de suite jusqu'à obtenir un seul niveau
d'indentation partout. Cette règle de gymnastique encourage non seulement à
produire du code plus lisible mais c'est aussi un moyen de favoriser l'écriture
de toutes petites fonctions ayant qu'une seule responsabilité.

### 2. Éviter l'usage du mot-clé `else`

Cette règle est simple ! Il s'agit de bannir autant que faire se peut l'usage du
mot-clé `else` dans les structures conditionnelles interrompant l'exécution du
programme le plus tôt possible. Il existe trois manières de s'éviter l'usage du
mot-clé `else` dans une fonction.

#### Utiliser une instruction `return`

```php
class ServiceLocator implements ServiceLocatorInterface
{
    public function getService($name)
    {
        // Try to leave the function as early as possible
        // The service is already created, so return it
        // immediately and skip the rest of the method.
        if (isset($this->services[$name])) {
            return $this->services[$name];
        }

        if (!isset($this->definitions[$name])) {
            throw new \InvalidArgumentException(sprintf(
                'No registered service definition called "%s" found.',
                $name
            ));
        }

        $this->services[$name] = $service = call_user_func_array($this->definitions[$name], [$this]);

        return $service;
    }
}
```

#### Lever une exception.

```php
class ServiceLocator implements ServiceLocatorInterface
{
    public function register($name, \Closure $definition)
    {
        // Try to leave the function as early as possible
        // Throw an exception whenever a method requirement
        // is not met.
        if (!is_string($name)) {
            throw new \InvalidArgumentException('Service name must be a valid string.');
        }

        $this->definitions[$name] = $definition;

        return $this;
    }
}
```

#### Injecter une stratégie dite « Null Object ».

Lorsqu'un objet dépend d'un collaborateur facultatif, il est fréquent de tester
à l'exécution du programme si ce collaborateur n'est pas nul avant d'appeler une
méthode sur celui-ci. Le snippet ci-après dévoile un exemple de collaborateur
facultatif.

```php
namespace Application;

use Framework\Http\Response;
use Framework\Routing\Exception\RouteNotFoundException;
use Framework\Templating\ResponseRendererInterface;
use Psr\Log\LoggerInterface;

class ErrorHandler
{
    private $logger;
    private $renderer;

    public function __construct(ResponseRendererInterface $renderer, LoggerInterface $logger = null)
    {
        $this->renderer = $renderer;
        $this->logger = $logger;
    }

    public function handleException(\Exception $exception)
    {
        // Check if logger is injected before logging anything.
        if (null !== $this->logger) {
            $this->logger->critical($exception->getMessage());
        }

        $vars = ['exception' => $exception];
        if ($exception instanceof RouteNotFoundException) {
            return $this->render('errors/404.twig', $vars, Response::HTTP_NOT_FOUND));
        }

        // ...
    }
}
```

Dans cet exemple, la fonction `handleException()` teste l'existence de l'objet
`logger`. S'il existe, alors sa méthode `critical()` est appelée pour
enregistrer une erreur critique dans les journaux d'erreur. Ici, c'est encore
acceptable car le nombre de conditions de ce type est limité. Néanmoins, l'idéal
consiste à rendre la dépendance obligatoire tel que l'illustre le code
ci-dessous :

```php
class ErrorHandler
{
    // ...

    public function __construct(ResponseRendererInterface $renderer, LoggerInterface $logger)
    {
        $this->renderer = $renderer;
        $this->logger = $logger;
    }

    public function handleException(\Exception $exception)
    {
        $this->logger->critical($exception->getMessage());

        // ...
    }
}
```

L'interface `LoggerInteface` est ici une implémentation du patron « Stratégie ».
Il est donc maintenant aisé de proposer différentes implémentations de celle-ci
selon l'environnement d'exécution de l'application. En environnement de
développement, une véritable implémentation de journal d'erreurs sera injectée
tandis que dans un mode de production, il s'agira de passer un objet « logger »
null tel que celui présenté ci-après.

```php
namepace Framework\Logger;

use Psr\Log\LoggerInterface;

class NullLogger implements LoggerInterface
{
    public function critical($message, array $context = [])
    {
        // ... do nothing!
    }
}
```

Au final, la classe `ErrorHandler` n'a plus besoin d'être polluée par de
multiples conditions inutiles. L'implémentation du patron d'architecture
« *Null Object* » élimine les conditions et rend ainsi le code plus lisible et
plus facilement testable. En effet, en éliminant la condition, il ne reste plus
qu'un seul et unique chemin d'exécution pour le code contre deux auparavant. Il
faut donc écrire un seul test unitaire pour couvrir le code au lieu de deux.
Grâce au patron « *Null Object* », le code s'utilise de la manière suivante :

```php

$twig = new \Twig_Environment(new \Twig_Loader_Filesystem(__DIR__.'/views'));
$renderer = new TwigRendererAdapter($twig);

// Development mode with a real logger
$errorHandler = new ErrorHandler($renderer, new RealLogger(__DIR__.'/logs/dev.log'));
$errorHandler->handleException($exception);

// Production mode with a null logger
$errorHandler = new ErrorHandler($renderer, new NullLogger());
$errorHandler->handleException($exception);
```

Les bénéfices directs de cette règle d'évitement du mot-clé `else` pour la
qualité du code sont une meilleure lisibilité, une diminution du code dupliqué
et des complexités cyclomatiques du code plus faibles. En effet, moins il y a de
chemins d'exécution que peut prendre le code et plus faibles sont les
complexités cyclomatiques des fonctions.

### 3. Encapsuler les types primitifs

L'idée de cette règle consiste à encapsuler les types primitifs scalaires
(`int` `bool`, `float`, `string`, etc.) dans des classes afin de forcer des
types orientés objets. Cette règle est toutefois largement discutable pour un
langage tel que PHP dont la philosophie même est d'offrir un typage dynamique.

Cependant, elle trouve du sens à partir du moment où l'on cherche à garantir une
cohérence et une forte cohésion des types du programme. Une classe `String` qui
encapsule une valeur scalaire de chaîne de caractères peut aussi offrir des
méthodes de manipulation de la chaîne (`toLower()`, `toUpper()`, `explode()`,
etc.) plutôt que des fonctions natives PHP.

Encapsuler des types primitifs dans des objets a un véritable intérêt quand il
s'agit de regrouper des données communes à un même concept et d'offrir des
méthodes de manipulation de ces données. Un excellent exemple est celui d'une
classe `Money` qui encapsule les attributs d'une valeur fiduciaire (la somme
d'argent et le code ISO de la devise) ainsi que les opérations possibles sur
cette monnaie.

```php
$money1 = Money::createFromString('1 725.78 EUR');
$money2 = Money::createFromString('12.22 EUR');

$money3 = $money1->add($money2);
$money4 = $money3->multiply(10);

// Split the amount of money in 5 equal shares.
$shares = $money4->split(5);
```

Encapsuler les données et opérations de manipulation d'une devise monétaire dans
une classe simplifie leur usage par rapport à de simples variables scalaires et
fonctions PHP.

### 4. Maximum un opérateur d'objet par ligne

Cette règle stipule qu'il ne doit pas y avoir plus d'un opérateur d'objet par
ligne. En d'autres termes, le symbole `->` ne doit pas être utilisé plus d'une
fois sur une ligne.

Dit autrement, un objet doit communiquer avec son collaborateur le plus proche
et non les collaborateurs de ses collaborateurs. C'est ce que l'on appelle plus
communément la « Loi de Déméter » ou principe de connaissance minimale.

Dans la méthode `addPiece()` de la classe `ChessBoard` ci-dessous, l'échiquier
a bien trop de connaissances d'implémentation de la matrice. Ce code montre un
couplage fort entre l'échiquier et les détails d'implémentation comme les objets
de rangée (`Row`) et de colonne (`Column`).

```php
class ChessBoard
{
    private $matrix;

    public function addPiece($x, $y, Piece $piece)
    {
        $this->matrix->getRow($x)->getColumn($y)->add($piece);
    }
}
```

Idéalement, l'échiquier ne devrait parler qu'à son collaborateur le plus proche,
l'objet `Matrix` tel qu'illustré ci-dessous.

```php
class ChessBoard
{
    private $matrix;

    public function addPiece($x, $y, Piece $piece)
    {
        $this->matrix->add($x, $y, $piece);
    }
}
```

De même, l'objet `Matrix` ne doit communiquer qu'avec ses collaborateurs les
plus proches, les objets de rangées.

```php
class Matrix
{
    private $rows;

    public function add($x, $y, Piece $piece)
    {
        $this->getCell($x, $y)->add($piece);
    }

    public function getCell($x, $y)
    {
        return $this->rows[$x]->getColumn($y);
    }
}
```

Enfin, l'objet `Row` ne doit communiquer qu'avec ses collaborateurs les
plus proches, les objets de colonnes.

```php
class Row
{
    private $columns;

    public function getColumn($y)
    {
        return $this->columns[$y];
    }

    public function addPieceOnCell($y, Piece $piece)
    {
        $this->getColumn($y)->add($piece);
    }
}
```

En validant cette règle, le code devient plus lisible mais surtout plus
facilement testable. En effet, dans un test unitaire, il suffit alors de créer
l'objet que l'on teste ainsi que ses collaborateurs les plus proches. Il n'y a
plus besoin de connaître les collaborateurs des collaborateurs.

> Contrairement à Java où le mot-clé `this` est facultatif, la variable `$this`
> est obligatoire en PHP pour référencer l'objet courant. Par conséquent, on
> accepte jusqu'à deux opérateurs d'instance par ligne au lieu d'un pour valider > cette quatrième règle de gymnastique des objets.

### 5. Ne pas abréger

Il est loin le temps où les espaces de mémoire vive et de stockage étaient
limités et se comptaient en quelques octets, voire kilo-octets. Inutile donc
d'abréger les noms des variables, des méthodes et des classes. Des noms bien
choisis pour les structures de données favorisent largement la lisibilité et la
compréhension du code par ses pairs.

Il faut toujours garder à l'esprit que l'on écrit d'abord du code pour soi-même
et pour les autres, et non pour la machine. Une citation est d'ailleurs très
célèbre dans le monde d'informatique.

> There are only two hard things in Computer Science: cache invalidation and
> naming things.
>
> -- *Phil Karlton*

Traduite littéralement en français, cette citation signifie qu'*il existe
seulement deux choses difficiles à réaliser en ingénierie informatique :
invalider des caches et savoir nommer les choses correctement.*

### 6. Développer des petites classes

Ce principe est tout simplement une réciproque du principe de responsabilité
unique de SOLID. Il s'agit de développer de petites classes ne dépassant pas un
certain seuil de nombre de lignes de code. Idéalement, il convient de ne pas
dépasser entre 150 et 200 lignes de code par classe en fonction de la verbosité
du langage de programmation utilisé.

En suivant ce principe, cela force le développeur à remanier régulièrement son
code afin d'extraire dans de nouvelles classes plus petites des responsabilités
identifiées. Le nombre de lignes de code dans une classe ainsi que le nombre de
ses attributs et de ses collaborateurs donnent facilement des indices sur le
niveau de complexité de celle-ci. Plus il y a de méthodes, d'attributs et de
collaborateurs, et plus il y a de risques pour que la classe ait plus d'une
responsabilité.

### 7. Limiter le nombre de propriétés d'instance dans une classe

Dans son ouvrage, Jeff Bay recommande de ne pas définir plus de deux variables
d'instance par classe. Pourquoi deux et pas trois, cinq ou dix ? Moins il y en a
dans la classe et plus cela force le développeur à mieux encapsuler les autres
attributs dans d'autres classes. Ainsi chaque classe encapsule de manière
atomique un concept, une règle métier ou une responsabilité.

Il est bien sûr très difficile en PHP de respecter cette règle. Le plus
important c'est de définir un seuil raisonnable du nombre maximum d'attributs
d'instance par classe, et le respecter autant de fois que c'est possible. Au
final, il y aura toujours des classes qui valideront ce seuil et d'autres qui le
dépasseront. Un maximum de cinq à sept propriétés d'instance maximum par classe
est un seuil raisonnable dans la plupart des cas.

### 8. Éviter les classes de collection de premier ordre

Lorsqu'il s'agit de manipuler des listes d'objets, il est recommandé d'avoir
recours à des objets de collection plutôt que de simples tableaux. Les classes
de collections génériques, dites de premier ordre, ne sont pas conseillées. Par
exemple, une classe générique nommée `Collection` ou `List`.

Il est en fait plutôt conseillé de créer une classe de collection spécifique
pour chaque type d'objet qu'elle encapsule. Ainsi, la classe de collection
spécifique contrôlera qu'elle reçoit bien des objets qu'elle supporte. De plus,
la collection offrira des méthodes spécifiques pour filtrer les éléments, leur
appliquer des opérations en lot ou bien en extraire certains qui correspondent à
un critère spécifique.

```php
namespace Framework\Routing;

class RouteCollection
{
    /** @var Route[] */
    private $routes;

    public function __construct()
    {
        $this->routes = [];
    }

    public function getName(Route $route)
    {
        foreach ($this->routes as $name => $oneRoute) {
            if ($route === $oneRoute) {
                return $name;
            }
        }

        throw new \RuntimeException('Route is not registered in the collection.');
    }

    public function merge(RouteCollection $routes, $override = false)
    {
        if ($routes === $this) {
            throw new \LogicException('A routes collection cannot merge into itself.');
        }

        foreach ($routes as $name => $route) {
            $this->add($name, $route, $override);
        }
    }

    public function match($path)
    {
        foreach ($this->routes as $route) {
            if ($route->match($path)) {
                return $route;
            }
        }
    }

    public function add($name, Route $route, $override = false)
    {
        if (isset($this->routes[$name]) && !$override) {
            throw new \InvalidArgumentException(sprintf(
                'A route already exists for the name "%s".',
                $name
            ));
        }

        $this->routes[$name] = $route;
    }
}
```

La classe `RouteCollection` ci-dessus extraite du mini-framework développé en
cours est un exemple d'implémentation de cet exercice de gymnastique qui porte
sur les collections. Ici, la classe `RouteCollection` traite de manière uniforme
des objets de type `Route`. Elle offre des mécanismes pour ajouter de nouvelles
routes à la collection, fusionner deux collections entre elles ou bien
rechercher l'objet de route qui correspond au critère spécifié.

### 9. Limiter l'usage d'accesseurs et mutateurs publiques

Afin de garantir le principe d'encapsulation du paradigme orienté objet, les
bonnes pratiques recommandent généralement de définir les attributs d'un objet
avec une portée privée, voire protégée. Cela limite ainsi la portée de
l'attribut uniquement à la classe dont il est issu, voire aux classes dérivées
dans le cadre d'une portée protégée. En revanche, l'accès direct aux attributs
depuis l'extérieur de l'objet est ainsi strictement interdit.

Dans ces conditions, l'objet agit donc comme une boîte noire dont les composants
internes ne sont pas visibles de l'extérieur par les entités qui le manipulent.
Seules des méthodes publiques de l'objet permettent au code extérieur de lire
l'état de celui-ci ou bien de le modifier.

Bien qu'un attribut est défini avec une portée privée, l'erreur consiste à lui
associé un couple d'accesseur et mutateur publiques. Les accesseurs sont les
méthodes dites « *getter* » et les mutateurs sont les méthodes
traditionnellement dénommées « *setter* ». Le snippet ci-dessous montre un
exemple de classe qui expose à outrance ce type de méthodes.

```php
class BankAccount
{
    private $amount;
    private $currency;

    public function __construct($initialBalance, $currency)
    {
        $this->amount = $initialBalance;
        $this->currency = $currency;
    }

    public function getCurrency()
    {
        return $this->currency;
    }

    public function setCurrency($currency)
    {
        $this->currency = $currency;
    }

    public function setBalance($newAmount)
    {
        $this->amount = $newAmount;
    }

    public function getBalance()
    {
        return $this->amount;
    }
}
```

Exposer un couple de méthodes *getter* et *setter* pour chaque attribut privé
revient quelque part à le rendre publique et donc violer le principe
d'encapsulation. L'état de l'objet peut ainsi être dévoilé et modifié par le
code client sans réel contrôle comme le montre le listing ci-dessous.

```php
$account = new BankAccount(500, 'EUR');

// Owner has just deposited 300 on his\her bank account
$account->setBalance($account->getBalance() + 300);

// Owner has just withdrawed 150 from his\her bank account
$account->setBalance($account->getBalance() - 150);

// Owner has changed the currency of his\her bank account
$account->setCurrency('USD');
```

Comme le montre le code ci-dessus, le solde du compte bancaire est publiquement
dévoilé par le *getter* mais il est aussi sauvagement modifié par le *setter*
sans aucun contrôle

Par exemple, que se passerait-il si le possesseur du compte bancaire souhaite
retirer 150 EUR bien que son compte bancaire ne dispose que d'un solde de 100
EUR ? Au niveau du programme, il est semblerait légitime de se demander si cette
opération est acceptable ou non en fonction de la politique de la banque qui
tient le compte. La banque autorise-t-elle une facilité de caisse pendant une
période de découvert ? Si oui, jusqu'à quel montant ? Le code actuel ne permet
pas de le prendre en compte.

La première instruction quant à elle semble montrer que le client du compte
bancaire vient de déposer 300 sur son compte. Hors, s'agit-il de 300 EUR, 300
USD ou bien 300 cacahuètes ? Le risque ici c'est de se retrouver avec un nouveau
solde de compte bancaire incohérent !

Enfin, la dernière instruction modifie la devise du compte bancaire. Par
conséquent, le solde du compte bancaire initialement en Euro passe subitement en
Dollars américains. Avec un taux de change défavorable, le client du compte
bancaire se retrouve lésé par sa banque ! Cette opération ne devrait d'ailleurs
pas être possible. En définitive, cette méthode ne devrait jamais être définie
dans la classe `BankAccount` avec une portée publique.

Cette neuvième et dernière règle de gymnastique des objets recommande donc
d'éviter d'exposer publiquement des méthodes accesseurs et mutateurs sur
l'objet. À la place, l'objet doit proposer uniquement des méthodes utiles pour
le manipuler et contrôler son nouvel état comme le montre la nouvelle classe
ci-dessous.

```php
class BankAccount
{
    private $balance;
    private $maxAllowedOverdraft;

    private function __construct(Money $initialBalance, Money $maxAllowedOverdraft)
    {
        static::compareMoney($initialBalance, $maxAllowedOverdraft);

        $this->balance = $initialBalance;
        $this->maxAllowedOverdraft = $maxAllowedOverdraft;
    }

    public static function open($initialBalance, $maxAllowedOverdraft)
    {
        return new self(
            Money::fromString($initialBalance),
            Money::fromString($maxAllowedOverdraft)
        );
    }

    public function getBalance()
    {
        return $this->balance;
    }

    public static function compareMoney(Money $money1, Money $money2)
    {
        if (!$money1->isSameCurrency($money2)) {
            throw new CurrencyMismatchException($money1->getCurrency(), $money2->getCurrency());
        }
    }

    public function deposit($amount)
    {
        $deposit = Money::fromString($amount);

        // Check both current money and given money are compatible together
        static::compareMoney($this->balance, $deposit);

        // Update new balance
        $this->balance = $this->balance->add($deposit);

        // Return new actual balance after money deposit
        return $this->balance;
    }

    public function withdraw($amount)
    {
        $withdrawal = Money::fromString($amount);

        // Check both current money and given money are compatible together
        static::compareMoney($this->balance, $withdrawal);

        // Check the bank account overdraft allowance
        $newBalance = $this->balance->subtract($withdrawal);
        $minAllowedBalance = $this->maxAllowedOverdraft->negate();
        if ($newBalance->lowerThan($minAllowedBalance)) {
            throw new MaxAllowedOverdraftReachedException()
        }

        // Update new balance
        $this->balance = $newBalance;

        // Return new actual balance after money withdrawal
        return $this->balance;
    }
}
```

Dans cette nouvelle classe, les anciens attributs primitifs `$amount` et
`$money` ont été regroupés au sein d'une même classe `Money` afin de valider la
règle #3 (*Encapsuler les types primitifs*). Cette classe `Money` offre ainsi
toute une palette de nouvelles méthodes pour comparer et modifier des valeurs
fiduciaires.

De plus, la portée du constructeur de la classe `BankAccount` a été transformée
en privé afin de forcer l'usage du constructeur statique `BankAccount::open()`.
Ce dernier construit des objets et initialise des objets `Money` à partir de
chaînes de caractères. Le véritable constructeur contrôle quant à lui que ces
paramètres sont cohérents pour compléter la construction de l'objet
`BankAccount`. En d'autres termes, la devise du solde initial du compte bancaire
et la devise du montant autorisé du découvert doivent être identiques.

Le choix de la méthode statique intitulée `open()`, c'est pour mieux
correspondre au vocabulaire du monde bancaire. En effet, ce n'est pas le client
qui « crée » un compte bancaire chez sa banque mais c'est elle qui lui « ouvre »
un nouveau compte.

```php
$account = BankAccount::open('500.00 EUR', '1000.00 EUR');
```

Une fois le compte bancaire créé, son possesseur peut alors soit déposer de
l'argent dessus ou bien en retirer. L'accesseur `getBalance()` et son mutateur
`setBalance()` associés ont disparu au profit de deux nouvelles méthodes plus
expressives : `deposit()` et `withdraw()`.

```php
$account = BankAccount::open('500.00 EUR', '1000.00 EUR');
$account->deposit('250.50 EUR');
$account->withdraw('62.00 EUR');
$account->withdraw('1250.00 EUR');
```

Les deux méthodes `deposit()` et `withdraw()` créent des instances de `Money` à
partir des représentations en chaînes de caractères des montants à créditer ou
débiter du compte. Puis, elles comparent que ces montants à créditer ou débiter
du compte bancaire sont cohérents. Leur devise doit en effet correspondre à la
devise du solde actuel du compte bancaire.

Une fois ce contrôle effectué, le nouveau montant d'espèces déposé est ajouté au
solde actuel du compte bancaire. L'état du nouveau solde est mis à jour puis
retourné en sortie de la fonction `deposit()`.

Dans le cas du retrait d'argent, le montant à débiter est en plus validé à
condition que la somme débitée n'entraîne pas un dépassement du maximum autorisé
de découvert. Si tout est bon, alors le montant est débité du compte bancaire et
le nouveau solde est mis à jour avant d'être retourné en sortie de la fonction
`withdraw()`.

Le Principe SOLID
-----------------

En programmation informatique, **SOLID** est un acronyme représentant cinq
principes de base pour la programmation orientée objet. Ces cinq principes sont
censés apporter une ligne directrice permettant le développement de logiciels
plus fiables, plus robustes, plus maintenables, plus extensibles et plus
testables.

* **S**ingle Responsability Principle ou « principe de responsabilité unique »
  qui stipule qu'une classe doit avoir une et une seule responsabilité. Si l'on
  parvient à identifier dans une classe au moins deux raisons valables de la
  modifier, c'est que cette classe dispose d'au moins deux responsabilités.

* **O**pen Close Principle ou « principe Ouvert / Fermé » qui stipule qu'une
  classe doit être ouverte à l'extension et fermée aux modifications. En
  d'autres termes, il doit être possible de facilement enrichir les capacités
  d'un objet sans que cela implique de modifier le code source de sa classe. Des
  patrons de conception comme la *Stratégie* ou le *Décorateur* répondent à ce
  principe en favorisant la composition d'objets plutôt que l'héritage.

* **L**iskov Substitution Principle ou « principe de substitution de Liskov »
  qui définit qu'une instance de type T doit pouvoir être remplacée par une
  instance de type G, tel que G sous-type de T, sans que cela modifie la
  cohérence du programme. En d'autres termes, il s'agit de conserver les mêmes
  prototypes ainsi que les mêmes conditions d'entrée / sortie lorsqu'une classe
  dérive une classe parente et redéfinit ses méthodes. Cela vaut aussi pour les
  types d'exceptions. Si une méthode de la classe parente lève une exception de
  type `E` alors cette même méthode redéfinie dans une sous-classe `C'` qui
  hérite de `C` doit aussi lever une exception de type `E` dans les mêmes
  conditions.

* **I**nterface Segregation Principle ou « principe de ségrégation d'interface »
  qui recommande de découper de grosses interfaces en plus petites interfaces
  spécialisées. Ainsi les classes clientes peuvent implémenter une ou plusieurs
  petites interfaces spécialisées plutôt qu'une grosse interface afin d'obtenir
  seulement les méthodes dont elles ont besoin.

* **D**ependency Inversion Principle ou « principe d'inversion des dépendances »
  qui stipule que les objets doivent dépendre d'abstractions plutôt que
  d'implémentations. Cela signifie qu'il est préférable de typer des arguments
  avec des types abstraits (classes concrètes ou interfaces) plutôt que
  des types concrets (classes concrètes) afin de diminuer les couplages
  et favoriser d'autres implémentations.

Objets de Valeur
----------------

Un « Objet de Valeur » (ou *Value Object* en anglais) est une instance qui
encapsule un certain nombre d'attributs et dont l'état global représente une
seule et unique valeur. Les objets de valeur répondent aussi aux trois règles
suivantes :

1. L'état interne d'un objet de valeur ne possède pas d'identité propre. En
   d'autres termes, l'objet de valeur ne contient pas d'identifiant unique qui
   le distingue d'un autre objet de valeur représentant la même valeur.
2. L'état interne d'un objet de valeur n'est plus modifiable une fois que
   l'objet a été construit. Cette propriété lui confère une capacité de cache
   pour une durée infinie. L'état de l'objet est garanti d'être toujours le
   même.
3. Deux objets de valeur contenant chacun la même valeur (donc le même état)
   sont permutables et utilisables indifféremment l'un de l'autre.

De nombreux concepts du quotidien sont propices à une modélisation sous la forme
d'objets de valeur :

* Une valeur monétaire,
* Une date dans le calendrier,
* Un poids,
* Une masse,
* Un élément chimique de la table périodique des éléments,
* Une molécule,
* Un point dans un plan à deux dimensions,
* Un point dans un plan à trois dimensions,
* Une adresse postale,
* Des coordonnées GPS,
* Une couleur RVB,
* Une couleur CMJN,
* etc.

La classe ci-dessous représente un point dans un plan à deux dimensions.

```php
class Point
{
    private $x;
    private $y;

    public function __construct($x, $y)
    {
        if (!is_int($x)) {
            throw new \InvalidArgumentException('$x must be a valid integer.');
        }

        if (!is_int($y)) {
            throw new \InvalidArgumentException('$y must be a valid integer.');
        }

        $this->x = $x;
        $this->y = $y;
    }

    public static function fromString($coordinates)
    {
        if (!is_string($coordinates)) {
            throw new \InvalidArgumentException('$coordinates must be a valid string that represents coordinates: (x,y)');
        }

        if (!preg_match('#^\((?P<x>-?\d+),(?P<y>-?\d+)\)$#', $coordinates, $point)) {
            throw new \InvalidArgumentException('Invalid 2D coordinates representation.');
        }

        return new self((int) $point['x'], (int) $point['y']);
    }

    public function getX()
    {
        return $this->x;
    }

    public function getY()
    {
        return $this->y;
    }

    public function translate($a, $b)
    {
        return new self($this->x + $a, $this->y + $b);
    }

    public function negate()
    {
        return new self(-$this->x, -$this->y);
    }
}
```

La classe `Point` ci-dessus encapsule les coordonnées cartésiennes d'un point
dans un plan à deux dimensions (abscisse et ordonnée). Le constructeur vérifie
que les coordonnées respectent certaines contraintes. Pour des raisons de
simplicité, les coordonnées doivent être exprimées uniquement en entiers
(positifs ou négatifs). Il existe aussi un constructeur statique qui offre un
moyen alternatif de construire l'objet à partir d'une représentation en chaîne
du duo de coordonnées.

```php
$a = Point::fromString('(0,0)');
$b = Point::fromString('(1,2)');
$c = Point::fromString('(-1,-6)');
$d = Point::fromString('(-4,12)');
```

Comme il s'agit d'objets de valeur, aucun d'entre eux n'a d'identité propre.
Deux objets qui renferment le même état sont donc égaux et permutables.

```php
$a = Point::fromString('(0,0)');
$b = Point::fromString('(0,0)');

var_dump($a == $b); // true
```

De plus, il est impossible de changer les coordonnées d'un point une fois que
celui-ci a été construit par le constructeur. Par conséquent, les méthodes de
manipulation des points (négation, translation, rotation, etc.) retournent
toutes de nouvelles instances de points.

```php
$a = Point::fromString('(1,2)');
$b = $a->negate();
$c = $a->translate(2, 3);
```

Dans un système informatique, les objets de valeur offrent ainsi un moyen simple
et puissant de manipuler et de tester unitairement des données atomiques.

Patrons de Conception
---------------------

Les patrons de conception (aka *design patterns*) sont des solutions
architecturales abstraites et conceptuelles pour répondre à des problématiques
récurrentes du génie logiciel. Les patrons de conception ont été inventés à
l'origine dans les années 70 par Christopher Alexander puis formalisés dans les
années 90 par le GoF (*Gang of Four*) dans leur ouvrage *Elements of Reusable
Object-Oriented Software* paru en 1995. Le Gang des Quatre était composé des
quatre ingénieurs informaticiens suivants :

* Erich Gamma
* Richard Helm
* Ralph Johnson
* John Vlissides

Ils ont conçu les 23 patrons de conception décrits dans leur ouvrage.  Les
patrons de conception sont des solutions abstraites. Elles sont donc agnostiques
des langages de programmation. Un patron de conception peut être implémenté dans
un langage de programmation particulier (PHP, Java, JavaScript, C#, etc.) à
condition que ce dernier supporte le paradigme de la programmation orientée
objet. Les patrons de conception ont aussi les autres avantages suivants :

* Ils apportent un vocabulaire commun pour les développeurs,
* Ils favorisent le découplage et l'extensibilité du code,
* Ils favorisent la testabilité unitaire du code,
* Ils encouragent des bonnes pratiques orientées objet (principe SOLID,
  composition d'objets au lieu de l'héritage, etc.),
* Ils permettent de capitaliser un savoir et un savoir-faire.

Christopher Alexander décrit les patrons de conception de la manière suivante :

> Chaque patron décrit un problème qui se manifeste constamment dans notre
> environnement, et donc décrit le cœur de la solution à ce problème, d’une
> façon telle que l’on puisse réutiliser cette solution des millions de fois,
> sans jamais le faire deux fois de la même manière.
>
> *Christopher Alexander, 1977*

### Familles de Patrons

Les 23 patrons de conception sont répartis en trois familles :

* Les patrons **Créationnaux** encapsulent les protocoles de création et
  d'initialisation de certains objets. Les patrons *Singleton* et
  *Méthode de Fabrique* sont des exemples de patrons créationnels.

* Les patrons **Structuraux** organisent les classes d'un programme les unes par
  rapport aux autres. Ces patrons sont aussi ceux qui favorisent le découplage
  et la réutilisabilité du code en encourageant notamment la composition
  d'objets plutôt que l'héritage. Les principaux patrons structuraux sont entre
  autres le *Composite*, le *Décorateur*, la *Façade* et l'*Adaptateur*.

* Les patrons **Comportementaux** organisent la manière dont les objets
  collaborent et communiquent les uns avec les autres. Ils sont chargés de mieux
  distribuer les responsabilités des objets et expliquent le fonctionnement de
  certains algorithmes. Les patrons *Gabarit de Méthode*, *Itérateur*,
  *Médiateur* et *Stratégie* sont trois exemples de patrons comportementaux.

### Le Patron Singleton

> Ce patron vise à assurer qu'il n'y a toujours qu'une seule et unique instance
> d'une classe en fournissant une interface pour la manipuler. C'est un des
> patrons les plus simples et les plus couramment implémentés. L'objet qui ne
> doit exister qu'en une seule instance comporte une méthode pour obtenir cette
> unique instance et un mécanisme pour empêcher la création d'autres instances.
>
> *Wikipédia*.

Par exemple, dans un logiciel modélisant l'organisation politique de la
République française, une classe `PresidentDeLaRepublique` peut implémenter le
patron *Singleton*. Ainsi, ce patron garantira qu'il y a toujours qu'un seul et
unique Président de la République en fonction à l'instant T.

```php
PresidentDeLaRepublique::actuellementEnFonction('François Hollande');

$president1 = PresidentDeLaRepublique::getInstance();
$president2 = PresidentDeLaRepublique::getInstance();

var_dump($president1 === $president2); // true
```
Dans ce snippet, la comparaison avec le symbole `===` vérifie que les objets
`$president1` et `$president2` encapsulent non seulement le même état interne
et qu'il s'agit aussi de deux références vers la même instance en mémoire.

### Le Patron Méthode de Fabrique

> La Fabrique (*Méthode de Fabrique*) est un patron de conception créationnel
> utilisé en programmation orientée objet. Elle permet d'instancier des objets
> dont le type est dérivé d'un type abstrait. La classe exacte de l'objet n'est
> donc pas connue par l'appelant.
>
> *Wikipédia*.

Il existe plusieurs types de fabriques :

* La *fabrique statique* qui utilise une fonction statique d'une classe pour
  créer un objet. Dans le projet du mini-framework, un exemple de fabrique
  statique est la méthode statique `createFromMessage()` des classes `Request`
  et `Response`.

* La *fabrique simple* est un idiome de programmation qui vise à encapsuler la
  création d'un objet à l'intérieur d'une méthode d'instance. Par exemple, la
  méthode `getService()` de la classe `ServiceLocator` est un exemple
  d'implémentation d'une *fabrique simple* qui centralise la création d'objets.

* La *méthode de fabrique* est la vraie définition du patron *Fabrique*. Il
  s'agit de définir dans un type abstrait (classe abstraite ou interface) une
  méthode publique dédiée à la création et à l'initialisation d'un objet. Dans
  le cas d'une classe abstraite, celle-ci pré-implémente cette méthode publique
  de fabrique. Cette méthode publique délègue ensuite une partie du processus à
  une méthode protégée abstraite implémentée par chaque sous-classe de fabrique
  concrète. Chaque fabrique concrète est responsable de produire un et un seul
  type d'objet. Retrouvez le patron Fabrique dans les slides de présentation de
  conférence [ici](https://speakerdeck.com/hhamon/design-patterns-the-practical-approach-in-php).

### Le Patron Composite

> Ce patron permet de composer une hiérarchie d'objets, et de manipuler de la
> même manière un élément unique, une branche, ou l'ensemble de l'arbre. Il
> permet en particulier de créer des objets complexes en reliant différents
> objets selon une structure en arbre.
> 
> Ce patron impose que les différents objets aient une même interface, ce qui
> rend uniformes les opérations de manipulation de la structure. Par exemple
> dans un traitement de texte, les mots sont placés dans des paragraphes
> disposés dans des colonnes, elles mêmes dans des pages. Pour manipuler
> l'ensemble, une classe composite implémente une interface. Cette dernière est
> héritée par les objets qui représentent les textes, les paragraphes, les
> colonnes et les pages.
>
> *Wikipédia*.

Dans le projet de mini-framework, la classe `CompositeFileLoader` du composant
`Routing` est une implémentation du patron `Composite`. Elle permet d'imbriquer
un ou plusieurs objets de même type et de les utiliser comme s'ils étaient qu'un
seul et unique objet.

### Le Patron Proxy

> Un proxy est une classe qui se substitue à une autre classe. Par convention et
> simplicité, le proxy implémente la même interface que la classe à laquelle il
> se substitue. L'utilisation de ce proxy ajoute une indirection à l'utilisation
> de la classe à substituer.
>
> *Wikipédia*.

Dans l'exemple du mini-framework, l'objet `Kernel` est un substitut (*proxy*)
à l'objet `HttpKernel`. Ces deux classes implémente une interface commune :
`KernelInterface`. La méthode `handle()` de l'objet `Kernel` agit comme un proxy
pour la même de même nom de l'objet `HttpKernel` imbriqué.

### Le Patron Décorateur

> Ce patron permet d'attacher dynamiquement des responsabilités à un objet. Une
> alternative à l'héritage en favorisant la composition d'objets. Ce patron est
> inspiré des poupées russes.
> 
> Un objet peut être caché à l'intérieur d'un autre objet décorateur qui lui
> rajoutera des fonctionnalités, l'ensemble peut être décoré avec un autre objet > qui lui ajoute des fonctionnalités et ainsi de suite. Cette technique
> nécessite que l'objet décoré et ses décorateurs implémentent le même contrat
> publique, qui est typiquement définie par une classe abstraite ou une
> interface.
>
> *Wikipédia*.

Retrouvez le patron Décorateur dans les slides de présentation de conférence
[ici](https://speakerdeck.com/hhamon/design-patterns-the-practical-approach-in-php).

### Le Patron Façade

> Ce patron fournit une interface unifiée sur un ensemble d'interfaces d'un
> système. Il est utilisé pour réaliser des interfaces de programmation. Si un
> sous-système comporte plusieurs composants qui doivent être utilisés dans un
> ordre précis, une classe façade sera mise à disposition, et permettra de
> contrôler l'ordre des opérations et de cacher les détails techniques des
> sous-systèmes.
>
> *Wikipédia*.

Retrouvez le patron Façade dans les slides de présentation de conférence
[ici](https://speakerdeck.com/hhamon/practical-design-phpatterns).

### Le Patron Adaptateur

> Ce patron convertit l'interface d'une classe en une autre interface exploitée
> par une application. Permet d'interconnecter des classes qui sans cela
> seraient incompatibles.
> 
> Il est utilisé dans le cas où un programme se sert d'une bibliothèque de
> classe, et que, à la suite d'une mise à jour de la bibliothèque, cette
> dernière ne correspond plus à l'utilisation qui en est faite, parce que son
> interface a changé. Un objet adapteur expose alors l'ancienne interface en
> utilisant les fonctionnalités de la nouvelle.
>
> *Wikipédia*.

Dans le projet du mini-framework, la classe `TwigRendererAdapter` est un exemple
d'implémentation du patron *Adaptateur* qui adapte l'interface incompatible de
la classe `Twig_Environment` avec celle attendue par le framework :
`ResponseRendererInterface`.

### Le Patron Gabarit de Méthode

> Ce patron définit la structure générale d'un algorithme en déléguant certains
> passages afin de permettre à des sous-classes de modifier l'algorithme en
> conservant sa structure générale. C'est un des patrons les plus simples et les
> plus couramment utilisés en programmation orientée objet. Il est utilisé
> lorsqu'il y a plusieurs implémentations possibles d'un calcul.
>
> Une classe d'exemple (ou *template*) comporte des méthodes d'exemple, qui,
> utilisées ensemble, implémentent un algorithme par défaut. Certaines méthodes
> peuvent être vides ou abstraites. Les sous-classes de la classe template
> peuvent remplacer tout ou partie de certaines méthodes et ainsi créer un
> algorithme dérivé.

L'implémentation de ce patron se traduit par une classe qui définit une méthode
publique ou protégée, et déclarée comme *finale*. De ce fait, la méthode ne peut
plus être redéfinie par les sous-classes, ce qui garantit que son algorithme
sera toujours exécuté de la même manière. En revanche, certaines étapes de cet
algorithme peuvent être redéfinies ou implémentées grâce à des méthodes
protégées.

Dans le projet du mini-framework un exemple d'implémentation du patron
*Gabarit de Méthode* est la méthode publique finale *getMessage()* de la classe
*AbstractMessage* du composant *Http*. Cette méthode est déclarée finale pour
garantir que cet algorithme soit toujours exécuté de la même manière et qu'il ne
puisse pas être redéfini par des sous-classes. En revanche, une partie de
l'algorithme est laissé à la charge des sous-classes en les forçant à
implémenter la méthode abstraite `createPrologue()`.

### Le Patron Stratégie

> Dans ce patron, une famille d'algorithmes de même nature sont encapsulés de
> manière à être interchangeables. Les algorithmes peuvent changer
> indépendamment de l'application qui les utilise. Il comporte trois
> intervenants : le contexte, la stratégie et les implémentations.
>
> La stratégie est l'interface commune aux différentes implémentations -
> typiquement une classe abstraite ou une interface. Le contexte est l'objet qui
> va associer un algorithme avec un processus.
>
> *Wikipédia*.

Dans le projet du mini-framework, l'interface `FileLoaderInterface` du composant
`Routing` agit comme une stratégie pour l'objet `Router` qui l'utilise. En
proposant plusieurs implémentations de l'interface, donc plusieurs stratégies,
le routeur peut ainsi charger la configuration des routes depuis un fichier PHP
ou bien un fichier XML. Pour supporter un format YAML, il suffirait alors de
créer une nouvelle classe `YamlFileLoader`.

### Le Patron Itérateur

> Ce patron permet d'accéder séquentiellement aux éléments d'un ensemble sans
> connaître les détails techniques du fonctionnement de l'ensemble. C'est un des
> patrons les plus simples et les plus fréquents. Selon la spécification
> originale, il consiste en une interface qui fournit les méthodes `next()` et
> `current()`.
>
> *Wikipédia*.

Les itérateurs sont particulièrement adaptés pour les objets de type collection.
Un objet qui collectionne une série d'objets en son sein peut implémenter le
patron « *Itérateur* » afin de simplifier les opérations d'accès aux objets
collectionnés. L'autre avantage d'un objet itérateur c'est sa capacité à être
utilisé facilement comme argument de la structure `foreach` de PHP.

Dans le projet du mini-framework, la classe `RouteCollection` du composant
`Routing` est un exemple d'implémentation du patron *Itérateur* en PHP grâce à
l'interface native `Iterator` de PHP.

### Le Patron Médiateur

> Dans ce patron, il y a un objet qui définit comment plusieurs objets
> communiquent entre eux en évitant à chacun de faire référence à ses
> interlocuteurs. Ce patron est utilisé quand il y a un nombre non négligeable
> de composants et de relations entre les composants.
> 
> Par exemple dans un réseau de cinq composants, il peut y avoir jusqu'à vingt
> relations (chaque composant vers quatre autres). Un composant médiateur est
> placé au milieu du réseau et le nombre de relations est diminué : chaque
> composant est relié uniquement au médiateur. Le médiateur joue un rôle
> similaire à un sujet dans le patron Observateur et sert d'intermédiaire pour
> assurer les communications entre les objets.
>
> *Wikipédia*.

Dans le projet du mini-framework, pour diminuer les nombreux couplages entre
l'objet `HttpKernel` et ses collaborateurs, ce dernier utilise un médiateur. Il
s'agit de l'objet `EventManager` qui propage des événements et notifie les
écouteurs associés. Quand le noyau HTTP propage un signal dans le framework, des
écouteurs se déclenchent successivement à son écoute, puis le traitent.

Gestion Sémantique de Version
-----------------------------

Le versionnage sémantique est un ensemble de bonnes pratiques qui définissent
comment les numéros des versions d'un logiciel doivent être incrémentés en
fonction des types de patches intégrés au code :

1. correctif de bogues ou de faille de sécurité,
2. ajout d'une nouvelle fonctionnalité,
3. ajout d'un changement qui brise la compatibilité avec les versions
   précédentes du logiciel.

En gestion sémantique de version, chaque version est exprimée avec un triplet
de valeurs de type `x.y.z`. Dans cette notation, les lettres `x`, `y` et `z`
représentent respectivement les numéros de version *majeure*, *mineure* et
*patch*. Par exemple : `2.6.97`.

L'incrémentation de l'un ou de l'autre de ces trois numéros quand une nouvelle
version du logiciel est publiée est soumis au respect des règles suivantes :

1. le numéro de version MAJEURE quand il y a des changements
   rétro-incompatibles (cassure de compatibilité avec les versions antérieures),
2. le numéro de version MINEURE quand il y a des changements
   rétro-compatibles (ajout de nouvelles fonctionnalités),
3. le numéro de version de PATCH quand il y a des corrections d'anomalies
   rétro-compatibles (correctif d'un bogue ou d'une faille de sécurité).

Les logiciels Open-Source modernes comme Symfony suivent à la lettre cette bonne
pratique pour gérer leurs versions. Cela assure aux utilisateurs du framework
des mises à jour du code de leur application sans risque de casser la
compatibilité dans le cas où les mises à jour concernent les versions mineures
et patch. Plus d'informations sur le versionnage sémantique
[ici](http://semver.org/lang/fr/).

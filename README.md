# Design Patterns

## Table of Contents

- [Creational Patterns](#creational-patterns)
    - [Factory Method](#factory-method)

---

## Creational Patterns

### Factory Method

```php
interface Superhero
{
    public function useSuperpower(): string;
}

class DCSuperhero implements Superhero
{
    public function __construct(protected string $name, protected string $superpower)
    {
    }

    public function useSuperpower(): string
    {
        return "<br> $this->name uses $this->superpower. <br>";
    }
}

abstract class SuperheroFactory
{
    abstract public function create(): Superhero;

    public function assembleTeam(int $count): array
    {
        $team = [];

        for ($i = 0; $i < $count; $i++) {
            $team[] = $this->create();
        }

        return $team;
    }
}

class DCSuperheroFactory extends SuperheroFactory
{
    public function create(): Superhero
    {
        $superheroes = ['Superman', 'Batman', 'The Flash'];
        $superpowers = ['flight', 'super strength', 'speed force'];

        $name = $superheroes[array_rand($superheroes)];
        $superpower = $superpowers[array_rand($superpowers)];

        return new DCSuperhero($name, $superpower);
    }
}

function assembleHeroes(SuperheroFactory $factory, int $count): void
{
    $heroes = $factory->assembleTeam($count);

    foreach ($heroes as $hero) {
        echo $hero->useSuperpower();
    }
}

echo "Assembling DC Superheroes: <br>";
assembleHeroes(new DCSuperheroFactory(), 3);
```

The output:

```
Assembling DC Superheroes:

Superman uses flight.

The Flash uses super strength.

Batman uses speed force.
```

Short Example: 

```php
interface Superhero
{
    public function useSuperpower(): string;
}

class Batman implements Superhero
{
    public function useSuperpower(): string
    {
        return "Batman uses Batarang!";
    }
}

class Superman implements Superhero
{
    public function useSuperpower(): string
    {
        return "Superman uses flight!";
    }
}

function createSuperhero(string $name): Superhero
{
    $superheroClasses = [
        'batman' => Batman::class,
        'superman' => Superman::class,
    ];

    if (isset($superheroClasses[$name])) {
        return new $superheroClasses[$name]();
    }

    throw new Exception("Unknown superhero: $name");
}

$batman = createSuperhero('batman');
echo $batman->useSuperpower();
```

Output:

```
Batman uses Batarang!
```
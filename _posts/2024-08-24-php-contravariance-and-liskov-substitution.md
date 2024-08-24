---
title: 'Контрвариантность и Принцип подстановки Барбары Лисков в ПХП / Contravariance & Liskov substitution principle in PHP'
description: '#php #solid #contravariance #liskov #basics'
layout: post
---
---

<div style="text-align: center"><img src="/assets/2024-08-24-php-contravariance-and-liskov-substitution.jpg" alt="PHP Performance" width="300"></div>

Читал, заполняя свой пробел в знаниях 
[статью Предводителева "Про вариантность в программировании"](https://t.me/sergei_predvoditelev/47) 
(очень рекомендую). И хотел остановиться на самом неочевидном моменте - контрвариантность (или контр**а**вариантность) в PHP.

На [php.net](https://www.php.net/manual/ru/language.oop5.variance.php) и у Сергея в статье тема описана, возможно вам 
легче понять смысл, обратившись к этим источникам. Но мне кажется в этом посте понять будет легче, так как рассмотрена 
лишь контрвариантность.

## Пример с кодом

Весь код можете найти и запустить по ссылке: https://onlinephp.io/c/2e414

Проговорю поэтапно:

```php
<?php
class Food {}

class AnimalFood extends Food {}

class CatAnimalFood extends AnimalFood {}
```

Выше у нас простая иерархия из 3 классов, где:

- `Food` - родительский класс (надтип) для остальных двух
- `CatAnimalFood` дочерний (подтип) для других двух.

```php
class Animal
{
    public function eat(AnimalFood $food): void
    {
        echo sprintf("Animal of class '%s' eats '%s'\n", self::class, get_class($food));
    }
}

class Cat extends Animal{
    /** @param object $food Вместо типа object можно либо тот же {@see AnimalFood}, любой родительский {@see Food}, но не дочерний {@see CatAnimalFood} или другой тип (например, string) */
    public function eat(object $food): void
    {
        echo sprintf("Cat of class '%s' eats '%s'\n", self::class, get_class($food));
    }
}
```

**Контрвариантность в PHP представлена только при переопределении (overwriting) метода.**

Выше в коде видим родительский класс `Animal` и дочерний `Cat`.

В рассмотренном случае при переопределении (в данном случае в методе `Cat::eat`) принимает более общий тип `object`, 
чем в родительском классе `Animal` (в параметре методе `Animal::eat` принимается тип `AnimalFood`).

Это демонстрирует **контрвариантность  — допускается ТОЛЬКО САМ ТИП и его ЛЮБОЙ НАДТИП, но не допускается подтип или 
любой другой тип**.

Другими словами. в параметре метода `Cat::eat` мы можем использовать тип `AnimalFood`, `Food` и даже общий для всех 
классов надтип `object`. Но любой подтип как например дочерний `CatAnimalFood` или вообще `string` нельзя.

## Для чего нужна контрвариантность в PHP

Для соблюдения принципа **подстановки Барбары Лисков** (Liskov Substitution Principle).

В оригинале принцип описан тяжелым для понимания языком, но смысл можно передать достаточно простым предложением:

> Объекты надтипа могут быть заменены объектами его подтипа без нарушения работы приложения

А вот пример действия этого принципа в коде:

```php
(new Animal())->eat(new AnimalFood()); // Animal of class 'Animal' eats 'AnimalFood'
(new Cat())->eat(new AnimalFood());    // Cat of class 'Cat' eats 'AnimalFood'
```

Т.е. контрвариантность обеспечивает, чтобы любой дочерний класс (в нашем случае `Cat`) всегда можно подставить вместо 
родительского (`Animal`) без ошибки.

Соблюдение этого принципа вовсе не означает, что всегда возможно обратное - использование родительского вместо 
дочернего:

```php
(new Cat())->eat(new class {});    // Cat of class 'Cat' eats 'class@anonymous /.../....php:27$0'
(new Animal())->eat(new class {}); // PHP Fatal error: ...
```

## Еще небольшой пример

С появлением в PHP типа `union` мы можем не убирая типизацию при переопределении метода предусмотреть использование 
несвязанного типа (в приведенном примере `string`).

```php
class Dog extends Animal{
    public function eat(object|string $food): void
    {
        $foodAsString = is_object($food) ? get_class($food) : $food;
        echo sprintf("Dog of class '%s' eats '%s'\n", self::class, $foodAsString);
    }
}

(new Dog())->eat(new AnimalFood()); // Dog of class 'Cat' eats 'AnimalFood'
(new Dog())->eat("string");         // Dog of class 'Dog' eats 'string'

(new Animal())->eat("string");
// PHP Fatal error:  Uncaught TypeError: Animal::eat(): Argument #1 ($food) must be of type AnimalFood, string given
```

## Обсуждение в телеграм-канале

Обсуждение и [пост в моем телеграм канале](https://t.me/programming_ionov). Не был согласен с постом 
[Владимир Романичев](https://t.me/vladimirromanichev). Как же он не прав)
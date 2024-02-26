---
title: "`.copyWithin()`"
description: "Копирует одну часть массива в другую."
authors:
  - vitya-ne
related:
  - js/arrays
  - js/array-splice
  - js/array-with
tags:
  - doka
---

## Кратко

Метод `copyWithin()` копирует последовательную часть элементов массива из одного места в другое. Длина массива при этом не меняется. Метод использует [поверхностное копирование (shallow copy) элементов](/js/shallow-or-deep-clone/).

## Пример

Скопируем два последних элемента в начало массива:

```js
const workDays = ['ПН', 'ВТ', 'СР', 'ЧТ', 'ПТ']

console.log(workDays.copyWithin(0, 3))
// ['ЧТ', 'ПТ', 'СР', 'ЧТ', 'ПТ']
```

Скопируем два первых элемента в конец массива:

```js
const workDays = ['ПН', 'ВТ', 'СР', 'ЧТ', 'ПТ']

console.log(workDays.copyWithin(-2, 0))
// ['ПН', 'ВТ', 'СР', ПН', 'ВТ']
```

## Как пишется

`Array.copyWithin()` принимает три аргумента:

- индекс элемента, определяющий позицию вставки скопированных значений;
- индекс элемента, определяющий позицию начала диапазона копируемых значений;
- необязательный параметр, определяющий индекс элемента, перед которым заканчивается диапазон копируемых значений.

`Array.copyWithin()` возвращает изменённый массив.

Метод `copyWithin()` является изменяющим методом. После выполнения метода изменится сам массив, на котором он был вызван.

Если индекс конца диапазона копирования не указан, будут скопированы элементы до конца массива.

Отрицательные индексы используются для отсчёта от последнего элемента массива.

Если индексы не соответствуют размеру массива, никаких изменений не произойдёт.

## Как понять

Метод `copyWithin()` может применяться как производительный способ перемещения диапазона значений из одной позиции массива в другую.

При выполнении копирования незаполненные ячейки будут также скопированы. Например, скопируем два первых элемента и вставим их на место двух последних:

```js
const companies = ['HP']

companies[2] = 'IBM'
companies[4] = 'Intel'

console.log(companies)
// ['HP', <1 empty item>, 'IBM', <1 empty item>, 'Intel']

console.log(companies.copyWithin(3, 0))
// ['HP', <1 empty item>, 'IBM', 'HP', <1 empty item>]
```

## Подсказки

  💡 Метод `copyWithin()`, производит преобразования к целому для аргументов, определяющих индексы. Это позволяет избежать случайных ошибок, однако приводит к некоторым не очевидным особенностям выполнения. Например, вызов `copyWithin()` без указания второго аргумента не вызовет ошибок, так как в данном случае произойдёт преобразование `undefined` в 0, что соответствует начальному индексу в массиве. Для улучшения читаемости кода следует указывать параметр, определяющий позицию начала диапазона копируемых значений явно:

```js
const colors = ['red', 'green', 'blue', 'white']

// Работает
console.log(colors.copyWithin(2))
// ['red', 'green', 'red', 'green']

// То же самое, но понятнее
console.log(colors.copyWithin(2, 0))
// ['red', 'green', 'red', 'green']
```

💡 Если копируемые элементы массива являются объектами, то в результате работы метода `toSpliced()` скопированные элементы на новых позициях будут содержать ссылки, а не новые объекты. Изменение этих объектов будут видны на старых и новых позициях:

```js
const months = [['март', 'апрель'], 'июнь', 'июль', ['сентябрь']]

// скопируем последний элемент в начало массива
months.copyWithin(0, -1);
console.log(months)
// [['сентябрь'], 'июнь', 'июль', ['сентябрь']]

// изменим значение скопированного элемента:
months[0][0] = 'май'

// изменение так же видны в источнике копирования
console.log(months)
// [['май'], 'июнь', 'июль', ['май']]
```
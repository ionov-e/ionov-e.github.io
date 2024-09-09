---
title: 'Управление памятью в PHP: Сборка мусора, WeakReference и WeakMap'
description: '#php #memory #perfomance #garbage-collector #weak-reference #weak-map'
layout: post
---
---

<img src="/assets/2024-07-29-php-inspecting-opcodes-header.jpg" alt="PHP Performance" width="500">

https://habr.com/ru/articles/748352/

Замечательная статья - простыми словами про устройство памяти и сборщика мусора в PHP.
До ее прочтения у меня были проблемы в понимании:
- Когда обычно начинает работать сборщик мусора (**циклические ссылки**)
- Наконец я прям понял как работают и для чего нужны **WeakReference** и **WeakMap**

Обсуждение и [пост в моем телеграм канале](https://t.me/programming_ionov/8).
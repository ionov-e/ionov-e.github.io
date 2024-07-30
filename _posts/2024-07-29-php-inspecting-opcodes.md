---
title: Про OPCodes и способы дампа (PHP)
description: #php #opcode #perfomance
layout: post
---
# Про OPCodes и способы дампа (PHP)

#php #opcode #perfomance

<img src="/assets/2024-07-29-php-inspecting-opcodes-header.jpg" alt="PHP Performance" width="500">

Мне очень понравилась статейка: [https://php.watch/articles/php-dump-opcodes](https://php.watch/articles/php-dump-opcodes). Особенно с раздела **Inspecting OPCodes**.

Может для многих это уже давно пройденный этап, но поделюсь тем, что мне было интересно:

1) Очень "прикольно" было увидеть, что функция мейн появляется на входе нашего скрипта, как во многих других языках

2) Я догадывался, но не видел в явном виде до этого - циклы выполняются через GOTO функцию

3) Я хотел увидеть как протестировать результат opcode оптимайзера в моем конкретном случае

4) Не только функции из списка ( [https://php.watch/articles/php-zend-engine-special-inlined-functions](https://php.watch/articles/php-zend-engine-special-inlined-functions) ) стоит использовать через use или с указанием корневого неймспейса. Плюс, наглядно продемонстрировано как это работает, когда этого не делаешь

Ссылка на [мой телеграм канал](https://t.me/programming_ionov) (только что завел) с этим постом и обсуждениями
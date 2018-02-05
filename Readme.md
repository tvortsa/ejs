# EJS

Embedded JavaScript шаблоны.

[![Build Status](https://travis-ci.org/visionmedia/ejs.png)](https://travis-ci.org/visionmedia/ejs)

- - -
NOTE: Версия 2 EJS Вносит некоторые изменения в эту версию (особенно,
удаление фильтров).  Работа над v2 ведется здесь:
https://github.com/mde/ejs

Файлы проблемм EJS v2 здесь: https://github.com/mde/ejs/issues
- - -

## Установка

    $ npm install ejs

## Фичи

  * Совместима с системой видов [Express](http://expressjs.com)
  * Статическое кэширование промежуточного JavaScript
  * Небуферизованный код для условных выражений etc `<% code %>`
  * Экранирование html по умолчанию с `<%= code %>`
  * Неэкранированная буферизация по умолчанию с `<%- code %>`
  * Поддержка тегов
  * Поддержка фильтров для дизайнерских шаблонов
  * Includes
  * Client-side support
  * Новая строка с slurping with `<% code -%>` or `<% -%>` or `<%= code -%>` or `<%- code -%>`

## Пример

    <% if (user) { %>
	    <h2><%= user.name %></h2>
    <% } %>
    
## Посмотрите 

[живой пример](https://runnable.com/ejs" target="_blank"><img src="https://runnable.com/external/styles/assets/runnablebtn.png" style="width:67px;height:25px;)

## Использование

    ejs.compile(str, options);
    // => Функция

    ejs.render(str, options);
    // => строка

## Опции

  - `cache`           Скомпилированные функции кэшируются, требуется `filename`
  - `filename`        Используется `cache` для ключа caches
  - `scope`           Контекст выполнения функции
  - `debug`           Output generated function body
  - `compileDebug`    When `false` no debug instrumentation is compiled
  - `client`          Возвращает standalone скомпилированной функции
  - `open`            Открывающий тэг, по умолчанию "<%"
  - `close`           Закрывающий тэг, по-умолчанию "%>"
  - *                 все остальные template-local переменные

## Includes

 Includes ссылаются на шаблоны в операторе `include` ,
 например если у вас есть "./views/users.ejs" и "./views/user/show.ejs"
 вы можете использовать `<% include user/show %>`. Это вставит файл(ы)
 буквально вставит template, _no_ IO выполняется после компиляции, 
 таким образом для этих включенных шаблонов доступны локальные переменные.

```
<ul>
  <% users.forEach(function(user){ %>
    <% include user/show %>
  <% }) %>
</ul>
```

## Пользовательские разделители

Пользовательские разделители также могут применяться глобально:

    var ejs = require('ejs');
    ejs.open = '{{';
    ejs.close = '}}';

Which would make the following a valid template:

    <h1>{{= title }}</h1>

## Фильтры

EJS условно поддерживает концепцию "filters". "filter chain"
то удобный для дизайнеров api для манипуляций данными, без написания JavaScript.

Фильтры могут применяться, by supplying модификатору _:_ , так например если мы хотим получить массив `[{ name: 'tj' }, { name: 'mape' },  { name: 'guillermo' }]` и вывести список имен мы можем сделать это простым фильтром:

Шаблон:

    <p><%=: users | map:'name' | join %></p>

Вывод:

    <p>Tj, Mape, Guillermo</p>

Render call:

    ejs.render(str, {
        users: [
          { name: 'tj' },
          { name: 'mape' },
          { name: 'guillermo' }
        ]
    });

Или капитализировать имя первого пользователя при выводе:

    <p><%=: users | first | capitalize %></p>

## Список фильтров

Доступные на сегодня фильтры:

  - first
  - last
  - capitalize
  - downcase
  - upcase
  - sort
  - sort_by:'prop'
  - size
  - length
  - plus:n
  - minus:n
  - times:n
  - divided_by:n
  - join:'val'
  - truncate:n
  - truncate_words:n
  - replace:pattern,substitution
  - prepend:val
  - append:val
  - map:'prop'
  - reverse
  - get:'prop'

## Добавление фильтров

 Для добавления фильтров просто добавьте метод к объекту `.filters` :
 
```js
ejs.filters.last = function(obj) {
  return obj[obj.length - 1];
};
```

## Макеты

  В настоящий момент EJS не имеет понятия блоков, только compile-time `include`s,
  Однако вы все равно можете использовать эту функцию для реализации «макетов» 
  просто включая верхний и нижний колонтитулы::

```html
<% include head %>
<h1>Title</h1>
<p>My page</p>
<% include foot %>
```

## client-side support

  include `./ejs.js` or `./ejs.min.js` и `require("ejs").compile(str)`.

## License 

(The MIT License)

Copyright (c) 2009-2010 TJ Holowaychuk &lt;tj@vision-media.ca&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

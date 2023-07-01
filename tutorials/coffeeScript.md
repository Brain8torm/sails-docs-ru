# Использование CoffeeScript в приложении на Sails

**Рекомендуемым языком для разработки Node.js+Sails приложений является JavaScript.**

Впрочем Sails также поддерживает использование CoffeeScript для написания вами кода различных приложений (вроде [действий](http://www.sailsjs.com/documentation/concepts/actions-and-controllers) и [моделей](http://www.sailsjs.com/documentation/concepts/core-concepts-table-of-contents/models-and-orm)).  Вы можете включить поддержку этого языка в три действия:

1. Выполните `npm install coffee-script --save` в папке вашего приложения.
2. Добавьте следующую строку в начало  файла `app.js`:
```javascript
require('coffee-script/register');
```
3. Запустите приложение командой `node app.js` вместо `sails lift`.

### Использование генераторов CoffeeScript

Если вы хотите использовать CoffeeScript для написания файлов контроллеров, моделей или конфигурации, просто следуйте этим инструкциям:
 1. Установите генератор для CoffeeScript (не обязательно): <br/>`npm install --save-dev sails-generate-controller-coffee sails-generate-model-coffee`
 2. Для генерации демонстрационного кода, добавьте `--coffee` когда используете один из поддерживаемых генераторов в командной строке:
```bash
sails generate api <foo> --coffee
# Генерирует api/models/Foo.coffee и api/controllers/FooController.coffee
sails generate model <foo> --coffee
# Генерирует api/models/Foo.coffee
sails generate controller <foo> --coffee
# Генерирует api/controllers/FooController.coffee
```

<docmeta name="displayName" value="Использование CoffeeScript">

# Использование MongoDB с Node.js/Sails.js

Sails поддерживает популярную [базу данных MongoDB](https://www.mongodb.com/) с помощью [адаптера sails-mongo](https://www.npmjs.com/package/sails-mongo).

> Сперва убедитесь в наличии доступа к запущенному серверу MongoDB, либо на локальной машине, либо в облаке.  Эта строка 'mongodb://root@localhost/foo' относится к локально установленной MongoDB, используя "foo" в качестве имени базы данных.  Убедитесь, что заменили [URL подключения](https://sailsjs.com/documentation/reference/configuration/sails-config-datastores#?the-connection-url) соответствующей строкой для вашей базы данных.

### Локальная разработка с MongoDB

Для использования MongoDB в вашем Node.js/Sails приложении во время разработки:

1. Выполнить `npm install sails-mongo` папке вашего приложения.
2. В вашем файле `config/datastores.js`, измените `default` конфигурацию хранилища данных:

    ```js
    default: {
      adapter: 'sails-mongo',
      url: 'mongodb://root@localhost/foo'
    }
    ```
3. В файле `config/models.js`, измените атрибут `id`, чтобы иметь соответствующие `type` и `columnName` для первичных ключей MongoDB:

    ```js
    attributes: {
      id: { type: 'string', columnName: '_id' },
      //…
    }
    ```

Вот и все!  Снова запустите свое приложение, и все будет в порядке.

#### Чувствительность к регистру

После настройки вашего проекта на использование MongoDB, вы можете заметить, что ваши Waterline [запросы](https://sailsjs.com/documentation/reference/waterline-orm/queries) не чувствительны к регистру по умолчанию. Чтобы сделать запросы чувствительными к регистру, можете использовать [`.meta({makeLikeModifierCaseInsensitive: true})`](https://sailsjs.com/documentation/reference/waterline-orm/queries/meta).

### Развертывание вашего приложения с помощью MongoDB

Чтобы использовать MongoDB в рабочей среде, отредактируйте настройки вашего адаптера в `config/env/production.js`:

```js
adapter: 'sails-mongo',
```

Вы также можете настроить свой [URL подключения](https://sailsjs.com/documentation/reference/configuration/sails-config-datastores#?the-connection-url) -- однако многие разработчики предпочитают не проверять конфиденциальные учетные данные в системе управления версиями.  Другим вариантом является использование переменной окружающей среды:

```
sails_datastores__default__url=mongodb://heroku_12345678:random_password@ds029017.mLab.com:29017/heroku_12345678
```

> Чтобы использовать MongoDB в вашей промежуточной среде, измените `config/env/staging.js`.  В зависимости от вашего приложения, возможно, будет приемлемо проверить учетные данные вашей промежуточной базы данных в системе управления версиями, поскольку они представляют меньшую угрозу безопасности.


### Низкоуровневое использование MongoDB (расширенное)

Как и во всех [адаптерах баз данных Sails](https://sailsjs.com/documentation/concepts/extending-sails/adapters/available-adapters), вы можете использовать любые из [методов моделей Waterline](https://sailsjs.com/documentation/reference/waterline-orm/models) для взаимодействия с вашими моделями, когда используется `sails-mongo`.

Для многих приложений это все, что вам понадобится - от "hello world" до продакшена.  Даже, если вы столкнетесь с ограничениями, обычно их можно обойти без написания кода, специфичного для Mongo. Однако в ситуациях, когда альтернативы нет, можно использовать драйвер Mongo непосредственно в вашем приложении Sails.

Чтобы напрямую получить доступ к низкоуровневому &ldquo;нативному&rdquo; клиенту MongoDB, используйте свойство [`.manager`](https://sailsjs.com/documentation/reference/waterline-orm/datastores/manager) [экземпляра хранилища данных](https://sailsjs.com/documentation/reference/application/sails-get-datastore).

Начиная с `sails-mongo` v2.0.0 и выше, вы можете получить доступ к объекту [`MongoClient`](https://mongodb.github.io/node-mongodb-native/3.5/api/MongoClient.html) через `manager.client`. Это даст доступ к последним улучшениям MongoDB, таких как [`ClientSession`](https://mongodb.github.io/node-mongodb-native/3.5/api/ClientSession.html),
а вместе с этим к транзакциям, [изменению потоков](https://mongodb.github.io/node-mongodb-native/3.5/api/ChangeStream.html) и другим новым особенностям.

```js
var mongoClient = Pet.getDatastore().manager.client;

var results = await mongoClient.db('test')
.collection('pet')
.find({}, { name: 1 })
.toArray();

console.log(results);
```

Для получения полного списка методов, доступных в собственном клиенте MongoDB, смотрите [справочник API Node.js MongoDB Driver](https://mongodb.github.io/node-mongodb-native/3.5/api/Collection.html).


<docmeta name="displayName" value="Использование MongoDB">

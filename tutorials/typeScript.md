# Использование TypeScript в Sails приложении

**Рекомендуемым языком для создания приложений Node.js+Sails является JavaScript.**

Но Sails также поддерживает использование TypeScript для написания пользовательского кода приложения (например [действия](http://www.sailsjs.com/documentation/concepts/actions-and-controllers) и [модели](https://sailsjs.com/documentation/concepts/models-and-orm)).  Вы можете включить его поддержку всего за несколько шагов:

1. Выполните команду `npm install typescript ts-node --save` в папке проекта.
2. Установите необходимые типизации для вашего приложения. По крайней мере те, которые вы, вероятно, захотите:
   ```
   npm install @types/node --save
   npm install @types/express --save
   ```
3. AДобавьте следующую строку в начало файла `app.js`:
```javascript
require('ts-node/register');
```
4. Запустите приложение с помощью `node app.js` вместо `sails lift`.

Чтобы вы начали, вот пример традиционного Sails [контроллера](https://sailsjs.com/documentation/concepts/actions-and-controllers), написанного наTypescript, любезно предоставленный [@oshatrk](https://github.com/oshatrk):

```typescript
// api/controllers/SomeController.ts
declare var sails: any;

export function hello(req:any, res:any, next: Function):any {
  res.status(200).send('Hello from Typescript!');
}
```

Чтобы попробовать этот пример, настройте маршрутизатор так, чтобы его цель указывала на `someController.hello`, повторно запустите, а затем перейдите к маршруту в вашем браузере или с помощью такого инструмента, как Postman.

<docmeta name="displayName" value="Использование TypeScript">

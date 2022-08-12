## **1. Creating the Fake Server**

1. создать пустую папку и зайти в неё:
	- `mkdir fake-server`
	- `cd ./fake-server`

2. создать пустой файл server.js
  - `cat server.js`

3. Инициализация Node.js
  - `npm init`
  - отвечаем всё ОК

4. Установка модуля сервера:
	- `npm i json-server`

5. Создать/отредактировать свой файл: **package.json**

    <details>
    <summary>package.json</summary>

    ```js
    {
      "name": "fake-server",
      "version": "1.0.0",
      "description": "fake server with fake database",
      "main": "server.js",
      "scripts": {
        "start": "node server.js"
      },
      "author": "IURII BUDARIN",
      "license": "ISC",
      "dependencies": {
        "json-server": "^0.17.0"
      }
    }
    ```
    </details>

6. create **.gitignore** file and add node_modules

7. add in file **server.js**:

    <details>
    <summary>server.js</summary>

    ```js
      const jsonServer = require('json-server');
      const server = jsonServer.create();
      const router = jsonServer.router('db.json'); // <== Will be created later
      const middlewares = jsonServer.defaults();
      const port = process.env.PORT || 3200; // <== You can change the port

      server.use(middlewares);
      server.use(router);

      server.listen(port);
    ```
    </details>

8. Идём в свой github и создаём новый репо. После чего добавляем свой файлы в github:
  ```
    git init
    git commit -m "first commit"
    git branch -M main
    git remote add origin https://github.com/ynbudarin/fake-server.git
    git push -u origin main
  ```

## **2. Creating the DataBase**

1. Create **db.json** file
	`cat db.json`
	
  <details>
  <summary>db.json</summary>

    ```json
      {
      "users": [
        {
          "id": 1,
          "first_name": "Justina",
          "last_name": "Ginglell",
          "email": "jginglell0@networkadvertising.org",
          "gender": "Female"
        },
        {
          "id": 2,
          "first_name": "Marion",
          "last_name": "Jenman",
          "email": "mjenman1@surveymonkey.com",
          "gender": "Male"
        },
        {
          "id": 10,
          "first_name": "Ingrim",
          "last_name": "Offen",
          "email": "ioffen9@slideshare.net",
          "gender": "Male"
        }
      ]
    }
    ```
  </details>

2. push db.json file to github
  ```
	git add .
	git commit -m "creating the database"
	git push
  ```

3. Локальная проверка (запускаем сервер):
	`npm start`
	- зайти по адресу http://127.0.0.1:3000
	- остановить: CTRL+C


## **3. Перенос сервера в HEROKU**

1. создать аккаунт (Я создал: fake-server-2022)
  - создать своё свой [новый app](https://dashboard.heroku.com/apps)

2. Для простоты выбираю Deployment method - GitHub (я привязываю heroku к свою аккаунту)
  - по имени ищу свой репо в github и подключаю

3. соглашаюсь Automatic deploys

4. Нажать Deploy Branch
	- должна пойти компиляция (там же можно глянуть логи самого процесса)

5. Теперь наша ссылка готова: https://fake-server-2022.herokuapp.com/

## **4. Push local changes**
  - внёс изменения локально (в своём VS Code)
	- проверил локально через heroku, что всё работает: - `heroku local web`
	- запушил в свой github:
			`git add .`
			`git commit -m "change to be a async race app"`
			`git push`
	- идём в heroku (через heroku ps:scale web=0 не удалось обновиться)
	- делаем Manual deploy и ждём сообщения ниже на странице: Your app was successfully deployed.

## **5. Отладка**
  - согласно инструкции надо [скачать heroku CLI](https://devcenter.heroku.com/articles/getting-started-with-nodejs?singlepage=true)
  - `heroku login` - залогинимся и можно работать

1. [Полезное](https://devcenter.heroku.com/articles/getting-started-with-nodejs?singlepage=true#declare-app-dependencies)

  - `git --version`
  - `npm --version`
  - `node --version`

2. посмотреть логи по нашему приложению:
  - `heroku logs --tail --app fake-server-2022`

3. Наш app работает в контейнере Dyno и может уснуть. Проверим его статус:
  - `heroku ps --app fake-server-2022`
  - По умолчанию ваше приложение развертывается на бесплатном динамометрическом стенде. Бесплатные динамометры засыпают через полчаса бездействия (если они не получают никакого трафика).

4. Как запустить или вырубить использование Dyno-контейнера:
  - `heroku ps:scale web=1 --app fake-server-2022` - стартануть
  - `heroku ps:scale web=0 --app fake-server-2022` - остановить

5. Зайти в консоль контейнера нашего приложения
  - `heroku run bash --app fake-server-2022`

6. Запустить своё приложение локально (средствами Heroku CLI):
  - `heroku local web`
  - надо находиться в папке с файлами
  - heroku пофиг на ваш порт, так что адрес будет http://localhost:5000

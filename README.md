# Запуск приложения:
из корневой папки проекта (там где находится app.js) 
    в командной строке ввести по порядку:

### 1) Установка серверных зависимостей 
```bash
npm install
```

### 2) Установка клиентских зависимостей и создание оптимизированной сборки  (запускает "npm run client:install" и "npm run client:build") 
```bash
npm run install
```                            

### 3) Запуск приложения
```bash
npm run start
```
должно выдать   
```bash
APP: started
SERVER: Server started on port 5000
```



# Особенности записи исполняемых скриптов:

### 1) Используемая библиотека 

   Была использована библиотека VM2 (https://www.npmjs.com/package/vm2)

   Как выяснилось работать с прототипами она запрещает из соображений безопасности.
   Так же в ней нельзя использовать внешние модули (   "const fs = require('fs')" написать не получится, да и оно вроде не должно быть нужно ) 

### 2) Особенности языка и базы данных

  Так как в скрипте используются функции, взаимодействующие с базами данных, то nodejs требуется на это время. 
  Но из-за особенностей языка если просто вызвать эти функции, они выдадут "[object Promise]"(обещание) вместо желаемого значения.
  Поэтому нужно использовать структуру "async await" - в асинхронной функции дожидаемся ответа,
                            пример: 
```javascript
async function newValue  (dt) {
    var value = await AIO.const(dt, (...) )
    return value 
}
```

(подробнее: https://learn.javascript.ru/async-await,
            https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/async_function)                                                               

В дочернем процессе в модуле я уже задал глобальную асинхронную функцию в которой выполняется скрипт:
```javascript
const start = async ()=> {
    ${calc.c_script}    
}
start()
```
это позволяет вводить в самом скрипте функции с await без добавления лишней глобальной фукнции (если например нужна глобальная константа)
но если добавляете свои функции с получением данных, то обязательно нужно делать их асинхронными.

вот пример из вашего тестового скрипта: 
```javascript
let gv = async function(parameter_id) {
    const value = await AIO.val(parameter_id, from, to, 'AVG') 
    return value
}

let fact = await gv('10_1189_18');
```
## На всякий случай еще раз список функций требующих await:
 * await AIO.val(parameterId, ts)
 * await AIO.val(parameterId, from, to, aggregate)
 * await AIO.valx(parameterId, ts)
 * await AIO.valx(parameterId, from, to, aggregate)
 * await AIO.const(constId, ts, object, tag)
<<<<<<< HEAD
 * await AIO.saveToJournal(journalId, ts, tag, value, details)
=======
 * await AIO.saveToJournal(journalId, ts, tag, value, details)
>>>>>>> 3096e792f707066c45428ca29dac35b7f07b0835

### DaData Подсказки по ФИО

Коллекция запросов к API [ДаДаты](https://dadata.ru/) - подсказки по ФИО. Проверка параметра count.\
К кажому запросу написаны тесты на JS для проверки статус-кода и тела ответа.

Использованы переменные: 
- content-type
- token
- array
- expected_count

Использован общий скрипт для коллекции и скрипты к каждому запросу.\
Content-Type ответа может быть предустановлен в переменной "content-type":  application/json или application/xml

#### Примеры скриптов:

##### Post-response script для коллекции:

Скрипт подготавливает данные ответа для последующей проверки в тестах. 
```
let jsonObject; 
let array; // будущий массив с подсказками

let contentType = pm.collectionVariables.get("content-type");

if (pm.response.code === 400) {
    return; // тесты для проверки кода 400 не используют массив с подсказками
}

if (contentType === 'application/xml') {
    jsonObject = xml2Json(responseBody);
    array = jsonObject.SuggestResponse.suggestions;
} else {
    jsonObject = pm.response.json();
    array = jsonObject.suggestions;
}

pm.collectionVariables.set("array", array);
```
##### Post-response script теста 02_fio_count_11 (значение параметра count 11):
```
pm.environment.set("expected_count", 11);

pm.test('Response code must be equal 200', () => {
    pm.expect(pm.response.code).eql(200);
});

let arr = pm.collectionVariables.get("array");

pm.test('Number of suggestions must be equal to expected value', () => {
    expectedCount = pm.environment.get('expected_count');
    pm.expect(arr.length).eql(expectedCount);
});
```

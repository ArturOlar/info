### Indexing (Create) document with auto generated ID:

```
POST /products/_doc
{
  "name": "Coffee Maker",
  "price": 64,
  "in_stock": 10
}
```

---

### Indexing (Create) document with custom ID:

```
PUT /products/_doc/100
{
  "name": "Toaster",
  "price": 49,
  "in_stock": 4
}
```

---

### Delete document

```
DELETE /products/_doc/101
```

---

### Retrieving documents by ID

```
GET /products/_doc/100
```

---

### Adding a new field to the Document

```
POST /products/_update/100
{
  "doc": {
    "tags": ["electronics"]
  }
}
```

---

### Updating an existing field

```
POST /products/_update/100
{
  "doc": {
    "in_stock": 3
  }
}
```

---

### Scripts

В реквесті ми можемо писати скрипт. Скрипт дозволяє нам застосовувати якусь специфічну логіку в залежності від наших вимог. <br>
Для того щоб писати скрипт, в тілі реквеста потрібно вказати `script`, далі вказати `source` і де написати скрипт. <br>
Значення `ctx` - мається на увазі `context`. Тобто якщо подивитись на приклад нижче, ми хочемо оновити значення `in_stock` в документа 100 в індексі `products`. <br>
Для цього вказуємо `ctx` - що означає контекст документа, далі `_source.in_stock--` означає що в цьому документі зменшуємо значення `in_stock` на 1

Приклади:

- Зменшити значення `in_stock` на 1
```
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock--"
  }
}
```

- Вказати значення `in_stock` = 10
```
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock = 10"
  }
}
```

- Вказати значення `in_stocke` за допомогою параметрів. Це може бути корисно, якщо значення `in_stock` динамічне, тобто воно задається по якійсь логіці
```
POST /products/_update/100
{
  "script": {
    "source": "ctx._source.in_stock -= params.quantity",
    "params": {
      "quantity": 4
    }
  }
}
```

---

### Upserts

Означає що: якщо документ існує, то застосується скрипт (тобто відбудеться increase поля in_stock), якщо документа не існує, то відбується створення документа

```
POST /products/_update/101
{
  "script": {
    "source": "ctx._source.in_stock++"
  },
  "upsert": {
    "name": "Blender",
    "price": 399,
    "in_stock": 5
  }
}
```

---

### Оновити декілька документів за допомогою query 

Замість `match_all` - можемо вказати будь яке query (умови(a) по якій шукати документи) яке поверне список документів, в яких потрібно зменшити поле `in_stock` на 1
```
POST /products/_update_by_query
{
  "script": {
    "source": "ctx._source.in_stock--"
  },
  "query": {
    "match_all": {}
  }
}
```

---

### Видалити декілька документа за допомогою query

Замість `match_all` - можемо вказати будь яке query (умови(a) по якій шукати документи) яке поверне список документів, які потрібно видалити
```
POST /products/_delete_by_query
{
  "query": {
    "match_all": { }
  }
}
```

---

### Batch processing

Batch processing - дозволяє виконувати декілька дії в одному реквесті, він використовує форма NDJSON. Цей формат трохи специфічний, приклади будуть нижче. В batch processing ми можемо використовувати дії: `index`, `create`, `update`, `delete`. <br>
Є різниця між `indexing` та `create` документів. Вцілому, і одне і інше створює документ. Але у випадку із `index` - якщо документ існує, то він його оновить, а у випадку із `create`, якщо документ існує і ми спробуємо створити іще один із таким самим `_id` - то буде помилка

Щоб виконати batch операцію, потрібно:
- Content-Type: application/x-ndjson
- Кожний рядок в боді реквеста повинен закінчуватись на \n або \r\n
- Якщо якась дія зафейлилась, на інші дії це не впливає, вони будуть собі виконуватись. В респонсі ми отримаємо результат по кожній дії, чи вона була успішна чи ні.

---

#### Indexing documents

```
POST /_bulk
{ "index": { "_index": "products", "_id": 200 } }
{ "name": "Espresso Machine", "price": 199, "in_stock": 5 }
{ "create": { "_index": "products", "_id": 201 } }
{ "name": "Milk Frother", "price": 149, "in_stock": 14 }
```

Як видно із прикладу вище, в першому рядку вказуємо дію яку хочемо виконати: `index`; вказуємо ім'я індекса: `products`; та id докумета - 200. <br>
В другому рядку, вказуємо просто ключ та значення
В третьому рядку вказуємо дію `create` і так само ім'я індекса та id документа
І в четвертому рядку, вказуємо просто ключ та значення

---

#### Updating and deleting documents

```
POST /_bulk
{ "update": { "_index": "products", "_id": 201 } }
{ "doc": { "price": 129 } }
{ "delete": { "_index": "products", "_id": 200 } }
```

Також, можна вказати API `POST products/_bulk`, щоб не вказувати ім'я індекса в реквесті. Це при умові, якщо ми виконуємо операції в межах індекса який вказаний в API 
```
POST /products/_bulk
{ "update": { "_id": 201 } }
{ "doc": { "price": 129 } }
{ "delete": { "_id": 200 } }
```
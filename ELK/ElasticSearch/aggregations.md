### Aggregations

В еластіку є види агрегацій:
- `Metric Aggregations` - калькулює дані по таких метриках як: `sum, avg, min, max, etc.`
- `Bucket Aggregations` - групує документи в сегменти на основі значень полів чи інших критеріїв. В найпростішому вигляді, про це можна думати як про `GROUP BY` в mysql. Коли еластік агрегує дані, то створює так званий `bucket`, це по суті результат даних. Далі, ми можемо над цим результатом даних проводити якісь іще додаткові агрегації (див Nested Aggregation)

---

### Metric Aggregations

Базові функції для агрегацій:
- `sum` - загальна сума
- `avg` - середнє значення
- `min` - мінімальне значення
- `max` - максимальне значення
- `cardinality` - вираховує кількість унікальних значень. Аналогія в sql це функція `distinct()`. Приклад в sql `SELECT COUNT(DISTINCT color) FROM cars`, тобто, ми отримуємо саме кількість різних кольорів, а не назви різних кольорів.
- `value_count` - вираховує кількість документів. Наприклад `SELECT COUNT(*) FROM cars` або `SELECT COUNT(*) FROM cars WHERE color = blue`
- решту можна знайти в документації https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics.html

На прикладі нижче, отримуємо суму значень по полю `total_amount`. В прикладі нижче `total_sales` означає просто ключ під яким буде відображена сума. Note: також вказуємо `size = 0`, щоб не отримувати документи, а тільки результат агрегації.

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "total_sales": {
      // can be: sum, avg, max, min
      "sum": {
        "field": "total_amount"
      }
    }
  }
}
```

Також, можна робити агрегацію в різних розрізах

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "total_sales": {
      "sum": {
        "field": "total_amount"
      }
    },
    "avg_sales": {
      "avg": {
        "field": "total_amount"
      }
    },
    "min_sales": {
      "min": {
        "field": "total_amount"
      }
    },
    "max_sales": {
      "max": {
        "field": "total_amount"
      }
    }
  }
}
```

Також, якщо хочемо отримати інфу по всіх агрегаціях, то можемо використати наступний запит

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "amount_stats": {
      "stats": {
        "field": "total_amount"
      }
    }
  }
}
```

---

### Bucket Aggregations

- `terms` - агрегує дані для кожного унікального поля. Наприклад `SELECT color, count(*) FROM cars GROUP BY color`

---

#### Terms Приклад

На прикладі нижче групуємо всі документи по полю статус. <br>
- можемо вказати ключ `missing`, для полів в яких відсутнє або null поле status. <br>
- також, можемо вказати ключ `min_doc_count`, яке означає що в результат будуть потрапляти тільки ті документи, де їх кількість більше ніж 300.
- також, можемо вказати ключ `"order": { "_key/_count": { "asc/desc" } }`, який означає що ми хочемо сортувати дані.

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "amount_stats": {
      "terms": {
        "field": "status",
        "missing": "N/A",
        "min_doc_count": 0,
        "order": {
          "_count": "desc"
        }
      }
    }
  }
}
```

---

#### Nested Aggregation

На прикладі вище, ми отримаємо результат погрупований по статусах, наприклад ось так воно буде виглядати:
```
{
  "key": "processed",
  "doc_count": 208
},
{
  "key": "completed",
  "doc_count": 204
},
```

Ми також можемо розширити статистику, наприклад включити `stats` для кожного статусу замовлення із прикладу вище. Це можна зробити таким чином:

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "status_terms": {
      "terms": {
        "field": "status"
      },
      "aggs": {
        "status_stats": {
          "stats": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

Тобто, групуємо дані по полю `status`, та потім генеруємо статистику для кожного статусу по полю `total_amount`. Результат буде наступний:

```
{
  "key": "processed",
  "doc_count": 208,
  "status_stats": {
    "count": 208,
    "min": 10.27,
    "max": 281.77,
    "avg": 109.0629326923077,
    "sum": 22685.09
  }
},
{
  "key": "completed",
  "doc_count": 204,
  "status_stats": {
    "count": 204,
    "min": 10.93,
    "max": 260.59,
    "avg": 113.54058823529411,
    "sum": 23162.28
  }
},
```

---

#### Range aggregation

Можемо також групувати дані по діапазону. Наприклад, в нас є поле `total_amount` яке є типом даних `int`, і ми створюємо 3 бакети (до 50, 50-100, від 100). В цьому випадку в нас буде результат по 3 бакетах, і по цих бакетах так само можемо отримати статистичну інформацію (як було показано вище)  

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "amount_distribution": {
      "range": {
        "field": "total_amount",
        "ranges": [
          {
            "to": 50
          },
          {
            "from": 50,
            "to": 100
          },
          {
            "from": 100
          }
        ]
      }
    }
  }
}
```

Також, можна будувати подібний range по полях із типом даних `date`

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "purchased_ranges": {
      "date_range": {
        "field": "purchased_at",
        "ranges": [
          {
            "from": "2016-01-01",
            "to": "2016-01-01||+6M"
          },
          {
            "from": "2016-01-01||+6M",
            "to": "2016-01-01||+1y"
          }
        ]
      }
    }
  }
}
```
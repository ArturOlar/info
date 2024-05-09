### Add field mappings for `reviews` index

```
PUT /reviews
{
  "mappings": {
    "properties": {
      "rating": { "type": "float" },
      "content": { "type": "text" },
      "product_id": { "type": "integer" },
      "author": {
        "properties": {
          "first_name": { "type": "text" },
          "last_name": { "type": "text" },
          "email": { "type": "keyword" }
        }
      }
    }
  }
}
```

### Index a `reviews` document

```
PUT /reviews/_doc/1
{
  "rating": 5.0,
  "content": "Outstanding course! Bo really taught me a lot about Elasticsearch!",
  "product_id": 123,
  "author": {
    "first_name": "John",
    "last_name": "Doe",
    "email": "johndoe123@example.com"
  }
}
```

--- 

### Retrieving mappings for the `reviews` index

`GET /reviews/_mapping`

### Retrieving mapping for the `content` field

`GET /reviews/_mapping/field/content`

### Retrieving mapping for the `author.email` field

`GET /reviews/_mapping/field/author.email`

---

### Using dot notation for the `author` object

Це аналогія створенні мапінгу для об'єктів використовуючи `.` як ієрархію об'єкта

```
PUT /reviews_dot_notation
{
  "mappings": {
    "properties": {
      "rating": { "type": "float" },
      "content": { "type": "text" },
      "product_id": { "type": "integer" },
      "author.first_name": { "type": "text" },
      "author.last_name": { "type": "text" },
      "author.email": { "type": "keyword" }
    }
  }
}
```
---

### Add new field mapping to existing index

```
PUT /reviews/_mapping
{
  "properties": {
    "created_at": {
      "type": "date"
    }
  }
}
```
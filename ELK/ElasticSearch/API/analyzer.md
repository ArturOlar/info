### Get info about how analyzer analyze data using "standard" analyzer

```
POST /_analyze
{
  "text": "Some text!",
  "analyzer": "standard"
}
```

---

### Get info about how analyzer analyze data specifying analyzer components

```
POST /_analyze
{
  "text": "Some text!",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase"]
}
```
 
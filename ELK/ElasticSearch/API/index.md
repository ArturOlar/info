### Deleting an index

```
DELETE /index_name
```

---

### Creating an index (with settings)

```
PUT /index_name
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 2
  }
}
```

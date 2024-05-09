### Search all documents withing an index

```
GET /index_name/_search
{
  "query": {
    "match_all": {}
  }
}
```
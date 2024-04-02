```
GET /_sql/translate
{
  "query": "SELECT itemid FROM iteminfo_test WHERE creatorname = 'emmett'"
}

GET iteminfo_test/_search
{
  "size": 1000,
  "query": {
    "term": {
      "creatorname.keyword": {
        "value": "emmett"
      }
    }
  },
  "_source": false,
  "fields": [
    {
      "field": "itemid"
    },
    {
      "field": "creatorname"
    }
  ],
  "sort": [
    {
      "_doc": {
        "order": "asc"
      }
    }
  ]
}
```


#### 查询字段

```js
GET /hy_mock_data_five_minute_liuyin/_search
{
  "query": {
    "match_all": {}
  },
  "script_fields": {
    "id": {
      "script": {
        "lang": "painless",
        "source": """
          String rangeBegin = doc['rangeBegin'].value.toString();
          String monitorCode = doc['monitorCode.keyword'].value;
          String provinceName = doc['provinceName.keyword'].value;
          String cityName = doc['cityName.keyword'].value;
          return rangeBegin+"_"+monitorCode+"_"+provinceName+"_"+cityName;
        """
      }
    }
  }
}
```



#### 修改字段

```js
POST /hy_mock_data_five_minute_liuyin/_update_by_query
{
  "query": {
    "match_all": {}
  },
  "script": {
    "lang": "painless",
    "source": """
          String rangeBegin = ctx._source.rangeBegin.toString();
          rangeBegin = rangeBegin.replace(" ","").replace(":","").replace("-","");
          String monitorCode = ctx._source.monitorCode;
          String provinceName = ctx._source.provinceName;
          String cityName = ctx._source.cityName;
          String finalString =  rangeBegin+"_"+monitorCode+"_"+provinceName+"_"+cityName;
          ctx._source.id=finalString;
"""
  }
}
```

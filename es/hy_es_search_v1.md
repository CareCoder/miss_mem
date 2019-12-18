#### 省车辆来源地统计
```json
GET /liuyin_traffic_location/_search
{
  "size": "3",
  "query": {
    "bool": {
      "filter": {
        "range": {
          "rangeTime": {
            "gt": "2018-01-01 00:00:00",
            "lte": "2018-02-01 00:10:00"
          }
        }
      }
    }
  },
  "aggs": {
    "nested_city_list": {
      "nested": {
        "path": "provinceList"
      },
      "aggs": {
        "terms_province_names": {
          "terms": {
            "field": "provinceList.proviceName.keyword",
            "include": "四川",
            "size": 1000
          },
          "aggs": {
            "nested_province_list": {
              "nested": {
                "path": "provinceList.cityList"
              },
              "aggs": {
                "terms_city_names": {
                  "terms": {
                    "field": "provinceList.cityList.cityName.keyword",
                    "size": 1000
                  },
                  "aggs": {
                    "sum_count": {
                      "sum": {
                        "field": "provinceList.cityList.count"
                      }
                    }
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}
```


#### 全国车流量统计
```json
GET /liuyin_traffic_location/_search
{
  "size": "11",
  "query": {
    "bool": {
      "filter": {
        "range": {
          "rangeTime": {
            "gt": "2018-01-01 00:00:00",
            "lte": "2018-01-01 00:10:00"
          }
        }
      }
    }
  },
  "aggs": {
    "nested_city_list": {
      "nested": {
        "path": "provinceList"
      },
      "aggs": {
        "terms_city_names": {
          "terms": {
            "field": "provinceList.proviceName.keyword",
            "size": 1000,
            "order": {
              "city_list_counts.value": "desc"
            }
          },
          "aggs":{
            "city_list_counts":{
              "sum":{
                "field": "provinceList.count"
              }
            }
          }
        }
      }
    }
  }
}
```


#### 摄像头数据统计
```json
GET /liuyin_test_camera/_search
{
  "size": "0",
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "cameraCode.keyword": {
              "value": "A0001"
            }
          }
        }
      ],
      "filter": {
        "range": {
          "rangeTime": {
            "gt": "2018-01-02 00:00:00",
            "lte": "2018-01-02 23:59:59"
          }
        }
      }
    }
  },
  "aggs": {
    "camera_codes": {
      "terms": {
        "field": "cameraCode.keyword",
        "size": "1000",
        "order": {
          "counts.value": "desc"
        }
      },
      "aggs": {
        "counts": {
          "sum": {
            "field": "count"
          }
        }
      }
    }
  }
}
```

#### 按时间和摄像头统计车流量
```json
GET /liuyin_test_camera/_doc/_search
{
	"size": "0",
	"query": {
		"bool": {
			"filter": {
				"range": {
					"rangeTime": {
						"gte": "2018-08-14 00:00:00",
						"lte": "2018-08-14 02:00:00"
					}
				}
			}
		}
	},
	"aggs": {
		"city_name": {
			"date_histogram": {
				"field": "rangeTime",
				"interval": "1h",
				"format": "yyyy-MM-dd HH:mm:ss"
			},
			"aggs": {
				"count_sum": {
					"sum": {
						"field": "count"
					}
				}
			}
		}
	}
}
```


#### 每个摄像头的最新数据
```json
GET /liuyin_test_camera/_doc/_search
{
	"size": "0",
	"aggs": {
		"terms_camera_code": {
			"terms": {
				"field": "cameraCode.keyword",
				"size": 100
			},
			"aggs": {
				"top_range_time": {
					"top_hits": {
						"sort": {
							"rangeTime": {
								"order": "desc"
							}
						},
						"_source": {
							"includes": ["cameraCode", "rangeTime", "count", "tpi", "jamLength", "avgSpeed"]
						},
						"size": 1
					}
				}
			}
		}
	}
}
```

//按摄像头 时间粒度 的所有省的车流量统计数据
```json
GET /liuyin_traffic_location/_search
{
  "size": "1",
  "query": {
    "bool": {
      "must": {
        "terms": {
          "cameraCode.keyword": [
            "A0006",
            "A0003"
          ]
        }
      },
      "filter": {
        "range": {
          "rangeTime": {
            "gt": "2019-08-18 00:00:00",
            "lte": "2019-08-19 00:00:00"
          }
        }
      }
    }
  },
  "aggs": {
    "city_name": {
      "date_histogram": {
        "field": "rangeTime",
        "interval": "1h",
        "format": "yyyy-MM-dd HH:mm:ss"
      },
      "aggs": {
        "nested_city_list": {
          "nested": {
            "path": "provinceList"
          },
          "aggs": {
            "terms_city_names": {
              "terms": {
                "field": "provinceList.proviceName.keyword",
                "size": 1000
              },
              "aggs": {
                "sum_count": {
                  "sum": {
                    "field": "provinceList.count"
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}
```


//按摄像头 时间粒度 的所有市的车流量统计数据
```json
GET /liuyin_traffic_location/_search
{
  "size": "0",
  "query": {
    "bool": {
      "must": {
        "terms": {
          "cameraCode.keyword": [
            "A0001"
          ]
        }
      },
      "filter": {
        "range": {
          "rangeTime": {
            "gt": "2019-08-18 00:00:00",
            "lte": "2019-08-19 00:00:00"
          }
        }
      }
    }
  },
  "aggs": {
    "time_histogram": {
      "date_histogram": {
        "field": "rangeTime",
        "interval": "1h",
        "format": "yyyy-MM-dd HH:mm:ss"
      },
      "aggs": {
        "nested_city_list": {
          "nested": {
            "path": "provinceList.cityList"
          },
          "aggs": {
            "terms_city_names": {
              "terms": {
                "field": "provinceList.cityList.cityName.keyword",
                "size": 1000
              },
              "aggs": {
                "sum_count": {
                  "sum": {
                    "field": "provinceList.cityList.count"
                  }
                },
                "source_location": {
                  "top_hits": {
                    "_source": {
                      "includes": [
                        "provinceList.cityList.provinceName"
                      ]
                    },
                    "size":"1"
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}
```

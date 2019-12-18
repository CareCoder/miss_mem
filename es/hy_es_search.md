#### 省车辆来源地统计
```json
{
    "size": "0",
    "query": {
        "bool": {
            "must": {
                "term": {
                    "provinceName.keyword": "四川"
                }
            },
            "filter": {
                "range": {
                    "rangeTime": {
          						"gt": "2019-08-13 00:00:00",
          						"lte": "2019-08-14 00:00:00"
                    }
                }
            }
        }
    },
    "aggs": {
        "city_list": {
            "nested": {
                "path": "cityList"
            },
            "aggs": {
                "city_names": {
                    "terms": {
                        "field": "cityList.cityName",
                        "size": "1000",
                        "order" : {
                          "city_list_counts.value" : "desc"
                        }

                    },
                    "aggs": {
                        "city_list_counts": {
                            "sum": {
                                "field": "cityList.count"
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
{
	"size": "0",
	"query": {
		"bool": {
			"filter": {
				"range": {
					"rangeTime": {
            "gt": "2019-08-13 00:00:00",
            "lte": "2019-08-14 00:00:00"
					}
				}
			}
		}
	},
	"aggs": {
		"city_names": {
			"terms": {
				"field": "provinceName.keyword",
				"size": "1000",
				"order": {
					"city_list_counts.value": "desc"
				}

			},
			"aggs": {
				"city_list_counts": {
					"sum": {
						"field": "count"
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
		    {"term": {
		      "cameraCode": {
		        "value": "A0001"
		      }
		    }}
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
				"field": "cameraCode",
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
				"format": "yyyy-MM-dd HH:mm:ss",
				"order":{
				  "_key":"desc"
				}
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
				"field": "cameraCode",
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
							"include": ["cameraCode", "rangeTime", "count", "tpi", "jamLength", "avgSpeed"]
						},
						"size": 1
					}
				}
			}
		}
	}
}
```


POST test/_doc/1/_update
{
    "doc" : {
        "name" : "new_name"
    }
}

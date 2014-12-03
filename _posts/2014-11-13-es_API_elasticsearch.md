# ***es 1.0 API***
 
## 简述
>提供elasticsearch功能，透明化elasticsearch  

---

## 常用类

### MappingTypes
>  用于createMapping或getMapping时的实体类  
>
``` java
...
    private String fieldName;
    private BaseCoreTypes type;
    private BaseCoreIndexs index;
    private BaseCoreStores store;
    private BaseCoreDateFormats format;
    private BaseCoreIndexAnalyzers indexAnalyzer;
...
```
| Name         |   Resume     |
| :-------:    |:--------     |
| fieldName    | field的name  |
| type         | 类型          |
| index        | 是否使用分词插件|  
| store        | 是否存储      | 
| format       | 日期时间类型   | 
| indexAnalyzer| 分词插件      |


>#### 1. BaseCoreTypes
>指定field的type类型  
>
| Name      |   类型        |
| :-------: |:--------     |
| String    | 字符串        |
| Integer   | 整形          |
| Long      | 长整形        |  
| Date      | 日期          | 
| Float     | 单精度浮点数   | 
| Double    | 双精度浮点数   | 
| Boolean   | 布尔          | 
| Object    | object       | 
| Auto      | auto          | 
| Nested    | 嵌套          | 
| Ip        | ip            | 
| geo_point | 经纬度         | 


>#### 2. BaseCoreIndexs
>指定是否使用分词  
>
| Name         |  使用分词插件|
| :-------:    |:--------   |
| not_analyzed | 否         |
| analyzed     | 是         |

>#### 3. BaseCoreStores
>指定是否存储
>
| Name      |   Resume  |
| :-------: |:--------  |
| TRUE      | 是        |
| FALSE     | 否        |

>#### 4. BaseCoreDateFormats
>指定日期时间格式  
>
    简介略

>#### 5. BaseCoreIndexAnalyzers
>指定field使用的分词插件，在创建mapping的时候无需设置此项   
>需要设置分词插件只需BaseCoreIndexs选择analyzed  
>目前指定分词插件使用ik  
>该项用于getMapping时保存index_analyzer
>
| Name      |   Resume    |
| :-------: |:--------    |
| ik        | 使用ik分词插件 |


### DataFormats
>  CRUD的数据结构实体类
>
``` java
...
    private String field;
    private Object fieldValue;
    private Greater greater;
    private Object gValue;
    private Lesser lesser;
    private Object lValue;
    private Relastions rel;
    private Object lat;
    private Object lon;
    private String from;
    private String to;
    private boolean includeLower;
    private boolean includeUpper;
...
    public DataFormats(String field, Object fieldValue){
		this.field = field;
		this.fieldValue = fieldValue;
	}
	public DataFormats(String field,Greater greater,Object gValue){
		this.field = field;
		this.greater = greater;
		this.gValue = gValue;
	}
	public DataFormats(String field,Lesser lesser,Object lValue){
		this.field = field;
		this.lesser = lesser;
		this.lValue = lValue;
	}
	public DataFormats(String field,Greater greater,Object gValue,
			Lesser lesser,Object lValue,Relastions rel){
		this.field = field;
		this.lesser = lesser;
		this.lValue = lValue;
		this.greater = greater;
		this.gValue = gValue;
		this.rel = rel;
	}
	public DataFormats(String field,Object lat,Object lon,
			String from,String to,boolean includeLower,boolean includeUpper){
		this.field = field;
		this.lat = lat;
		this.lon = lon;
		this.from = from;
		this.to = to;
		this.includeLower = includeLower;
		this.includeUpper = includeUpper;
	}
...
```
| Name         |   Resume                      |
| :-------:    |:--------                      |
| field        | field的name                    |
| fieldValue   | field的value                   |
| greater      | 关系为大于或大于等于              |  
| gValue       | 大于或大于等于的值                | 
| lesser       | 关系为小于或小于等于              | 
| lValue       | 小于或小于等于的值                |
| rel          | greater和lesser关系为and或or    |
| lat          | 经度值                          |
| lon          | 纬度值                          |  
| from         | 从point标点指定开始范围，如"0km"   | 
| to           | 从point标点指定结束范围，如"100km" | 
| includeLower | 是否包含开始范围                  |
| includeUpper | 是否包含最大范围                  |


>#### 1. DataFormats.Greater
> 指定关系为大于或大于等于  
>
| Name      |   Resume  |
| :-------: |:--------  |
| gt        | 大于       |
| gte       | 大于等于    |

>#### 2. DataFormats.Lesser
>指定关系为小于或小于等于
>
| Name      |   Resume  |
| :-------: |:--------  |
| lt        | 小于       |
| lte       | 小于等于    |

>#### 3. DataFormats.Relastions
> 指定greater和lesser关系为and或or
>
| Name      |   Resume  |
| :-------: |:--------  |
| AND       | 与        |
| OR        | 或        |

>#### 4. DataFormats(String field, Object fieldValue)
>指定field:fieldvalue

>#### 5. DataFormats(String field,Greater greater,Object gValue)
>指定field的右区间

>#### 6. DataFormats(String field,Lesser lesser,Object lValue)
>指定field的左区间

>#### 7. DataFormats(String field,Greater greater,Object gValue,Lesser lesser,Object lValue,Relastions rel)
>指定field的左区间和右区间及左右区间的"与"或"或"的关系

>#### 8. DataFormats(String field,Object lat,Object lon,String from,String to,boolean includeLower,boolean includeUpper)
>指定经纬度、范围、是否包含范围始末

---

## 接口

### public String createIndices(String indices)
>功能：新定义一个indices
> #### 1.url
    http://ip:port/springes/createIndices?indices=indices
> #### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark   |
| :-------: |:--------:| :-------: |:--------:|
| indices   | String   | YES       |          |

> #### 3.返回
>
| Message                          |   Mark             |
| :-------:                        |:--------:          |
| "{200,Success}"                  |  成功               |
| "{10001,Unknow error}"           | 创建失败             |
| "{10005,Index is already exists}"|  indices已存在       |
| "{10002,No index defined}"       |  indices未定义   |

### public String createMapping(String indexName, String type, String fieldTypes)
>功能：定义mapping
> #### 1.url
    http://ip:port/springes/createMapping?indexName=indexName&type=type&fieldTypes=fieldTypes
> #### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| type      | String   | YES       |                |
| fieldTypes| String   | YES       | List\<MappingTypes>.toString()  |

>#### 3.返回
>
| Message                        |   Mark             |
| :-------:                      |:--------:          |
| "{200,Success}"                |  成功               |
| "{10001,Unknow error}"         | 创建失败             |
| "{10003,No type defined}"      |  type未定义    |
| "{10002,No index defined}"     |  index未定义   |
| "{10006,No index exists}"      |  index不存在       |
| "{10010,cannot be empty}"      |  fieldTypes不能为空|


### public String getMapping(String indexName, String type)
>功能：获取指定index和type的mapping
>#### 1.url
    http://ip:port/springes/getMapping?indexName=indexName&type=type
>#### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| type      | String   | YES       |                |

>#### 3.返回
>
| Message                        |   Mark             |
| :-------:                      |:--------:          |
| "{200,Success,mappingsStr}"    |  成功               |
| "{10001,Unknow error}"         | 获取过程中发生错误    |
| "{10003,No type defined}"      |  type未定义    |
| "{10002,No index defined}"     |  index未定义   |
| "{10006,No index exists}"      |  index不存在       |
| "{10007,No type exists}"       |  type不存在       |


### public String index(String indexName, String type, String dataFormatsStr)
>功能：插入一条记录
>#### 1.url
    http://ip:port/springes/index?indexName=indexName&type=type&dataFormatsStr=dataFormatsStr
>#### 2.描述
>
| Name          |   Type   |IsNecessary|   Mark         |
| :-------:     |:--------:| :-------: |:--------:      |
| indexName     | String   | YES       |                |
| type          | String   | YES       |                |
|dataFormatsStr | String   | YES       | List\<DataFormats>.toString()     |

>#### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success}"                |  成功                |
| "{10001,Unknow error}"         | 插入过程中发生错误      |
| "{10003,No type defined}"      |  type未定义       |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10007,No type exists}"       |  type不存在           |
| "{10010,cannot be empty}"      |  dataFormatsStr不能为空|


### public String bulkIndex(String indexName, String type, String jsonStr)
>功能：批量插入
>#### 1.url
    http://ip:port/springes/bulkIndex?indexName=indexName&type=type&jsonStr=jsonStr
>#### 2.描述
>
| Name          |   Type   |IsNecessary|   Mark         |
| :-------:     |:--------:| :-------: |:--------:      |
| indexName     | String   | YES       |                |
| type          | String   | YES       |                |
|dataFormatsStr | String   | YES       | List\<List\<DataFormats>>.toString()     |

>#### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success}"                |  成功                |
| "{10001,Unknow error}"         | 插入过程中发生错误      |
| "{10003,No type defined}"      |  type未定义       |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10007,No type exists}"       |  type不存在           |
| "{10010,cannot be empty}"      |  jsonStr|


### public String delete(String indexName, String type, String id)
>功能：删除指定index,type,id的一条数据
>#### 1.url
    http://ip:port/springes/delete?indexName=indexName&type=type&id=id
>#### 2.描述
>
| Name          |   Type   |IsNecessary|   Mark         |
| :-------:     |:--------:| :-------: |:--------:      |
| indexName     | String   | YES       |                |
| type          | String   | YES       |                |
| id            | String   | YES       |                |

>#### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success}"                |  成功                |
| "{10001,Unknow error}"         | 删除过程中发生错误      |
| "{10004,No Id defined}"        |  id未定义         |
| "{10003,No type defined}"      |  type未定义       |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10007,No type exists}"       |  type不存在           |


### public String deleteIndex(String indexName)
>功能：删除整个index
>#### 1.url
    http://ip:port/springes/deleteIndex?indexName=indexName
>#### 2.描述
>
| Name          |   Type   |IsNecessary|   Mark         |
| :-------:     |:--------:| :-------: |:--------:      |
| indexName     | String   | YES       |                |
>#### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success}"                |  成功                |
| "{10001,Unknow error}"         | 删除过程中发生错误      |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |


### public String deleteType(String indexName, String type)
>功能：根据index，删除指定type
>#### 1.url
    http://ip:port/springes/deleteType?indexName=indexName&type=type
>#### 2.描述
>
| Name          |   Type   |IsNecessary|   Mark         |
| :-------:     |:--------:| :-------: |:--------:      |
| indexName     | String   | YES       |                |
| type          | String   | YES       |                |
>#### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success}"                |  成功                |
| "{10001,Unknow error}"         | 删除过程中发生错误      |
| "{10003,No type defined}"      |  type未定义       |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10007,No type exists}"       |  type不存在           |


### public String deleteByQuery(String indexName, String type, String jsonStr)
>功能：删除符合jsonStr的数据
>#### 1.url
    http://ip:port/springes/deleteByQuery?indexName=indexName&type=type&jsonStr=jsonStr
>#### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| type      | String   | YES       |                |
|jsonStr    | String   | YES |List\<DataFormats>.toString()|
>#### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success}"                |  成功                |
| "{10001,Unknow error}"         | 删除过程中发生错误      |
| "{10003,No type defined}"      |  type未定义       |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10007,No type exists}"       |  type不存在           |
| "{10010,cannot be empty}"      |  jsonStr不能为空|


### public String queryById(String indexName, String id)
>功能：根据id查找数据
>### 1.url
    http://ip:port/springes/queryById?indexName=indexName&id=id
>### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| id        | String   | YES       |                |
>### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success,data}"           |  成功                |
| "{10001,Unknow error}"         | 查找过程中发生错误      |
| "{10004,No Id defined}"        |  id未定义         |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |


### public String queryByI(String indexName, String jsonStr)
>功能：根据index查找符合jsonStr的数据
>### 1.url
    http://ip:port/springes/queryByI?indexName=indexName&jsonStr=jsonStr
>### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| jsonStr   | String   | YES       |List\<DataFormats>.toString()|
>### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success,data}"           |  成功                |
| "{10001,Unknow error}"         | 查找过程中发生错误      |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10010,cannot be empty}"      |  jsonStr不能为空|


### public String queryByIT(String indexName,String type, String jsonStr)
>功能：根据index和type查找符合jsonStr的数据
>### 1.url
    http://ip:port/springes/queryByIT?indexName=indexName&type=type&jsonStr=jsonStr
>### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| type      | String   | YES       |                |
| jsonStr   | String   | YES       |List\<DataFormats>.toString()|
>### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success,data}"           |  成功                |
| "{10001,Unknow error}"         | 查找过程中发生错误      |
| "{10003,No type defined}"      |  type未定义       |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10007,No type exists}"       |  type不存在           |
| "{10010,cannot be empty}"      |  jsonStr不能为空|


### public String queryByIFS(String indexName,int from,int size, String jsonStr)
>功能：根据index查找符合jsonStr的数据，并指定从from开始，数量为size
>### 1.url
    http://ip:port/springes/queryByIFS?indexName=indexName&from=from&size=size&jsonStr=jsonStr
>### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| from      | int      | YES       |                |
| size      | int      | YES       |                |
| jsonStr   | String   | YES       |List\<DataFormats>.toString()|
>### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success,data}"           |  成功                |
| "{10001,Unknow error}"         | 查找过程中发生错误      |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10010,cannot be empty}"      |  jsonStr不能为空|


### public String queryByITFS(String indexName,String type,int from,int size, String jsonStr)
>功能：根据index和type查找符合jsonStr的数据，并指定从from开始，数量为size
>### 1.url
    http://ip:port/springes/queryByITFS?indexName=indexName&type=type&from=from&size=size&jsonStr=jsonStr
>### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| type      | String   | YES       |                |
| from      | int      | YES       |                |
| size      | int      | YES       |                |
| jsonStr   | String   | YES       |List\<DataFormats>.toString()|
>### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success,data}"           |  成功                |
| "{10001,Unknow error}"         | 查找过程中发生错误      |
| "{10003,No type defined}"      |  type未定义       |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10007,No type exists}"       |  type不存在           |
| "{10010,cannot be empty}"      |  jsonStr不能为空|


### public String queryByIForIds(String indexName, String jsonStr)
>功能：根据index查找符合jsonStr的数据的ID
>### 1.url
    http://ip:port/springes/queryByIForIds?indexName=indexName&jsonStr=jsonStr
>### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| jsonStr   | String   | YES       |List\<DataFormats>.toString()|
>### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success,data}"           |  成功                |
| "{10001,Unknow error}"         | 查找过程中发生错误      |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10010,cannot be empty}"      |  jsonStr不能为空|


### public String queryByITForIds(String indexName,String type, String jsonStr)
>功能：根据index和type查找符合jsonStr的数据的ID
>### 1.url
    http://ip:port/springes/queryByITForIds?indexName=indexName&type=type&jsonStr=jsonStr
>### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| type      | String   | YES       |                |
| jsonStr   | String   | YES       |List\<DataFormats>.toString()|
>### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success,data}"           |  成功                |
| "{10001,Unknow error}"         | 查找过程中发生错误      |
| "{10003,No type defined}"      |  type未定义       |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10007,No type exists}"       |  type不存在           |
| "{10010,cannot be empty}"      |  jsonStr不能为空|


### public String update(String indexName, String type, String id, String jsonStr)
>功能：更新指定id的数据
>### 1.url
    http://ip:port/springes/update?indexName=indexName&type=type&id=id&jsonStr=jsonStr
>### 2.描述
>
| Name      |   Type   |IsNecessary|   Mark         |
| :-------: |:--------:| :-------: |:--------:      |
| indexName | String   | YES       |                |
| type      | String   | YES       |                |
| id        | String   | YES       |                |
| jsonStr   | String   | YES       |List\<DataFormats>.toString()|
>### 3.返回
>
| Message                        |   Mark              |
| :-------:                      |:--------:           |
| "{200,Success,data}"           |  成功                |
| "{10001,Unknow error}"         | 查找过程中发生错误      |
| "{10004,No Id defined}"        |  id未定义         |
| "{10003,No type defined}"      |  type未定义       |
| "{10002,No index defined}"     |  index未定义      |
| "{10006,No index exists}"      |  index不存在          |
| "{10007,No type exists}"       |  type不存在           |
| "{10010,cannot be empty}"      |  jsonStr不能为空|


# MRS SDK

HuaWei OpenStack `Map Reduce` 服务SDK
- 服务入口: `conn.map_reduce`
- 服务类型: `map-reduce`

## API接口文档

请查阅 [官方接口文档](https://docs.otc.t-systems.com/en-us/api/mrs/en-us_topic_0037324628.html)


## 数据源接口
### 创建数据源
```python
ds = {
    "name": "SDK-unittest",
    "url": "/sdk/unittest/input",
    "is_protected": False,
    "is_public": False,
    "type": "hdfs",
    "description": ""
}
data_source = conn.map_reduce.create_data_source(**ds)
```

### 更新数据源
```python

updated = {
    "name": "SDK-test",
    "type": "hdfs",
    "url": "/user/hadoop/input1",
    "description": "This is public input",
    "is_protected": True
}
data_source = conn.map_reduce.update_data_source("data_source_id", **updated)
```


### 查询数据源列表
```python
query = dict(sort_by="-created_at", limit=20, marker="last-data-source-id")
data_sources = list(conn.map_reduce.data_sources(**query))
```

### 查询数据源详情
```python
data_source = conn.map_reduce.get_data_source("data-source-id")
```

### 删除数据源
```python
conn.map_reduce.delete_data_source("data-source-id")
```

### 集群管理接口
### 创建集群并执行作业
### 扩容集群节点
### 查询集群详情
### 终止集群

## 作业二进制对象
### 创建作业二进制对象
```python
binary = {
    "name": "SDK-unittests",
    "url": "/sdk/mapreduce/input1",
    "is_protected": False,
    "is_public": False,
    "description": ""
}
binary = conn.map_reduce.create_job_binary(**binary)
```

### 更新作业二进制对象
```python
updated = {
    "url": "/sdk/unittest/input1",
    "description": "SDK unittests"
    "name": "new-name",
}
job_binary = conn.map_reduce.update_job_binary("binary-id", **updated)
```

### 查询作业二进制列
```python
query = dict(sort_by="-created_at", limit=20, marker="last-job-binary-id")
job_binaries = list(conn.map_reduce.job_binaries(**query))
```

### 查询作业二进制详
```python
job_binary = conn.map_reduce.get_job_binary("job-binary-id")
```

### 删除作业二进制对象
```python
conn.map_reduce.delete_job_binary("job-binary-id")
```


## 作业对象接口
### 新增作业并执行
```python
# TODO
```

### 创建作业对象
```python
job = {
    "name": "SDK-unittests",
    "mains": [
        "job-binary-id-script"
    ],
    "libs": [
        "job-binary-id-input"
    ],
    "is_protected": False,
    "interface": [
    ],
    "is_public": False,
    "type": "MapReduce",
    "description": "SDK unittest, Feel Free to delete"
}
job = conn.map_reduce.create_job(**job)
```

### 更新作业对象
```python
updated = {
    "name": "new-name",
    "type": "Spark",
    "description": "SDK Unittets"
}
_job = conn.map_reduce.update_job("job-id", **updated)
```

### 执行作业对象
```python
_job_execution = {
    "cluster_id": "cluster-id",
    "input_id": "job-binary-input-id",
    "output_id": "job-binary-output-id",
    "is_protected": False,
    "is_public": False,
    "job_configs": {
        "configs": {
            "mapred.map.tasks": "1",
            "mapred.reduce.tasks": "1"
        },
        "args": [
            "wordcount ",
            "arg2"
        ],
        "params": {
            "param2": "value2",
            "param1": "value1"
        }
    }
}

job_execution = conn.map_reduce.execute_job("job-id", **_job_execution)
```

### 查询作业对象列表
```python
query = dict(sort_by="-created_at", limit=20, marker="last-job-id")
jobs = list(conn.map_reduce.jobs(**query))
```

### 查询作业对象详情
```python
conn.map_reduce.get_job("job-id")
```

### 查询作业exe对象列表
```python
# TODO
```

### 查询作业exe对象详情
```python
# TODO
```

### 删除作业对象
```python
conn.map_reduce.delete_job("job-id")
```


### 作业执行对象接口
### 查询作业执行对象列表
```python
  query = {
    "limit": 20,
    "sort_by": "-name",
    "marker": "job-id"
}
executions = list(conn.map_reduce.job_executions(**query))
```

### 查询作业执行对象详情
```python
job_execution = conn.map_reduce.get_job_execution("job-execution-id")
```

### 取消作业执
```python
job_execution = conn.map_reduce.cancel_job_execution("job-execution-id")
```

### 删除作业执行对象
```python
conn.map_reduce.delete_job_execution("job-execution-id")
```




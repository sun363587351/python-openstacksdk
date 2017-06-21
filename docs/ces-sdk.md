# CloudEyeService SDK

HuaWei OpenStack `Cloud Eye` service SDK
- service entry: `connection.cloud_eye`
- service type: `cloud-eye`

## API document
Not provided for now.

## initial SDK client
You can find how to initial SDK client in the [quickstart](huawei-sdk?id=_2-build-v3-client) page .

## Metric

### List Metric

> Query parameter ``dimensions`` could at most have three items

```python
query = {
    "namespace": "SYS.ECS",
    "metric_name": "cpu_util",
    "dimensions": [{
        "name": "instance_id",
        "value": "d9112af5-6913-4f3b-bd0a-3f96711e004d"
    }],
    "order": "desc",
    "marker": "SYS.ECS.cpu_util.instance_id:9f31d05a-76d5-478a-b864-b1b5e8708482",
    "limit": 10
}

metrics = connection.cloud_eye.metrics(**query)
for metric in metrics:
    logging.info(metric)
```

### List Favorite Metrics

```python
favorite_metrics = connection.cloud_eye.favorite_metrics()
for metric in favorite_metrics:
    logging.info(metric)
```


## Alarm

### List Alarm

```python
query = {
    "limit": 1,
    "marker": "last-alarm-id",
    "order": "desc"
}
for alarm in connection.cloud_eye.alarms(**query):
    logging.info(alarm)
```

### Get Alarm

> Most of get resource function support both ``plain`` ID or resource ``Instance`` with id parameter

```python
# plain ID
alarm = connection.cloud_eye.get_alarm("some-alarm-id")
```

> or

```python
# Instance with ID
alarm = connection.cloud_eye.get_alarm(alarm.Alarm(id="some-alarm-id"))
```


### Delete Alarm

> Most of delete resource function support both ``plain`` ID or ``Instance`` with id parameter

```python
connection.cloud_eye.delete_alarm("some-alarm-id")
```

> or

```python
connection.cloud_eye.delete_alarm(alarm.Alarm(id="some-alarm-id"))
```

### Enable Alarm

```python
connection.cloud_eye.enable_alarm("some-alarm-id")
```

> or

```python
connection.cloud_eye.enable_alarm(alarm.Alarm(id="some-alarm-id"))
```


### Disable Alarm

```python
connection.cloud_eye.disable_alarm("some-alarm-id")
```

> or

```python
connection.cloud_eye.disable_alarm(alarm.Alarm(id="some-alarm-id"))
```


## Metric Data

### Add Metric Data
```python
    def get_epoch_time(datetime_):
        if datetime_:
            seconds = time.mktime(datetime_.timetuple())
            return int(seconds) * 1000
        else:
            return None

    collect_time_1 = end_time = datetime.datetime(2017, 6, 21, hour=10, minute=30)
    collect_time_2 = end_time = datetime.datetime(2017, 6, 21, hour=10, minute=35)
    data = [
        {
            "metric": {
                "namespace": "MINE.APP",
                "dimensions": [
                    {
                        "name": "instance_id",
                        "value": "33328f02-3814-422e-b688-bfdba93d4050"
                    }
                ],
                "metric_name": "cpu_util"
            },
            "ttl": 604800,
            "collect_time": get_epoch_time(collect_time_1),
            "value": 60,
            "unit": "%"
        },
        {
            "metric": {
                "namespace": "MINE.APP",
                "dimensions": [
                    {
                        "name": "instance_id",
                        "value": "33328f02-3814-422e-b688-bfdba93d4050"
                    }
                ],
                "metric_name": "cpu_util"
            },
            "ttl": 604800,
            "collect_time": get_epoch_time(collect_time_2),
            "value": 70,
            "unit": "%"
        }
    ]
    connection.cloud_eye.add_metric_data(data)
```

### List Metric Aggregation

- `filter`: valid aggregation includes ``average``, ``variance``, ``min``, ``max``
- `from` & `to`: unix epoch milli seconds
- `period`:
    - `1`: real-time
    - `300`: every 5 minutes
    - `1200`: every 20 minutes
    - `3600`: every hour
    - `14400`: every 4 hour
    - `86400`: every day


```python
    def get_epoch_time(datetime_):
        if datetime_:
            seconds = time.mktime(datetime_.timetuple())
            return int(seconds) * 1000
        else:
            return None

    _from = datetime.datetime(2017, 6, 21, hour=10)
    _to = datetime.datetime(2017, 6, 21, hour=11)
    query = {
        "namespace": "MINE.APP",
        "metric_name": "cpu_util",
        "from": get_epoch_time(_from),
        "to": get_epoch_time(_to),
        "period": 300,
        "filter": "average",
        "dimensions": [{
            "name": "instance_id",
            "value": "33328f02-3814-422e-b688-bfdba93d4050"
        }]
    }
    for aggregation in connection.cloud_eye.metric_aggregations(**query):
        logging.info(aggregation)
```


## Quota

### List Quota

> currently, only ``Alarm`` quota is available

```python
quotas = connection.cloud_eye.quotas()
for quota in quotas:
    logging.info(quota)
```
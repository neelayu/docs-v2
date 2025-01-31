---
title: tasks.lastSuccess() function
description: The `tasks.lastSuccess()` function returns ...
menu:
  influxdb_cloud_ref:
    name: tasks.lastSuccess
    parent: InfluxDB tasks
weight: 301
---

The `tasks.lastSuccess()` function returns the time of last successful run of the
InfluxDB task or the value of the `orTime` parameter if the task has never successfully run.

```js
import "influxdata/influxdb/tasks"

tasks.lastSuccess(orTime: 2020-01-01T00:00:00Z)
```

## Parameters

### orTime
The default time value returned if the task has never successfully run.

_**Data type:** Time | Duration_

## Examples

##### Query data since the last successful task run
```js
import "influxdata/influxdb/tasks"

option task = {
  name: "Example task",
  every: 30m
}

from(bucket: "example-bucket")
  |> range(start: tasks.lastSuccess(orTime: -task.every))
  // ...
```
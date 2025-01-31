---
title: v1.tagValues() function
description: The `v1.tagValues()` function returns a list unique values for a given tag.
aliases:
  - /influxdb/v2.0/reference/flux/functions/influxdb-v1/tagvalues/
menu:
  influxdb_2_0_ref:
    name: v1.tagValues
    parent: InfluxDB v1
weight: 301
influxdb/v2.0/tags: [tags]
related:
  - /influxdb/v2.0/query-data/flux/explore-schema/
  - /{{< latest "influxdb" "v1" >}}/query_language/explore-schema#show-tag-values, SHOW TAG VALUES in InfluxQL
introduced: 0.16.0
deprecated: 0.88.0
---

{{% warn %}}
`v1.tagValues()` was deprecated in **Flux v0.88.0** in favor of
[`schema.tagValues()`](/influxdb/v2.0/reference/flux/stdlib/influxdb-schema/tagvalues/).
{{% /warn %}}

The `v1.tagValues()` function returns a list of unique values for a given tag.
The return value is always a single table with a single column, `_value`.

```js
import "influxdata/influxdb/v1"

v1.tagValues(
  bucket: "example-bucket",
  tag: "host",
  predicate: (r) => true,
  start: -30d
)
```

## Parameters

### bucket
Bucket to return unique tag values from.

_**Data type:** String_

### tag
Tag to return unique values from.

_**Data type:** String_

### predicate
Predicate function that filters tag values.
_Defaults to `(r) => true`._

_**Data type:** Function_

### start
Oldest time to include in results.
_Defaults to `-30d`._

Relative start times are defined using negative durations.
Negative durations are relative to now.
Absolute start times are defined using [time values](/influxdb/v2.0/reference/flux/language/types/#time-types).

_**Data type:** Duration_

## Examples
```js
import "influxdata/influxdb/v1"

v1.tagValues(
  bucket: "my-bucket",
  tag: "host",
)
```

## Function definition
```js
tagValues = (bucket, tag, predicate=(r) => true, start=-30d) =>
  from(bucket: bucket)
    |> range(start: start)
    |> filter(fn: predicate)
    |> keep(columns: [tag])
    |> group()
    |> distinct(column: tag)
```

_**Used functions:**
[from](/influxdb/v2.0/reference/flux/stdlib/built-in/inputs/from/),
[range](/influxdb/v2.0/reference/flux/stdlib/built-in/transformations/range/),
[filter](/influxdb/v2.0/reference/flux/stdlib/built-in/transformations/filter/),
[group](/influxdb/v2.0/reference/flux/stdlib/built-in/transformations/group/),
[distinct](/influxdb/v2.0/reference/flux/stdlib/built-in/transformations/selectors/distinct/),
[keep](/influxdb/v2.0/reference/flux/stdlib/built-in/transformations/keep/)_

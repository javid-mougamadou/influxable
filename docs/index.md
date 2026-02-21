---
layout: default
title: Influxable
description: A lightweight Python ORM / ODM / Client for InfluxDB
last_modified_at: 2026-02-21
keywords:
  - Javid Mougamadou Influxable InfluxDB Python ORM Driver
  - Javid Mougamadou
  - Influxable
  - InfluxDB Python
  - Python ORM Driver
  - ORM InfluxDB
  - documentation
  - installation
  - time series
---

![Influxable logo](assets/images/logo.svg)

# Influxable

[![PyPI version](https://img.shields.io/badge/pypi-1.4.0-blue)](https://pypi.org/project/influxable/)
[![Build status](https://img.shields.io/badge/build-passing-green)](https://github.com/Javidjms/influxable)
[![Code coverage](https://img.shields.io/badge/coverage-100-green)](https://github.com/Javidjms/influxable)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE.txt)

A lightweight Python ORM / ODM / Client for InfluxDB

## Table of Contents

- [Note](#note)
- [Genesis](#genesis)
- [Changelog](#changelog)
- [Features](#features)
- [Dependencies](#dependencies)
- [Installation](#installation)
- [Getting started](#getting-started)
- [Auto Generation of Measurements](#auto-generation-of-measurements)
- [Influxable commands](#influxable-commands)
- [Testing](#testing)
- [Supporting](#supporting)
- [License](#license)

## Note

This project is currently in development.

A better documentation and testing scripts will be added in the next release.

## Genesis

I worked on a project with InfluxDB. I needed to build an API for InfluxDB and to plug with Python libraries (scipy, pandas, etc ...).

That's why I decided to create this repository in order to deal with InfluxDB in a smooth way and to manipulate Python object.

## Changelog

### 1.4.0

- Add integration with Influxdb OSS 2.0 Authentication (Experimental)

### 1.3.0

- Add *group_by()* method for *GROUP BY tags* instructions
- Add *range()* method for *GROUP BY time()* instructions
- Add *into()* method for *INTO* instructions
- Add *tz()* method

### 1.2.1

- Handle chinese characters.

## Features

- Add automation for measurement class generation (command: *autogenerate*)
- Admin commands allowing to manage the database (ex: *create_user()*, *show_series()*)
- Measurement class allowing to make queries in order to fetch/save points (ex: *Measurement.where()*, *Measurement.bulk_save()*)
- Group by commands
- Different serializers for easy data manipulation (ex: *PandasSerializer*)

## Dependencies

- Python 3 (Tested with Python 3.7.3)
- InfluxDB (Tested with InfluxDB 1.5.4)

## Installation

The package is available on PyPI. You can install it via pip:

```bash
pip install influxable
```

## Getting started

### Connection

You can set your environment variable for the connection of InfluxDB in order to override the default values:

```bash
INFLUXDB_URL=http://localhost:8086
INFLUXDB_DATABASE_NAME=default

# Optional
INFLUXDB_USER=admin
INFLUXDB_PASSWORD=changme

# OSS 2.0
INFLUXDB_AUTH_TOKEN=mytoken
```

Then you just have to import the influxable package and create an instance of *Influxable*:

```python
from influxable import Influxable

client = Influxable()
```

You can also set connection variable in *Influxable* constructor:

```python
# Without authentication
client = Influxable(
    base_url='http://localhost:8086',
    database_name='default',
)

# With authentication
client = Influxable(
    base_url='http://localhost:8086',
    database_name='default',
    user='admin',
    password='changeme',
)

# With token authentication
client = Influxable(
    base_url='http://localhost:8086',
    database_name='default',
    token='my_token',
)
```

### Measurement

```python
from influxable import attributes, serializers
from influxable.measurement import Measurement

class TemperatureMeasurement(Measurement):
    parser_class = serializers.MeasurementPointSerializer  # Default
    measurement_name = 'temperature'

    time = attributes.TimestampFieldAttribute()
    phase = attributes.TagFieldAttribute()
    value = attributes.FloatFieldAttribute()
```

**Fields:** GenericFieldAttribute (IntegerFieldAttribute, FloatFieldAttribute, StringFieldAttribute, BooleanFieldAttribute), TagFieldAttribute, TimestampFieldAttribute, DateTimeFieldAttribute

**Parser Classes:** MeasurementPointSerializer (default), JsonSerializer, FormattedSerieSerializer, FlatFormattedSerieSerializer, FlatSimpleResultSerializer, PandasSerializer

### Simple Measurement

```python
from influxable.measurement import SimpleMeasurement

my_measurement = SimpleMeasurement('temperature', ['value'], ['phase'])
```

### Query

You can query with *Measurement.get_query()*:

```python
from influxable.db import Field

points = TemperatureMeasurement\
  .get_query()\
  .select('phase', 'value')\
  .where(
     Field('value') > 15.2,
     Field('value') < 30.5,
  )\
  .limit(100)\
  .evaluate()
```

### Saving Data

You can create data by using *Measurement.bulk_save()*

```python
points = [
    TemperatureMeasurement(phase="HOT", value=10, time=1463289075),
    TemperatureMeasurement(phase="COLD", value=10, time=1463289076),
]
TemperatureMeasurement.bulk_save(points)
```

## Auto Generation of Measurements

You can automatically generate measurement classes file with the bash command *autogenerate*

```bash
influxable autogenerate  # (default to auto_generate_measurement.py)
influxable autogenerate -o measurement.py
```

## Influxable commands

- **autogenerate**: automatic generation of measurement classes

```bash
influxable autogenerate
influxable autogenerate -o measurement.py
```

- **populate**: create a measurement filled with a set of random data

```bash
influxable populate
influxable populate --min_value 5 --max_value 35 -s 2011-01-01T00:00:00 -id 1
influxable populate --help
```

## Testing

First, you need to install pytest via the file *requirements-test.txt*

```bash
pip install -r requirements-test.txt
```

Then, you can launch the *pytest* command:

```bash
pytest -v
```

## Supporting

Feel free to post issues, your feedback or if you reach a problem with influxable library.

If you want to contribute, please use the pull requests section.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/Javidjms/influxable/releases).

## Contributors

- [Javid Mougamadou](https://github.com/Javidjms)

## Credits

- Logo designed by [Maxime Bergerard](https://github.com/maximebergerard)

## References

- [InfluxDB Website](https://docs.influxdata.com/platform/introduction)
- [InfluxDB Github Repository](https://github.com/influxdata/influxdb)
- [InfluxDB-Python Github Repository](https://github.com/influxdata/influxdb-python)

## License

[MIT](LICENSE.txt)

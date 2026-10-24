You can create an object to fetch the data from an online source. By default, the data source is as follows for Johns Hopkins:

```
```python
covid = Covid()
```
```

Note that due to high traffic, sometimes the servers are unresponsive. I experienced this multiple times.

You can explicitly mention the data source as follows:

```
```python
covid = Covid(source="john_hopkins")
```
```

You can specify Worldometers explicitly as follows:

```
```python
covid = Covid(source="worldometers")
```
```

You can see the source of the data as follows:

```
```python
covid.source
```
```

Based on the data source, this returns a relevant string, as shown here:

```
```python
'john_hopkins'
```
```

You can get status by country name as follows:

```
```python
covid.get_status_by_country_name("italy")
```
```

This returns a dictionary, as follows:

```
```python
{'id': '86',
 'country': 'Italy',
 'confirmed': 4188190,
 'active': 283744,
 'deaths': 125153,
 'recovered': 3779293,
 'latitude': 41.8719,
 'longitude': 12.5674,
 'last_update': 1621758045000}
```
```
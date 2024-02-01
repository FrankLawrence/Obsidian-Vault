# Data acquisition
```python
import numpy as np
import pandas as pd
pd.options.display.max_rows = 10
np.set_printoptions(precision=4, suppress=True)
```
# Reading csv tables
```python
# read table
pd.read_csv('example.csv')
# specify seperator
pd.read_csv('example.csv', sep=',')
pd.read_csv('example.csv', sep='\s+')
# specify header row
pd.read_csv('example.csv', header=None)
pd.read_csv('example.csv', names=['a','b','c','d','message'])
# index columns on the left
names=['a','b','c','d','message']
pd.read_csv('example.csv', names=names, index_col='message')
pd.read_csv('example.csv', index_col=['key1','key2'])
# skip rows in document
pd.read_csv('example.csv', skiprows=[0,2,3])
# show specific rows
pd.read_csv('example.csv', nrows=5) # first 5 rows
```
# Reading JSON Data
```json
obj = """
{"name": "Wes",
 "places_lived": ["United States", "Spain", "Germany"],
 "pet": null,
 "siblings": [{"name": "Scott", "age": 30, "pets": ["Zeus", "Zuko"]},
              {"name": "Katie", "age": 38,
               "pets": ["Sixes", "Stache", "Cisco"]}]
}
"""
```
```python
import json
# load json data
result = json.loads(obj)
# print data in json format
asjson = json.dumps(result)
# table from entry
pd.DataFrame(result['siblings'], columns=['name', 'age'])
```
---
```json
// example.json
[{"a": 1, "b": 2, "c": 3},
 {"a": 4, "b": 5, "c": 6},
 {"a": 7, "b": 8, "c": 9}]
```
```python
# read table from json
data = pd.read_json('example.json')
# print in json format
print(data.to_json())
```
# XML and HTML: Web Scraping
## parsing HTML
```python
tables = pd.read_html('example.html')
tables[0]
# first 5 lines
failures = tables[0].head()
# print values in one column
close_timestamps = pd.to_datetime(failures['Closing Date']) # convert the month -day-year dates in a column to a different time format
close_timestamps.dt.year.value_counts() # print occurences of the dates
```
## parsing XML
```python
from lxml import objectify
path = 'Performance_MNR.xml'
parsed = objectify.parse(open(path))
root = parsed.getroot()
```
```python
data = []
skip_fields = ['PARENT_SEQ'] # fields(columns) to ignore when appending data
for elt in root.INDICATOR: # for each item
	el_data = {}
	for child in elt.getchildren(): # for each tag in each item
		if child.tag in skip_fields:
			continue
		el_data[child.tag] = child.pyval # add the value of the correct tags
	data.append(el_data)
# print out in table
pd.DataFrame(data)
```
# Binary Data Formats
## Microsoft Excel Files
```python
!pip install xlrd
!pip install openpyxl
xlsx = pd.ExcelFile('example.xlsx', engine='openpyxl')
# print out in table
pd.read_excel(xlsx, 'Sheet')
```
## Interacting with web APIs
```python
import requests
url = 'https://example_with_json.com'
resp = requests.get(url)
# get title of first entry
data = resp.json()
data[0]['title']
# use tags of entries as column titles
pd.DataFrame(data, columns=['number', 'title', 'labels', 'state'])
```
## Interacting with databases
```python
import sqlite3
query = """
CREATE TABLE test
(a VARCHAR(20), b VARCHAR(20),
 c REAL,        d INTEGER
);"""
con = sqlite3.connect('mydata.sqlite')
con.execute(query)
con.commit()
```
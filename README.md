## WARC diff tools   [![Build Status](https://travis-ci.org/harvard-lil/WARC-diff-tools.svg?branch=master)](https://travis-ci.org/harvard-lil/WARC-diff-tools)
#### *P.S.A This is a work in progress, so please use at your own risk!*

All suggestions / issues / pull requests are welcome.

Compare two WARC files to find the differences between them using various methods.

WARC compare is currently set up for Python 2.7 only
WARC comparison works much faster in pypy (installation instructions here: http://doc.pypy.org/en/latest/install.html)

Install:
```
$ git clone https://github.com/harvard-lil/WARC-diff-tools
$ cd WARC-diff-tools
$ cp config/settings.example.py config/settings.py
$ pip install -r requirements.txt
```


Set up DB:
```
$ psql
$ createdb warcdiff
$ ./manage.py migrate
```

### Examples
To set up (in python repl):
```python
from warc_compare import WARCCompare
wc = WARCCompare('path/to/older/warc', '/path/to/newer/warc')
```

You now have access to a list of resources in the WARCS:
```python
wc.resources
```
output is a list of resources split up by the keys:
- missing (resources that existed in the old WARC but have been removed, meaning maybe they are no longer there or maybe the path has changed)
- added (resources that did not exist in the old WARC and have been added in the new WARC)
- modified (resources that have existed in both (meaning, the path is the same for both) but one of them is not like the other
- unchanged (resources that have existed in both and nothing has changed)

```python
wc.resources['added']
```
to see just a list of added resources

Check if resource has changed (just checks the recorded hash of the resource, so changes might be absolutely negligible) :
```python
wc.resource_changed('/path/to/resource/')
```
output: Boolean, True for changed, False for not changed.

Calculate similarity of all WARC responses using minhash, simhash, and sequence match:
```python
wc.calculate_similarity()
```

Calculate the similarity of resources with dissimilar urls by passing in a list of url pairs
```python
wc.calculate_similarity(url_pairs=[('path/to/older_url1', 'path/to/newer_url2'), ('path/to/older_url3', 'path/to/newer_url4'), ])
```

Calculate just the minhash value of all WARC responses:
```python
wc.calculate_similarity(simhash=False, sequence_match=False)
``` 

Create HTML comparison diffs of a single response:
```python
wc.get_visual_diffs('/path/to/resource')
```
output: HTML with deletions marked up, HTML with insertions marked up, HTML with both deletions and insertions marked up

Create HTML comparison diffs of a single response with two different paths (if the resource has been moved or renamed, for instance):
```python
wc.get_visual_diffs('/path/to/resource', 'path/to/other/resource')
```





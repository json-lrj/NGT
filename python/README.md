
# python NGT

[日本語](README-jp.md)

## Install

You **MUST** install the NGT library according to the [README](../README.md#build) before installing python NGT.
Python bindings with ctypes (ngt) and pybind11 (ngtpy) are both installed as follows.

```
cd NGT_ROOT/python
python setup.py sdist
pip install dist/ngt-1.2.0.tar.gz
```

## Documents

[ngtpy (pybind11) reference](README-ngtpy.md)

## Simple samples

### ngt (ctypes)

```python
  from ngt import base as ngt
  import random

  dim = 10
  objects = []
  for i in range(0, 100) :
      vector = random.sample(range(100), dim)
      objects.append(vector)

  query = objects[0]
  index = ngt.Index.create(b"tmp", dim)
  index.insert(objects)
  # You can also insert objects from a file like this.
  # index.insert_from_tsv('list.tsv') 

  index.save()
  # You can load saved the index like this.
  # index = ngt.Index(b"tmp")

  result = index.search(query, 3)

  for i, o in enumerate(result) :
      print(str(i) + ": " + str(o.id) + ", " + str(o.distance))
      object = index.get_object(o.id)
      print(object)
```

### ngtpy (pybind11)

ngtpy(pybind11) can reduce the processing times than ngt(ctypes). It is more effective especially for the short search time. 

```python
  import ngtpy
  import random

  dim = 10
  objects = []
  for i in range(0, 100) :
      vector = random.sample(range(100), dim)
      objects.append(vector)

  query = objects[0]

  ngtpy.create(b"tmp", dim)
  index = ngtpy.Index(b"tmp")
  index.batch_insert(objects)
  index.save()

  result = index.search(query, 3)

  for i, o in enumerate(result) :
      print(str(i) + ": " + str(o[0]) + ", " + str(o[1]))
      object = index.get_object(o[0])
      print(object)
```

See also [sample.py](sample/sample.py).

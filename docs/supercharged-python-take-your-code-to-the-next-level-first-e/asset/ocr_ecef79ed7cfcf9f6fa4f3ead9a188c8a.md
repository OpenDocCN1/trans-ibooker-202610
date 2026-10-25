```
try:
    item = pickle.load(f)
except EOFError:
    print('Loaded', len(loaded), 'items.')
    break

print(type(item), item)
loaded.append(item)
```

# 8.13 USING THE “SHELVE” PACKAGE

The `shelve` package builds a filewide database on top of the `pickle` interface. The former contains the ability of the latter, so that you don’t have to import both at the same time.

```
import shelve
```

The interface to this package is simple. All you need to do is to open a file through `shelve.open`, which provides a direct entrée into the shelving interface. The object returned can then be used as a virtual dictionary.

Click here to view code image

```
shelf_obj = shelve.open(db_name)
```

You can choose any name you wish for `shelf_obj`, which is only a variable, of course. The `db_name` is the same as the file name, minus its `.db` extension, which is automatically added to the name of the disk file or device.
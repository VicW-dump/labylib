![Labylib](https://storage.googleapis.com/public.victorwesterlund.com/github/VictorWesterlund/labylib/labylib.png)

### Cosmetics API for Labymod
![GitHub release (latest by date including pre-releases)](https://img.shields.io/github/v/release/VictorWesterlund/labylib?include_prereleases)
![GitHub last commit](https://img.shields.io/github/last-commit/VictorWesterlund/labylib)
![Maintenance](https://img.shields.io/maintenance/yes/2021)

Modify LabyMod cosmetics programmatically with Python.

|![VicW](https://crafatar.com/renders/body/53c40674-f0a2-4f95-9ce1-479bdd1d8b67?scale=2) | Created by VicW | 
|--|--|

_labylib or Victor Westerlund is in no way sponsored by or affiliated with LabyMod or LabyMedia GmbH._<br>
_This program is offered as-is and might stop working at any time_

## Installation
1. Download and install [Python 3](https://www.python.org/downloads/) for your architecture.
2. Clone this repo to your machine, or [download a zip](/VictorWesterlund/labylib/archive/master.zip)
```bash
$ git clone https://github.com/VictorWesterlund/labylib/
$ gh repo clone VictorWesterlund/labylib
```

## Quickstart
**1. Import a cosmetics module from `labylib/<cosmetic>`.**

[A list of all modules and classes can be found here](https://github.com/VictorWesterlund/labylib/wiki/Module-reference-sheet)
```python
from labylib import Cape
```
**2. Initialize a module class.**

All labylib classes take a `PHPSESSID` as their first argument.

_Example with `Cape` where a file-path is expected as a second argument:_
```python
texture = Cape.Texture("<String PHPSESSID>","<String PATH_TO_PNG>") # labylib = Cape.Texture("772nnas663jkc8ahbb2","/home/VicW/coolCape-2.png")
```

**3. Submit a cosmetic update**
```python
texture.update()
```
Python's [Built-in-exceptions](https://docs.python.org/3/library/exceptions.html#exception-hierarchy) are rasied as needed for missing texture files (`FileNotFoundError`) etc. If a request was sucuessfully sent to the Labymod endpoint, but it returned something falsey (not `OK`). A custom-defined `RequestError` exception will be raised; containing the response received from the endpoint.
```python
try:
  texture.update()
except RequestError as error:
  print("Caugh RequestError exception:" + error)
# "Caugh RequestError exception: Session expired."
```

## Advanced usage
### HTTP POST Headers
Request headers and cookies can be accessed and modified pre-submission by applying standard list modifications on the followin objects: `self.headers` and `self.cookies`
```python
texture = Cape.Texture("<String PHPSESSID>","<String PATH_TO_PNG>")

texture.headers["Origin"] = "https://example.com/"
texture.cookies["Foo"] = "Bar"

labylib.update()
```
### HTTP POST Body
Binary form-data can be appended by calling `self.appendBinaryFormData(name,payload)`.
```python
texture = Cape.Texture("<String PHPSESSID>","<String PATH_TO_PNG>")

texture.appendBinaryFormData(b"foo",b"bar")
texture.appendBinaryFormData(b"file","/home/VicW/home/VicW/coolCape-2.png") # Note that 'payload' is a String in this case (as opposed to Binary)
```
Special form-data types ('names'):
|name|Description
|--|--|
|`'file'`| Submit cosmetic texture file as BLOB by passing `payload` a binary-encoded PNG.<br>_`self.bOpen()` can be used to 'open()' a file as binary string._

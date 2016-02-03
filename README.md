# pyqgis_wrappers
Common functions to be used in pyqgis and python qgis plugins

**NOTE:** Just a sandbox for now.  Don't consider the API stable.

These are wrappers around common pyqgis idioms that allow for faster development and hopefully remove
some of the (sometimes) ugly C/C++ based API (read: not Python-ish)

The current plan is to flesh out ideas here and then migrate them to core QGIS as they become stable or well designed.

What can it do so far?
----------------------

### Opening a project

```
with qgisapp(sys.argv, guienabled=True) as app:
    canvas = QgsMapCanvas()
    with open_project(pfile, canvas=canvas) as project:
        print project
```

or without the `with` block

```
with qgisapp(sys.argv, guienabled=True) as app:
    canvas = QgsMapCanvas()
    project = open_project(pfile, canvas=canvas)
    print project
```

### Rendering a composer template from a file.

```python
import sys
from qgis.gui import QgsMapCanvas
from wrappers import render_template, map_layers, open_project, qgisapp

pfile = r"project.qgs"
template = r"template.qpt"

with qgisapp(sys.argv, guienabled=True) as app:
    canvas = QgsMapCanvas()
    with open_project(pfile, canvas=canvas) as project:
        settings = project.map_settings
        render_template(template, settings, canvas, r"out.pdf")
```

or without the `with` block

```python
with qgisapp(sys.argv, guienabled=True) as app:
    canvas = QgsMapCanvas()
    project = open_project(pfile, canvas=canvas)
    settings = project.map_settings
    render_template(template, settings, canvas, r"out.pdf")
    project.close()
```

### Listing loaded layers

```python
from wrappers import map_layers
layers = map_layers()
mylayer = map_layers(name='mylayer')
```

filter by regex

```
from wrappers import map_layers
for layer in map_layers(".*Bound.*"):
    print layer.name()
```

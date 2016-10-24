PostGIS Projector
=================

The PostGIS Projector provides an SQLAlchemy module to describe the `spatial_ref_sys` table in PostGIS.
It is designed to provide a unified way to interact with coordinate reference information
in GIS projects that incorporate both Python and PostGIS.

Install using `pip install pg-projector` to use the version on PyPI.

Features
--------

* A SQLAlchemy model for a `Projection` object based on the `spatial_ref_sys` table
* A module-level registry of important SRIDs, for creating database models adapted to different
  geographic scenarios.
* Generates `wkt`, `proj4` and `rasterio`-compatible CRS mappings from database objects
- Compatible with both standard and custom projections
- Optional `Flask` integration

The codebase is small and (hopefully) pretty self-explanatory.

Examples
--------

## Using the global SRID store

```python
from pg_projector import setup_names
setup_names(
    mars=949900,
    earth=4326)
```

In another file:
```python
from sqlalchemy import Column, Integer, declarative_base
from geoalchemy2 import Geometry
from pg_projector import srid

Model = declarative_base()
class MarsGeometry(Model):
    id = Column(Integer, primary_key=True)
    geom = Column(Geometry('Polygon',srid.mars))
```


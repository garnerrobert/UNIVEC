# Quick start
_UNIVEC version - 0.52.0 pre-alpha_

This page offers a basic guide to constructing a UNIVEC file and introduces simple parameters.

## Beginning a UNIVEC file
### The declaration
It is not strictly necessary to include a declaration at the beginning of a UNIVEC file, however it is considered good practice to do so.

A declaration will contain the magic number ``^UNIVEC``, the version of UNIVEC used and the encoding of the file.

It is usually situated on line 1.

For example -

``^UNIVEC 0.52.0 utf-8^``

Note how there is nothing specifying what the individual components of the declaration are, this information is assumed given the presence of the magic number ``^UNIVEC``.

> [!WARNING]
> Ensure the file has the extension ``.uvc``.

### Specifying the coordinate reference system
In order for GIS software to know how to visualise your data, you need to specify the coordinate reference system (CRS) your data are in.

You can use any CRS from any registry you wish (remember - UNIVEC stores, but does not interpret, the data), however, you will obviously need to ensure compatibility with the destination GIS software.

To specify the CRS, use ``!CRS``.

For example,

``!CRS EPSG:4326!``

When ``!CRS`` is present, the final ``!`` is expected and signifies the end of the CRS information.

This is usually on line 2 or 3, except if there is a comment before it.

#### Specifying a CRS using well-known text
UNIVEC supports the [Open Geospatial Consortium well-known text standard](https://www.ogc.org/standards/wkt-crs/) for representation of coordinate reference systems.

Below is how to include the WKT in a UNIVEC file -

```
!!CRS
WKT as per OGC standard
!!
```

## The geospatial data
UNIVEC supports three types of geospatial vector data - Point, Line and Polygon.

### Declaring the geometry type
Before adding a feature, it is essential to declare the geometry type of that feature.

This is done using the following -

  ``[GTYPE POINT`` for points (single, standalone locations)
  
  ``[GTYPE LINE`` for lines (lines connecting two or more locations)
  
  ``[GTYPE POLYGON`` for polygons (closed shapes with defined borders)

#### Single geometry types across entire files
It is possible to declare a single geometry type covering all features in the one file.

To do this, after the CRS information, include -

  ``&GEOALL POINT&`` for points

  ``&GEOALL LINE&`` for lines

  ``&GEOALL POLYGON&`` for polygons

When writing the feature, ``[G`` should be used instead of ``[GTYPE`` as shown above.

> [!WARNING]
> If ``[GTYPE`` is used in a single-geometry file, the file will be invalid.

### Adding the geometry data
#### Points
```
[GTYPE POINT id=ID
EASTING,NORTHING
]
```

#### Lines
```
[GTYPE LINE id=ID
EASTING,NORTHING;
EASTING,NORTHING
]
```

#### Polygons
```
[GTYPE POLYGON id=ID
EASTING,NORTHING;
EASTING,NORTHING;
EASTING,NORTHING;
EASTING,NORTHING
]
```

> [!NOTE]
> Elevation, or z-axis, values in coordinates can be placed after the easting and northing values.
> This would make coordinates with a z-axis -
> ```
> EASTING,NORTHING,ELEVATION
> ```

## Attributes
Above you have seen the ``id=ID`` name–value pair attribute in use, however, it is also possible to add any attribute, with some being specified by the UNIVEC standard.

### Attributes specified by the UNIVEC standard
* ``id=`` - the unique identifier for the feature.
  * Must be an integer.
  * Each coordinate within ``LINE`` or ``POLYGON`` features is assigned an ID in the form of a float based on the order of its appearance (e.g. ``1.0``, ``1.1``, ``1.2``) with the first coordinate being assigned ``1.0``, however these float IDs do not need to be specified.
  * It is recommended to include an ``id=`` attribute with every feature.
* ``name=`` - the name of the feature. Is a string.
* ``date=`` - a date value, specified in accordance with [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html).
* ``color=`` - the color of the feature, specified as a hex triplet preceded by a hash.
* ``diameter=`` - the diameter, in millimeters, of an icon marking a ``POINT`` feature.
* ``thickness=`` - the thickness, in millimeters, of a ``LINE`` feature.
* ``opacity=`` - the opacity of a feature, as defined by a percentage.

## Comments
Comments are text that is not interpreted as part of the file.

Enclose comments as such -

``//COMMENT//``

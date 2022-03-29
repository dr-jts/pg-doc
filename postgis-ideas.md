# Geometry Creators

## ST_CloseRing
Creates a closed ring from a LineString
Instead of ST_AddPoint / ST_StartPoint

## ST_MakeRegularPolygon
`ST_MakeRegularPolygon(centerx, centery, radius, npts, [angleOffset] )`

# Geometry Accessors

## ST_Explode
Like ST_Dump, but only returns geometry elements (so simpler and less confusing SQL)

# Geometry Editors

## ST_Combine(geometry or collections, geometry or collection)
Combines two geometries or collections together, returning an appropriate geometry type (which might be atomic or empty if the one or both of the inputs are empty).
Remove empty geometries (perhaps provide option to keep empty atomic geometries?)

# Processing

## Variable-Width Buffer
In JTS Lab.

See this for use case:
https://gis.stackexchange.com/questions/90167/how-to-create-a-buffer-on-a-linestring-with-varying-or-increasing-decreasing-wid

# Overlay

## ST_Overlay
Input: collection or array of polygons
Output: overlay of all polygons (GeometryCollection)

Possible: allow two-input version
Possible: aggregate function

Implementation: initially node/polygonize/point-in-poly to remove holes
Later: OverlayNG enhancement?  FORSE?

## ST_NodeErrors 
Display intersections remaining after ST_Noding fails.

# Line Handling

## ST_LineSubstringByIndex(line, index1, index2)
Extract a subsequence from a line based on segment indices

## ST_LineExtend(line, distance)
Also allow negative distance to extend start of line backwards
See https://gis.stackexchange.com/questions/345463/how-can-i-extend-a-linestring-to-the-edge-of-an-enclosing-polygon-in-postgis

## ST_LineAddPoint(line, point, [ index to add after ]) 
Inserts a point into a LineString at end of the line. Optionally allow specifying the index to add the point after.

## ST_LineMergeWin 
Window function to merge lines in a network, returning an id indicating which path each input line is part of.  Paths can then be merged using ST_LineMerge as needed.

# Affine Transformation

## ST_Viewport
Transform a geometry by translating and scaling to fit a viewport rectangle (actually the envelope of an arbitrary geom)
Optional parameter to specify if scaling is isometric or anamorphic?

AffineTransformation - Resize and Set position
ST_TransScale is too hard to use.  Need a function which sets absolute size of geom, and absolute position

Point Generation
Halton Points

Poisson Disc Point Generation
In a polygon?

# Linear Networks

## Minimum Spanning Tree
https://github.com/banbar/GIS-Programming

## Longest Path

## Shortest Path



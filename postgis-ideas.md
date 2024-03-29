Ideas for new PostGIS functionality.

## Geometry Creators

### ST_CloseRing
Creates a closed ring from a LineString
Instead of ST_AddPoint / ST_StartPoint

### ST_MakeRegularPolygon
`ST_MakeRegularPolygon(centerx, centery, radius, npts, [angleOffset] )`

### COGO / Bearing & Distance

## Geometry Accessors

### ST_Explode
Like ST_Dump, but only returns geometry elements (so simpler and less confusing SQL)

### ST_Boundary with Boundary Node Rule

See `ST_Relate` with Boundary Node Rule.

## Geometry Editors

### ST_Combine(geometry or collections, geometry or collection)
Combines two geometries or collections together, returning an appropriate geometry type (which might be atomic or empty if the one or both of the inputs are empty).
Remove empty geometries (perhaps provide option to keep empty atomic geometries?)

## Processing

### `ST_MinimumDiameter`

Minimum Diameter (width) of the points in a geometry

### `ST_MaximumDiameter`

Maximum Diameter (width) of the points in a geometry. 
This is determined by the furthest two of the points defining the Minimum Bounding Circle.

### Variable-Width Buffer
In JTS Lab.

See this for use case:
https://gis.stackexchange.com/questions/90167/how-to-create-a-buffer-on-a-linestring-with-varying-or-increasing-decreasing-wid

### `ST_Centreline`
Compute centerline(s) of a polygonal geometry.

See:
* <https://github.com/MapServer/MapServer/pull/5854>
* <https://bl.ocks.org/veltman/403f95aee728d4a043b142c52c113f82>
* <https://eox.at/2015/12/curved-labels/>

## Overlay

### ST_Overlay
Input: collection or array of polygons
Output: overlay of all polygons (GeometryCollection)

Possible: allow two-input version
Possible: aggregate function

Implementation: initially node/polygonize/point-in-poly to remove holes
Later: OverlayNG enhancement?  FORSE?

### `ST_Split` with a distance tolerance

### ST_NodeErrors 
Display intersections remaining after ST_Noding fails.

## Line Handling

### ST_LineSubstringByIndex(line, index1, index2)
Extract a subsequence from a line based on segment indices

### ST_LineExtend(line, distance)
Also allow negative distance to extend start of line backwards
See https://gis.stackexchange.com/questions/345463/how-can-i-extend-a-linestring-to-the-edge-of-an-enclosing-polygon-in-postgis

### ST_LineAddPoint(line, point, [ index to add after ]) 
Inserts a point into a LineString at end of the line. Optionally allow specifying the index to add the point after.

### `ST_LineMerge` with Distance Tolerance

Snap line endpoints within distance tolerance to form single closed line.

For use case see <https://gis.stackexchange.com/questions/440285/closing-a-concave-v-shaped-multilinestring-in-postgis-to-form-a-polygon>

### ST_LineMergeWin 
Window function to merge lines in a network, returning an id indicating which path each input line is part of.  Paths can then be merged using ST_LineMerge as needed.


## Linear Referencing

### ST_LineNotBetween

Keep sections of a line which do NOT contain given M value range.
Inverse of `ST_LineBetween`.

See <https://gis.stackexchange.com/questions/427321/st-difference-drops-m-values-how-to-subtract-line-segments>

## Clustering

### `ST_ClusterWithinWin`
Window function version of `ST_ClusterWithin`.

* Preserves attributes
* More flexibiity in how clusters are processed

### `ST_ClusterIntersectingWin`
Is this subsumed by `ST_ClusterWithinWin` ?

### Clustering without Point touches
New function?  Or option to existing one?

## Affine Transformation

### ST_Viewport
Transform a geometry by translating and scaling to fit a viewport rectangle (actually the envelope of an arbitrary geom)
Optional parameter to specify if scaling is isometric or anamorphic?

### ST_TransScale
AffineTransformation - Resize and Set position.
`ST_TransScale` is too hard to use.  Need a function which sets absolute size of geom, and absolute position

### ST_Translate taking two points
* Provide a signature taking two points, which determine the X and Y offsets and source anchor point.
* Also, provide a signature that just takes one target point, which the geometry centroid is moved to.

## Point Generation
### Halton Points

### Poisson Disc Point Generation
In a polygon?

## Linear Networks

### Minimum Spanning Tree
See <https://github.com/banbar/GIS-Programming>

### Longest Path

### Shortest Path



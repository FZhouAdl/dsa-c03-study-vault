---
title: "Geospatial functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-geospatial
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Geospatial functions¶

Geospatial functions operate on [GEOGRAPHY](/sql-reference/data-types-geospatial#label-data-types-geography) and [GEOMETRY](/sql-reference/data-types-geospatial#label-data-types-geometry) and convert GEOGRAPHY and GEOMETRY values to and from other representations (such as VARCHAR).

Sub-category| Function| Notes  
---|---|---  
Conversion / Input / Parsing| [ST_GEOGFROMGEOHASH](/sql-reference/functions/st_geogfromgeohash)| GEOGRAPHY only  
| [ST_GEOGPOINTFROMGEOHASH](/sql-reference/functions/st_geogpointfromgeohash)| GEOGRAPHY only  
| [ST_GEOGRAPHYFROMWKB](/sql-reference/functions/st_geographyfromwkb)| GEOGRAPHY only  
| [ST_GEOGRAPHYFROMWKT](/sql-reference/functions/st_geographyfromwkt)| GEOGRAPHY only  
| [ST_GEOMETRYFROMWKB](/sql-reference/functions/st_geometryfromwkb)| GEOMETRY only  
| [ST_GEOMETRYFROMWKT](/sql-reference/functions/st_geometryfromwkt)| GEOMETRY only  
| [ST_GEOMFROMGEOHASH](/sql-reference/functions/st_geomfromgeohash)| GEOMETRY only  
| [ST_GEOMPOINTFROMGEOHASH](/sql-reference/functions/st_geompointfromgeohash)| GEOMETRY only  
| [TO_GEOGRAPHY](/sql-reference/functions/to_geography)| GEOGRAPHY only  
| [TO_GEOMETRY](/sql-reference/functions/to_geometry)| GEOMETRY only  
| [TRY_TO_GEOGRAPHY](/sql-reference/functions/try_to_geography)| GEOGRAPHY only  
| [TRY_TO_GEOMETRY](/sql-reference/functions/try_to_geometry)| GEOMETRY only  
Conversion / Output / Formatting| [ST_ASGEOJSON](/sql-reference/functions/st_asgeojson)|   
| [ST_ASWKB](/sql-reference/functions/st_aswkb)|   
| [ST_ASBINARY](/sql-reference/functions/st_aswkb)| Alias for ST_ASWKB  
| [ST_ASEWKB](/sql-reference/functions/st_asewkb)|   
| [ST_ASWKT](/sql-reference/functions/st_aswkt)|   
| [ST_ASTEXT](/sql-reference/functions/st_aswkt)| Alias for ST_ASWKT  
| [ST_ASEWKT](/sql-reference/functions/st_asewkt)|   
| [ST_GEOHASH](/sql-reference/functions/st_geohash)|   
Constructor Functions| [ST_MAKELINE](/sql-reference/functions/st_makeline)|   
| [ST_MAKEGEOMPOINT](/sql-reference/functions/st_makegeompoint)| GEOMETRY only  
| [ST_GEOMPOINT](/sql-reference/functions/st_makegeompoint)| Alias for ST_MAKEGEOMPOINT  
| [ST_MAKEPOINT](/sql-reference/functions/st_makepoint)| GEOGRAPHY only  
| [ST_POINT](/sql-reference/functions/st_makepoint)| Alias for ST_MAKEPOINT  
| [ST_MAKEPOLYGON](/sql-reference/functions/st_makepolygon)|   
| [ST_POLYGON](/sql-reference/functions/st_makepolygon)| Alias for ST_MAKEPOLYGON  
| [ST_MAKEPOLYGONORIENTED](/sql-reference/functions/st_makepolygonoriented)| GEOGRAPHY only  
Accessor Functions| [ST_DIMENSION](/sql-reference/functions/st_dimension)|   
| [ST_ENDPOINT](/sql-reference/functions/st_endpoint)|   
| [ST_POINTN](/sql-reference/functions/st_pointn)|   
| [ST_SRID](/sql-reference/functions/st_srid)|   
| [ST_STARTPOINT](/sql-reference/functions/st_startpoint)|   
| [ST_X](/sql-reference/functions/st_x)|   
| [ST_XMAX](/sql-reference/functions/st_xmax)|   
| [ST_XMIN](/sql-reference/functions/st_xmin)|   
| [ST_Y](/sql-reference/functions/st_y)|   
| [ST_YMAX](/sql-reference/functions/st_ymax)|   
| [ST_YMIN](/sql-reference/functions/st_ymin)|   
Relationship and Measurement Functions| [HAVERSINE](/sql-reference/functions/haversine)|   
| [ST_AREA](/sql-reference/functions/st_area)|   
| [ST_AZIMUTH](/sql-reference/functions/st_azimuth)|   
| [ST_CONTAINS](/sql-reference/functions/st_contains)|   
| [ST_COVEREDBY](/sql-reference/functions/st_coveredby)|   
| [ST_COVERS](/sql-reference/functions/st_covers)|   
| [ST_DISJOINT](/sql-reference/functions/st_disjoint)|   
| [ST_DISTANCE](/sql-reference/functions/st_distance)|   
| [ST_DWITHIN](/sql-reference/functions/st_dwithin)| GEOGRAPHY only  
| [ST_HAUSDORFFDISTANCE](/sql-reference/functions/st_hausdorffdistance)| GEOGRAPHY only  
| [ST_INTERSECTS](/sql-reference/functions/st_intersects)|   
| [ST_LENGTH](/sql-reference/functions/st_length)|   
| [ST_NPOINTS](/sql-reference/functions/st_npoints)|   
| [ST_NUMPOINTS](/sql-reference/functions/st_npoints)| Alias for ST_NPOINTS  
| [ST_PERIMETER](/sql-reference/functions/st_perimeter)|   
| [ST_WITHIN](/sql-reference/functions/st_within)|   
Transformation Functions| [ST_BUFFER](/sql-reference/functions/st_buffer)| GEOMETRY only  
| [ST_CENTROID](/sql-reference/functions/st_centroid)|   
| [ST_COLLECT](/sql-reference/functions/st_collect) (Scalar and Aggregate)| GEOGRAPHY only  
| [ST_DIFFERENCE](/sql-reference/functions/st_difference)| GEOGRAPHY only  
| [ST_ENVELOPE](/sql-reference/functions/st_envelope)| Deprecated for GEOGRAPHY  
| [ST_INTERPOLATE](/sql-reference/functions/st_interpolate)| GEOGRAPHY only  
| [ST_INTERSECTION](/sql-reference/functions/st_intersection)| GEOGRAPHY only  
| [ST_INTERSECTION_AGG](/sql-reference/functions/st_intersection_agg) (Scalar and Aggregate)| GEOGRAPHY only  
| [ST_SETSRID](/sql-reference/functions/st_setsrid)| GEOMETRY only  
| [ST_SIMPLIFY](/sql-reference/functions/st_simplify)|   
| [ST_SYMDIFFERENCE](/sql-reference/functions/st_symdifference)| GEOGRAPHY only  
| [ST_TRANSFORM](/sql-reference/functions/st_transform)| GEOMETRY only  
| [ST_UNION](/sql-reference/functions/st_union)| GEOGRAPHY only  
| [ST_UNION_AGG](/sql-reference/functions/st_union_agg) (Scalar and Aggregate)| GEOGRAPHY only  
Utility Functions| [ST_ISVALID](/sql-reference/functions/st_isvalid)|   
H3 Functions| [H3_CELL_TO_BOUNDARY](/sql-reference/functions/h3_cell_to_boundary)| GEOGRAPHY only  
| [H3_CELL_TO_CHILDREN](/sql-reference/functions/h3_cell_to_children)| GEOGRAPHY only  
| [H3_CELL_TO_CHILDREN_STRING](/sql-reference/functions/h3_cell_to_children_string)| GEOGRAPHY only  
| [H3_CELL_TO_PARENT](/sql-reference/functions/h3_cell_to_parent)| GEOGRAPHY only  
| [H3_CELL_TO_POINT](/sql-reference/functions/h3_cell_to_point)| GEOGRAPHY only  
| [H3_COMPACT_CELLS](/sql-reference/functions/h3_compact_cells)| GEOGRAPHY only  
| [H3_COMPACT_CELLS_STRINGS](/sql-reference/functions/h3_compact_cells_strings)| GEOGRAPHY only  
| [H3_COVERAGE](/sql-reference/functions/h3_coverage)| GEOGRAPHY only  
| [H3_COVERAGE_STRINGS](/sql-reference/functions/h3_coverage_strings)| GEOGRAPHY only  
| [H3_GET_RESOLUTION](/sql-reference/functions/h3_get_resolution)| GEOGRAPHY only  
| [H3_GRID_DISTANCE](/sql-reference/functions/h3_grid_distance)| GEOGRAPHY only  
| [H3_GRID_DISK](/sql-reference/functions/h3_grid_disk)| GEOGRAPHY only  
| [H3_GRID_PATH](/sql-reference/functions/h3_grid_path)| GEOGRAPHY only  
| [H3_INT_TO_STRING](/sql-reference/functions/h3_int_to_string)| GEOGRAPHY only  
| [H3_IS_PENTAGON](/sql-reference/functions/h3_is_pentagon)| GEOGRAPHY only  
| [H3_IS_VALID_CELL](/sql-reference/functions/h3_is_valid_cell)| GEOGRAPHY only  
| [H3_LATLNG_TO_CELL](/sql-reference/functions/h3_latlng_to_cell)| GEOGRAPHY only  
| [H3_LATLNG_TO_CELL_STRING](/sql-reference/functions/h3_latlng_to_cell_string)| GEOGRAPHY only  
| [H3_POINT_TO_CELL](/sql-reference/functions/h3_point_to_cell)| GEOGRAPHY only  
| [H3_POINT_TO_CELL_STRING](/sql-reference/functions/h3_point_to_cell_string)| GEOGRAPHY only  
| [H3_POLYGON_TO_CELLS](/sql-reference/functions/h3_polygon_to_cells)| GEOGRAPHY only  
| [H3_POLYGON_TO_CELLS_STRINGS](/sql-reference/functions/h3_polygon_to_cells_strings)| GEOGRAPHY only  
| [H3_STRING_TO_INT](/sql-reference/functions/h3_string_to_int)| GEOGRAPHY only  
| [H3_TRY_COVERAGE](/sql-reference/functions/h3_try_coverage)| GEOGRAPHY only  
| [H3_TRY_COVERAGE_STRINGS](/sql-reference/functions/h3_try_coverage_strings)| GEOGRAPHY only  
| [H3_TRY_GRID_DISTANCE](/sql-reference/functions/h3_try_grid_distance)| GEOGRAPHY only  
| [H3_TRY_GRID_PATH](/sql-reference/functions/h3_try_grid_path)| GEOGRAPHY only  
| [H3_TRY_POLYGON_TO_CELLS](/sql-reference/functions/h3_try_polygon_to_cells)| GEOGRAPHY only  
| [H3_TRY_POLYGON_TO_CELLS_STRINGS](/sql-reference/functions/h3_try_polygon_to_cells_strings)| GEOGRAPHY only  
| [H3_UNCOMPACT_CELLS](/sql-reference/functions/h3_uncompact_cells)| GEOGRAPHY only  
| [H3_UNCOMPACT_CELLS_STRINGS](/sql-reference/functions/h3_uncompact_cells_strings)| GEOGRAPHY only

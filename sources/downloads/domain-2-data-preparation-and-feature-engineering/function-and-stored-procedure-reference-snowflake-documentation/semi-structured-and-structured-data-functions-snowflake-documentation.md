---
title: "Semi-structured and structured data functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-semistructured
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Semi-structured and structured data functions¶

These functions are used with:

  * [Semi-structured data formats](/user-guide/semistructured-data-formats) (including JSON, Avro, and XML)
  * [Semi-structured data types](/sql-reference/data-types-semistructured) (including VARIANT, OBJECT, and ARRAY)
  * [Structured data types](/sql-reference/data-types-structured) (including structured OBJECT, structured ARRAY, and MAP)



## List of semi-structured and structured data functions¶

The functions are grouped by type of operation performed:

  * Parsing JSON and XML data.
  * Creating and manipulating [ARRAYs](/sql-reference/data-types-semistructured#label-data-type-array) and [OBJECTs](/sql-reference/data-types-semistructured#label-data-type-object).
  * Extracting values from semi-structured and structured data (for example, from an ARRAY, OBJECT, or MAP).
  * Converting/casting semi-structured data types and structured data types to/from other data types.
  * Determining the data type for values in semi-structured data (that is, type predicates).



Sub-category| Function| Notes  
---|---|---  
**JSON and XML Parsing**| [CHECK_JSON](/sql-reference/functions/check_json)|   
| [CHECK_XML](/sql-reference/functions/check_xml)|   
| [JSON_EXTRACT_PATH_TEXT](/sql-reference/functions/json_extract_path_text)|   
| [PARSE_JSON](/sql-reference/functions/parse_json)|   
| [PARSE_XML](/sql-reference/functions/parse_xml)|   
| [STRIP_NULL_VALUE](/sql-reference/functions/strip_null_value)|   
**Array/Object Creation and Manipulation**| [ARRAY_AGG](/sql-reference/functions/array_agg)| See also [Aggregate functions](/sql-reference/functions-aggregation).  
| [ARRAY_APPEND](/sql-reference/functions/array_append)|   
| [ARRAY_CAT](/sql-reference/functions/array_cat)|   
| [ARRAY_COMPACT](/sql-reference/functions/array_compact)|   
| [ARRAY_CONSTRUCT](/sql-reference/functions/array_construct)|   
| [ARRAY_CONSTRUCT_COMPACT](/sql-reference/functions/array_construct_compact)|   
| [ARRAY_CONSTRUCT_STRUCTURED](/sql-reference/functions/array_construct_structured)|   
| [ARRAY_CONTAINS](/sql-reference/functions/array_contains)|   
| [ARRAY_DISTINCT](/sql-reference/functions/array_distinct)|   
| [ARRAY_EXCEPT](/sql-reference/functions/array_except)|   
| [ARRAY_FLATTEN](/sql-reference/functions/array_flatten)|   
| [ARRAY_GENERATE_RANGE](/sql-reference/functions/array_generate_range)|   
| [ARRAY_INSERT](/sql-reference/functions/array_insert)|   
| [ARRAY_INTERSECTION](/sql-reference/functions/array_intersection)|   
| [ARRAY_MAX](/sql-reference/functions/array_max)|   
| [ARRAY_MIN](/sql-reference/functions/array_min)|   
| [ARRAY_POSITION](/sql-reference/functions/array_position)|   
| [ARRAY_PREPEND](/sql-reference/functions/array_prepend)|   
| [ARRAY_REMOVE](/sql-reference/functions/array_remove)|   
| [ARRAY_REMOVE_AT](/sql-reference/functions/array_remove_at)|   
| [ARRAY_REPEAT](/sql-reference/functions/array_repeat)|   
| [ARRAY_REVERSE](/sql-reference/functions/array_reverse)|   
| [ARRAY_SIZE](/sql-reference/functions/array_size)|   
| [ARRAY_SLICE](/sql-reference/functions/array_slice)|   
| [ARRAY_SORT](/sql-reference/functions/array_sort)|   
| [ARRAY_TO_STRING](/sql-reference/functions/array_to_string)|   
| [ARRAY_UNION_AGG](/sql-reference/functions/array_union_agg)| See also [Aggregate functions](/sql-reference/functions-aggregation).  
| [ARRAY_UNIQUE_AGG](/sql-reference/functions/array_unique_agg)| See also [Aggregate functions](/sql-reference/functions-aggregation).  
| [ARRAYS_OVERLAP](/sql-reference/functions/arrays_overlap)|   
| [ARRAYS_TO_OBJECT](/sql-reference/functions/arrays_to_object)|   
| [ARRAYS_ZIP](/sql-reference/functions/arrays_zip)|   
| [OBJECT_AGG](/sql-reference/functions/object_agg)| See also [Aggregate functions](/sql-reference/functions-aggregation).  
| [OBJECT_CONSTRUCT](/sql-reference/functions/object_construct)|   
| [OBJECT_CONSTRUCT_KEEP_NULL](/sql-reference/functions/object_construct_keep_null)|   
| [OBJECT_CONSTRUCT_KEEP_NULL_STRUCTURED](/sql-reference/functions/object_construct_keep_null_structured)|   
| [OBJECT_DELETE](/sql-reference/functions/object_delete)|   
| [OBJECT_INSERT](/sql-reference/functions/object_insert)|   
| [OBJECT_PICK](/sql-reference/functions/object_pick)|   
| [PROMPT](/sql-reference/functions/prompt)|   
**Higher-order**| [FILTER](/sql-reference/functions/filter)| See also [Use lambda functions on data with Snowflake higher-order functions](/user-guide/querying-semistructured#label-higher-order-functions).  
| [REDUCE](/sql-reference/functions/reduce)| See also [Use lambda functions on data with Snowflake higher-order functions](/user-guide/querying-semistructured#label-higher-order-functions).  
| [TRANSFORM](/sql-reference/functions/transform)| See also [Use lambda functions on data with Snowflake higher-order functions](/user-guide/querying-semistructured#label-higher-order-functions).  
**Map Creation and Manipulation**| [MAP_CAT](/sql-reference/functions/map_cat)|   
| [MAP_CONSTRUCT](/sql-reference/functions/map_construct)|   
| [MAP_CONTAINS_KEY](/sql-reference/functions/map_contains_key)|   
| [MAP_DELETE](/sql-reference/functions/map_delete)|   
| [MAP_ENTRIES](/sql-reference/functions/map_entries)|   
| [MAP_INSERT](/sql-reference/functions/map_insert)|   
| [MAP_KEYS](/sql-reference/functions/map_keys)|   
| [MAP_PICK](/sql-reference/functions/map_pick)|   
| [MAP_SIZE](/sql-reference/functions/map_size)|   
**Extraction**| [FLATTEN](/sql-reference/functions/flatten)| [Table function](/sql-reference/functions-table).  
| [GET](/sql-reference/functions/get)|   
| [GET_IGNORE_CASE](/sql-reference/functions/get_ignore_case)|   
| [GET_PATH , :](/sql-reference/functions/get_path)| Variation of GET.  
| [OBJECT_KEYS](/sql-reference/functions/object_keys)| Extracts keys from key/value pairs in [OBJECT](/sql-reference/data-types-semistructured#label-data-type-object).  
| [XMLGET](/sql-reference/functions/xmlget)|   
**Conversion/Casting**| [AS_*<object_type>*](/sql-reference/functions/as)|   
| [AS_ARRAY](/sql-reference/functions/as_array)|   
| [AS_BINARY](/sql-reference/functions/as_binary)|   
| [AS_CHAR , AS_VARCHAR](/sql-reference/functions/as_char-varchar)|   
| [AS_DATE](/sql-reference/functions/as_date)|   
| [AS_DECIMAL , AS_NUMBER](/sql-reference/functions/as_decimal-number)|   
| [AS_DOUBLE , AS_REAL](/sql-reference/functions/as_double-real)|   
| [AS_INTEGER](/sql-reference/functions/as_integer)|   
| [AS_OBJECT](/sql-reference/functions/as_object)|   
| [AS_TIME](/sql-reference/functions/as_time)|   
| [AS_TIMESTAMP_*](/sql-reference/functions/as_timestamp)|   
| [STRTOK_TO_ARRAY](/sql-reference/functions/strtok_to_array)|   
| [TO_ARRAY](/sql-reference/functions/to_array)|   
| [TO_JSON](/sql-reference/functions/to_json)|   
| [TO_OBJECT](/sql-reference/functions/to_object)|   
| [TO_VARIANT](/sql-reference/functions/to_variant)|   
| [TO_XML](/sql-reference/functions/to_xml)|   
**Type Predicates**| [IS_*<object_type>*](/sql-reference/functions/is)|   
| [IS_ARRAY](/sql-reference/functions/is_array)|   
| [IS_BOOLEAN](/sql-reference/functions/is_boolean)|   
| [IS_BINARY](/sql-reference/functions/is_binary)|   
| [IS_CHAR , IS_VARCHAR](/sql-reference/functions/is_char-varchar)|   
| [IS_DATE , IS_DATE_VALUE](/sql-reference/functions/is_date-value)|   
| [IS_DECIMAL](/sql-reference/functions/is_decimal)|   
| [IS_DOUBLE , IS_REAL](/sql-reference/functions/is_double-real)|   
| [IS_INTEGER](/sql-reference/functions/is_integer)|   
| [IS_NULL_VALUE](/sql-reference/functions/is_null_value)|   
| [IS_OBJECT](/sql-reference/functions/is_object)|   
| [IS_TIME](/sql-reference/functions/is_time)|   
| [IS_TIMESTAMP_*](/sql-reference/functions/is_timestamp)|   
| [TYPEOF](/sql-reference/functions/typeof)|

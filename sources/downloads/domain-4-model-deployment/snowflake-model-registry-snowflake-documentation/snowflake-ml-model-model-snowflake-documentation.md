---
title: "snowflake.ml.model.Model | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/latest/api/model/snowflake.ml.model.Model
cert_domain: domain-4-model-deployment
crawl_depth: 1
crawled: 2026-08-23
---

# snowflake.ml.model.Model¶

_class _snowflake.ml.model.Model¶
    

Bases: `object`

Model Object containing multiple versions. Mapping to SQL’s MODEL object.

Methods

delete_version(_version_name : str_) → None¶
    

Drop a version of the model.

Parameters:
    

**version_name** – The name of the version.

first() → [ModelVersion](snowflake.ml.model.ModelVersion.html#snowflake.ml.model.ModelVersion "snowflake.ml.model._client.model.model_version_impl.ModelVersion")¶
    

The first version of the model.

get_tag(_tag_name : str_) → Optional[str]¶
    

Get the value of a tag attached to the model.

Parameters:
    

**tag_name** – The name of the tag, can be fully qualified. If not fully qualified, the database or schema of the model will be used.

Returns:
    

The tag value as a string if the tag is attached, otherwise None.

last() → [ModelVersion](snowflake.ml.model.ModelVersion.html#snowflake.ml.model.ModelVersion "snowflake.ml.model._client.model.model_version_impl.ModelVersion")¶
    

The latest version of the model.

rename(_model_name : str_) → None¶
    

Rename a model. Can be used to move a model when a fully qualified name is provided.

Parameters:
    

**model_name** – The new model name.

set_tag(_tag_name : str_, _tag_value : str_) → None¶
    

Set the value of a tag, attaching it to the model if not.

Parameters:
    

  * **tag_name** – The name of the tag, can be fully qualified. If not fully qualified, the database or schema of the model will be used.

  * **tag_value** – The value of the tag




show_tags() → dict[str, str]¶
    

Get a dictionary showing the tag and its value attached to the model.

Returns:
    

The model version object.

show_versions() → DataFrame¶
    

Show information about all versions in the model.

Returns:
    

A Pandas DataFrame showing information about all versions in the model.

unset_tag(_tag_name : str_) → None¶
    

Unset a tag attached to a model.

Parameters:
    

**tag_name** – The name of the tag, can be fully qualified. If not fully qualified, the database or schema of the model will be used.

version(_version_or_alias : str_) → [ModelVersion](snowflake.ml.model.ModelVersion.html#snowflake.ml.model.ModelVersion "snowflake.ml.model._client.model.model_version_impl.ModelVersion")¶
    

Get a model version object given a version name or version alias in the model.

Parameters:
    

**version_or_alias** – The name of the version or alias to a version.

Raises:
    

**ValueError** – When the requested version does not exist.

Returns:
    

The model version object.

versions() → list[[ModelVersion](snowflake.ml.model.ModelVersion.html#snowflake.ml.model.ModelVersion "snowflake.ml.model._client.model.model_version_impl.ModelVersion")]¶
    

Get all versions in the model.

Returns:
    

A list of ModelVersion objects representing all versions in the model.

Attributes

comment¶
    

The comment to the model.

default¶
    

The default version of the model.

description¶
    

The description for the model. This is an alias of comment.

fully_qualified_name¶
    

Return the fully qualified name of the model that can be used to refer to it in SQL.

name¶
    

Return the name of the model that can be used to refer to it in SQL.

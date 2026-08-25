---
title: "snowflake.cortex.CompleteOptions | Snowflake Documentation"
source: https://docs.snowflake.com/en/developer-guide/snowpark-ml/reference/1.6.2/api/model/snowflake.cortex.CompleteOptions.html
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

You are viewing documentation about an older version (1.6.2).  [View latest version](/en/developer-guide/snowpark-ml/reference/1.52.0/index)

# snowflake.cortex.CompleteOptions¶

_class _snowflake.cortex.CompleteOptions(_* args_, _** kwargs_)¶
    

Bases: `dict`

Options configuring a snowflake.cortex.Complete call.

Methods

clear() → None. Remove all items from D.¶
    

copy() → a shallow copy of D¶
    

fromkeys(_value =None_, _/_)¶
    

Create a new dictionary with keys from iterable and values set to value.

get(_key_ , _default =None_, _/_)¶
    

Return the value for key if key is in the dictionary, else default.

items() → a set-like object providing a view on D's items¶
    

keys() → a set-like object providing a view on D's keys¶
    

pop(_k_[, _d_]) → v, remove specified key and return the corresponding value.¶
    

If key is not found, d is returned if given, otherwise KeyError is raised

popitem()¶
    

Remove and return a (key, value) pair as a 2-tuple.

Pairs are returned in LIFO (last-in, first-out) order. Raises KeyError if the dict is empty.

setdefault(_key_ , _default =None_, _/_)¶
    

Insert key with a value of default if key is not in the dictionary.

Return the value for key if key is in the dictionary, else default.

update([_E_ , ]_**F_) → None. Update D from dict/iterable E and F.¶
    

If E is present and has a .keys() method, then does: for k in E: D[k] = E[k] If E is present and lacks a .keys() method, then does: for k, v in E: D[k] = v In either case, this is followed by: for k in F: D[k] = F[k]

values() → an object providing a view on D's values¶
    

Attributes

max_tokens _: typing_extensions.NotRequired[int]_¶
    

Sets the maximum number of output tokens in the response. Small values can result in truncated responses.

temperature _: typing_extensions.NotRequired[float]_¶
    

A value from 0 to 1 (inclusive) that controls the randomness of the output of the language model. A higher temperature (for example, 0.7) results in more diverse and random output, while a lower temperature (such as 0.2) makes the output more deterministic and focused.

top_p _: typing_extensions.NotRequired[float]_¶
    

A value from 0 to 1 (inclusive) that controls the randomness and diversity of the language model, generally used as an alternative to temperature. The difference is that top_p restricts the set of possible tokens that the model outputs, while temperature influences which tokens are chosen at each step.

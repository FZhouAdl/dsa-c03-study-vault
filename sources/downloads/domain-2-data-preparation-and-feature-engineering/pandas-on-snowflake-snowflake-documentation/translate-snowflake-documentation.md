---
title: "TRANSLATE | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions/translate
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

Categories:
    

[String & binary functions](/sql-reference/functions-string) (General)

# TRANSLATE¶

Replaces characters in a string. Specifically, given a string, a set of characters to replace, and the characters to substitute for the original characters, TRANSLATE makes the specified substitutions.

Attention

This function doesn’t translate between languages. See the [TRANSLATE (SNOWFLAKE.CORTEX)](/sql-reference/functions/translate-snowflake-cortex) function for translating text between natural languages.

## Syntax¶
[code] 
    TRANSLATE( <subject>, <sourceAlphabet>, <targetAlphabet> )
    
[/code]

## Arguments¶

`_subject_`
    

A string expression that is translated. If a character in `_subject_` isn’t in `_sourceAlphabet_`, the character is added to the result without any translation.

`_sourceAlphabet_`
    

A string with all characters that are modified by this function. Each character is either translated to the corresponding character in the `_targetAlphabet_` or omitted in the result. A character is omitted in the result if the `_targetAlphabet_` has no corresponding character (that is, has fewer characters than the `_sourceAlphabet_`).

`_targetAlphabet_`
    

A string with all characters that are used to replace characters from the `_sourceAlphabet_`.

If `_targetAlphabet_` is longer than `_sourceAlphabet_`, Snowflake reports the following error:
[code]
    String '(target alphabet)' is too long and would be truncated.
    
[/code]

## Returns¶

This function returns a value of type VARCHAR.

## Collation details¶

Arguments with collation specifications currently aren’t supported. Collation specifications are ignored without returning an error.

## Examples¶

Translate the character `ñ` to `n`:
[code] 
    SELECT TRANSLATE('peña','ñ','n') AS translation;
    
[/code]
[code] 
    +-------------+
    | TRANSLATION |
    |-------------|
    | pena        |
    +-------------+
    
[/code]

Translate `X` to `c`, `Y` to `e`, `Z` to `f`, and remove `❄` characters:
[code] 
    SELECT TRANSLATE('❄a❄bX❄dYZ❄','XYZ❄','cef') AS translation;
    
[/code]
[code] 
    +-------------+
    | TRANSLATION |
    |-------------|
    | abcdef      |
    +-------------+
    
[/code]

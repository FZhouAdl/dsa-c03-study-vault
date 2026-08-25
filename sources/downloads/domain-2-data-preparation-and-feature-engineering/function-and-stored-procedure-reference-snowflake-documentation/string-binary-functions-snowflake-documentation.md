---
title: "String & binary functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-string
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# String & binary functions¶

This family of functions performs operations on a string input value, or binary input value (for certain functions), and returns a string or numeric value.

The functions are grouped by type of operation performed.

Function Name| Binary Input Supported| Collation Supported| Notes  
---|---|---|---  
**General Manipulation**| | |   
[ASCII](/sql-reference/functions/ascii)| | |   
[BIT_LENGTH](/sql-reference/functions/bit_length)| ✔| |   
[CHR , CHAR](/sql-reference/functions/chr)| | |   
[CONCAT , ||](/sql-reference/functions/concat)| ✔| ✔|   
[CONCAT_WS](/sql-reference/functions/concat_ws)| ✔| ✔|   
[INSERT](/sql-reference/functions/insert)| ✔| |   
[LENGTH, LEN](/sql-reference/functions/length)| ✔| |   
[LPAD](/sql-reference/functions/lpad)| ✔| |   
[LTRIM](/sql-reference/functions/ltrim)| | |   
[OCTET_LENGTH](/sql-reference/functions/octet_length)| ✔| |   
[PARSE_IP](/sql-reference/functions/parse_ip)| | |   
[PARSE_URL](/sql-reference/functions/parse_url)| | |   
[REPEAT](/sql-reference/functions/repeat)| | |   
[REVERSE](/sql-reference/functions/reverse)| ✔| |   
[RPAD](/sql-reference/functions/rpad)| ✔| |   
[RTRIM](/sql-reference/functions/rtrim)| | |   
[RTRIMMED_LENGTH](/sql-reference/functions/rtrimmed_length)| | |   
[SOUNDEX](/sql-reference/functions/soundex)| | |   
[SOUNDEX_P123](/sql-reference/functions/soundex_p123)| | |   
[SPACE](/sql-reference/functions/space)| | |   
[SPLIT](/sql-reference/functions/split)| | ✔| Provides partial support for collation. For details, see the documentation of the function.  
[SPLIT_PART](/sql-reference/functions/split_part)| | |   
[SPLIT_TO_TABLE](/sql-reference/functions/split_to_table)| | |   
[STRTOK](/sql-reference/functions/strtok)| | |   
[STRTOK_TO_ARRAY](/sql-reference/functions/strtok_to_array)| | |   
[STRTOK_SPLIT_TO_TABLE](/sql-reference/functions/strtok_split_to_table)| | |   
[TRANSLATE](/sql-reference/functions/translate)| | |   
[TRIM](/sql-reference/functions/trim)| | |   
[UNICODE](/sql-reference/functions/unicode)| | |   
[UUID_STRING](/sql-reference/functions/uuid_string)| | |   
**Full-Text Search**| | |   
[SEARCH](/sql-reference/functions/search)| | |   
[SEARCH_IP](/sql-reference/functions/search_ip)| | |   
**Case Conversion**| | |   
[INITCAP](/sql-reference/functions/initcap)| | |   
[LOWER](/sql-reference/functions/lower)| | |   
[UPPER](/sql-reference/functions/upper)| | |   
**Regular Expression Matching**| | |   
[[ NOT ] REGEXP](/sql-reference/functions/regexp)| | | Alias for RLIKE.  
[REGEXP_COUNT](/sql-reference/functions/regexp_count)| | |   
[REGEXP_EXTRACT_ALL](/sql-reference/functions/regexp_substr_all)| | | Alias for REGEXP_SUBSTR_ALL.  
[REGEXP_INSTR](/sql-reference/functions/regexp_instr)| | |   
[REGEXP_LIKE](/sql-reference/functions/regexp_like)| | | Alias for RLIKE.  
[REGEXP_REPLACE](/sql-reference/functions/regexp_replace)| | |   
[REGEXP_SUBSTR](/sql-reference/functions/regexp_substr)| | |   
[REGEXP_SUBSTR_ALL](/sql-reference/functions/regexp_substr_all)| | |   
[[ NOT ] RLIKE](/sql-reference/functions/rlike)| | |   
**Other Matching/Comparison**| | |   
[CHARINDEX](/sql-reference/functions/charindex)| ✔| ✔| Alias for POSITION. Provides partial support for collation. For details, see the documentation of the POSITION function.  
[CONTAINS](/sql-reference/functions/contains)| ✔| ✔| Provides partial support for collation. For details, see the documentation of the function.  
[EDITDISTANCE](/sql-reference/functions/editdistance)| | |   
[ENDSWITH](/sql-reference/functions/endswith)| ✔| ✔| Provides partial support for collation. For details, see the documentation of the function.  
[[ NOT ] ILIKE](/sql-reference/functions/ilike)| | | Case-insensitive alternative for LIKE.  
[ILIKE ANY](/sql-reference/functions/ilike_any)| | | Case-insensitive alternative for LIKE ANY.  
[JAROWINKLER_SIMILARITY](/sql-reference/functions/jarowinkler_similarity)| | |   
[LEFT](/sql-reference/functions/left)| ✔| ✔|   
[[ NOT ] LIKE](/sql-reference/functions/like)| | |   
[LIKE ALL](/sql-reference/functions/like_all)| | |   
[LIKE ANY](/sql-reference/functions/like_any)| | |   
[POSITION](/sql-reference/functions/position)| ✔| ✔| Provides partial support for collation. For details, see the documentation of the function.  
[REPLACE](/sql-reference/functions/replace)| | |   
[RIGHT](/sql-reference/functions/right)| ✔| ✔|   
[STARTSWITH](/sql-reference/functions/startswith)| ✔| ✔| Provides partial support for collation. For details, see the documentation of the function.  
[SUBSTR , SUBSTRING](/sql-reference/functions/substr)| ✔| ✔|   
**Compression/Decompression**| | |   
[COMPRESS](/sql-reference/functions/compress)| ✔| |   
[DECOMPRESS_BINARY](/sql-reference/functions/decompress_binary)| ✔| |   
[DECOMPRESS_STRING](/sql-reference/functions/decompress_string)| ✔| |   
**Encoding/Decoding**| | |   
[BASE64_DECODE_BINARY](/sql-reference/functions/base64_decode_binary)| | |   
[BASE64_DECODE_STRING](/sql-reference/functions/base64_decode_string)| | |   
[BASE64_ENCODE](/sql-reference/functions/base64_encode)| ✔| |   
[HEX_DECODE_BINARY](/sql-reference/functions/hex_decode_binary)| | |   
[HEX_DECODE_STRING](/sql-reference/functions/hex_decode_string)| | |   
[HEX_ENCODE](/sql-reference/functions/hex_encode)| ✔| |   
[TRY_BASE64_DECODE_BINARY](/sql-reference/functions/try_base64_decode_binary)| | | Error-handling version of BASE64_DECODE_BINARY.  
[TRY_BASE64_DECODE_STRING](/sql-reference/functions/try_base64_decode_string)| | | Error-handling version of BASE64_DECODE_STRING.  
[TRY_HEX_DECODE_BINARY](/sql-reference/functions/try_hex_decode_binary)| | | Error-handling version of HEX_DECODE_BINARY.  
[TRY_HEX_DECODE_STRING](/sql-reference/functions/try_hex_decode_string)| | | Error-handling version of HEX_DECODE_STRING.  
**Cryptographic/Checksum**| | |   
[MD5 , MD5_HEX](/sql-reference/functions/md5)| | | Intended primarily for checksum operations. Not recommended for cryptography.  
[MD5_BINARY](/sql-reference/functions/md5_binary)| | | Intended primarily for checksum operations. Not recommended for cryptography.  
[MD5_NUMBER_LOWER64](/sql-reference/functions/md5_number_lower64)| | | Intended primarily for checksum operations. Not recommended for cryptography.  
[MD5_NUMBER_UPPER64](/sql-reference/functions/md5_number_upper64)| | | Intended primarily for checksum operations. Not recommended for cryptography.  
[SHA1 , SHA1_HEX](/sql-reference/functions/sha1)| | |   
[SHA1_BINARY](/sql-reference/functions/sha1_binary)| | |   
[SHA2 , SHA2_HEX](/sql-reference/functions/sha2)| | |   
[SHA2_BINARY](/sql-reference/functions/sha2_binary)| | |   
**Hash (Non-cryptographic)**| | |   
[HASH](/sql-reference/functions/hash)| ✔| | Allows data types other than string and binary. Not intended for cryptography.  
[HASH_AGG](/sql-reference/functions/hash_agg)| ✔| | Allows data types other than string and binary. Not intended for cryptography.  
**Collation**| | |   
[COLLATE](/sql-reference/functions/collate)| | |   
[COLLATION](/sql-reference/functions/collation)| | |   
**AI Functions**| | |   
[AGENT_RUN (SNOWFLAKE.CORTEX)](/sql-reference/functions/agent_run-snowflake-cortex)| | |   
[AI_AGG](/sql-reference/functions/ai_agg)| | |   
[AI_CLASSIFY](/sql-reference/functions/ai_classify)| | |   
[AI_COMPLETE](/sql-reference/functions/ai_complete)| | |   
[AI_COUNT_TOKENS](/sql-reference/functions/ai_count_tokens)| | |   
[AI_EMBED](/sql-reference/functions/ai_embed)| | |   
[AI_FILTER](/sql-reference/functions/ai_filter)| | |   
[AI_MULTI_EMBED](/sql-reference/functions/ai_multi_embed)| | |   
[AI_REDACT](/sql-reference/functions/ai_redact)| | |   
[AI_SENTIMENT](/sql-reference/functions/ai_sentiment)| | |   
[AI_SIMILARITY](/sql-reference/functions/ai_similarity)| | |   
[AI_SUMMARIZE_AGG](/sql-reference/functions/ai_summarize_agg)| | |   
[AI_TRANSLATE](/sql-reference/functions/ai_translate)| | |   
[CLASSIFY_TEXT (SNOWFLAKE.CORTEX)](/sql-reference/functions/classify_text-snowflake-cortex)| | |   
[COMPLETE (SNOWFLAKE.CORTEX)](/sql-reference/functions/complete-snowflake-cortex)| | |   
[DATA_AGENT_RUN (SNOWFLAKE.CORTEX)](/sql-reference/functions/data_agent_run-snowflake-cortex)| | |   
[THREAD_MESSAGES (SNOWFLAKE.CORTEX)](/sql-reference/functions/thread_messages-snowflake-cortex)| | |   
[EMBED_TEXT_768 (SNOWFLAKE.CORTEX)](/sql-reference/functions/embed_text-snowflake-cortex)| | |   
[EMBED_TEXT_1024 (SNOWFLAKE.CORTEX)](/sql-reference/functions/embed_text_1024-snowflake-cortex)| | |   
[ENTITY_SENTIMENT (SNOWFLAKE.CORTEX)](/sql-reference/functions/entity_sentiment-snowflake-cortex)| | |   
[EXTRACT_ANSWER (SNOWFLAKE.CORTEX)](/sql-reference/functions/extract_answer-snowflake-cortex)| | |   
[FINETUNE (SNOWFLAKE.CORTEX)](/sql-reference/functions/finetune-snowflake-cortex)| | |   
[PARSE_DOCUMENT (SNOWFLAKE.CORTEX)](/sql-reference/functions/parse_document-snowflake-cortex)| | |   
[SPLIT_TEXT_MARKDOWN_HEADER (SNOWFLAKE.CORTEX)](/sql-reference/functions/split_text_markdown_header-snowflake-cortex)| | |   
[SPLIT_TEXT_RECURSIVE_CHARACTER (SNOWFLAKE.CORTEX)](/sql-reference/functions/split_text_recursive_character-snowflake-cortex)| | |   
[SENTIMENT (SNOWFLAKE.CORTEX)](/sql-reference/functions/sentiment-snowflake-cortex)| | |   
[SUMMARIZE (SNOWFLAKE.CORTEX)](/sql-reference/functions/summarize-snowflake-cortex)| | |   
[TRANSLATE (SNOWFLAKE.CORTEX)](/sql-reference/functions/translate-snowflake-cortex)| | |   
[COUNT_TOKENS (SNOWFLAKE.CORTEX)](/sql-reference/functions/count_tokens-snowflake-cortex)| | |   
[TRY_COMPLETE (SNOWFLAKE.CORTEX)](/sql-reference/functions/try_complete-snowflake-cortex)| | |   
[SEARCH_PREVIEW (SNOWFLAKE.CORTEX)](/sql-reference/functions/search_preview-snowflake-cortex)| | |

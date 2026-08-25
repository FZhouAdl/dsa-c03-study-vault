---
title: "Encryption functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-encryption
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Encryption functions¶

Encryption functions encrypt or decrypt VARCHAR or BINARY values.

Function| Notes  
---|---  
[ENCRYPT](/sql-reference/functions/encrypt)| Encrypts VARCHAR or BINARY values using a passphrase.  
[DECRYPT](/sql-reference/functions/decrypt)| Decrypts VARCHAR or BINARY values using a passphrase.  
[TRY_DECRYPT](/sql-reference/functions/try_decrypt)| Error-handling version of DECRYPT.  
[ENCRYPT_RAW](/sql-reference/functions/encrypt_raw)| Encrypts BINARY values using a binary key and an initialization vector.  
[DECRYPT_RAW](/sql-reference/functions/decrypt_raw)| Decrypts BINARY values using a binary key and an initialization vector.  
[TRY_DECRYPT_RAW](/sql-reference/functions/try_decrypt_raw)| Error-handling version of DECRYPT_RAW.
